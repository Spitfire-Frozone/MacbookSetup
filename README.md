# MacOS Setup

## Table of Contents
- [Introduction](#introduction)
- [Browser](#browser)
- [Terminal](#terminal)
    - [Terminal Configurations](#terminal-configurations)
- [Package Manager](#package-manager)
    - [Programs for universal installation](#programs-for-universal-installation)
- [Coding Environment](#coding-environment)
- [Communications](#communications)
- [Permissions](#permissions)
    - [Certificates and Keys](#certificates-and-keys)
    - [VPN](#vpn)
- [Organisation](#work-priority)
    - [Email Client](#email-client)
    - [Calender](#calender)
- [Making Documents](#making-documents)
    - [IDE or Code Editor](#ide-or-code-editor)
    - [Server Mounting](#server-mounting)
- [Recreational](#recreational)˜
- [All the Small Things](#all-the-small-things)

## Introduction
So you have a new Macbook. And you want to set it up. What do you include or not include? After you have turned it on and gone through the initial setting up instructions you will be left in a home screen with a dock full of apps you don't want and all the power that comes from untapped potential. 

What do you want to install on your computer?  

## Browser
- Browser (Chrome, Firefox, Safari comes installed but I hate it) + sign in I'm going with Firefox because of this with Chrome (https://www.ghostery.com/blog/manifest-v3-the-ghostery-perspective)
    + Theme
    + Password Manager Extension (Dashlane) https://website.dashlane.com/
    + Simple Tab Groups https://addons.mozilla.org/en-US/firefox/addon/simple-tab-groups/
    + Background Change https://superuser.com/questions/1495946/how-do-i-change-the-background-image-of-home-page-in-firefox

Click on Dock, remove thigns you don't want, select Dock itself, turn hiding on.
Dock -> Dock Preferences: 
Magnification -> Toggle on
Show recent applications -> Toggle off
Open Finder -> Home Folder (House Icon) -> Left Click -> Add to Dock
    Click on Home Folder -> Display As Folder, View content as list


## Terminal
- Terminal (zsh comes as standard - iterm2 https://www.slant.co/topics/525/~best-terminal-emulators-for-mac#1 -> https://iterm2.com). For this you will need to switch to a bash configuration.
    + In terminal: chsh -s /bin/bash (to switch to bash) and chsh -s /bin/zsh (to switch to zsh)

#### *Terminal Configurations*
You can add configurations from old macs using Airdrop. CMD+Shift+. to view hidden files on a mac (Temporarily rename hidden files to ones without a dot for transferring) Use the terminal to re-add the `.` at the start as Finder does not like doing this.

- iterm configurations

Transfer .iterm2/ and iterm2.sh_shell_integration.bash. 
iterm2->Preferences->General->Window (Smart Window Placement, Zoom maximises vertically only)
iterm2->Preferences->Appearance->General (Compact Theme)
iterm2->Preferences->Profiles->Window->Transparency (50%)
iterm2->Preferences->Profiles->Terminal->Unlimited Scrollback

- bash configurations 

Transfer .bashrc and .bash_profile. 

- x11 Forwarding 
If you want your terminal to be able to display graphics from remote servers locally, you will need to set up some sort of x11 forwarding for your shell. You will need an app like xquartz (https://www.xquartz.org/) for this which I have downloaded, but I imagine other options are available.


<details> <summary> My .bashrc </summary>

which my bash_profile just sources.
```m
# .bashrc

#  Source global definitions
# if [ -f "${HOME}/.bashrc" ]; then
# . "${HOME}/.bashrc"
# fi

# colors!
green="\[\033[0;32m\]"
blue="\[\033[0;34m\]"
purple="\[\033[0;35m\]"
lightgrey="\[\033[0;37m\]"
darkgrey="\[\033[1;30m\]"
white="\[\033[1;37m\]"
reset="\[\033[0m\]"

# Change command prompt
source ~/.git-prompt.sh
# to show when working directory contains changes
export GIT_PS1_SHOWDIRTYSTATE=1
# '\u' adds the name of the current user to the prompt
# '\$(__git_ps1)' adds git-related stuff
# '\W' adds the name of the current directory
#export PS1="$lightgrey[\u@\h$white\$(__git_ps1) $lightgrey\W]\$ $reset"

# for supposedly slightly faster prompt than the above line (see git-prompt.sh for details)
export PROMPT_COMMAND='__git_ps1 "$lightgrey[\u@\h$white" " $lightgrey\W]\$ $reset"'

# Suppress The default interactive shell is now zsh.
export BASH_SILENCE_DEPRECATION_WARNING=1

# User specific aliases and functions

# Alias' for mounting to CERN work area
alias sshlxp='ssh -Y dspiteri@lxplus.cern.ch'
#alias lxpmount='sshfs dspiteri@lxplus.cern.ch:/afs/cern.ch/work/d/dspiteri ~/lxp-work/'
#alias unmount='umount dspiteri@lxplus.cern.ch:/afs/cern.ch/work/d/dspiteri'
#alias funmount='diskutil unmount force /Users/Headquarters/lxp-work'
#alias mountfresh='funmount lxpmount'

# Alias' for mounting to Glasgow work area
alias glasssh='ssh -Y dspiteri@ppelx.physics.gla.ac.uk'
#alias glasmount='sshfs dspiteri@ppelx.physics.gla.ac.uk:/home/ppe/d/dspiteri ~/glas-work/'
#alias gunmount='umount dspiteri@ppelx.physics.gla.ac.uk:/home/ppe/d/dspiteri'
#alias gunmountnow='diskutil umount force /Users/Headquarters/glas-work'

test -e "${HOME}/.iterm2_shell_integration.bash" && source "${HOME}/.iterm2_shell_integration.bash"

alias vscode="open -a /Applications/Visual\ Studio\ Code.app"
alias vscreate="touch"

#¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´><(((º>
newFileInVS(){
################################################################################
if [ "${1}" = "--interrogate" ]; then
IFS= read -d '' functionInformation << "EOF"
<functionInformation>
<class>
utility
</class>
<description>
This function creates a file that doesn't exist and opens it in VSCode.
</description>
</functionInformation>
<creationTime>
2022
</creationTime>
<lastModificationTime>
2022-10-21T1840Z
</lastModificationTime>
EOF
return
fi
################################################################################
touch "${1}"
vscode "${1}"
}


#¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´¯`·.¸¸.·´><(((º>
listDirectorySizes(){
################################################################################
if [ "${1}" = "--interrogate" ]; then
IFS= read -d '' functionInformation << "EOF"
<functionInformation>
<class>
utility
</class>
<description>
This function lists the directories at the current directory in order of size
(amount of data at it) along with the actual size. The order listed is from
highest to lowest.
</description>
</functionInformation>
<creationTime>
2013
</creationTime>
<lastModificationTime>
2015-11-09T1219Z
</lastModificationTime>
EOF
return
fi
################################################################################
du -sk * | sort -rn | \
while read size entry; do
# If the size is greater than 1048576, then it is at least 1 GB in size.
if [ "${size}" -gt 1048576 ]; then
newSize=`echo "${size}000 / 1048576" | bc | sed -e "s/\(...\)$/\.\1/"`
printf "% 10s %s\n" "${newSize} GB" "${entry}"
# If the size is greater than 1024, then it is at least 1 MB in size.
elif [ "${size}" -gt 1024 ]; then
newSize="$(echo ""${size}"000 / 1024" | bc | sed -e "s/\(...\)$/\.\1/")"
printf "% 10s %s\n" ""${newSize}" MB" "${entry}"
# If the size is anything else, then display the size in kb.
else
printf "% 10s %s\n" "${size} kB" "${entry}"
fi
done
}
```
</details>

Here notice the line that I include is 
```m
export BASH_SILENCE_DEPRECATION_WARNING=1
```
If you don't include these everytime you open a shell you will get 
'The default interactive shell is now zsh' appearing, though you can use zsh if you want. 

- vim configurations

Transfer .vimrc

<details><summary> My .vimrc </summary>

```m
" Disable compatibility with vi which can cause unexpected issues.
set nocompatible

" Enable type file detection. Vim will be able to try to detect the type of file in use.
filetype on

" Enable plugins and load plugin for the detected file type.
filetype plugin on


"--------------------- Indentation Options
" New lines inherit the indentation of previous lines
set autoindent

" Use space characters instead of tabs.
set expandtab

" Load an indent file for the detected file type.
filetype indent on

" Set shift width to 4 spaces.
set shiftwidth=4

" When shifting lines, round the indentation to the nearest multiple of shiftwidth.
set shiftround

" Insert “tabstop” number of spaces when the “tab” key is pressed.
set smarttab 

" Set tab width to 4 columns.
set tabstop=4


"--------------------- Search Options
" Use highlighting when doing a search.
set hlsearch

" Ignore capital letters during search.
set ignorecase

" While searching though a file incrementally highlight matching characters as you type.
set incsearch

" Show partial command you type in the last line of the screen.
set showcmd

" Show matching words during a search.
set showmatch

" Override the ignorecase option if searching for capital letters.
" This will allow you to search specifically for capital letters.
set smartcase



"--------------------- Text Rendering Options
" Avoid wrapping a line in the middle of a word
set linebreak

" Enable syntax highlighting
syntax enable

" Enable line wrapping
set wrap 


"--------------------- User Interface Options
" Use colors that suit a dark background.
set background=dark

" Highlight cursor line underneath the cursor horizontally.
set cursorline

" Highlight cursor line underneath the cursor vertically.
set cursorcolumn

" Always display the status bar"
set laststatus=2

" Disable beep on errors.
"set noerrorbells

" enable mouse for scrolling and resizing" 
set mouse=a

" Add numbers to each line on the left-hand side.
set number

" Show line number on the current line and relative numbers on all other lines.
"set relativenumber 

" Always show cursor position
set ruler

"Set the window’s title, reflecting the file currently being edited.
set title

" Maximum number of tab pages that can be opened from the command line.
set tabpagemax=50

" Flash the screen instead of beeping on errors.
set visualbell

" Enable auto completion menu after pressing TAB.
set wildmenu

" Make wildmenu behave like similar to Bash completion.
set wildmode=list:longest

" There are certain files that we would never want to edit with Vim.
" Wildmenu will ignore files with these extensions.
set wildignore=*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx



"--------------------- Code Folding Options
" Fold based on indention levels.
set foldmethod=indent

" Only fold up to three nested levels.
set foldnestmax=3 

" Disable folding by default.
set nofoldenable



"--------------------- Miscellaneous Options
" Allow backspacing over indention, line breaks and insertion start
set backspace=indent,eol,start

" Display a confirmation dialog when closing an unsaved file
set confirm

" Hide files in the background instead of closing them
set hidden

" Set the commands to save in history default number is 20.
set history=1000

" Do not save backup files.
set nobackup

"Ignore file’s mode lines; use vimrc configurations instead.
set nomodeline

" Show the mode you are on the last line.
set showmode

" Do not let cursor scroll below or above N number of lines when scrolling.
set scrolloff=10

"Enable spellchecking
set spell

" Turn syntax highlighting on.
syntax on
```
</details>

## Package Manager
- Homebrew https://brew.sh/
    + In-terminal installation. Don't forget to add homebrew to your PATH afterwards (brew will give you this prompt and will tell you how to do this as well)

<details><summary>Important Note</summary>
Homebrew bottles (binary package) depend on Command Line Tools (CLT) for installation. After an upgrade of macOS, CLT's should be reinstalled yet it does not necessarily mean an xcode installation. To check if you have CLT installed, use
    
    brew doctor
and if you don't have it then you run
    
    xcode-select --install
Xcode is an integrated development environment (IDE) that is comprised of software development tools for macOS. You won’t need Xcode to use Homebrew, but some of the software and components you’ll want to install will rely on Xcode’s Command Line Tools package.
</details>

#### *Programs for universal installation*
```bash
brew install git 
```

There are going to be on occasion files that you want to install that can't be handled by Brew, for these create a home directory to put them all in
```bash
mkdir BuildfromSource
```

If you no longer want a package 
First, uninstall the package, and then automatically remove all unused dependencies (downloaded at the same time)

```bash
brew uninstall <package>
brew autoremove
```

## Coding Environment

### Python
For Python Development you want Anaconda to be your package manager as it handles python dependencies a lot better than pip. Create a python area
```bash
   brew install --cask anaconda
   mkdir Python && cd Python
   /opt/homebrew/anaconda3/bin/conda init "$(basename "${SHELL}")"
   <EXIT AND REENTER SHELL>
   conda activate
   conda config --set auto_activate_base false
   conda install anaconda anaconda-navigator #Navigator for anaging conda environments
```   


Once you have an installation of conda and you have verified it works, what you can do it start off with your first managed conda enviroment. Working a bit like a container or an image, each conda environment will have whatever packages you install in it. it will manage conflicts and dependencies and each instance will be seperate from each other and your system python.

Here I am going to set my standard that I will name my conda environments after collective nouns for snakes/natural habitats where snakes live: cobraden, vipernest, snekpit, adderbed, serpentknot, mambapipe; Because hur de hur hur isn't me so clever. 

To start I am going to create an enviroment that has pandas.
```bash
    conda create --name cobraden pandas
```
If you get the message 
> Executing transaction: \ WARNING conda.core.envs_manager:register_env(51): Unable to register environment. Path not writable or missing.

You might need to change access permission to your environments file like below
```bash
sudo chmod 775 /Users/dshq/.conda/environments.txt
```
and then run again, removing the enviroment you just ran.
If you are starting a fresh terminal and you want to use a particular environment you can check whats available using
```bash
    conda info --envs
``` 
With the asterisk next to the one that is active. If you want to switch to another one then you just need to activate it.
```bash
    conda activate cobraden
```
If you want to edit the environment while in it you just install it
```bash
    conda install matplotlib
```
If you want to see everything that is active in this environment
```bash
    conda list
```
And if you want to leave said active environment
```bash
    conda deactivate cobraden
```

- Jupyter Notebooks
- GitLab/GitHub area
- Tensorflow?

### C++
For C++ Development you will need to install the command-line xcode tools. These will have a version of the c++ compiler clang installed
```bash
xcode-select --install
```
which a prompt will come up, for you to click, and this may take a couple of minutes to install.
```bash
clang --version
```
Somewhere adjacent to the Python directory you will create, create a directory that will store all your cpp files. (I called mine VSCodeCpp). Inside this directory you will create a subdirectory for each cpp project you will work on.



## Communications 
Download The Mac clients from the Web
- Skype
- Discord
- Slack
- Zoom.us


## Permissions 
Mostly copied over using Airdrop

#### *Certificates and Keys* 
I have a Grid Certificate Issued to me by the UK certification Authority. If you have no certificates, you will not be prompted for one, so in the first instance you will have to manually add it into your browser. 
Firefox -> Open Applicaiton Menu -> Settings -> Privacy and Security -> View Certificates -> Import.

#### *VPN*
I have a series of sites that are hosted on the ScotGrid private network, so in order to access them I will need a VPN access. The VPN type is not in the standard list of configurations so I will need to download an openVPN client (https://openvpn.net/client-connect-vpn-for-mac-os/).

Open VPNConnect -> Import File (.ovpn)
You will need to generate the file and then paste your cert and private key into the relevant places. I have two VPN files to install: scotgrid-tcp.ovpn and scotgrid-udp-ovpn.


## Organisation

#### *Email Client*
I had used Outlook Application before and it works well but I decided to move to MailSpring, but while Mailspring has lots of [nice features](https://www.getmailbird.com/best-mac-email-client/#Mailspring_Best_Open-Source_Email_App_for_Mac), I didn't take to it because it doesn't handle email folders at all well. It will not treat emails automatically forwarded into an email folder as unread, so it won't tell you you have an email. Also you only get three email accounts free.
    
Installing and setting up Outlook
1) Uni of Glasgow account: https://www.gla.ac.uk/myglasgow/it/uofgemail/setupyourdevice/
2) CERN Account: https://espace.cern.ch/mmmservices-help/AccessingYourMailbox/Pages/default.aspx
>   IMAP IMAP imap.cern.ch port 993| 
>   SMPT smpt.cern.ch startTLS port 587. Turn microsoft cloud link off.
>   Note that if you want to connect you device to the CERN network you will need to register your device: https://information-technology.web.cern.ch/help/connect-your-device
3) Primary Personal Email account  

#### *Calendar*
Here I am importing the calendar from my old mac over to this new one. 

- Open the calander on the old mac and navigate to the left hand side with the calander checkboxes. Each of these is a different calander. 
- Select one and then go to File -> Export -> Export (which will direct you to save an .ics file somewhere). Alternatively if you want all of your calanders then you can export them all using File -> Export -> Calander Archive, and then transfer that .icbu file across instead. I prefer this latter option, but this is good iff your new calander is empty
- Once you have exported all the calanders you want then you can use airdrop to transfer them to the new mac. 
- Then in the new mac open up the calander and File  -> Import and then point it to the file to import. This may take some time to impliment.
- Delete any file(s) that you have imported.


## Making and Editing Documents
- Presentations (Keynote) - comes installed 
- Windows compatible shit (Microsoft Office) (for when some boomer gives you a docx to edit)
    + Installing Office 365 instructions: https://www.gla.ac.uk/myglasgow/anywhere/office365/gettingoffice/

- Latex (TexMaker, TexLive) (For more complicated document)
#### *IDE or Code Editor* 
Options: Sublime Text Editor 4/Atom/VSCode. I went with [VScode](https://code.visualstudio.com/docs/?dv=osx)

Add opening alias to .bashrc 
```
alias vscode="open -a /Applications/Visual\ Studio\ Code.app"
```

VSCode Configurations
https://code.visualstudio.com/docs/getstarted/introvideos
Extensions: **Python, Pylance, C/C++, C/C++ Extension Pack, LivePreview (see .html in vscode), OpenVPN, GitLens, ROOTFileViever, Markdown Preview Enhanced, Seti-icons, vscode-pdf, PostScript Language, PostScript Preview**
(Command, Shift P) Theme: **Cobalt2**
(Command, Shift P) `File Icon Theme` **Seti UI**

Code => Settings => Settings
- Workbench › Tree: Indent (8 -> 15)
- Workbench › Tree: Render Indent Guides (always)
- Workbench: Color Customizations (open settings. json)
  >   "workbench.colorCustomizations": {
  >      "tree.indentGuidesStroke": "#cded4d"
  >   }

If you want to code in the environment we just created above 'cobraden'. Then after you have installed Python, Type ">Python:Select Interpreter" and then select cobraden.

- Changing Default Applications
    Left click a file -> Get Info -> Open With (application) + Change All -> Continue.

### Server Mounting

Transferring files over an SSH connection, by using either SFTP or SCP, is good for moving small amounts of data to your laptop such that you can work on it locally. Alternatively if you have ssh access to a remote area, then you may want to log in and edit files there. However this comes at the downside that unless you have admin provelidges on the remote site, you don't have access to the local changes/programs on the mac, which you have now just tailored to be super sweet.

The best way to consistenly work on files in remote locations, with all the goodies in your area local to your mac (at least i've found) is to share entire directories filesystems, between two remote environments. While this can be accomplished by configuring an SMB or NFS mount, both of these require additional dependencies and can introduce security concerns or other overhead. Alternatively, (and what I reccommend) is installing SSHFS to mount a remote directory by using SSH alone. This has the significant advantage of requiring no additional configuration, and permissions are inherited from the SSH user on the remote system. SSHFS is particularly useful when you need to read from a large set of files interactively on an individual basis. 

#### *-- THIS MAY NO LONGER WORK WITH SONOMA 14.1.1 (23B81) --*

```bash
brew install --cask macfuse
```

Alternatively you can install macfuse (formerly osxfuse) for Mac from the [macFUSE Project](https://osxfuse.github.io/), and then once you have this you can install sshfs. 

You would think that you just do "brew install --cask sshfs" now, but no. Homebrew has disabled (depreciated) all FUSE formulae on macOS for reasons, you will have to download an older version of the [sshfs package](https://osxfuse.github.io/) (2.5.0)

<details> <summary>Important note</summary>
So while you CAN download [sshfs](https://github.com/libfuse/sshfs/releases) from libfuse repo to get the latest version (3.7.3), sshfs versions 3 and above depend on libfuse3 and there is no FUSE 3 for OS X yet. So while sshfs 3.7.3 exists it's not usable for mac (macFUSE 4.4.1 uses the libfuse 2.9.9 API). Until that is available, you need to stick with SSHFS 2.x. (see [here](https://github.com/osxfuse/fuse)). If in the future macFUSE is updated to a newer version of fuse, then assuming it's still not installable in brew you will have to build from source an unofficial version managed by the [deadbeefsociety/sshfs](https://github.com/deadbeefsociety/sshfs) repo (it was the most active fork for the project).You will also need the [Glib](https://developer.gnome.org/glib/stable/) library (which you should have), [Meson](https://mesonbuild.com/) (build system akin to cmake), and [Ninja](https://ninja-build.org/) (lightweight build system for unit testing). Brew-installing meson will get ninja by default.

    brew install meson
    brew install pkg-config

 For more detailed installation follow the installations instructions on https://github.com/deadbeefsociety/sshfs
  
    mv Downloads/sshfs-sshfs-3.7.3/ BuildfromSource/
    cd BuildfromSource/sshfs-sshfs-3.7.3/
    mkdir build; cd build
    meson setup ..
</details>

Once these have been installed, you can create directories to house the remote server filesystems, ideally one for each you want to do (I will have one for CERN and one for Glasgow) and then create alises for the mounting and unmouting in your .bashrc

```bash
mkdir RemoteFSs; cd RemoteFSs
mkdir lxp-work glas-work; cd ..
vscode .bashrc
```
>    ADD/CHANGE aliases for terminal (un)mounting commands
```
# Alias' for mounting to CERN work area
alias sshlxp='ssh -Y dspiteri@lxplus.cern.ch'
alias lxpmount='sshfs dspiteri@lxplus.cern.ch:/afs/cern.ch/work/d/dspiteri ~/RemoteFSs/lxp-work/'
alias unmount='umount dspiteri@lxplus.cern.ch:/afs/cern.ch/work/d/dspiteri'
alias funmount='diskutil unmount force /Users/dshq/RemoteFSs/lxp-work'
alias mountfresh='funmount lxpmount'

# Alias' for mounting to Glasgow work area
alias glasssh='ssh -Y dspiteri@ppelx.physics.gla.ac.uk'
alias glasmount='sshfs dspiteri@ppelx.physics.gla.ac.uk:/home/ppe/d/dspiteri ~/RemoteFSs/glas-work/'
alias gunmount='umount dspiteri@ppelx.physics.gla.ac.uk:/home/ppe/d/dspiteri'
alias gunmountnow='diskutil umount force /Users/dshq/RemoteFSs/glas-work'
```

The first time I tried to use one of the mounting commands, I got a pop-up that said that the System Extentions was blocked. In the Privacy and Security tab, there is an option to enable System Extentions, but in Apple Silicon (Ventura) to enable system extensions, you need to modify your security settings in the Recovery environment. To do this for Apple Silicon chips, 

1) Shut down your system.
2) Reboot then press and hold the Touch ID or power button until "Continue to hold for more options" appears and then continue to hold. For T2 chips you press `Command+R` as soon as you see the apple logo.
3) Click Options when given a choice of how to boot.
4) In the top menu select Utilities -> Startup Security Utility. 
5) Select Security Policy, Select Reduced Security and tick only the checkbox "Allow user management of kernel extensions... [Requires password]
6) Shut down and reboot as normal.

The next time you try the command, when the Privacy and Security pop-up appears you can now see an Allow button, which you press (again requires your password). Then for the changes to take effect you need to restsrt one more time. After this the commands above should work.

#### *-- THIS IS AN ALTERNATIVE WAY THAT I KNOW WORKS --*
So you can actually use vscode itself to mount to remote directories and all this requires is that your ~/.ssh/config file is up to date, as it will use that to piggy-back a connection off of

```bash
vscode ~/.ssh/config
```
>   ADD details about the servers you want to remote into and make sure you forward your keys (if you use them for authentification by default).
```
# Read more about SSH config files: https://linux.die.net/man/5/ssh_config
Host lxplus
    HostName lxplus.cern.ch
    User dspiteri
    ForwardAgent yes

Host *.beowulf.cluster
    User d-ipl-spiteri
    ForwardAgent yes

Host glasgow
    HostName ppelogin2.ppe.gla.ac.uk
    User dspiteri
    ForwardAgent yes
```

In any case you will want to add these aliases listed above into your .bashrc anyway abecause when you are finished you will use these to connect from either vscode, or any terminal itself. 

```bash
vscode .bashrc
```
>    ADD/CHANGE aliases for sshcommands
```
# Alias' for mounting to CERN work area
alias sshlxp='ssh -Y dspiteri@lxplus.cern.ch'

# Alias' for mounting to Glasgow work area
alias glasssh='ssh -Y dspiteri@ppelx.physics.gla.ac.uk'
```
Now while the lxplus one will work out of the box, to connect to the glasgow servers you need to do more work. 

Since there are VM's set up on the server where most of the work is done, you will need to set up a link to one of the servers for some reason. So
1) Log into to a vm on the Glasgow servers and download the software required for the vscode tunnel (https://code.visualstudio.com/docs/remote/tunnels) for more information
```bash
glasssh
ssh -A ppe-vm-06
curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz
tar -xf vscode_cli.tar.gz
./code tunnel 
```
2) Follow the instructions appearing on the terminal -> Select GitHub account, will give you an 8 digit alphanumeric code.
3) Connect to GitHub on https://github.com/login/device and insert the code displayed on your terminal 
4) In the terminal a prompt appears for you to name this machine, do so: 'dshq2ppe' for me
5) Open https://vscode.dev/tunnel/{name_from_#4} in your browser and select GitHub -> Allow
6) Authenticate the log-in being wary of browsers that block pop-ups, and hope/pray that when you click Allow when the "You are about ti connect to an OS version that is unsupported by Visual Studio Code", it doesn't mess everything up.
7) Go back to VS Code and click on the SSH Icon, then on the ‘Connect to tunnel’ command and the name of your virtual machine should appear, click on it.


Open up vscode and whenever you want to connect, you clock on the last icon in the top left of the window that looks like a monitor with a circle in the bottom right with two opposing chevrons in it. This will show you a drop-down menu giving you the options of attempting a connection to any service with a complete host (so in my case 'lxplus' or 'glasgow'). Selecting one of these opens a prompt for the server password. Here 

The first time you log in it will be slow but subsequent mountings should be faster as the metadata for the transfer should be available locally. 

If this deoesn't work in your local version of vscode, you can always connect directly using your browser in (5) and working from there. 

## Backups
- Time Machine
- File Vault (if you have a T2 Security Chip - Option + Apple Top Left Corner -> System Information -> Controller )

## Recreational 
- Plex
- Spotify
- Audacity
- VLC

## Printers
There comes a time in every persons life when we have to go trhough the pain of trying to connect to a printer. 
For installing the UoG printers, start here:
https://myprinting.gla.ac.uk/printsetup/print_landing/index.html

You will need to download the printer drivers first. Be wary that because i'm running the latest Mac OS (macOS13 - Ventura) I got an error saying OS wasn't supported by the drivers, so proceed with caution. 

1. Click Download MacOS Drivers and then click Allow if prompted to download the drivers.
2. Go to the Downloads folder via Finder.
3. Double click the file just downloaded: mac-ps-vxxx-xx.dmg.
4. Double click the installer package Canon_PS_Installer.pkg.
5. Follow the wizard steps to install the driver, clicking Continue and Agreeing to the driver terms.
6. When prompted, click Install and enter your Apple computers password.
7. Navigate to https://mobileprint.gla.ac.uk with your GUID and Password
9. Select your operating system
10. Select Advanced and then click Continue
11. Select Mac Safecom Mobile Print and follow the instructions to finish the installation
    a System Settings -> Printers and Scanners -> Add Printer, Scanner or Fax...
    b Select the Globe icon and Enter these settings       
    ```bash        
        Protocol: 	Internet Printing Protocol - IPP
        Address: 	mobileprint.gla.ac.uk:631
        Queue: 	ipp/r/ba14a1649451724/8A0C7DF9
        
        Name: 	Glasgow Uni Printing (or any name that you prefer!)
        Use: Select Software...
            Printer model: 	Canon iR-ADV C5840/5850 PS
    ```
Now after you add this, if you go into Printers and Scanners again, you should see this printer there.

## All The Small things
Apple -> System Preferences -> Bluetooth -> Show Icon in Menu Bar
Private Folder https://setapp.com/how-to/password-protect-folder-on-mac 
