## Don't bet everything on the technique

Of course, you need technical skills. However, it is a bit less challenging to get such knowledge these days, as there are beautiful resources in various formats (blogs, videos, podcasts, and many more).

Keep it simple. It's a rewarding strategy. CTFs are like puzzles and riddles. The game will have its own logic, but, at least, there is one.

## Check the level

If the CTF is marked as "easy," it does not necessarily mean it's technically easy, but it will likely include a known CVE, so you can exploit it with a public PoC. In my experience, [Pwnkit](https://nvd.nist.gov/vuln/detail/CVE-2021-4034) works pretty well.

You can often solve old CTFs, while the scenario may involve much difficult paths. It's a bit unfair, but creators usually don't have the time to update their machines with the latest patches. Besides, the idea is to provide flawed environments.

Unlike TryHackMe, the HTB (HackTheBox) community only gives the solution to those who manage to unlock the precious flags **before**. There's a forum where you can get more hints, for free, but nobody will disclose the entire solution, which I find a better approach[^1].

I've unlocked several machines with alternative paths and used the flags to discover the expected solution, and I'm Okay with that. If I can get the flag, I deserve the knowledge. 

[^1]: It's not perfectly exact, as some hackers have a different opinion and like to share the entire solution on their blogs, their YouTube channel, or "non- official" forums. If I were a BlackHat, I would likely try to flag _eager_ players or even hack them with tricked .zip files. However, I did not take that path, so I don't know. Just consider the risks.

## Use pre-configured and isolated environments

Building your own lab can be tedious, but it keeps getting easier, for example, using virtual machines and pen-testing distros like Kali Linux or Parrot OS. Sometimes, the platform even provides a container with all the necessary tools, so you don't have to waste your time in setups and configurations.

It's far better to use a robust and relatively secured environment than trying to build your own. Don't put yourself at risk, as your attacker's machine has to accept incoming connections (e.g., for a reverse shell), and if you misconfigure it, you will have security holes.

## Reuse information

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

## Reuse exploits

It's not uncommon that two different CTFs involve quite the same approach. Having your own customized kits and dirty scripts is a great asset that will save time, so don't repeat the same actions all the time.

You may also use [Metasploit](https://github.com/rapid7/metasploit-framework) instead of creating and uploading [reverse shells](https://github.com/jmau111-org/rsgen) manually.

## Leverage dedicated frameworks

Resources like [traitor](https://github.com/liamg/traitor) are particularly efficient in the context of a CTF. If your main goal is speed, which is one of the most meaningful metrics, it's a clever approach.

## Don't forget the fun

As CTFs can be associated with some kind of reputation, it's easy to become addicted to the dopamine influx. Just keep it cool, regardless of your approach.
