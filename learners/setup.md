---
title: Setup
---

This course is for you if you have used GitHub Copilot in the past to improve your existing code, but would like to explore it further to:

- Plan and develop software from scratch based on a defined set of project requirements
- Create agent skills to support high level, reusable, multi-step tasks that integrate with external tools, services or data sources
- Improve existing unit test cases and/or generate new unit tests
- Understand the key security risks associated with using AI coding assistants
- Use the Model Context Protocol to invoke custom service based tools
- Understand the fundamentals of using AI coding subagents and multi-agent orchestration

## Skills Prerequisites

The following are necessary to be able to participate in this course:

- You have a basic knowledge of programming in Python (using variables, lists, conditional statements, functions and importing external libraries)
- You have previously written Python scripts or iPython/Jupyter notebooks to accomplish tasks in your domain of work
- You have previously used Microsoft Visual Studio Code to develop code in any language (opening workspaces, editing files, installing extensions)
- You have previously used GitHub Copilot to obtain suggestions to improve your code

It is also helpful but not necessary to have some basic knowledge of Git version control (cloning repositories, making commits, pushing to a remote repository).

## Software Setup

To participate in this course you will need the following installed on your machine:

- Bash shell (Linux, Mac) or Git Bash for Windows
- Git version control (available from the Bash shell or Git Bash for Windows)
- Python version 3.8 or above
- Microsoft Visual Studio Code version 1.98 or above, with the following extensions installed:
  - GitHub Copilot, latest version

If you do not have these installed, see below for installation instructions.
Please open the tab for your operating system and follow the setup instructions.

:::::::::::::::: spoiler

### Windows

#### Bash Shell

We’ll be using Git Bash. If you’ve already installed Git Bash then go to the next section. Otherwise, go to [git for windows](https://gitforwindows.org/) and click Download, then install it. Most of the options can be left on default, but be sure you check these:

- **Choosing the default editor used by Git**: Make sure **Nano** is selected from the drop-down. If you’re comfortable with other editors, feel free to change it, but we recommend Nano - we use it as it’s present on Windows, Mac and Linux. If you change it, you might not quite match what we’re doing on-screen.
- **Adjusting your PATH environment**: Make sure **Git from the command line and also from 3rd-party software** is selected.
- **Choosing HTTPS transport backend**: Make sure **Use the native Windows Secure Channel Library** is selected.
- **Configuring the terminal emulator to use with Git Bash**: Make sure **Use Windows’ default console window** is selected.

#### Git Setup

We’ll be using Git Bash for both git and a shell to run it in.

#### Python

You will need Python 3.8 or above for this course.

NB: You may already have this installed on your system, in which case skip to the part below about testing your Python installation with GitBash.

The “Anaconda3” package provides everything Python-related you will need for the workshop. To install Anaconda, follow the instructions below.

Download the latest Anaconda Windows installer from [Anaconda](https://www.anaconda.com/download), you may need click 'Get Started' and register first. 

Double-click the installer and follow the instructions. **When asked “Add Anaconda to my PATH environment variable”, answer “yes”. It will warn you not to, but it’s required for it to be found by git bash**. After it’s finished, close and reopen any open terminals to reload the updated PATH and allow the installed Python to be found.

Once the Anaconda installation is finished you will be asked if you want the installer to initialize Anaconda3 by running conda init? You should select yes. Alternatively/additionally you will need to run the following command in Git Bash

```bash
conda init bash
```
Then close and reopen GitBash.

Please test the python install open GitBash (or your favorite terminal) and run the following command to verify that the installation was successful.

```bash
cd ~
python
```
You can then type the following to exit:

```bash
quit()
```

**Git Bash might hang on this command and not launch the Python interpreter.** 
**In this case close and reopen git bash and issue the following commands:**

```bash
cd ~
echo 'alias python="winpty python.exe"' >> ~/.bash_profile
source .bash_profile
python
```

Note: for older versions of git bash you will need to use .bashrc rather than .bash_profile

::::::::::::::::::::::::

:::::::::::::::: spoiler

### MacOS

#### Bash Shell

You can open a terminal by opening the “Terminal” application, which can be found in the “Utilities” folder which is in your “Applications” folder.
Z shell is the default shell on Mac, but you can run a Bash shell from the terminal by typing `bash` and pressing enter.

#### Git Setup

To use Git you must install the Apple Command Line Tools, this may take a few minutes.

You can obtain these [from Apple](https://idmsa.apple.com/IDMSWebAuth/signin.html?path=%2Fdownload%2Fall%2F%3Fname%3Dcommand%2520line%2520tools%2520for%2520xcode%252012&appIdKey=891bd3417a7776362562d2197f89480a8547b108fd934911bcbea0110d07f757&rv=0) (requires your Apple ID)

-   Select **Command Line Tools for Xcode 12 (or higher)** and click the link to download the dmg archive.
-   If prompted, choose to allow downloads from developer.apple.com
-   Open the downloaded dmg archive from the Downloads folder
-   Double-click the Command Line Tools.pkg icon to install

Alternatively, you can install the tools from the command line:

```bash
xcode-select --install
```
#### Python

You will need Python 3.8 or above for this course.
You may already have this installed on your system, in which case you do not have to do anything.

Download the latest Anaconda Mac OS X installer from [Anaconda](https://www.anaconda.com/download).
Double-click the .pkg file and follow the instructions.

::::::::::::::::::::::::


:::::::::::::::: spoiler

### Linux

#### Bash Shell

This will depend on the Linux distribution you are running, but you should be able to find a “Terminal” application in your desktop’s application menu that gives you access to Bash.

#### Git Setup

Git comes pre-installed on most Linux distributions. You can test if it’s installed by running `git --version`. If it’s not installed, you can install it by running `sudo apt-get install git` or `sudo yum install git`, depending on your distribution.

#### Python

You will need Python 3.8 or above for this course.
You may already have this installed on your system, in which case you do not have to do anything.

Download the latest Anaconda Linux Installer from [Anaconda](https://www.anaconda.com/download).

Install via the terminal like this (you will need to change the version number to the latest version):

First move to the folder where you downloaded the installer, this is likely to be the Downloads folder e.g.

```bash
cd ~/Downloads
```

```bash
bash Anaconda3-2025.12-2-Linux-x86_64.sh
```

Answer ‘yes’ to allow the installer to initialize Anaconda3 in your .bashrc.
::::::::::::::::::::::::

### Microsoft Visual Studio Code

The hands-on part of this topic will be conducted using Visual Studio Code (VSCode), a widely used IDE.
Please [download the appropriate version of Visual Studio Code](https://code.visualstudio.com/) for your operating system (Windows, macOS, or Linux)
and system architecture (e.g., 64-bit, ARM).

Follow or check [these instructions](https://code.visualstudio.com/docs/copilot/setup) to ensure you have GitHub Copilot installed within VSCode.
You only need to follow steps 1 and 2 from the first section.
