# Easy enumeration

Create a file called `enum.sh` and write something like that inside:

```bash
#!/bin/bash

ip=$1
port=$2
out="basic-enum"

mkdir -p $out
cd $out

nmap -v -p- -sV $ip > nmap.txt
gobuster dir -u http://$ip -t 50 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt > gobuster.txt
nikto -h $ip -p $port > nikto.txt
cd -
```

Use the above like that:

```
bash enum.sh <IP> <PORT>
```

or:

```
chmod +x enum.sh
./enum.sh <IP> <PORT>
```

Read `nmap.txt`, `gobuster.txt`, and `nikto.txt`
