
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
echo $PATH
```
- Create alias
```
$ alias up=uptime
```
- Change prompt example - Add date between brackets, then username @ hostname working directory : $ -> [Wed Sep 15]bob@caleston-lp10:~$
```
PS1='[\d]\u@\h:\w$'
```
