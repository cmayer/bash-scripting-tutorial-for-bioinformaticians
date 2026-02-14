# The most important bash commands

All bash commands have the following general syntax:
`command [option(s)] argument(s)`

### pwd
Prints the current working directory.

### ls
Lists the content of a directory. For options see examples above.

### cd
cd stands for change directory and allows you to change the current working directory. cd takes no other arguments besides the name of the directory you are changing to. Used without arguments it changes to your home directory.  

**Examples:**
```
cd /home/Leo/data/   Brings you to a new working direcory
cd                   Equivalent to cd ~. Changes to home directory
cd -                 Brings you back to the directory you visited before the                          last cd command.
```
### man
With `man` you can get an online manual on any of the common commands. In particular, it lists all options.

**Examples:**
`man ls`

### more and less

The commands more and less are so called “pager”. They type the content of files and pause the output at every page. Less has more options and is more powerful. Remember: less is more.

Usage:

Once less has been run, and is pausing the output of text, you can scroll up and down using the arrow keys, use pageup and pagedown to scroll up and down by pages, and can press the spacebar to go down a page, and enter to go down one line.

**Examples:**
`less document.txt      Requires tha the file document.txt exists. 

### head and tail

Use the head and tail commands to extract and print the first or last N lines of a file. The default value for N is 10.

**Examples:**
`head measurements.csv`                   Print the first 10 lines of the file.
`head -n 30 measurements.csv`       Print the first 30 lines of the file.
`tail measurements.csv`                   Print the last 10 lines of the file.
`tail -n 30 measurements.csv`       Print the last 30 lines of the file.

### cat
Syntax: `cat [options] list of files or no file
Prints the input again. If no file is specified, read from standard input.

This simple command has interesting applications:

**Concatenate files:**
`cat file1 file2 file3`        Prints the content of the files in the order they are specified.
Together with output redirection this can be used to concatenate files.

**Create and write to a file:**
```
cat > newfile.txt
Your text
Your text
<Ctrl-D>
```
Since this command simply outputs the input again and since the `>`sign redirects the output to a file, this command writes a new file or replaces an existing file. Type Ctrl-D as the end-of-file.
### cp
cp is a basic file copy utility. Its syntax is: 
`cp [options] source-name(s) destination-name`.  
The source-name can contain wild cards. 
If the source is a directory, the -r option (for recursive) is needed. With the -p option the creation data and ownership will be preserved. With the `-f` option, `cp` will not prompt the user whether an existing file should be overwritten.  With the `-n` option this command will not  overwrite existing files.

**Examples:**

Copy multiple files to a directory:
`cp *.html /home/httpd/html/`

Copy all files and subdirectories in the current working directory to the directory `backup` which is on the same level as the current directory. Does not asks whether files should be overwritten.  
`cp -rp  * ../backup/`

If you do
`cp -pr /home/tom/dir1 /home/tom/dir2`  
If dir2 does not exist, it will be created with a copy of the content of dir1. If dir2 exits, you will end up with `/home/tom/dir2/dir1`.
### mv
mv can be used to rename  and a move files or directories.  With the -f option mv will not prompt the user whether an existing file should be overwritten. With the `-i`option, it works in interactive mode and asks whether to overwrite an existing file.  With the -n option it will never overwrite an existing file. Specifying `-f` or `-i` after `-n` overrides the 'no-clobber' behaviour, as the last flag takes precedence.

**Examples:**

Move all the files that match `misplaced*` from the current directory into the subdirectory misc.  
`mv misplaced* misc/`

**Renames a file:**
`mv bad-filename good-filename`

### rm
The rm command deletes (removes) files and directories.

The most useful and dangerous command line options are `-r` and `-f`. But be careful: They can be deadly. With the -r (for recursive) option, rm deletes directories along with all subdirectories and files contained within that directory. With the -f option, files are deleted without further confirmation! This can be helpful if you want to delete a directory tree with a large number of files. The two option can be combined as -rf. **I had students who used this and deleted all their data.**

**Examples:**
Removing a single file:
`rm oldfile.txt`

A group of files can be removed in different ways:
`rm files*`
`rm file1 file2 file3`

