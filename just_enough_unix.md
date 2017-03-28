##Just Enough Unix
**Wed 29-Mar-2017**
**Cotting House, Rm 107, HBS**


### TOC
* [Introduction & Overview of our services](#intro)
* [Navigating directories](#navigating)
* Working with files 
* Handling large #s of files 
* Customizing your work environment 
* Unix on the HBS compute grid 

**NOTE: Materials for this seminar can be found at [http://bit.ly/bbs_unix](http://bit.ly/bbs_unix)<br>
**NOTE #2: Most of this material is a mashup of my previous work done at
[FASRC Tips & Tricks](https://rc.fas.harvard.edu/wp-content/uploads/2015/03/UnixtricksandTextProcessing.pdf), and work from 
Harvard Informatics [Basic Unix](http://informatics.fas.harvard.edu/basic-unix-workshop.html)
and [Tips & Tricks](http://informatics.fas.harvard.edu/unix-command-line-tips-and-tricks.html),
[Software Carpentry](http://swcarpentry.github.io/shell-novice/reference/), and
[Programming Historian](http://programminghistorian.org/lessons/intro-to-bash).

<a name="intro"></a>
### Introduction & Overview of our services
```bash
# at the unix command line 
whoami 

 
# URL for public PDF on RCS Services
# http://training.rcs.hbs.org/files/rcs_services.pdf
FILE=/var/tmp/rcs_services.pdf; \
  curl -o $FILE "http://training.rcs.hbs.org/files/rcs_services.pdf"; \
  open -a "Adobe Acrobat" $FILE
```

#### Why the shell?
* A shell is a program whose primary purpose is to read commands and run other programs.
* The shell’s main advantages are its high action-to-keystroke ratio, its support for automating repetitive tasks, and that it can be used to access networked machines.
* The shell’s main disadvantages are its primarily textual nature and how cryptic its commands and operation can be.


<a name="navigating"></a>
### Navigating directories

```bash
# Your home directory is where you land when you first log in or open up the Terminal program.
#   Usually you see your home folder name. This might not be true on the compute grid. We'll
#   talk about that later.
# ~ is short notation for your home directory
#
# The prompt '$' means that Unix is waiting for you to type something in.
[rfreeman@rfreeman-mbp: ~]$ 

# Directory Structure: Like windows and macs unix filesystems are divided into
#   directories in a hierarchical fashion. Directories are separated by a forward slash (/)
#   and all files hang off a single top-level directory called root (written as /).
ls -l /
ls -l /Users/
ls -l /Users/$USER

# Working Directory: As you move around the filesystem your working directory will change.
#   Your command prompt will give you the last 'chunk' of your working directory but the
#   pwd (print working directory) command will give you the full path. For example :
[rfreeman@rfreeman-mbp: ~]$ pwd
/Users/rfreeman/
[rfreeman@rfreeman-mbp: ~]$

# Moving around: We need to be able to move about the filesystem. The main command for this is cd (change directory). 
cd /Users
# this takes you back to 'home'
cd ~
# and is the same as
cd /Users/rfreeman

# Can use absolute naviation:
cd /Users/rfreeman/Desktop
#  or relative navigation
cd ~
# period means this directory. Useful in later commmands
cd ./Desktop
cd ~/Desktop
# double-period means go up one directory
cd ../rfreeman/Desktop

# Finally, pushd and popd allow you to hop from one place to another while remembering
#  (or forgetting) where you are / have been:
cd ~
pushd /
pwd
# now at /, let's go back where we were
pushd
pwd
# ok, now at ~, lets go back again
pushd
pwd
# ok, let's hop back again and stay, forgetting or past location
popd
pwd

# Listing Files

## Saw this command in action briefly earlier. Used for listing files in a directory
ls
# same as 
ls .
# same as
ls ~
# same as 
ls /Users/rfreeman

# many options: 
#   to show file types
ls -F
#   to show in listing format
ls -l
#   to show ALL files in listing format
ls -al
#   to show all files in listing format with human-readable sizes
ls -alh
#   or to reverse-sort them by time
ls -Falht
#   Oh! This is the same as
ls -F -a -l -h -t 
# need help on options? show manual entry for ls. Use 'q' to get out of the viewing (pager) program
man ls




```







```
