
# KodeKloud Linux Basics Course Notes

### Shell Commands
- Show environment settings
```
$ env
```
- Set environment variable
```
$ export NAME=paul
```
- Add to Path
```
$ export $PATH:/somedir
```
- Show default shell
```
$ echo $SHELL
```
- Show prompt settings
```
$ echo $PS1
```
- Change shell
```
$ sudo chsh -s /binsh <username>
```
- Show path
```
$ echo $PATH
```
- Create alias
```
$ alias up=uptime
```
- Change prompt example - Add date between brackets, then username @ hostname working directory : $ -> [Wed Sep 15]bob@caleston-lp10:~$
```
$ PS1='[\d]\u@\h:\w$'
```
### Linux Kernel
- The kernel is the interface between applciations/processes and the underlying h/w - CPU, Memory, Storage, I/O.  The kernel manages and schedules resources
- Kernel is responsible for 4 major tasks
  - Memory Mangement
  - Process managment - when can process
  - Device Drivers
  - Systems Calls and security
- Kernel is monolithic
- Kernel is modular - can extend its capability by loading dynamic extenxsions

- To kernel name type
```
$ uname
```
- To see kernel version type
```
$ uname -r
```
- See the kernel.org webssite to get more information on the Linux kernel

#### Kernel and User Space
- memory is divided into two spaces: Kernel space and user space
  - Kernel space unrestricted access to hardware and runs the Kernel code, kernel extensions and device drivers
  - User space - all process running outside kernel space which restricts access to h/w
    - user space applications - user space also called user land
    - examples: programs written in Java, C, Python

- Data for users stored in memory or disk.  Applications in the user space access data by making system calls to the kernel space, which in turn makes calls to device drivers to the underlying physical hardware


- system calls include open(), close()

#### Working Hardware
- dmesg
```
$ dmesg | grep -i usb
```

udevadm info --query=path --name=dev/

lspci - list info about pci devices - like network cards, video cards, etc.

lsblk - list information about block devices - like physical devices.  Type disk refers to the entire disk and part refers to partion carved out of the disk.  MAJ:MIN - major min number of device driver 

lscpu - CPU architecture - 32-bit or 64-bit processors

lsmem --summary - list memory on system

free -m - list free memory -m megabyte, -k kilobbyte

lshw - hardware output

#### sudo
- run command as root - with sudo you can determine which commands a user can run as super user and also see a list of commands the user has run as root
```
$ sudo lshw
```

## Linux Boot Sequence
- Four stages
  -   BIOS Post
  -   Boot Loader - GRUB2
  -   Kernel Initialization
  -   INIT process (systemd)

Start a linux device in stopped or halted state or reboot running system

BIOS POST - power on self test - make sure all the h/w can start
BIOS loads boot code - located in first segment of harddrive - loadks kernel
- Grand Unified Boot Load (GRUB 2)
- Kernel is decompressed after loading - the kernel is loaded into memory
  -   After kernel is loaded it looks for an init process to setup user space
- INIT process starts systemd
  -   systemd start services, mounts drives, etc.

To see what init process is using run:
```
$ ls -l /sbin/init
```

### Run Levels
Linux can run in multiple modes set by the run level.  Type the following to see the level
```
$ runlevel
```
Run levels include:
- graphical
- command line

During boot the system checks the runlevel

in systemd runlevel are called systemd targets

To see systemd target type:
```
$ systemctl get-default
$ ls -ltr /etc/systemd/system/default.target
```

Change systemd target:
```
$ sudo systemctl set-default multi-user.target
```

### File types
"Everything is a file in Linux" - every object in linux is a "type" of file
Three types of files
- regular files - images, scripts, configuration, shell scripts, jpeg
- directory - stores other directories and files
- special files
  -   character files - device
  -   Block files - under /dev - block device reads from and writes to a device in chunks of block either memory or hard disk
  -  Links: Hard links and symbolic links
  -  Sockets - communication between to files
  -  Named pipes - send information bi-directionally between process


Identify file type
````
$ file /home/dir
$ file bob.sock
````
or use list - ls command to identify file type by the output
```
ls -ld /home/michael
```
Identified by first letter d - directory, s - socket, b - block device, l link, p - pip - - regular file


### Filesystem Hierarchy
/ - root partition
/bin - date, cp, etc commands
/boot
/dev
/etc - stores most config files
/home
/lib - shared libraries imported in to programs
/media - USB drive mounted under media all external media is mount under media
/mnt -  used to temporariy mount file systems
/opt - install any 3rd part apps
/tmp - store temporary data
/usr - old systems kept home directory,  Now user space programs are kept here.  vi, browser
/var - system rights logs and cached data to var

