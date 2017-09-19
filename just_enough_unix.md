## Just Enough Unix
**Wed 29-Mar-2017**<br>
**Cotting House, Rm 107, HBS**


### TOC
* [Introduction & Overview of our services](#intro)
* [The Filesystem and Navigating directories](#navigating)
* [Working with files and directories](#working_with_files)
* [Handling large numbers of files](#handling_large_numbers)
* [Customizing your work environment](#customizing_environment)
* [Unix on the HBS compute grid](lsf_commands.md)
* [Things we won't hit -- For Further Study!](#further_study)

**NOTE:**  Materials for this seminar can be found at [http://bit.ly/bbs_unix](http://bit.ly/bbs_unix)<br>


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
* The shell’s main advantages are 
  * its high action-to-keystroke ratio
  * ability to chain simple programs to make larger programs
  * its support for automating repetitive tasks, and
  * it can be used to access networked machines (e.g. AWS or a remote compute cluster)
* The shell’s main disadvantages are its primarily textual nature and how cryptic its commands and operations can be.


<a name="navigating"></a>
### The Filesystem and Navigating directories

An overview of the Unix/Linux filesystem: <br>
![An example Unix filesystem](/images/home-directories.svg)

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


# COMMANDS FORMAT:
#
# cd LOCATION
# ls [options] PATHNAME
# pushd DIRNAME
# pwd
# popd

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

<a name="working_with_files"></a>
### Working with files and directories

```bash
# COMMANDS FORMAT:
#
# mkdir [options] DIRNAME
# rmdir [options] DIRNAME
# cp [options] SOURCE [SOURCE] DESTINATION
# mv [options] SOURCE [SOURCE] DESTINATION
# rm [options] 
# cat [PATHNAME ...]
# head [options] [PATHNAME ...]
# tail [options] [PATHNAME ...]
# less|more [options] [PATHNAME]
# grep [options] PATTERN [PATHNAME ...]


# create directories with mkdir (make directory)
cd ~
mkdir somedir
# make multiple levels with -p option (without it you get an error)
mkdir -p newdir/nodir

# empty directories can be removed with rmdir (remove directory)
#   first one will error
rmdir newdir
# delete lower one first, then upper. Faster way later
rmdir newdir/nodir
rmdir newdir


# copying (cp) and moving/renaming (mv) files
#   this has the standard format of cp/mv source [source] destination
#   if using multiple sources, the destination must be a directory
# make a copy of a file, or make the copy somewhere else
cp file1 file2
cp file1 dir1/file1
cp file1 dir1/file2
cp file1 file2 dir1/
# copy the contents of a folder, or the whole folder & its contents recursively
cp folder1/* dir1/
cp -R folder1/ dir1/

# rename a file, just move it someplace else, or move & rename
mv file1 file2
mv file1 dir1/
mv file1 dir1/file2
# move a folder, or a bunch of things
mv folder1/ folder2/
mv *.txt folder1/ textfiledir/

# let's delete stuff:
#   remove one or more files
rm file1
rm file1 file2
# this will throw an error 
rm folder1/
rm -rf folder1/

## Looking at files & their contents

# Show the contents of a file by concatenating (cat) it
cat file1
# concatenate multiple files by cat'ing them in sequences
cat file1 file2
# and now save to a new file by output redirection
cat file1 file2 > file3
# don't care what order
cat file* > some_order.txt
# we do care about the order:
cat file{1,2,3} > specific_order.txt

# look at the start of a file
head specific_order.txt
#  how about only 5 lines? Special '!$' means last parameter on previous line
head -n 5 !$
# is the same as 
head -n 5 specific_order.txt
# we can do the same thin for the end of the file:
tail !$
tail -n 5 !$
# but if we wish to look thru the file one page at a time:
#   use space to advance by page, b for back, ret for one line, q for quit, ? for help
less specific_order.txt

# finally, what everybody loves... searching inside files with grep
#   let's look at some haiku
cd ~/Desktop
cd data-shell/writing
cat haiku.txt
#  now find the word not (use !$ or tab-complete)
grep not !$
#  find the
grep The !$
#  OK, only the word The
grep -w The !$
#  find lines with line #s, or just the lines w/ word it
grep -n it !$
grep -nw it !$
#  case insensitive
grep -n -w -i The !$
#  well, everything else but 'the'
grep -n -w -v the !$
#  two more... get the count of The
grep -c The haiku.txt
#  find it only at the start of hte line
grep -E "^The" !$


```

<a name="handling_large_numbers"></a>
### Handling Large Numbers of Files
```bash

## Listing directories with too many files (>1K - 2K). Unbearable > 5K
#     this next one will fail as file count passes 5K
ls -al
ls -1    # only asks for name, nothing else

# Let's try a loop
mkdir a/ b/
for file in a*.txt; do
    echo $file
    mv $file a/
done
for file in b*.txt; do
    echo $file
    mv $file b/
done

# let's try this more efficiently
find . -iname "a*.txt" -type f -exec mv -v {} a/ \;

# what about renaming them? or making a copy with a new name?
for file in *.txt; do
    echo $file
    # next line removes the last .suffix
    cp $file ${file%.*}.copy.txt
    # or better...
    cp $file ${file%.*}.copy.${file##*.}
done

## Tranferring files

# Command Format:
# scp [options] [username@host:]PATHNAME [username@host:]PATHNAME
# rsync [options] [username@host:]PATHNAME [username@host:]PATHNAME

# best way is either scp (secure copy) or rsync (remote sync)
#   the latter can still be done locally -- it could be all on a local machine

# secure copy uses the format scp username@server:remotefile localfile
scp -v rfreeman@researchgrid:~/.bashrc ~/.bashrc_remote

# and back the other way
scp -v ~/.bashrc rfreeman@researchgrid:~/.bashrc_laptop

# if you want to do this with a directory, use -r (recursive)
scp -r rfreeman@researchgrid:~/src ~/src_remote

# a better tool to use is rsync:
# using rsync, best cmd-line tool for large copy jobs. Pay attention to final slashes
rsync -av folder1 folder2/	# copy folder1 and subs into folder2/ (optional on destination)
rsync -av folder1/ folder2/	# copy folder1 contents into folder2/

# copy folder1 and subs to folder2 on remote server
rsync -av folder1 rfreeman@researchgrid:folder2

# copying files to regal w/ new datestamps
rsync -a --no-times --size-only --progress folder1 /export/projects/myfolder/mysubdir
```

<a name="customizing_environment"></a>
### Customizing your Environment

Your bash environment on your machine/account, whether local or remote, depends on what is 
in two files: `.bash_profile` and `.bashrc`. These files are read and executed upon login, 
creating a subshell, or executing `bash` or `sh` directly.

If you don't have a `.bash_profile`, or your `.bash_profile` doesn't specifically source `.bashrc` -- read and execute its commands into the current environment -- then customizations don't work.

```bash
cd ~
cat .bash_profile
# it should have something like `source .bashrc`
#   if not, then we'll need to create it
#   use ctrl (^) characters to eXit, Write out, etc. while in nano
nano .bash_profile

.bash_profile -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Source global definitions if this file exists
if [ -f ~/.bashrc ]; then
	. ~/.bashrc
fi
-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

# OK, and now for our .bashrc...
#

.bashrc -=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# Source global definitions
if [ -f /etc/bashrc ]; then
	. /etc/bashrc
fi

# User specific aliases and functions
alias l.='ls -d .*'
alias ll='ls -al'
alias lt='ls -alt'
alias lthead='ls -alt | head'
alias llhead='ll | head'

# set up execution environment
#
# safe:
export PATH=$PATH:$HOME/bin
# not safe:
export PATH=$HOME/bin:$PATH

# and now our command prompt:
#
# fix command prompt
#   time, account, host, & current dir in blue
#   many examples at http://bit.ly/unix_shell_colors
#   and prompt at http://bit.ly/unix_ps1_prompt
#   though be careful of unix platform differences

export PS1="\[\033[1;36m\][\$(date +%T), \u@\h: \W]$\[\033[0m\] "

-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
```

<a name="further_study"></a>
### Things we won't hit:

#### Miscellaneous commands
```bash
wc -l   word count (lines)<br>
|       chaining unix commands with piping<br>
[ab]    match either a or b in filenames<br>
?       match one character in filenames<br>
$(cmd)  insert output from cmd in place<br>
--help  see if command has help screen<br>
--version   see if command will tell you its version<br>
```

#### Line control and Navigation
```bash
Left and right arrow keys   Move the cursor left and right<br>
Ctrl-d  Delete character under cursor<br>
Ctrl-a  Move to the beginning of the line<br>
Ctrl-e  Move to the end of the line<br>
Up arrow    Previous command (you can go through the whole command history this way)<br>
Down arrow  Next command<br>
Ctrl-k  Delete from cursor to the end of the line<br>
```

#### Still useful but less commonly used keystrokes
```bash
Ctrl-w  Delete to beginning of word<br>
Ctrl-r  Search for most recent command containing<br>
Ctrl-xx Move between current cursor position and beginning of line<br>
# You can combine these together. For instance a useful one is Ctrl-a Ctrl-k which goes to the beginning of the line and deletes it.<br>
```

#### Command Control Shortcuts
```bash
Ctrl-s  Stop the output to the screen<br>
Ctrl-q  Continue the output to the screen<br>
Ctrl-c  Kill the currently running command<br>
Ctrl-z  Suspend the currently running command (follow by bg to push into the background)<br>
Ctrl-l  Clears the screen (Useful if your terminal fills up and becomes confusing<br>
```

#### Command History
```bash
!!      Run last command<br>
!mycmd  Run the most recent command that starts with mycmd (e.g. !cd)<br>
!mycmd:p    Print out the command that !mycmd would run (also adds it as the latest command in the command history)<br>
```
