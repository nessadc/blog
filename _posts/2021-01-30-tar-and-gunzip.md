---
layout: post
title: Archiving and Compressing Files 
---

### tar - Archiving
To *archive* files you will use `tar`. `tar` combines many files and directories into one archive file that usually ends in "tar" however this is only by convention. This archive can be a regular file or a device, such as a tape archive. This is where the name of tar comes from: **t**ape **ar**chive.

```
####################
# Basic tar options
####################
c - Create

v - Verbose

f <file_name> - File option, next argument after 'f' flag should be the name of the archive file. Use with the rest of the options to specify a specific archive file.

t - Table of contents mode, use to list contents of archive file 

x - Extract mode

p - Preserve permissions, tar sets the permissions **after** checking the entire archive

z - Filter the archive through gzip, when used with Extract it will decompress, with Create, it will compress

-C - Specify different directory to extract to

--same-owner - Keep the same ownership as exists in the archive, other wise it defaults to the user running the tar command
```

**Examples**

Creating a tar archive
`$ tar cvf archive.tar file1 file2`

Extracting an archive
`$ tar xvf archive.tar`

Extracting an archive to a different directory
`$ tar xvf archive.tar -C /path/to/dir`

See what is inside of archive
`$ tar tvf archive.tar`


### gzip - Compressing
Standard Unix compression program is `gzip` (GNU Zip). Files ending in `.gz` are GNU Zip archives.

To use:
* Compress by running `gzip file`
* Uncompress by running `gunzip file.gz`

### Dealing with .tar.gz files
This is a common filetype that you will encounter on Linux systems. You will first have to uncompress (get rid of the *.gz*) and then extract the archive (the *.tar* part). 

This is the long way and you should build up the muscle memory of this first:
```
$ gunzip file.tar.gz

$ tar xvf file.tar
```
After getting tired of the above, shortcuts can be done numerous ways.

`zcat` is the same as using `gunzip -dc`.

`$ gunzip -dc archive.tar.gz | tar xvf - -C random_dir/`

`$ zcat archive.tar.gz | tar xvf - -C random_dir/`

`tar` comes with a shortcut for `zcat`. You can use the "z" flag. 

Shortcuts for creating+compressing and extracting+decompressing:
```
$ tar zcvf archive.tar.gz file1 file2

$ tar zxcf archive.tar.gz -C /path/to/dir/
```

### Quick note on zip/unzip
To interact with `.zip` files, you will need `zip` and `unzip` on the Linux machine.

Create a zip file

`$ zip archive file1 file2`

Unzip a zip file
`$ unzip archive.zip`

Unzip a zip file to a specific directory
`$ unzip archive.zip -d /path/to/dir`

List contents of a zip file
`$ zip -l archive.zip`
