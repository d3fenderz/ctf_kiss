# CTF KISS 😘

Low-hanging fruits and common strategies that require minimum efforts to solve CTFs

## General

### Don't bet everything on the technique

Of course, you need technical skills. However, it is a bit less challenging to get such knowledge these days, as there are beautiful resources in various formats (blogs, videos, podcasts, and many more).

Keep it simple. It's a rewarding strategy. CTFs are like puzzles and riddles. The game will have its own logic, but, at least, there is one.

### Check the level

If the CTF is marked as "easy," it does not necessarily mean it's technically easy, but it will likely include a known CVE, so you can exploit it with a public PoC. In my experience, [Pwnkit](https://nvd.nist.gov/vuln/detail/CVE-2021-4034) works pretty well.

You can often solve old CTFs, while the scenario may involve much difficult paths. It's a bit unfair, but creators usually don't have the time to update their machines with the latest patches. Besides, the idea is to provide flawed environments.

Unlike TryHackMe, the HTB (HackTheBox) community only gives the solution to those who manage to unlock the precious flags **before**. There's a forum where you can get more hints, for free, but nobody will disclose the entire solution, which I find a better approach[^1].

I've unlocked several machines with alternative paths and used the flags to discover the expected solution, and I'm Okay with that. If I can get the flag, I deserve the knowledge. 

### Use pre-configured and isolated environments

Building your own lab can be tedious, but it keeps getting easier, for example, using virtual machines and pen-testing distros like Kali Linux or Parrot OS. Sometimes, the platform even provides a container with all the necessary tools, so you don't have to waste your time in setups and configurations.

It's far better to use a robust and relatively secured environment than trying to build your own. Don't put yourself at risk, as your attacker's machine has to accept incoming connections (e.g., for a reverse shell), and if you misconfigure it, you will have security holes.

### Reuse information

CTFS usually provide "side quests" (~ intermediary steps) where you may get critical information, so don't be confused on what to do next.

While the following cases are not always valid, it's safe to assume the creators included it for a reason:

* read the instructions left by the author before anything (e.g., especially for [the impossible challenge](https://tryhackme.com/room/theimpossiblechallenge))
* HTML comments can reveal credentials in the easiest CTFs, so don't forget to inspect the source code
* if you get a private SSH key, it's probably to unlock the user's flag
* if you get a password (it can be hashed), you will likely use it to access to another instance
* if there's a Basic authentication, you can try very common combinations manually (like `admin:admin`, `admin:admin123`), but Hydra and other Brute-Force tools can automate the process
* sometimes, the usernames are not given explicitly, but the pattern is very easy to guess, like `kmiddleton`
* you can try the same credentials on different instances

CTFs emulate common flaws, like using the same passwords everyhwere. That's why you need to assume such scenario.

### Reuse exploits

It's not uncommon that two different CTFs involve quite the same approach. Having your own customized kits and dirty scripts is a great asset that will save time, so don't repeat the same actions all the time.

You may also use [Metasploit](https://github.com/rapid7/metasploit-framework) instead of creating and uploading [reverse shells](https://github.com/jmau111-org/rsgen) manually.

### Leverage dedicated frameworks

Resources like [traitor](https://github.com/liamg/traitor) are particularly efficient in the context of a CTF. If your main goal is speed, which is one of the most meaningful metrics, it's a clever approach.

### Don't forget the fun

As CTFs can be associated with some kind of reputation, it's easy to become addicted to the dopamine influx. Just keep it cool, regardless of your approach.

## Easy enumeration

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

## Easy Cipher/Decipher

CTFs (easy to medium levels) usually involve some decipher. The creators left encoded messages to help players solve the CTF. 

It can be fun, but some of them are like chasing treasures. The most basic ones will only use base64-encoded strings, but it's rarely that simple. A typical case will involve several layers of various encoding.

### Typical "nooby" mistakes

It's true you don't need any tool to encode/decode base64 strings, but make sure you use something like that in your terminal:

```
echo -n bangerbanger | base64
```

The `-n` option is important to prevent any unwanted additional line during the echo that could mess up everything. The terminal is fine, but ensure you don't make such mistake that can cause headaches.

### Best resources to decipher

I recommend [CyberChef](https://gchq.github.io/CyberChef/), even if it's not perfect, as it allows combining various ciphers at will with a very convenient interface.

The Burp Suite Community Edition also includes a module to decode/encode strings, but it hasn't the same level of customization.

## Hashing

Exploiting hashes is often a mandatory step during CTFs, as hashing is a fundamental principle of hacking, and tools like [John](https://www.kali.org/tools/john/) or Hashcat are important to know for any aspiring hacker.

### What's the problem with hashes?

Hashes are short sequences of characters with a fixed size that will represent the data, for example, passwords. Databases do not store passwords in plain text but hashed passwords.
To check passwords during the login, the same hashing function (~ algorithm) is used, as, theoretically, you cannot have the same output for two different inputs.

However, hashing is not inherently secure, even when it's not reversible. For example, the MD5 algorithm is considered flawed due to bad collisions (same output for different inputs). While it's rare, it has happened.

Besides, hackers have tools (e.g., rainbow tables) to identify hashes for common passwords. That's why strong password policies and salts are essential, but it's another topic.

### Tooling

John can generate hashes from various file types and crack a large range of formats, from hashed passwords to password-protected .zip and even .kbdx databases that only rely on a [weak] master password.

[Hashcat](https://hashcat.net/wiki/doku.php?id=example_hashes) is a bit more complex to use but still very efficient to recover passwords from hashes with valid assumptions and permutations:

```
hashcat -m7401 hash.txt -a3 -1?l?u?d ?1?1?1?1?1?1?1?1 --increment --increment-min 7
```

The above defines additional rules like uppercase, lowercase, special chars, minimum length, which can literally save days of work, comparing to the default options.

## Subdomain enumeration

### When?

When basic enumeration does not reveal anything useful, you can try subdomain enumeration. While it's not the ultimate solution, many CTFs "hide" custom instances in subdomains.

It's a classic troll that aims to lure gamers. Typically, the most obvious interface (e.g., http://{IP}) contains a WordPress or a static HTML/CSS website, but engaging a Brute-Force attack on the login would be a rabbit hole (e.g., strong passwords).

### How?

There are various scripts, including [my custom Bash](https://github.com/jmau111-org/se) that aggregates results from other tools.

## Easy paths for escalation

At this point, you will likely have the user flag, so a local user to perform a privilege escalation.

I would recommend using automated tools such as [PEASS-ng](https://github.com/carlospolop/PEASS-ng), but in case you prefer the light version, the following can have a high-success rate.

Besides, the scenario might already give you the path, so enumeration is not necessary.

## Linux privesc

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

### Windows privesc

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

## Best documentations

* [Hacktricks](https://book.hacktricks.xyz/)
* [HTB guides](https://help.hackthebox.com/en/)
* [GTFObins](https://gtfobins.github.io/)
* [Hacking Active Directory](https://zer1t0.gitlab.io/posts/attacking_ad/)
* [Exploit DB](https://www.exploit-db.com/)
* [The Hacker recipes](https://www.thehacker.recipes/)
* [OWASP](https://owasp.org/)

[^1]: It's not perfectly exact, as some hackers have a different opinion and like to share the entire solution on their blogs, their YouTube channel, or "non- official" forums. If I were a BlackHat, I would likely try to flag _eager_ players or even hack them with tricked .zip files. However, I did not take that path, so I don't know. Just consider the risks.