Removing a directory, without confirmation, can be done as follows:
rm -rf project-version-1.0

### mkdir
mkdir makes a new directory.

**Examples:**
`mkdir mydirectory`

### wc
This command counts the number of lines, words and bytes of the input.
`wc inputfile.txt`

### grep
grep is a powerful text search tool that can be used on files, piped streams of data, and input. grep can determine the lines a text string appears in or does not appear in. It can return lines numbers, lines before and after a match, and many things more.  
`grep` is very powerful and it has many options. Just type `man grep` to list them all. If a search pattern appears to often in too many files, it is sometimes useful to reduce the normal output of grep with the `-l` option. It tells grep only to return the name of the file the string was found in, instead of all occurrences together with the line of text.

**Examples:**
`grep barath bigtextfile.txt`

Also read the examples in the section about pipes and redirectors.

### history
Prints the history of commands you typed in the order they have been used.
### find
`find` is a powerful tool to find files with certain properties in a specific part of the directory tree. It can list files that have a certain name, or files that where created or modified in a certain period of time.

Often used option are: `-name` match-string, which searches for files that match the given string. The match-string is allowed to contain wild-cards. Used with the -ls option, find provides more information on the files that it has found. With the `-mmin <number> (-mtime [+|-]<number>)` option, files can be filtered according to their last modification date. The behaviour of this option is controlled with an optional sign in front of the number, which becomes obvious in the following example.

**Examples:**
To find all files on the system that begin with “temp”, starting from the root directory “/”:  
`find / -name "temp*" -ls`

To find all files in the directory sub-tree starting in the current directory that end with “.pdf” you may use  
`find . -name *.pdf`

To search for files in the `∼/data` directory which were last modified in a certain period of time.
`find ∼/data -mmin 11`      List files that where last changed exactly 11 minutes ago.
`find ∼/data -mmin -11`    List files that where last changed 11 or less minutes ago.
`find ∼/data -mmin +11`    List files that where last changed 11 or more minutes ago.

### bg and fg
Processes in unix are normally attached to the terminal, which means the user can interact with them, or they can be detached and run silently in the background, even when you are logged off.

Let us suppose you run the `mafft` program, which can be used to align multiple sequences. You probably want that the commend continues running, if you logout of your computer. This is accomplished as follows:

`mafft input.fas > output.fas &`

