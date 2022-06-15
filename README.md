
# KodeKloud Linux Basics Course Notes

A bunch of unedited notes from the KodeKloud Linux Basics Course.  

### Table of Contents
[KodeKloud Linux Basics: Working with the Shell Part 1](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-1)  
[KodeKloud Linux Basics: Linux Core Concepts](https://github.com/pslucas0212/KodeKloud-Linux-Core-Linux-Concepts)   
[Kode Cloud Linux Basics: Package Management](https://github.com/pslucas0212/Kode-Kloud-Linux-Package-Management)  
[KodeKloud Linux Basics: Working with the Shell Part 2](https://github.com/pslucas0212/KodeKloud-Linux-Working-with-the-Shell-Part-2)   
[KodeKloud Linux Basics: Networking](https://github.com/pslucas0212/KodeKloud-Linux-Networking/blob/main/README.md)     
[KodeKloud Linux Basics: Security and File Permission](https://github.com/pslucas0212/KodeKloud-Linux-Security-and-File-Permission)     
[KodeKloud Linux Basics: Service Management with systemd](https://github.com/pslucas0212/Kode-Kloud-Linux-Basics-Service-Management)   
[KodeKloud Linux Basics: Storage in Linux](https://github.com/pslucas0212/Kode-Kloud-Linux-Basics-Storage)












  



  
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
                         
                             
                         
                          

