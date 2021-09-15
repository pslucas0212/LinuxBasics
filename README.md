
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

