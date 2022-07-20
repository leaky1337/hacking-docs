# Linux

## logfiles
`/var/log/auth.log` => authentication  
`/var/log/wtmp` => login/logout/runlevel change, special format need to use **last / who**

`dmesg` kernel log events  
`journalctl` systemd logs (**-r** reverse, **-f** follow, **-u** unit e.g. -u ssh.service)

## Disk

`free -m or -g` (mega / gigabyte)  
`df -h` (disk free)  
`du -hs <file/folder>` (usage)
`df -T` (determine type of filesystem)
`mount -t <type>` (list mounts of given type)

### Mount/Unmount USB

`sudo fdisk -l` (list devices)  
`sudo mount /dev/sdb1 /mnt/usb` (mount device sdb with "location" sdb1 to /mnt/usb)  

`sudo umount /mnt/usb`

## Curriculum

`# find / -type f \( -name "*.csv" -o -name "*.doc" \)  2>/dev/null` => find with multiple values 


# Windows

cmd `ver` => show version  

`attrib <file/folder>` => show attributes (attrib to modify attributes, like hidden or system)  

`dir /TW /OD` => /T< which timefield> /O < sort by>  (here, Time=Last Write, sort by date)  

## Environment

`%< XYZ >%` => e.g. echo %username%  
`set a=b` => list all env or modify in current session  
`setx a b` => modify env variable permanantly in registry  **/M** for system-wide all users

## Links

`mklink source target`  => symbolic
`mklink /h source target` => hardlink  

**.lnk file** => quick and dirty  `type Symlink.lnk | find "\"`  

## locate files

`dir /s *.exe` => /s recursive  
`tree /F` => tree visualization (F with files)  
`forfiles /P C:\Windows /S /M notepad.exe /c "cmd /c echo @PATH"` => P Start, S recursive, M what to search, c execute on every file  

## find / findstr / sort

`find` => used for text  
`findstr` => regex ability   
`sort /R` => reverse  

```
n-th line of file
findstr /n .* C:\Users\test\Desktop\large.txt | findstr "^123:"
```
`findstr /n .*` => add linenumber then "grep"  

`type test.txt | find /c /v "" ` => like wc -l

