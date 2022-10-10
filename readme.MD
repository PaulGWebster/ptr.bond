# What is this?

My attempt to create a basic website for the domain ptr.bond using perl's mojolicious.

In a blog like style I am going to write down my experiences

## Infrastructure

My local machine is running Windows 11, using the WSL2 stack and Docker Desktop integration, the deployment target is [FreeBSD](https://www.freebsd.org) 13.1 hosted on [OVH](https://ovh.co.uk).

Before I upgraded to windows 11 I worked on Windows 10, which required enabling WSL2, I used the following [guide](https://www.windowscentral.com/how-install-wsl2-windows-10)

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

## Step 1 - initializing (git)

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
  * Commit (875bc4b)

## Step 2 - Adding remote source repository (github)
* Create a new project/repository on github [github - ptr.bond](https://github.com/PaulGWebster/ptr.bond)
  * Add a remote to the currently local only git repository and make the first push.
    * git remote add origin git@github.com:PaulGWebster/ptr.bond.git
    * git branch -M master
    * git push -u origin master
```
paul.webster@PAUL-PC-RGB:~/personal/ptr.bond$ git push -u origin master
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 16 threads
Compressing objects: 100% (12/12), done.
Writing objects: 100% (13/13), 711.78 KiB | 5.79 MiB/s, done.
Total 13 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), done.
To github.com:PaulGWebster/ptr.bond.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

## Step 3 - Creating a perl module (Dist::Zilla)
* Create a new distribution (this blog does not include a howto on getting setup with dzil, however the site linked for DistZilla contains a tutorial)
  * dzil new Author::Daemon::Site::Ptr::Bond and commit (c0a4ef8)
```
[DZ] making target dir /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond
[DZ] writing files to /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond
[DZ] dist minted in ./Author-Daemon-Site-Ptr-Bond
```

* Adjusted order of some of the blog to work better with markdown.
  * commit (983f60e)

## step 4 - Initilize local source freezing (Carton)
* Change to the module directory: cd Author-Daemon-Site-Ptr-Bond
* Cast an eye at [Mojolicious - Tutorial](https://docs.mojolicious.org/Mojolicious/Guides/Tutorial)
  * Run cpanm Mojolicious::Lite, noting the version installed 
    * Fetching http://www.cpan.org/authors/id/S/SR/SRI/Mojolicious-9.27.tar.gz ... OK 
    * so, 9.27
  * Add: Mojolicious::Lite to cpanfile 
    * echo "requires 'Mojolicious::Lite','9.27'" >> cpanfile
* Remember that I forgot to configure .gitignore
  * Change to the root of the repository: cd ..
  * Create .gitignore with the following contents(Well my selection at least):
```
# Never include
# docker compose or .env files
/docker-compose.override.yml
/docker-compose.yml
/.env
# VScode server files
/.vscode
# Carton local folder
/local
```
* Commit the gitignore
  * git add .gitignore && git commit -m "Initial gitignore" (23074dc)
* Question why I am ignoring docker-compose.yml (that's what you get when you copy paste from another project without reading it)
  * Remove docker-compose.yml from the .gitignore and commit (21c9dbc)
* gitignore now reads:
```
# Never include
# docker compose or .env files
/docker-compose.override.yml
/.env
# VScode server files
/.vscode
# Carton local folder
/local
```
* Now back to cpanfile, mojolicious and carton
  * cd Author-Daemon-Site-Ptr-Bond
* Before we initilize carton, let us commit our cpanfile (and for me some corrections to this file)
  * git commit -am "Add cpanfile to module and more corrections for readme.md" (0a2f0b7)
  * Note: -am means 'all' no need to use 'add' I checked what this would do with 'git status' first, generally this is bad practice.
* Now let us initilize carton
```
paul.webster@PAUL-PC-RGB:~/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond$ carton install
Installing modules using /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond/cpanfile
! Couldn't find module or a distribution Mojolicious::Lite (9.27)
! Installing the dependencies failed: Module 'Mojolicious::Lite' is not installed
! Bailing out the installation for /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond/.
Installing modules failed
```
* Ut oh, err hmmm... so what I did was remove the version requirement '9.27' in the cpanfile, the result was:
```
$ cat cpanfile
requires 'Mojolicious::Lite';
$ carton install
Installing modules using /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond/cpanfile
Successfully installed Mojolicious-9.27
1 distribution installed
Complete! Modules were installed into /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond/local
```
* Installing version 9.27, this is unusual I wonder what the issue is, (looks at pod/Carton)
  * Ok so this is some weird perl versioning thing; I have no idea why I will have to ask some people, but what I did to correct the problem is change my cpanfile to be: "requires 'Mojolicious','== 9.27'"
  * Next problem! I notice in git status that local/ is listed, which we added to .gitignore earlier, however we added it as /local, we are in a subdirectory effectively so ... time for a modification to .gitignore!
    * I modified the entry for local to be '*/local' and commited the gitignore (657be57)
  * Now when we run 'carton install', Complete! Modules were installed into /home/paul.webster/personal/ptr.bond/Author-Daemon-Site-Ptr-Bond/local -- Perfect!
  * So finally we can commit our cpanfile and cpanfile.snapshot
    * git add cpanfile* && git commit -m "initial carton enviroment created"
* Commit readme.md (0b2c25c)
## Step 5 - The first steps (Mojolicious+more dzil)

* We need to begin by copying the synopsis on the POD of [Mojolicious::Lite](https://metacpan.org/pod/Mojolicious::Lite)
* This will not suite our default dzil module layout, so we will need to modify it a 'little'.
  * 5 minutes of dist.ini modification later, commit (01e6a91)
  * 10 minutes later had to manually fix [Dist::Zilla::Plugin::Prereqs::FromCPANfile](https://github.com/PaulGWebster/Dist-Zilla-Plugin-Prereqs-FromCPANfile) - Bug reported, with hack that should not be merged
* Added a bin directory bin/site-ptr-bond so we can start our site! (commit: cf21d2e)
* Reduced Bond.pm to its basic skeleton for the time being, (commit: df97ea6)
* Paste the synopsis of Mojolicious::Lite into bin/site-ptr-bond, (commit: 446e901)
* We now have a working example by calling: bin/site-ptr-bond daemon yay!
  * But we could have just done that with a single script!
  * time to start using the modules we made.
* Commited readme.md (commit: ad16968)
* Made the first release to test dist.ini layout, updated Changes
* Commit all: df97ea6
* First push/release: 
```
[Git::Commit] WARNING: No changes listed under 0.001 in Changes
[Git::Commit] Committed Changes dist.ini
```
* Removing [NextRelease] as it seems to be colliding with Git::NextVersion, doing a second release to check version bumping is happening. This will take some testing.. 
* Well this will be a big post, I had to modify quite a bit ... so lets start with the immediate problem after all that testing. 
  * Problem 1
```
?? Author-Daemon-Site-Ptr-Bond-0.001.tar.gz
?? Author-Daemon-Site-Ptr-Bond-0.001/
?? Author-Daemon-Site-Ptr-Bond-0.002.tar.gz
?? Author-Daemon-Site-Ptr-Bond-0.002/
?? Author-Daemon-Site-Ptr-Bond-0.003.tar.gz
?? Author-Daemon-Site-Ptr-Bond-0.003/
?? Author-Daemon-Site-Ptr-Bond-0.004.tar.gz
?? Author-Daemon-Site-Ptr-Bond-0.004/
```
* -
  * So I think we need a new .gitignore, this time in our module directory:
```
Author-Daemon-Site-Ptr-Bond$ cat .gitignore 
# Ignore our built author resources
Author-Daemon-Site-Ptr-Bond*
Author-Daemon-Site-Ptr-Bond$ git status
Author-Daemon-Site-Ptr-Bond$ git status
## master...origin/master [ahead 1]
 M Changes
 M bin/site-ptr-bond
 M lib/Author/Daemon/Site/Ptr/Bond.pm
 M ../readme.MD
```
Looks better to me, I think I need to stop using bullet points...

Though that does bring up the question about any ignores in the parent directory that really belong in here ... there is at least one let me move that across to where it belongs. (commit: 1078977)

Now the other files, we have our dist.ini sorted so that it will not only add tags for our version but also bump out $VERSION's in source files, you can see the magic in dist.ini ... but wait at the moment that bump in version will mean that every single file will need commiting again because of the change ... gah one moment. (commit: 17e1c40)

Ok I do not have a solution for that right now :( but its not all to bad, if I had 100s of source files it sure would be though! So let me get all these files commited so I have a nice clean place to start from. (commit: 1730b77, 997faa0, b130e28) - Also added an additional ignore for '.build' and tidied up dist.ini

* Commit readme.md (b6ea703)

Next we should tell dzil(sorry! just something I spoted) that the main module is actually bin/site-otr-bond which is the executable to launch the actual site. (commit: 71474e2), also in this commit I found out that the issue I had earlier with MakeMaker refusing to read our cpanfile can be fixed by simply specifying as the error states 'eumm_version = 7.1101' for MakeMaker -_- (I presumed it would always use the latest version)

Well we do have a site up, and a reasonable dist.ini (mostly); this section was meant to be about mojolicious but seems to have turned into a dzil configuring spree, so without further a-do

## Step 6 The second steps (mojolicious)

Even though we will be putting he entire system in docker eventually, we should get the basics working first; notable putting our handlers and such within the module we created.

* Syncing document due to refactor (commit: 53a8327)
