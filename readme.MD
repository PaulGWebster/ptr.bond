# What is this?

My attempt to create a basic website for the domain ptr.bond using perl's 
mojolicious.

In a blog like style I am going to write down my experiences

## Infrastructure

My local machine is running Windows 11, using the WSL2 stack and Docker Desktop 
integration, the deployment target is [FreeBSD](https://www.freebsd.org) 13.1 
hosted on [OVH](https://ovh.co.uk).

Before I upgraded to windows 11 I worked on Windows 10, which required enabling
WSL2, I used the following 
[guide](https://www.windowscentral.com/how-install-wsl2-windows-10)

## Technologies used
I will expect this list to grow as the project progresses.

* WSL2 [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/)
* FreeBSD [The FreeBSD Project](https://www.freebsd.org)
* Debian [The universal operating system](https://www.debian.org)
* Git [git-scm](https://git-scm.com)
  * GitHub [Where the world builds software](https://github.com)
* VSCode [Code Editing ReDefined](https://code.visualstudio.com)
* perl [perl](https://www.perl.org)
  * Source management
    * Carton [MetaCpan - Carton](https://metacpan.org/pod/Carton)
    * DistZilla [Dist::Zilla](https://dzil.org)
  * Frameworks
    * Mojolicious [Web development can be fun again](https://mojolicious.org)

## Step 1 - initializing 

* Start VSCode, connect to debian/WSL2 instance.
* Choose folder ~/
* Create a folder for the project: mkdir -p personal/ptr.bond
* Swap to it: cd personal/ptr.bond
* Init a blank git repository: git init
* Create a basic readme: echo "WriteMe" > readme.MD
* In VSCode open the folder ~/personal/ptr.bond
* Open readme.md in vscode and begin writing
* Create a place for assets for the blog(From VSCode 'terminal'): mkdir -p assets/blog
* Take a picture of the enviroment so far 
  * [step1-1](assets/blog/step1-1.png "step1-1")
* Take a picture showing the integration of linux and windows via WSL2, take note of the linux icon in the bottom left of the explorer window which allows direct access to the linux file system for copying files to and from it.
  * [step1-2](assets/blog/step1-2.png "step1-2")
* Commit changes 988e73c
  * Decide that the images are to large to stick in the flow of text and convert them into standard clickable links
  * Re-commit (ac678f0)
* Correct what has been written so far and add appropriate capitalisation
* 