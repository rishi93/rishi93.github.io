---
layout: posts
title: "Cpython notes"
date: 2023-06-15
tags: opensource programming
---
> Cpython is the official implementation of Python. It is written in C (hence the name).

So a Python intrepreter is basically a large complex C program. Let's try to make
sense of it.

## Download the source
The source is hosted at `github.com/python/cpython`.

Every C program starts running from a `main` function. 

The `main` function for Cpython is located inside `Programs/python.c`.
```c
#include "Python.h"

int main(int argc, char **argv) {
    return Py_BytesMain(argc, argv);
}
```

```c
// C extensions should only #include <Python.h> and not
// directly include the other Python header files included by <Python.h>
```

This leads to the `Modules/main.c` file.
```c
int Py_BytesMain(int argc, char **argv) {
    _PyArgv args = {
        // something here
        // The argc and argv are packed into this structure
        // and passed into pymain_main
    }
    return pymain_main(&args);
}
```

This leads to another function in the same file.
```c
static int pymain_main(_PyArgv *args) {
    // TODO: Look into this function
    PyStatus status = pymain_init(args);

    // some grunt work here

    return Py_RunMain();
}
```

Deeper down the rabbit hole, in the same file.
```c
int Py_RunMain(void) {
    int exitcode = 0;

    pymain_run_python(&exitcode);

    pymain_free();

    // handle keyboard interrupt

    return exitcode;
}
```

Finally, we arrive at a function that actually does something interesting.
```c
static void pymain_run_python(int *exitcode) {
    PyInterpreterState *interp = _PyInterpreterState_GET(); // Interpreter state
    PyConfig *config = _PyInterpreterState_GetConfig(interp);// PyConfig

    if (config->run_command) {
        *exitcode = pymain_run_command(config->run_command);
    }
    else if (config->run_module) {
        *exitcode = pymain_run_module(config->run_module, 1);
    }
    else if (main_importer_path != NULL) {
        *exitcode = pymain_run_module(L"__main__", 0);
    }
    else if (config->run_filename != NULL) {
        *exitcode = pymain_run_file(config);
    }
    else {
        *exitcode = pymain_run_stdin(config);
    }
    pymain_repl(config, exitcode);


}
```

Let's take a look at the `pymain_run_command` since that seems to be the most barebones
way of running a line of Python code.

```c
static int pymain_run_command(wchar_t *command) {
    PyObject *unicode, *bytes;
    int ret;

    unicode = PyUnicode_FromWideChar

    bytes = PyUnicode_AsUTF8String(unicode);

    ret = PyRun_SimpleStringFlags(PyBytes_AsString(bytes), &cf);
}
```

### Questions here:
* What is a `wchar_t`? It looks like it means WideChar.
* What is Unicode and UTF8 string?
* It seems like we always do `Py_DECREF()` after we've finished using it.

This function takes us to the `Python/pythonrun.c` file.
```c
int PyRun_SimpleStringFlags(const char *command, PyCompiler *flags) {
    PyObject *m, *d, *v;
    m = PyImport_AddModule("__main__");

    d = PyModule_GetDict(m);

    v = PyRun_StringStringFlags(command, Py_file_input, d, d, flags);

    return 0 else return -1 if error;
}
```

This takes us to another function that uses an interesting structure.
```c
PyObject *PyRun_StringFlags(
    const char *str,
    int start,
    PyObject *globals,
    PyObject *locals,
    PyCompilerFlags *flags
) {
    PyObject *ret = NULL;
    PyArena *arena = _PyArena_New();

    mod = _PyParser_ASTFromString(start, flags, arena); // shortened code

    ret = run_mod(mod, globals, locals, flags, arena); // shortened code
}
```

This leads to a couple of interesting functions. Let's take a look at the `run_mod`
first.

```c
static PyObject* run_mod(...) {
    PyThreadState
    PyCodeObject *co = _PyAST_Compile(...);

    PyObject *v = run_eval_code_obj(tstate, co, globals, locals);

    return v;
}
```

`run_eval_code_obj` leads to `PyEval_EvalCode`

This inturn leads to `PyEval_EvalFrame`. This quickly leads to `_PyEval_EvalFrameDefault`

This is the core functionality of the interpreter.

What is `_Py_HOT_FUNCTION`
This function uses COMPUTED_GOTOS, which is faster than a switch statement, I presume.

This ...