Use this command to see mounted devices
```
$ df -hP
```
## Linux Package Management
Debian/Ubuntu
- DPKG/APT
Redhat/Fedora
- RPM

.deb -> debian package manager
.rpm -> Red Hat package manager

Package - a compressed archive that is a set code packages (binaries, files and meta-data) to run software (or an application)

Manifest of a package describe the list of dependencies and minimal versions of supporting package/softare that is required to install softare

Package manage
- insure integreity and authenticy of the padkage
- Simplfy package management - installation, cofnfiguration
- Group packages
- 

Types of package manageer
- DPKG - debian
- APT - new version that frontends DPAK
- APT-GET
- RPM - Red Hat
- Yum - front end for RPM
- DNF - new front end for RPM

#### RPM and YUM

RPM - Red Hat Package Manager
- CentOS, Fedora, Redhat
- .rpm 

RPM has five modes
- installation - $ rpm -ivh telnet.rpm
- uninstalling - $ rpm -e telnet.rpm
- upgrade - $ rpm -Uvh telnet.rpm 
- query - $ fpm -q telnet.rpm
- verify - $ rpm -VF

YUM - YellowDog Update Manager
- Works on RPM base distors
- software repository
- high level package manager - uses RPM under the covers
- Automaticatlly resolves dependencies - which RPM does not

Repoistory information is stored under /etc/yum.repos.d
For Red Hat -> /etc/yum.repos.d/redhat.repo
You may need to create your on repo file /etc/yum.repos.d/nginx.rpm

YUM install example
```
$ yum install httpd
$ yum -y install httpd
```
After checking repository for all code and packages, Yum displays a transcation log.  

```
$ yum repolist
```
To find where a particular app is located
```
$ yum provides scp
```
To remove package run:
```
$ yum remove httpd
```
To update a package
```
$ yum update telnet
```
Update all packages
```
$ yum update
```

#### DPKG and APT

Debian distros PureOS, Ubuntu
Debian Package Manager - DPKG - low lever package manager like RPM

- Installation/upgrade  - $ dpkg -i telnet.deb
- uninstalling -  $ dpkg -r telnet.deb
- List -  $ dpkg -l telnet
- Status - $ dpkg -s telnet
- Verifying -  $ dpkg -p <path to file>
  
 DPKG does not support depencies and we use APT and APT-GET
 
  APT - Advanced Pagkacge Manager
  
  APT - acts as a front end manager depending on DPKG and software repositories.  Repos list are found in /etc/apt/sources.list
  
  APT commands
  Refresh repo
  ```
  $ apt update
  ```
  Upgrade
  ```
  $ apt udpate
  ```
  Add repos
  ```
  $ apt edit-sources
  ```
  Install package
  ```
  $ apt install telnet
  ```
  Uninstall package
  ```
  $ apt remove telnet
  ```
  List 
  ```
  $ apt search telent
  ```
  
  #### APT vs APT-GET
  APT is more user frienldy then apt-get
  
  Installation output is easier to read
  
  Eassier to search
  ```
  $ apt search telnet
  ```
  ## File Compression and Archival
  
  Viewing File sizes
  Disk usage command
  ```
  $ du -sk test.img
  $ du -sh test.img
  $ du
  ```
  Archiving Files
  - tar cf 
  - tar stands for Tape Archive
  - archived files are call tar balls
  To 'tar' up a file
  ```
  $ tar -cf test.tar file1 file2 file3
  ```
  - -tf see contents of tar files
  - -xf to extract tarfile
  - -cfz - to compress archive file
  
  Compression options
  - bzip2
  - gzip
  - xz
  
 - unzip with gunzip for gzip
  
 - zcat,bzcat,xcat - reads file without uncompressing the file
  
  #### Searching for files and directories
  Locating file or directory
  
  Find files with <city>.txt
  ```
  $ locate City.txt
  ```
  This command may not "work" if the database hasn't been update. Update database
  
 ``` 
  $ sudo updatedb
  ```
  Alternativel use the find command with -name for name of file
  ```
  $ find /home/michael -name City.txt
  ```
  Search with in file use GREP. case sensitive
  - search for word second
  ```
  $ grep second sample.txt
  ```
  Case insentive
  ```
  $ grep -i capitl sample.text
  ```
  Search recursively
  ```
  $ grep -r "third line" capital.txt
  ```
  Search for a string through set of files in a directory
  ```
  $ grep -ir 172.16.123.197 /etc
  ```
  Find only whole words
  ```
  $ grep -w exam examples.text
  ```
  Skipe lines with the search word
  ```
  $ grep -v examp examples.text
  ```
  To print line berfore or after finding the pattern
 ```
  $ grep -A1 arsenal premier.txt
  $ grep -B1 arsenal premier.txt
  $ grep -AB1 aresenal premier.txt
  ```
  
  #### IO Redirection
  Three data streams created when you launch a linux command
  - Standard Input - stdin - accepts input - $ cat sample.txt
  - Standard ouput - stdout
  - standard Error - stderr - redirect to file
  
  Redirect stdout
  ``` 
  $ echo $SHELL > shell.txt
  ```
  Append stdout to a file
  ```
  $ echo "some text" >> shell.txt
  ```
  
