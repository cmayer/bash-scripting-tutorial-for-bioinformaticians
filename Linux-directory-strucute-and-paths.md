# The directory structure of Linux computers

Most unix systems have a very similar directory structure for system files and user home directories. The directory at the top is called the **root directory**. All files and subdirectories can be reached from the root directory.  The home directories of the users can be found in the "/home" directory. Normal users will rarely look into files outside of their home directory. Nevertheless, some basic information helps normal users to understand their computer.
The directory structure shown below includes home directories of four normal users, who’s home directories are called Lisa, Alex, Leo and Tom. Leo has three subdirectories in his home folder. The directory data contains two data files and the directory documents contains one pure text document and one Libre-office document.

For linux programs, file extensions are irrelevant. Nevertheless, it is recommended to use file extensions such as .txt and .fas so that users can immediately see file types of their files.  

## File names and absolute paths

When working with files or directories, it is often necessary to specify the path to the file or directory. A path specifies the order in which directories have to traversed in order to reach the destination. Starting point is either be the **root directory**  or the **current working directory**. Since the current working directory has not been introduced, we come back to this case later. In Unix, directories in a path are separated by the "/" characters. Note that the "/" symbol not only separates subdirectories in a path or the directory and the file, but it also depicts the root if a path string starts with a "/".

Abbildung 1: Typical directory structure of linux systems.
![[Linux typical directory structure 1.png]]

**Remark:** A path starting at the root is an absolute path.
- The root directory has the absolute path “/”.

The data folder located in the home directory of Leo contains two files with the filenames
"COI.fas" and "homo.fas".  
- The home directory has the absolute path "/home".  
- Leos home directory has the absolute path "/home/Leo".
- Leos data directory has the absolute path "/home/Leo/data".  
- Leos file COI.fas has the absolute path "/home/Leo/data/COI.fas". 
- The ls program has the absolute path "/bin/ls".

The absolute path to the users home directory can be abbreviated with the tilde symbol: ∼.

## The current working directory

All shells have in common, that commands are executed relative to a working directory the user is in. This is called the current working directory. When opening a new terminal, the working directory is normally the home directory of the user. 

## The pwd command
The pwd command prints the current working directory the user is in.

**Exercise:** Open a terminal window. At the command prompt type pwd and submit the command by pressing return.

## The ls command

The ls command lists the files and sub-directories in the current working directory.

In order to execute this command, simply type `ls` and then submit the command with return. The `-a` option tells `ls` to list all files, including hidden ones. The `-l` option tells `ls` to list files in the long format, one file per line, including information about the file sizes, ownership, and modification dates/times.

**Example:**
**ls -al**
This will list all files in the current directory, including all hidden files, and will list their file sizes, ownership, and date/time information. Other useful options are:

• `-t` files are listed in the order they have been modified. Recently modified files come first.
• `-tr` files are listed in the order they have been modified but most recently modified files are listed last.
• `-h` reports the file size not in bytes with in a human friendly format.  
In order to list only specific files or directories one can specify the name of the file or directory.

**More examples:**
```
ls -al test.text        Lists only the file test.txt.  
ls -al testdir          Lists the content of the directory testdir.  
ls -al *est*            Lists all files or direct. that have “est” in their name.
```

A typical line of output of `ls -l` looks as follows:  
`-rw-r–r– 1 Leo <group> <filesize> <modification date> <name>`
The most left character, here the “-”, tells the type of entry we have listed. Possible characters are a "-" for a normal file, a "d" for a directory, or an "l" for a symbolic link. The following characters show the access permissions for this file (see the chmod command below). The next entry, here a `1`, has different meanings for files and directories. For files it is the number of hard links for this file, for directories it is the number of files and subdirectories contained in this directory. This count includes the current directory and the parent directory, so that this number is 2 for an empty directory.  The following two names are the user name and the group name, who own this file. Finally comes the file size, the date of last modification and the file name.

## Relative paths

A path to a file or directory cannot only be specified as an absolute path, i.e. starting at the root, but also as a so called relative path, which is a path specified relative to the current working directory.  
If the current working directory is “/home/Leo/data”, then the we can excess the file COI.txt with its relative path “COI.txt”. With relative paths we can also access files outside of the current working directory. For this it is important to know the following special relative directories:
``
. specifies the current directory,
.. specifies the parent directory of (one directory up in the directory structure),
``

Examples:
```
ls -al ..             Lists parent directory of current directory  
ls -al ../data    Lists directory data in parent directory if it exists
ls -al /              Lists root directory.  
ls -al /tmp.      Lists tmp directory in root directory. This is an absolute path. 
ls -al tmp.       Lists tmp directory in current directory. This is a relative path.
```
Warning: In Unix all commands and file names are case sensitive. Therefore, you cannot use LS -AL instead of ls -al.

