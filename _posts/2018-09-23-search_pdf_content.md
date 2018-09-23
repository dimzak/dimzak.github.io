---
title: Search content of multiple pdf files from terminal
key: 20180923
tags: linux tricks pdfgrep
---

There are often times where you just want to search something in multiple pdf files.  
Usually it's fine when these files are like 1 or 2 so you can open them and hit `Ctrl + F` to search.  
But what if you want to search a keyword in 20 files? :smile:

Today I wanted to get all search results when looking for the keyword `ECS` in a directory.  
This directory contains pdf books and AWS documentation in pdf format so it was obviously very time consuming  
to open each file separately.  
Luckily there is a tool catering this need, `pdfgrep`.

To install it use below commands:
```javascript
apt-get install pdfgrep
```
or
```javascript
yum install pdfgrep
```
====
To see `pdfgrep options` hit 
```javascript
pdfgrep --help
```

In my case I used below command to search every file for keyword `ECS`:
```javascript
pdfgrep -ri "ECS"
```
and behold all relevant results!