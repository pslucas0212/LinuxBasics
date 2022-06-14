
# KodeKloud Linux Basics Course Notes

A bunch of unedited notes from the KodeKloud Linux Basics Course.  

### Table of Contents
[KodeKloud Linux Basics: Working with the Shell Part 1](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-1)  
[KodeKloud Linux Basics: Linux Core Concepts](https://github.com/pslucas0212/KodeKloud-Linux-Core-Linux-Concepts)   
[Kode Cloud Linux Basics: Package Management](https://github.com/pslucas0212/Kode-Kloud-Linux-Package-Management)  
[KodeKloud Linux Basics: Working with the Shell Part 2](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-2)   
[KodeKloud Linux Basics: Networking](https://github.com/pslucas0212/KodeKloud-Linux-Networking/blob/main/README.md)     
[KodeKloud Linux Basics: Security and File Permission](https://github.com/pslucas0212/KodeKloud-Linux-Security-and-File-Permission)     















#### IP Tables
SSH & SCP - remotely access and securly copy remote servers.  To connect to remote server you need to have user base and ssh based authentication along wiht Port 22 open.  
  
Apply network security enterprse wide by using Cisco ASA, Juniper NFGW, Barracuda NGFS, and Fortinet.  Alternatively apply these at a server level using firewalld and iptables on Linux and firewalls on Windows.   

Example: client/laptop 172.16.238.187 with App server 172.16.238.10 and db server on 172.16.238.11 and software repo server 172.16.238.15.  Block Internet.
  
SSH to dev server.  To filter traffic we will use iptables.  On Ubuntu you may need to install iptables.  
To install ip tables on ubuntu and check default rules configured on the system:
 ```
 $ sudo apt install iptables
 ```
 List current iptable rules
 ```
 $ sudo iptables -L 
 ```
We will see three set of rules or chains: input, forward, and output.  
- Input - traffic coming into the servier
- Output - traffic leaving the server to connect to other server
- Forward - Forward traffic to other servers in the network.  Not commonly used in Linux servers  
  
Default rules allow all traffi into and out of the server.  Called a chain because it's a chain of rules.  Either acceppt or drop the packet based on the condition of the rule.  It looks at the conditions in the order of the chain and when a condition is matched the rule is invoked.  It continues through the chain rules until a condition is met.  

Besides the source in the chain rule we can control the target, target port and target protocol
                             
Input rule example:
```
$ iptables -A INPUT -p tcp -s 172.16.238.187 ---dport 22 -j ACCEPT  
```  
Even though we just created a specific rule for 172.16.238.187, the default rule allows all incoming traffice.  Thus we have to create a second rule that drops all incoming requests to port 22.  we only want the 172.16.238.187 client to connect on port 22.

Option | Description
------ | -----------
-A | Add Rule or append rule
-p | Protocol
-s | Source or ip range
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
This rule will be added as the next rule to the iptable and the iptable rules are read from "Top to Botton"  When a rule is met, the remaing rules are skipped.  The sequence of the rules are important as they are executed from to bottom.
                             
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
                             
Use -i option to insert a rule at the top fo the chain.  In the xample we are allow 443 trafic outgoing to the caleston web server
```
$ iptables -I OUTPUT -p tcp -d 172.16.238.100 --dport 443 -j Accept
```
                             
To delete a rule use the -D for delete and the rule line number:
```
$ iptables -D OUTPUT 5
```
                             
Typically to restrict access to a machine from a particular source you would create the first rule to accept the particluar server (IP address) and the second rule would be to drop all other IP addresses.  

Empheral port range - 32768 - 60999

## Cronjobs
Schedule jobs on Linux using Cron.  Use the crond service to schedule jobs on Linux.  User can specify time date and fequency of running a job
                             
To schedule a job in the user you want to run the job, run the crontab -e command. -- do not use sudo to scehdule job because the job will be scheduled to run with suod - this opens contrab in VI:
```
$crontab -e
``` 
Scheudle job example in the job config file.  The first five fields define frequency of the job.  Sixth field defines the command
```
# m h  dom mon dow  command                             
  0 21 *   *   *    uptime >> /tmp/system-report.txt
```  
To schedule job to run 8:10 AM 19th February
Field 1 | Field 2 | Field 3 | Field 4 | Field 5 | Field 6
--------|---------|---------|---------|---------|--------
10 | 8 | 19 | 2 | * | uptime >> /tmp/system-report.txt
minute | hour | day | month | weekday | command
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
                         
                             
                         
                          

