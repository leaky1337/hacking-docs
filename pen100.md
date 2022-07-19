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

# Curriculum

`# find / -type f \( -name "*.csv" -o -name "*.doc" \)  2>/dev/null` => find with multiple values  