The `&` (ampersant)  tells unix to run `mafft` into the background. (Note that as long as the terminal exists, `mafft` can still write output to it, even if it runs in the background. Thus it is still connected to the terminal. If you forgot to type `&` or decide later to send the program to the background you can also do the following. Type Ctrl-Z (which stops the process that you are using currently), and then type the `bg` command on the prompt.

This sends `mafft` into the background. A program running in the background which was started from the current terminal can again be brought to the foreground by using the `fg` command. A program that was started in one terminal cannot be can brought to the foreground of another terminal. In particular, after logging out, the processes in the background cannot be brought to the foreground again.

Caution: If background processes in a terminal should not be terminated, but the terminal shall be closed, you need to use the `exit` command in the terminal. If you simply close the window, all background processes are terminated.
### ps
ps allows you to see the processes (programs) running on the system.

ps has five common options `-a`, `-u`, `-x`, `-m`, and `-f`. Newer version of ps don’t require the “-” before the options. With the a option, all processes on the system are shown. With the u option additional user information are displayed. Use the x option to displays an extended listing of processes, including ones that are detached. The m option allows the process names to stretch one line longer for each “m” you use. With the f option ps provides a tree layout of the processes, so you can see which processes are owned by which other processes.

Examples: A simple example to list and classify all running processes, allow them each to extend two lines longer, and show them in a tree layout:  
`ps -auxmmf`
### top
Displays the processes that are using the most resources.
Type "q" to exit top.

**Examples:**
`top`

### kill
The kill command can be used to kill processes or to send signals to processes. There are three command reasons to use kill: (1) If a program should not run anymore, (2) If a daemon or other program needs to be restarted or reset, a kill -HUP can be issued. (3) If a program has gone haywire or otherwise will not go away with a regular kill, a kill -9 can be issued.  Note that a `kill -9`does not allow the process to close files and leave in a clean state. **Best Practice:** Always try a standard kill first, wait a few seconds, and only proceed to kill -9 if the process remains unresponsive.

**Example:**
Use `top` or `ps` to find the process id (pid) of the process you want to kill or send a signal.
Assume, the process has pid=237:
```
kill 237          Normal shutdown for the process with pid 237
kill -9 237       Kill immediately, without allowing process to clean up
kill -HUP 237.    Sends the SIGHUP signal to the process with pid 237.
```
### date
Displays the current date and time to the console.

### gzip
Create a compressed Gnu zip archive for the specified file. `gzip` only handles compression of individual files. If directories shall be archived and compressed, use the `tar`command to create a single file for a collection of files and folders first. Use the `-d`option to decompress files. 
The gzip command has a number of options that can be viewed with `gzip --help`.

Examples:
`gzip large.csv.   Gzips a single file.
`gzip *.fas`                Gzips all fasta files in the current directory individually.

To decompress a gzipped file:
`gzip -d large.csv.gz`
`gzip -d *.fas.gz`

### tar
With the tar command you can (i) bundle files or directory trees to an archive or (ii) extract files from an exiting archive. Use the `-c`option to create an archive and the `-x`option to extract files from an archive. With the `-z`option, archives are compressed directly with gzip. With the `-j`option, files are compressed with bzip2. To view the content of an archive without extracting it use the `-t` option.

**Creating Archives:**
`tar -cvf archive.tar /path/to/directory`                    Creates archive for directory.
`tar -zcvf archive.tar.gz /path/to/directory`            Creates compressed archive.
`tar -zcvf backup.tar.gz folder1 folder2 file.txt`  Archive multiple input files/folders.

**Extracting Archives:**
`tar -xvf archive.tar`  
`tar -zxvf archive.tar.gz                   For compressed archives

**Extract to a Specific Directory:**
`tar -xvf archive.tar -C /target/path`

View the content of an archive without extracting it:
`tar -tf archive.tar`

### ssh
The Secure Shell (`ssh`) command facilitates encrypted connections to remote computers. By using the `-X` or `-Y` options, you can also tunnel graphical interfaces directly to your local machine via X11 forwarding.

**Example:**
`ssh -Y thomas@134.147.001.001`
You will then be asked for the password of the user thomas on the machine 134.147.001.001. After you are logged in, you have a terminal on the remote machine.

### scp
The Secure Copy command, `scp`, transfers files and directories to remote machines over an encrypted `ssh` connection. It functions much like the standard `cp` command, but is designed for data exchange between different hosts. 
As for the cp command you can specify the -p option to preserve modification dates and the -r option to copy directory with all its subdirectories recursively.

To copy a file from a remote computer to the computer you are working a:
`scp user@remote-computer:/remote-path/remote-file /local/path/local-file`
or vice versa
`scp /local/path/local-file user@remote-computer:/remote-path/remote-file`
You will then be prompted for the user password on the remote computer. 

**Example:**

Copy one picture file to the local machine:  
scp thomas@134.147.001.001:data/wurm.jpg /home/thomas/pics/

Copy all files and subdirectories in the data directory on the remote machine into the pics directory on the local machine. Note that the folder data is not copied, but only its content.  
scp -pr thomas@134.147.001.001:data/ /home/thomas/pics/

Note that the folder data is not copied, but only its content.  
Copy the data directory and all its content on the remote machine into the pics directory

on the local machine. scp -pr thomas@134.147.001.001:data /home/thomas/pics/ 11

Note that the folder data copied as well.  
Copy a local file onto a remote computer, you type:

scp /local/path/local-file user@remote-computer:/remote/path/remote-file

### chmod
Since Linux (and unix in general) is a multi-user OS, files must be kept safe from other users. Thus, Linux has very tight security restrictions in which all files and directories have separate access rights for owners, groups, and other users. We have leard above, that these permissions can be listed with the ls -la command. A typical file permission could look as follows: -rwxrw-r–. In this example the first character, reading from left to right, indicates that we have listed a normal file. Furthermore it says, that the owner can read the file ("r"), write to it ("w"), and  execute it ("x").  Group members can read and write to it, but not execute it (see the "-" instead of an "x"), and the other users can read it, but not write or execute it.

chmod allows you to change the file permissions, also called "file modes". The mode format is best described using a few examples. In these examples the "u" should be reads as user, "g" as group, "o" as others, and "a" as all. The character "r" means read permission, "w"  write permission, and "x" execute permission for files or “permission to change to this directory” for directories.

**Examples:**
To remove all permissions from group and others do:
`chmod go-rwx filename`

Read this as follows: From group and others, take away the permission to read, write and execute.

Allow others to execute a file:
`chmod o+x filename`

Allow others to change to a directory:
`chmod o+x directory-name`

To give everyone read and write permissions, but no permission to execute:
`chmod a=rw filename`

To make a directory writeable but not readable to group and others:
chmod go=w directory/

# Supplementary Commands

When working with files: 

```
stat filename         Prints detailed information about creation, last access                          and modification time
file <filename>       Prints basic file format information
fmt <filename>        Prints file in formatted way.
```

Network Functions:
`ping <machine>`

Filesystem Management:

```
alias name="value"           Create an alias for a command or value
chown <file or dir>          Change ownership
chgrp <file or dir>.         Change group
dd ...                       Byte copy
df <directory>               Report file system disk spaces
du <directory>               Report disk usage

rmdir <directory>            Removes empty directories
whereis <program>            Lists information where the binary program is found
which <program>              Lists the path to the program.

users                        displays a list of users currently logged in
```


# Redirecting standard input and output

Unix processes utilize three standard streams: **STDIN** for input, and two output channels, **STDOUT** and **STDERR**. All three can be redirected or piped from one program to the next.

## Redirecting standard input:

Many programs and scripts read user input from the command line while they are running. Instead of reading from the keyboard input, input can also be read from files using the `<` redirect.

Feed the content of a file to the standard input of a program:
`program < filename.txt`

## Redirecting standard output:

The output channels STDOUT and STDERR can be redirected into files or other programs instead of being displayed on the terminal.

Example:  
`program 1> file1 2> file2`
This writes STDOUT (channel 1) to file1 and STDERR (channel 2) to file2.

## Redirecting standard output using pipes:

The standard output (but not the standard error) of a program can be passed immediately to another program by using a pipe "|". The output that is piped is not written to the console.

**Examples:**
`ls | wc`
Pipes the output of `ls` to `wc`, which counts the lines, words and bytes. This yields the number of files and subdirectories found in the current working directory.

`grep homo file | grep sapiens | wc`
First finds all lines in the file "file" which contain the word "homo". These lines are piped to a second grep command, which filters the lines which contain "sapiens". The remaining lines are piped to the `wc`program, which counts the lines.

If the standard output of a program should be written to the console, but a copy of the output should also be written to file, we can use the `tee` command.  
`date | tee date.log`  
The output of the program `date` piped to the `tee` program, which sends the output the screen and also writes it to the specified file.

# Simplifying your work with the terminal and the shell. 

## Scrolling through the history buffer

When typing new commands, you can scroll through the history buffer using the up- and down arrow keys. With the left and right arrow keys, with backspace and by typing, you can edit previous commands and submit them again. Using Ctrl-a and Ctrl-e you can move to the beginning and the end of the line. With Ctrl-k the text behind the curser is cut out and with Ctrl-y it can be pasted back. Furthermore, on most linux systems, text can be selected by clicking on it with the left mouse button and it can be inserted with the middle mouse button. This also works for the linux command line.


## Auto completion - tab is your friend

Commands, directory  and file names can be auto completed by hitting the Tab key. The command or name will be auto completed as far as this can be done unambiguously. When hitting the tab key a second time, a list of all possible completions will be displayed. This is particularly useful when typing long file or directory paths, or commands for which the full name has been forgotten. Furthermore, this feature shows which commands are known.  
Always remember: Tab is your friend.

# Bash scripts

Starts basic, finally covers very advanced topics:
https://www.youtube.com/watch?v=efzQ1ngbgLg



https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger/
https://www.youtube.com/watch?v=9Ix_Kpr-iXQ

https://www.youtube.com/watch?v=boqC9QenshY

