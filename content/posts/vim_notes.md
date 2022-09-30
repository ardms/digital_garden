---
title: "Vim notes"
date: 2022-08-16
lastmod: 2022-08-16
draft: false
tags: ["Programming","vim","Notes"]
categories: ["Programming"]
---

# General notes along the way for Vim usage

## Difference checking

If you want to check two files for differences in a vertical split 
```
vertical diffsplit file1 file2
```
Then when you would like to change transfer changes of one file to the other `diffget` 
or depending on which window you would like to move the change `diffput`

## Spelling

To enable you start with `set:spell` to navigate through spell errors `]s` and `[s` finally to see options and change use `z=`

## grep and search in files

Use `vim text_to_search dir` use `**/` to search in all directories and subdirectories except those exclude by wildignore in the __vimrc__ file

## Installing in Linux or WLS

1. git clone https://github.com/vim/vim.git
2. cd vim/src
3. ./configure --enable-python3interp=dynamic  --with-features=huge
4. If needed also use `--with-python3-config-dir=~/`
5. sudo make
6. sudo make install

## Copy from windows clipboard to WLS vim

`:'<,'>w !clip.exe`
`:r !powershell.exe -Command Get-Clipboard`

