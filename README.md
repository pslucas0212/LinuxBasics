
# KodeKloud Linux Basics Course Notes

A bunch of unedited notes from the KodeKloud Linux Basics Course.  

### Table of Contents
[KodeKloud Linux Basics: Working with the Shell Part 1](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-1)  
[KodeKloud Linux Basics: Linux Core Concepts](https://github.com/pslucas0212/KodeKloud-Linux-Core-Linux-Concepts)   
[Kode Cloud Linux Basics: Package Management](https://github.com/pslucas0212/Kode-Kloud-Linux-Package-Management)
[KodeKloud Linux Basics: Working with the Shell Part 2](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-2)









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
#### Access Control Files
These files are found under /etc directory, are only accessible to root, and should never be modified directly with vi or VIM, but with their "special" editor.  

- /etc/passwd - contains information about users including user name, id, groups, home directory and shell
```
$ grep -i bob /etc/passwd
```
- /etc/shadow - containers users password that is hashed
- /etc/groups - contains groups
  
/etc/passwd contains user information  
```
$ grep -i pslucas /etc/passwd
```
USERNAME:PASSWORD:UID:GID:GECOS:HOMEDIR:SHELL  
pslucas:x:1000:1000::/home/pslucas:/bin/bash  
Password is always x as the password is kept in the /etc/shadow file.  The GECOS CSV comma separated other information include full name, phone number and location information is optional  
  
/etc/shadow
```
$ grep -i pslucas /etc/passwd 
```
USERNAME:PASSWORD:LASTCHANGE:MINAGE:MAXAGE:WARN:INACTIVE:EXPDATE
username, hashed password, the rest self explanatory - Note: LASTCHANGE the date since the password last changed is Epic.  Minimum and max days before they need to change password,  number of days to warn the user before the password expiration.  EXPDAT - nunmber of days when the account expires in an epic date format   
  
/etc/group
```
$ grep -i pslucas /etc/group
```
NAME:PASSWORD:GID:MEMBERS
Group name, password set to x saved in the shadow file, Group ID, memeber list comma separated
 
#### Linux File Permissions  
  
Use ls -l command to get information about file type and permissions  
  
File Type | Identifier
--------- | ----------
Directory | d
Regular File | -
Character Devicd | c
Link | l
Socket File | s
Pipe | p
Block device | b  
 
File permission example  -rwxrwxr-x
first three characters is for User - u  
seconde three charactars is for Group - g  
Third three characters is for Other - o  
  
File Permissions  
  
Bit | Purpose | Octal Value
--- | ------- | ------------
 r | Read | 4
 w | Write | 2
 x | Execute | 1  
  
Directory Permissions  
  
Bit | Purpose | Octal Value
--- | ------- | ------------
 r | Read | 4
 w | Write | 2
 x | Execute | 1
'-' | No Permission | 0  
  
Directory permission heirachy  
Permissions first check owner if owner then only owner permissions applied  
Next check group permission if not owner, but member of group then group permissions apply  
Finally check other permission
  
File Permission Example  
  
Example 1 | Example 2 | Example 3 | Example 4
--------- | --------- | --------- | ---------
rwx | rw- | -wx | r-x
4+2+1 | 4+2+0 | 0+2+1 | 4+0+1
7 | 6 | 3 | 5  
  
Changing file permissions  
chmod <permissions> file  
Change numerically or symbollically  
Symbolic mode example
 ```
 $ chmod u+rwx test-file
 $ chmod ugo+r test-file
 $ chmod o-rwx test-file
 $ chmod u+rwx,g+r-x,o-rwx test-file
```
Numeric mode example
 ```
 $ chmod 777 test-file
 $ chmod 555 test-file
 $ chmod 660 test-file
 $ chmod 750 test-file
```
  

Change ownership and group  - chown owner:group file
```
$ chown bob:developer test-file
$ chown bob andoid.pak
$ chgrp androd. test-file
```  
  
#### SSH and SCP  
SSH for logging into and executing commands on a remote computer  
ssh <hostname  or ip adderss>  
ssh <user@hostname>  
ssh -l <user> <hostname>
```
$ ssh devapp01
```
This uses the id that you are logged on locally to acess the remote server  
 
You can enable the passwordlesss login with keys - public and private key  
Setting up password-less SSH example:  
1. Create key-pair
```
$ ssh-keygen -t rsa
```
You can except the defaults or provide custoem information  
 
Notice where the keys are stored:  
Public key - /home/bob/.ssh/id_rsa.pub  
Private key - /home/bob/.ssh/id_rsa

2. Copy remote key to target remote server  
```  
$ ssh-copy-id bob@devapp01
```
The public key is now stored on the remote server here:  
/home/bob/.ssh/authorized_keys  
  
#### SCP  
SCP - secure copy for use with remote servers using TSL/SSL
```
$ scp /home/bob/caleston-code.tar.gz devapp01:/home/bob
```
Copy directory and preserve permissions example
```
$ scp -pr /home/bob/media /devapp01:/home/bob
```
  
#### IP Tables
SSH & SCP - remotely access and securly copy remote servers.  To connect to remote server you need to have user base and ssh based authentication along wiht Port 22 open.  
  
Apply network security enterprse wide by using Cisco ASA, Juniper NFGW, Barracuda NGFS, and Fortinet.  Alternatively apply these at a server level using firewalld and iptables on Linux and firewalls on Windows.   

Example: client/laptop 172.16.238.187 with App server 172.16.238.10 and db server on 172.16.238.11 and software repo server 172.16.238.15.  Block Internet.
  
SSH to dev server.  To filter traffic we will use iptables.  On Ubuntu you may need to install iptables.  
To install ip tables on ubuntu and check default rules configured on the system:
 ```
 $ sudo apt install iptables
 $ sudo aiptables -L 
 ```
We will see three set of rules or chains: input, forward, and output.  
- Input - traffic coming into the servier
- Output - traffice leaving the server to connect to other server
- Forward - Forward traffic to other servers in the network.  Not commonly used in Linux servers  
  
Default rules allow all traffi into and out of the server.  Called a chain because it's a chain of rules.  Either acceppt or drop the packet based on the condition of the rule.  It looks at the conditions in the order of the chain and when a condition is matched the rule is invoked.  It continues through the chain rules until a condition is met.  
                             
Input rule example:
```
$ iptables -A INPUT -p tcp -s 172.16.238.187 ---dport 22 -j ACCEPT  
```  
Option | Description
------ | -----------
-A | Add Rule
-p | Protocol
-s | Source
-d | Destination
--dport | Destination Port
-j | Action to Take  
                             
                             
Additional comment:
- -A add or append rules
- -s source IP address or IP addres range
                             
Second rule to drop all other IP address:  
```
$ iptables -A INPUT -p tcp --dport 22 -j drop
```
This rule will be added as the next rule to the iptable and the iptable rules are read from "Top to Botton"  When a rule is met, the remaing rules are skipped.  
                             
Additional rules for the example:
Source/Destination | Action
------------------ | ------
devdb01 | Allow outgoint connectiop to port 5432
caleston-repo-01 | Allow outgoing connection to port 80
Internet | Drop all outgoing connections, Port 80/443
Caleston-lp10 | Allow incoming on port 80  

Rules example:
```
$ iptables -A OUTPUT -p -tcp -d 172.16.238.11 -dport 5432 -j ACCEPT    
$ iptables -A OUTPUT -p -tcp -d 172.16.238.15 -dport 80 -j ACCEPT     
$ iptables -A OUTPUT -p -tcp -dport 80 -j DROP  
$ iptables -A OUTPUT -p -tcp -dport 443 -j DROP     
$ iptables -A INPUT -p -tcp -s 172.16.238.187 -dport 80 -j ACCEPT
```                               
                             
Use -i option to insert a rule at the top fo the chain
```
$ iptables -I OUTPUT -p tcp -d 172.16.238.100 --dport 443 -j Accept
```
                             
To delete a rule use the -D for delet and the rule line number:
```
$ iptables -D OUTPUT 5
```
                             
Typically to restrict access to a machine from a particular source you would create the first rule to accept the particluar server (IP address) and the second rule would be to drop all other IP addresses.  

## Cronjobs
Schedule jobs on Linux using Cron.  Use the crond service to schedule jobs on Linux.  
                             
To schedule a job - do not use sudo to scehdule job because the job will be scheduled to run with suod - this opens contrab in VI:
```
crontab -e
``` 
Scheudle job example in the job config file  
```
# m h  dom mon dow  command                             
  0 21 *   *   *    uptime >> /tmp/system-report.txt
```  
To schedule job to run 8:10 AM 19th February:
```
10 8 19 2 *
```
* indicates run the particluar field all the time.  Everyminute, every hour, day, month, every weekday  
  
If you want something to run every other minute, day, ext. use */2 in the appropriate field  
  
To list all jobs scheduled in cron run:
```
$ crontbab -l
```
List crontab log file
```
$ cat /tmp/system-report.txt
```
  
## Networking
#### DNS overview  
You can add hostname and ip address to the /etc/hosts file.  This is the source of truth for the host where this configed.  The destination system may have a different name.  This is known as host resolution.  You can ping, but ping maybe disabled on the target system, so consider using nslookup or dig.  
  
hosts file is ok for a small network.  It's easier to move all the hostnames and ip addressess to a DNS server and point all servers to a DNS server for name resolution.   
  
Every system has a dns hostname file to resolve the DNS location in the /etc/resolv.conf file.  Add 'nameserver 192.168.1.100' to the /etc/resolve.conf file.    

If you add information to the /ect/hosts file, the system looks for hostname resolution there and then the DNS server.    
  
You can change the order can be modified modifying the /etc/nsswitch.conf  
  
What if you ping an address not in the /etc/hosts or DNS server.  You can add a second (multiple) name servers in the /etc/hosts file and your system will look there if the systems are not listed in /etc/hosts or your DNS server.
 
##### Domain names  
.com .net .edu .org .io define the domain "purpose"  
  
Root -> . ; .com -> top level domain ; subdomaind -> www or maps or mail    
  
When asking for resolution, your system first goes to internal DNS.  If not found there then you are forwarded to an external DNS server and it is was forwared to different DNS serviers until the domain is resolved.   

Enable shortcut in /etc/resolv.conf
```
nameserver   192.168.1.100
search       mycompany.com prod.mycompmy.com
```
  
DNS Record Types   
How records are stored in DNS:  
A record types map hostname to an ip addresses 
AAAA record types map hostname to ipv6 addresses
CNAME Record types maps one hostname to another  hostname  - name to name mapping   

Other look up options:  
- nslookup - doesnt look at /etc/hsts
- dig 
  
  
#### Switching and Routing
How does server a reach server b.  We need a switch and an interface on both systems.  
```
$ ip link
```
Assign ip address to computer:  
```
$ ip addre add 192.168.1.10/24 dev eth0
```
 
Say wwe have another network with server c and server d, with a subnet 192.168.2.0 how do we reach the other network.  We use an intelligent (computer like device) called a router.  We connect the two networks via the router: 192.168.1.1 and 192.168.2.1.  We configure a gateway to route between the networks.   
  
Run route command to see routing options for your Linux machine
```
$ route
```
To add a route:
```
$ ip route add 192.168.2.0/24 via 192.168.1.1
```
For any network where you want an unknown ip address sent use the default router setting:
```
$ ip route add default via 192.168.1.1
```
You could also 0.0.0.0 in the gateway field.  However if you have a public and private internet you will need a second router and gateway "link"

To persist ip link, ip addr, ip addr, add these to the network interface file.
  
  
 #### Network Troubleshooting
 - time out error
    - run $ ip link to make sure interface link is up
    - Check to resolve that hostname is resolving - $ nslookup <server hostname>
    - Ping remote server - $ ping <host-name> - not the best tool to check connectivity as some devices may have it disabled
    - Run traceroute command to troubleshoot the routing - $ traceroute 192.168.2.5
    - On the server you can if a process is running on a particular port like port 80 - $ netstat -an | grep 80 | grep -i LISTEN
    - If web server for example is running try running the ip link command on the server
    - Bring link up with - $ ip link set dev <interface name> up
  

## Storage in Linux  
  
#### Disk Partitions
Basic Concepts like block devices  
Block device is a type of file that can be found under the /dev direction and represents a spinning disk or SSD. It is a called block storage cause data is written in blocks or "chunks" of data  
  
To see block storage run:
```
$ lsblk
$ ls -l /dev/ | grep "^b"
```
Each block device has a major and minor number.  The first number reprsent block device type and the second number identifies the whole disk and partitions created  
  
Major Number | Device Type
------------ | -----------
1 | RAM
3 | Hard Disk or CD ROM
6 | Parallel Printers
8 | SCSI DISK

The diks is broken down into smaller parts of the disk.  Partition segments space for use of a particular purpose.  You don't have to partition a disk.  You can use it as is.  But partitioning makes the disk more usable.  
  
Partition information can be foudn with the fdisk command
```
$ sudo fdisk -l /dev/sda
```  
There are three types of disk partitions
- Primary
- Extended
- Logical partition
  
Primary partition used to boot the system.  There is a limit 4 primary partition.   
  
Extend partitions host logical partitions - 1 or more.  Extended partition is like a disk drive with its own partitions.  

Partition table or partitioning scheme defines how a disk is partioned.  We see the traditional MBR partition - Master Boot Record which has been around for over 30 years.  Only 4 primary partitions in MBR and maximum size 2 TB.  If we want more partitions we would use a extended partition and carve out logical partions.   
  
GPT is another partition scheme and stands for GUID Partition Table.  A more recent partitioning scheme to address MBR limitations.  GPT allows unlimted number of partitions and no max sizer per partiion.   

Creating partitions
```
$ lsblk
```
If you see a non-partitioned disk run gdisk to partition.  gdisk will is menu driven.  Use $ gdisk ? to see options.   
  
Once your disk is partioned run:
```
$ sudo fdisk -l /dev/sdb
```
#### File Systems in Linux  
Partitioning alone do not make the disks usable.  TO write disk you must create a file system and mount to a direct to read and write data to the partition.  
  
EXT2 | EXT3 | EXT4
---- | ---- | ----
2 TB File Size | 2 TB File Size | 16 TB File Size
4 TB Volume size | 4 TB Volume size | 1 Exabyte
supports compression | Uses Journal | Uses Journal
Support Linux permissions | Backwards Compatible | Backwards Compatible
Long crash Recovery | Faster Crash Recovery | Uses chsksum for Journal
  
Creating file systems:
```
$ mkfs.ext4 /dev/sdb1
$ mkdir /mnt/ext4;
$ mount /dev/sdb1 /mnt/ext4
$ mount | grep /dev/sdb1
$ df -hP | grep /dev/sdb1
```
To make permanent after reboot after entry to /etc/fstab:  
  
Field | Purpose
----- | -------
Filesystem | Such as /dev/vdb1 to be mounted
Mountpoint | Directory to be mounted on
Type | Example ext2, ext3, ext4
Options | Such as RW or RO
Dump | 0=ignore, 1=backup
Pass | 0=ignore, 1 or 2 = FSCK filesystem check enforced
  
Add to file:
```
#<file system> <mount point>    <type>           <options>                              <dump>           <pass>
/dev/sda1      /                ext4             defaults,relatime,errors=panic            0               1 ~
```
or
```
echo "/dev/sdb1  /mnt/ext4   ext4 rw 0 0" >> /etc/fstab
```
    
 
#### DAS, NAS and SAN
Three types of storage - DAS -> Direct Attached Storage, NAS -> Network Attached Storage, SAN -> Storage Area Network (fiber attached)  
DAS - connects directly to the host system s
  

DAS | NAS | SAN
--- | --- | ---
Directly Attached | Network attached NFS/CIFS | Storage Area Network FC (Fibre Channel) or iSCSI
Block Storage | File storage | Block Storage
Fast and Reliable | Reasonably Fast and Reliable | Fast, secure and reliable
Dedicated to single host | Shared Storage | Highly Available
Ideal for small Businesses | Mid/Large Business | Enterprise Storage
Suitable for OS | Not suitable for OS | Not suitable for OS
  
 #### NFS File System  
 Saves data in form of files in client/server model.  NFS Server has /software/repos is shared across the network via a mount.  Sharing disk in NFS is called exporting.  
  
NFS server maintains exports file at /etc/exprots that maintains a list of servers that can access the folder
```
/software/repos hosta hostb hostc
```
Specific ports may have to be opened for NFS to work
```
$ exportsfs -a
$ exportsfs -o 10.16.13.201:/software/repos
```
On the server mount the drive with:
```
$ mount 10.16.13.201:/software/repos /mnt/software/repos
```
  
#### LVM
Logical Volume Manager allows grouping of multiple physcial drives into one volume group where you can then carve out logical volumes.   This allows logical volumes to be easily resized.  
   
 
Install LVM
```
$ apt-get install lvm2
$ pvcreate /dev/sdb
$ vgcreate caleston_vg /dev/sdb
$ pvdisplay
$ vgdisplay
$ lvcreate -L 1G -n vol1 caleston_vg
$ lvdisplay
$ lvs
$ mkfs.ext4 /dev/caleston_vg/vol1
$ mount -t ext4 /dev/caleston_vg/vol1 /mnt/vol1
```
Resize volumen - first see if there is enough size
```
$ vgs
$ lvresize -L +1G /dev/caleston_vg/vol1
$ dfh -hP /mnt/vol1
```
Resize file system size
```
$ resize2fs /dev/caleston_vg/vol1
$ df -hP /mnt/vol1
```

  
## Service Management with SYSTEM
Specs for the Service - start with all these conditions
- Program - /user/bin/project-mercury.sh - ExecStart= /bin/bas /user/bin/project-mercury.sh
- start Python application after Postgress DB is started - After=postgresql.service
- User Service account project_mercury - User=project_mercury
- auto restart on failure - Restart=on-failure
- Restart interval 10 seconds - RestartSec=10
- Log service events - automatically logged
- Load when booting into graphical mode - WantedBy graphical.target   
  
Build on conditions one at a time:    
Create mercury service file under /etc/systemd/system/project-mercury.service
```
[Unit]
Description=Pythong Django for Project Mercury
Documentation=http://wiki.caleston-dev.ca/mercury
After=postgresql.service
[Service]
ExecStart= /bin/bash /user/bin/project-mercury.sh
User=project_mercury
Restart=on-failure
RestartSec=10
[Install]
WantedBy graphical.target
```
  
To run type:
```
$ systemctl start project-mercury.service
$ systemctl status project-mercury.service
$ systmectl stop porject-mercury.service
$ systemctl restart project-mercury.service
$ systemctl reloaddameon
```
   
  
#### SYSTEMD Tools
Two major tools are systemctl and jorunalctl
  
sytemctl can 
- manage system state
- start, stop, restart, reload
- enable/disable
- list and manage units
- list and update targets
  
                             
                             
systemctl commands
```
$ systemctl start docker
$ systemctl stop docker
$ systemctl reastart docker
$ systemctl reload docker
$ systemctl enable docker
$ systemctl disable docker
$ systemctl status docker
$ systemctl daemon-reload
$ systecmctl edit project-mercury.service --full
```
Manage state
```
$ systemctl get-default
$ systemctl set-default mutli-user.target
$ systemctl list-units --all
```
  
journalctl - checks journal or log enteries
```
$ journalctl
$ journalctl -b
$ journalctl -u docker.service
```
                         
                             
                         
                          

