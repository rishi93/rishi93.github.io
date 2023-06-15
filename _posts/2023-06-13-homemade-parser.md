---
layout: posts
title: "Writing a parser from scratch"
date: 2023-06-13
tags: programming
---

> A parser turns a list of tokens into an Abstract Syntax Tree (AST).

## Why do we need an Abstract Syntax Tree (AST)?
* Abstract Syntax Tree (AST) -> A representation for code
* We want to turn the list of tokens into something easier for the interpreter to
consume.

## Example:
For example: If we wanted to evaluate: 
avg = (min + max) / 2

* We know the order of the operations is something like this: Let's use a tree
to describe it (insert tree diagram here)

* We need to evaluate the subtrees first, and work our way up from the leaves to the
root - a post order traversal.

## Grammar
* The grammatical structure of the language. We move one level up the Chomsky
hierarchy.

* Turning characters into tokens --> Regular language. We need a bigger hammer ->
Context free Grammar (CFG).

## Formal Grammar
* alphabet => a set of atomic pieces
* an infinite string of "strings" that are "in" the grammar.
* A formal grammar defines which strings are valid, and which aren't.
* Can't write down an infinite number of valid strings. Instead, we create a
finite set of rules.

* A terminal --> letter from the grammar's alphabet. You can think of it like
a literal value.
* Nonterminal --> A named reference to another rule in the grammar.

## Backus-Naur form
* Codify the grammar.

## Grammar for Lox expressions
A subset of the language. A handful of expressions.
* Literals: numbers, strings, booleans and nil.
* Unary expressions: A prefix ! or a prefix - sign.
* Binary expressions: +, -, *, / and comparison operators (==, !=, >=, <=, >, <).
* Parentheses: A pair of ( and ) around an expression.

Here's our grammar for those:
```C
expression      ->      literal | unary | binary | grouping;
literal         ->      NUMBER | STRING | "true" | "false" | "nil";
unary           ->      ( "-" | "!" ) expression;
binary          ->      expression operator expression;
operator        ->      "==" | "!=" | "<" | "<=" | ">" | ">=" | "+" | "-" | "*" | "/";
grouping        ->      "(" expression ")";
```

Remove ambiguity:
```C
expression      ->      equality ;
equality        ->      comparison ( ("!=" | "==") comparison)* ;
comparison      ->      term ( (">" | ">=" | "<" | "<=") term)* ;
term            ->      factor ( ("-" | "+") factor)* ;
factor          ->      unary ( ("/" | "*") unary)* ;
unary           ->      ("!" | "-" ) unary | primary;
primary         ->      NUMBER | STRING | "true" | "false" | "nil" | "(" expression ")" ;
```
