---
description: A list of all my most used terminal commands. I've listed out only the few frequently used ones.
---

# My Most Used Terminal Commands Cheat Sheet


> By default, Ubuntu uses bash and Mac, although has bash, the recommended shell is now ZSH with iTerm2.

> Ubuntu uses *~/.bashrc* for saving configuration.
> On Mac with bash, need to create a ~/.bash-profile file
> Mac with ZSH shell uses ~/.zshrc

# Basics
## 1. ls
* lists all files in a directory
* <code>ls -a</code>: Lists all files including **hidden** ones.
* <code>ls -al</code>: Shows detailed file info

## 2. cd
* <code>cd</code>: returns to *home* directory
* <code>cd ..</code>: returns to previous directory
* <code>cd ../..</code>: returns back or jumps multiple directory

## 3. sudo !!
* <code>sudo !!</code> puts <code>sudo</code> before the previous command.
* For example, if previous command was <code>apt-get update</code> and it returned error due to permission, <code>sudo !!</code> will fix it. Basically it's putting <code>sudo</code> before the previous executed command. Thus, it <code>sudo !!</code> = <code>sudo apt-get update</code>.

# File Related
## 4. cat
* <code>cat</code> basically prints out the content of a file. E.g <code>cat any.txt</code> will print out the text in the file *any.txt*.

## 5. head, tail
* Prints out the first few (<code>head</code>) or last few (<code>tail</code>) lines of a file.
* Default value is 10
* Can flag number of lines with <code>-n</code> flag

## 6. nano
* <code>nano any.txt</code>
* Kinda like defacto **CLI** text editor.
* Available in both Ubuntu and Mac.

## 7. touch
* To quickly create any file in a directoy
* <code>touch any.txt</code> will create a file called *any.txt* in the current directly.

## 8. mkdir, rm
* <code>mkdir sql</code> will create a folder *sql*.
* <code>mkdir -p sql/codes</code> will create the folder *codes* inside folder *sql*. So tree will be <code>/Users/Documents/sql/codes</code> provided current dir is <code>/Users/Documents</code>
* <code>rm any.txt</code> will remove the file *any.txt*

* <code>rm -rf sql</code> will recursively delete everything inside *sql* folder and *sql* folder.

# Move, Copy, Rename

## 9. mv
* Can move a file or rename a file
* <code>mv any.txt /Users/mishkat/Documents/</code> will move *any.txt* file to *Documents* directory.
* <code>mv any.txt mnr.txt</code> will rename *any.txt* to *mnr.txt*

## 10. cp
* Copies file
* <code>cp any.txt /Users/mishkat/Documents</code> will copy *any.txt* file to Documents directory.

# Disk Usage & Processes

## 11. df
* Shows how much space left
* <code>df -h</code> shows human readable info in *Gigabyte*.

## 12. du
* Shows space usage in the current directory
* <code>du -h</code> shows humnan readable space usage in megabyte.

## 13. ps
* Shows current running processes

## 14. Alias
* CMD alias can be set like: <code>alias slockon="xset led named 'Scroll Lock'"</code>.
* Executing <code>slockon</code> next time will enable scroll lock on Ubuntu.

# Finding Stuff

## 15. whereis
* <code>whereis python</code> will show the location of the default Python installation like <code>/usr/bin/python</code>
* Will show all the dir where Python is located in the system.

## 16. which
* <code>which python</code> will show which Python runtime the system is currently using.

## 17. locate
* <code>locate README.md</code> will try to find where README.md file is in the system. Faster than <code>find</code> command as it depends on cache.

# Internet!

## 18. ping
* <code>ping www.google.com</code> will ping Google.

## 19. wget, curl
* Both used to download files from internet.
* <code>wget http://releases.ubuntu.com/18.10/ubuntu-18.10-desktop-amd64.iso</code> will download the Ubuntu 18.10 iso file
* <code>curl http://releases.ubuntu.com/18.10/ubuntu-18.10-desktop-amd64.iso --output ubuntu.iso</code> can be used as well. **REMEMBER** the <code>--output</code> flag.


# Ubuntu System
## 20. apt: Package Manager
* Common CMD are <code>sudo apt-get update && sudo apt-get upgrade</code>


# Zip/Compressed file
## 21. tar
* <code>tar -xvf any.tar.gz</code> will extract the file in the current directory.
* <code>tar -xvf any.tar.gz /Users/mishkat/</code> will extract the file in folder mishkat.


## 22.Zip
* <code>zip -r folder.zip folder</code> will zip the folder named folder recursively which will include all the files and folders in it's subfolder.


# Manipulating I/O

> Upcoming in the roadmap.