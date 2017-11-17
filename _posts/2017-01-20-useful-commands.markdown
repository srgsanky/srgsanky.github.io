---
layout: post
title:  "Useful commands"
date:   2017-01-20
categories: shell bash git vim
---

* TOC
{:toc}

## VIM

### How to replace text inside parenthesis?
[Reference](http://stackoverflow.com/a/11630487/954439)

```
ci(
```

You can use a similar command to replace text inside other matching symbols like quotes.

### How to save the current file as a new file and start working on it?
[Reference](http://stackoverflow.com/a/9927057/954439)

```
:saveas filename
```

## Git

### How to show the whitespaces in a git diff in the command line?
[Reference](http://stackoverflow.com/a/11509388/954439)

```
git diff -R
```


## Bash

### How to cut columns of a line?

```
cut -d '.' -f 1
```

| Parameter     | Desc          |
| ------------- |:-------------:|
| d             | Delimiter     |
| f             | Field number  |

### How to reverse the characters in a line?

```
rev
```

### How to prevent commands from buffering the output?

Pre-pending the following to any command will prevent buffering

```
stdbuf -o0
```
