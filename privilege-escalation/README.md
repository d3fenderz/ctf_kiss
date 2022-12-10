# Easy paths for escalation

At this point, you will likely have the user flag, so a local user to perform a privilege escalation.

I would recommend using automated tools such as [PEASS-ng](https://github.com/carlospolop/PEASS-ng), but in case you prefer the light version, the following can have a high-success rate.

Besides, the scenario might already give you the path, so enumeration is not necessary.

## Linux

```
# find available sudo commands (requires sudo password!)
sudo -l

# find .bak files (you may replace with another file type)
find / -name *.bak 2>/dev/null

# find SUID to exploit (e.g., gtfobins)
find / -perm -4000 -type f -exec ls -lah {} 2>/dev/null \;

# find interesting capabilities to exploit
getcap -r / 2> /dev/null
```

## Windows

```
# initiate rdp connection on port 3389 with local user's credentials
xfreerdp /u:{user} /p:{password} /cert:ignore /v:{IP}

# check the admin group
net localgroup Administrator

# check current user privileges
SeImpersonatePrivilege
SeAssignPrimaryPrivilege
SeTakeOwnershipPrivilege
SeDebugPrivilege

# ldap search
ldapsearch -H ldap://ctfdomain.local:389 -x -b DC={YOUR_DC},DC=LOCAL

# brute force kerberos when there's no lockout policy
kerbrute passwordspray -d {DOMAIN} users.txt {PASSWORD}

# kerberos attack
impacket-getTGT {DOMAIN}/{USER}:{PASSWORD}
```
