---
layout: posts
title: "Linux tricks"
date: 2023-06-15
tags: linux
---

## Deleting all files with a certain pattern in the filename
```bash
rm *.c # This deletes all the .c files
```

## Deleting all files that do not follow a certain pattern in the filename
Suppose you want to delete all files except .c files
```bash
shopt -s extglob # Set the shell option extglob
rm !(*.c) # Remove all files with filenames that do not follow *.c pattern
shopt -u extglob # Unset the shell option extglob
```

### shopt
shopt means shell option
You can either use `-s` flag to set a particular option.

Or you can use the `-u` flag to unset a particular option.

The `extglob` flag lets you ... (TODO: Fill in after doing some research)