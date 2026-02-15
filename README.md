# Introduction to the linux command line and bash-scripting for bioinformaticians

This tutorial introduces bioinformatics students and beginners to the Linux command line and teaches them to write simple to moderately complex bash scripts for task automation.

## Why should I learn how to use Linux, the Linux command line and bash scripting?** 

Using the Linux command line may initially seem like a step backward in how we interact with computers. However, once we work with large datasets—such as thousands of files, each several megabytes in size, or single files containing several gigabytes of text—it becomes clear that graphical user interface (GUI) programs often fail to handle such data. For example, we wouldn’t open a text file the size of the human genome (3.2 GB) in Microsoft Word, nor would we store it as a .docx file. Similarly, we wouldn’t attempt to open large tabular datasets with millions of rows in Excel or save them as .xlsx files—these programs typically crash or become unresponsive with relatively small files.
Moreover, high-performance computing (HPC) environments, which are essential for many bioinformatics workflows, run primarily on Unix-like operating systems without GUIs and the vast majority of specialized bioinformatics tools are also designed as command-line programs.

Advantages of using the command line are:
**Efficiency of Workflows:** Complex analyses can be seamlessly chained together by typing commands, eliminating the need for manual clicks and reducing human error. Commands are executed by specifying their names, options, and arguments in a structured sequence.

**Handling Large-Scale Data:**
Bioinformatics datasets can range from gigabytes to terrabytes in size. The command line enables efficient processing of such datasets without memory constraints.

**Automation and Reusablity**
Analysing thousands to millions of input files, is far simpler and less error-prone when commands are explicitly written rather than selected via point-and-click. Scripts containing command sequences are reusable and serve as a verifiable record of your workflow.

Therefore, mastering the Unix command line and Bash shell is indispensable in bioinformatics. 

## How to get started:

To get hands-on with the command line, you’ll need a Unix-based system. Here are your options:
- Linux or macOS both natively support the command line.
- Windows users: Install a Linux partition alongside Windows or use Windows Subsystem for Linux (WSL). The latter is an option officially supported by  Microsoft’s for running a Linux environment on your Windows computer.

## Terminology
- A **terminal window** (or simply **terminal**) is the interface in which you type commands to communicate with your computer.
- A **shell** is a program that interprets the commands you enter in the terminal and executes them.
- The bash shell is the most widely used shell, but it is not the only shell you could use. Different shells can have different commands and input syntax. We will use the bash shell, since it is the default shell on most Linux distributions as well as on the Mac.

# Next steps
- get hold of a possibiltiy to pratice
- as an first introduction I recommend a few videos
- replicate what is shown in the videos. This is important for really understanding what you are doing. Open a terminal window and test all commands shown in the tutorials.

## Text editor
For editing text files that are needed in the following tutorials, I recommend using the nano editor.
```
nano filename.extension
```
This editor is preinstalled on most Linux, macOs and Unix based HPC systems. It is the simplest choice, in particular if no GUI is avaiable and should be used for practicing.

## Linux command line tutorials:
As a first introduction I recommend you to follow these videos:

- Very first and very short introduction. Only gives a simple overview without going into details:<br>
  https://www.youtube.com/watch?v=MDrc-LaW-vc

- A comprehensive Linux command line tutorial<br>
https://www.youtube.com/watch?v=16d2lHc0Pe8

- Learn 50 most important commands in 15 minutes<br>
https://www.youtube.com/watch?v=nzjkbQNmXAE

# Linux directory structure, absolute and relative paths

In this Chapter you will learn how a tyipical directory structure of Linux operating systems looks like and how you can navigate in this on the command line. Furthermore, it is shown how one can refer to files and directories using absolute and relative paths.
[Introduciton to Linux directory structure](Linux-directory-strucute-and-paths.md)



# List of the most important bash commmands

[Introduction to the most important bash commands.](https://github.com/cmayer/bash-scripting-tutorial-for-bioinformaticians/blob/main/Bash-command-reference.md)

# Introduction to bash scripting - Part 1

Bash scripting tutorial: [How to download all mitochondiral genomes of Anopheles species from NCBI]() 