Redirect STDERR
```
 $ dat missing_file 2> error.txt
 $ cat missing_file 2>> error.txt
 ```
  Dump stderr
```
$ cat missing_file > nul
 ```
  
#### Command Line Pipes
Use standard out of one command as the standard in for another command
```
$ grep hello sample.txt | less
```
Pipes output to less command
 
Tee command - standard output printed to screen before overwriting file
```
  $ echo $SHELL | tee shell.txt
```
Use -a to append to file
```
$ echo $SHELL | tee -a shell.txt
```
  
#### VI editor
Most popular text editor used with Linux

- open a file
```
 $ vi sample.txt
```

- Three operation modes in VI
  - Command
  - Instert
  - Last line
  
- Switch from command line mode to insert mode type i
- Switch to command line hit Esc key
- Switch to last line mode type :
- Switch to command mode hit esc key
  
Command Mode
- copy, cut, past, insert text
- use arrow keys or hjkl to navigate key
- copy line type yy
- paste use p
- zz to save file
- to delete charactaer, type x
- to delete entire line type dd
- to delete 3 lines type d3d
- undo edit type u
- to find text type /<text>, to move down type n to move N
- to write insert text type i, o, a, or A  Last line wil indidcate that you are in Insert mode
- to go to last line mode type :
    - to save file type :w
    - to quite type : then q
    - to write and quite type : then wq
 
VIM is VI improved - sometimes vi now points to vim.

## Security and File Permissions

- Acess Controls -
- PAM - Pluggable Authentication Modle - authenticate users to programs and services
- Network security - secuirty applied to services usingnetworking with iptables and firewalld
- SELinux - security policies to isolate services/processes running on the system
- SSH hardening - security login
  
#### Linux Accounts
- Every user on linux has a linux accoutn with user name, user id, and password to logon to the system.  Ever user has a unique user id.  Information about users is stored in the /etc/passwd file
- Group is a collection of users that have a common need for accessing particular Linux resources.  Information about groups is stored in the /etc/group file.  Each group have a unique id called the gid
  
- Each user has a username, unique id - UID and belongs to a group with a group id - GID.  Run the following information to get user information:
```
$ id <username>
$ grep <username> /etc/passwd
  ```
- we can see see the users home director and shell by grepping on their name in the /etc/passwd file
  
User account type refers to individual users that need access to Linux serves

- Superuser account has a uid = 0 and has complet control over the linux system
- System accounts - UID <100 or between 500 - 1000
- Service account - run nginx, etc.
                            
- Run who command to see who is logged in and the last time the system was rebooted
```
$ who
```
#### Switch users
You can use the su command to switch to root or another user, but this is not recommended as you need the password of the user you are switching to
```
$ su -
$ su -c whoami
```
Sudo is the recommended command for priveleged escalation.  To run commands as root user.  The user is prompted for their password.  Sudo users and privelees are defined in the /etc/sudoers file.    
sudoers file
  
#### User Management
Managing Users:
Commands to Add (create user) user; see user bob's uid and gid, home directory and shell; "see" bob's password setting in the /etc/shadow file, set Bob's password, check how you are logged into the system and change your password...
```
$ useradd bob
$ grep -i bob /etc/passwd
$ grep -i bob /etc/shadow
$ passwd bob
$ whoami
```
  
Common options to use with useradd
* -c custom comments
* -d customer home directory
* -e expirdy date
* -G creat use with mutilple secondary groups
* -s specifiy login shell
* -u specify UID

```
$ useradd -u 1099 -g 1009 -d /home/robert -s /bin/bash -c "Mercury Project Member" bob
$ id bob
$ grep -i bob /etc/passwd
```
Delete user  
```
  $ userdel bob
```
Create group
```
$ groupadd -g 1011 developer
```
Delete group
```
$ groupdel developer
```
