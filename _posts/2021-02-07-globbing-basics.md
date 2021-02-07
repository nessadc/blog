---
layout: post
title: Globbing Basics
---

Globbing is how the Linux shell carries out "filename expansion". Basically, when the shell encounters a special character (called wildcard characters), it will expand the command to include all filenames that match that specific character. 
The most common wildcard characters to know are `*` and `?`.

The `*` is used to represent any number of characters (including 0 matches). For example, here I'm telling the "ls" command to list any file names in the current directory that start with "hello". The files can have any number of characters after "hello". 

```
$ ls -l hello*
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello1
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello2
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello3
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello4
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_a
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_b
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_c
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_d
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_x
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_y
```

The `?` is used to represent any single character. In the following example, I will only find the files that have "hello" and then one just single character.
```
$ ls -l hello?
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello1
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello2
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello3
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello4
```

### Ranges - []
You can use `[]` to specify a range of characters to match. The shell will match files whose next character is in the range (i.e. enclosed in the square brackets).

```
$ ls -l hello_[xy]
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_x
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_y

$ ls -l hello_[axy]
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_a
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_x
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_y
```

If you put a `!` before the range, this becomes a logical NOT and the shell will not match files whose next character is in the range.

```
$ ls -l hello_[!axy]
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_b
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:26 hello_c
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:27 hello_d
```

### Curly Brackets - {}
If you put a list of terms inside curly brackets, the shell will try to match any filenames that fit any of the terms. This is an OR relationship.

```
$ ls -l {ab*,hello?}
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:23 abcrowl
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:22 abdoul
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:22 abfoul
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:22 absoul
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello1
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello2
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello3
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 20:24 hello4
```

The above is saying: "Match anything that has 'ab' in the beginning and whatever characters after OR anything that has 'hello' and exactly one character after."

Note that you cannot have spaces after commas in the curly brackets.

### Backslash - \
Use backslash to escape any of the above special characters. For example, if you have a file named `hello*`, you would do this `ls -l hello\*`. Warning: it is not recommended to actually name files with these special characters. 

```
$ ls -l hello\*
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello*
```

If you did not use `\`, globbing would take effect and you would see any filename that has "hello" in it.

```
$ ls -l hello*
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello*
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello1
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello2
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello3
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello4
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_a
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_b
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_c
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_d
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_x
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello_y
```

Single quotes also work for the same purpose. 

```
$ ls -l 'hello*'
-rw-rw-r--. 1 vagrant vagrant 0 Feb  7 21:17 hello*
```

```
$ stat 'hello*'
  File: ‘hello*’
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: fd02h/64770d    Inode: 33583764    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/ vagrant)   Gid: ( 1000/ vagrant)
Context: unconfined_u:object_r:user_home_t:s0
Access: 2021-02-07 21:17:49.252343751 +0000
Modify: 2021-02-07 21:17:49.252343751 +0000
Change: 2021-02-07 21:17:49.252343751 +0000
 Birth: -
```
