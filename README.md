# Introduction to the linux command line and bash-scripting for bioinformaticians

This is a tutorial teaching students and beginners in bioinformatics how to use the linux command-line as well as how to write simple to intermediate bash scripts with the aim of automating tasks.

## Why should I learn how to use Linux, the Linux command line and bash scripting?** 

Using the Linux command line might seem to be a step backward in how we interact with the computer. But as soon as we work with very large datasets, maybe thousands of files each having a few megabyte of data or single files with a few gigabyte of text, we could verify that programs using a Graphical User Interface (GUI) will not be able to handle this amount of data. We would not view text files of the size of the human genome (3.2GB) in Word or store it as a docx file, nore would we open large tables with maybe a few million lines in Excel, or store them in the xlsx format. These programs stop working for considerably smaller files. 
Furthermore, high-performance computing (HPC) environments, essential for many bioinformatics tasks, primarily run on Unix-like operating systems without GUI, and the majority of specialized bioinformatics software are command-line programs.

Advantages of using the command line are:
**Efficiency in Workflows:** Complex analyses can be seamlessly chained together by typing commands—eliminating the need for manual clicks and reducing human error. Commands are executed by specifying their names, options, and arguments in a structured sequence.

**Handling Large-Scale Data:**
Bioinformatics datasets can range from gigabytes to terrabytes in size. The command line enables efficient processing of such datasets without memory constraints.

**Automation:**
Analysing thousands to millions of input files, is far simpler and less error-prone when commands are explicitly written rather than selected via point-and-click. Scripts containing command sequences are reusable and serve as a verifiable record of your workflow.

**Reusablity:** 
Command-line scripts can be saved, shared, and reused, ensuring transparency and consistency in computational work. Once set up, workflows can be modified with little effort. 

Therefore, mastering the Unix command line and Bash shell is indispensable in bioinformatics. 

## How to get started:

You will need a Unix-based system to practice using the command line. The following options exist:
- Linux or macOS (both natively support Unix).
- Windows users: Install a Linux partition alongside Windows or use Windows Subsystem for Linux (WSL). The latter is an option officially supported by  Microsoft’s for running a Linux environment on your Windows computer.

## Terminology
- A **terminal window** (or simply **terminal**) is the interface in which you type commands to communicate with your computer.
- A **shell** is a program that interprets the commands you enter in the terminal and executes them.
- The bash shell is the most widely used shell, but it is not the only shell you could use. Different shells can have different commands and input syntax. We will use the bash shell, since it is the default shell on most Linux distributions as well as on the Mac.

# Using the Linux command line, introduciton outsourced to existing tutorials
The main focus of this introduction will be to show you how to automate data retreaval and data analysis with bash scripts. Therefore we will outsource the quick introduciotn to Linux and using the command line to the following tutorials.

## Replicate what is shown in the videos
Unterstanding the usage of a scripting language requires to replicate what is shown in the videos. Open a terminal window and test all commands shown in the tutorials.

## Text editor
For editing text files that are needed in the following tutorials, I recommend using the nano editor.

```
nano filename.extension
```

This editor is preinstalled on most Linux, macOs and Unix based HPC systems. It is the simplest choice, in particular if no GUI is avaiable.

## Linux command line tutorials:
- Very first and very short introduction. Only gives a simple overview without going into details:
  
https://www.youtube.com/watch?v=MDrc-LaW-vc

- A comprehensive Linux command line tutorial

https://www.youtube.com/watch?v=16d2lHc0Pe8

Please follow the tutorial and try all commands yourself. Just watching someone else doing it is not sufficient to understand the content.

- Learn 50 most important commands in 15 minutes
  
https://www.youtube.com/watch?v=nzjkbQNmXAE
