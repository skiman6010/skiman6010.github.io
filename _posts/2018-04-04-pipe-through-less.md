---
published: true
type: post
layout: post
title: Pipe Through less
---
I found an interesting command today while working on some MySQL databases for my company. I was getting annoyed at the spaghetti code that would from out of the table when I used the commandline. 
```
SELECT * FROM TABLE;
```
I would try to use this view my tables. I would have to limit my rows to keep the spaghetti to a minimum. I finally gave up and googled around until I found this command. 
```
pager less -SFX
```

FYI, this is for non-Windows users only. I guess if you used the Bash for Windows tool and then MySQL or Percona Server through that it would work. Anyway, this command tells MySQL to pipe the output through the `less` tool on Unix systems. With the `-SFX` flags we can set it to be a table output we can scroll through with the cursor keys (or J and K if you're a VIM user like me). So run that command and you are set. Any `SELECT` statement after that will display in a nice tabular layout. 

[Source](https://stackoverflow.com/questions/924729/how-to-best-display-in-terminal-a-mysql-select-returning-too-many-fields). Thanks for everything StackOverflow.

Oh if you just want a better layout without using the `less` tool, you can just add `\G` at the end of your command.
```
SELECT * FROM TABLE\G;
```
That will display the rows vertically instead of horizontally. 