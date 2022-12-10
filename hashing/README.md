# Hashing

Exploiting hashes is often a mandatory step during CTFs, as hashing is a fundamental principle of hacking, and tools like [John](https://www.kali.org/tools/john/) or Hashcat are important to know for any aspiring hacker.

## What's the problem with hashes?

Hashes are short sequences of characters with a fixed size that will represent the data, for example, passwords. Databases do not store passwords in plain text but hashed passwords.
To check passwords during the login, the same hashing function (~ algorithm) is used, as, theoretically, you cannot have the same output for two different inputs.

However, hashing is not inherently secure, even when it's not reversible. For example, the MD5 algorithm is considered flawed due to bad collisions (same output for different inputs). While it's rare, it has happened.

Besides, hackers have tools (e.g., rainbow tables) to identify hashes for common passwords. That's why strong password policies and salts are essential, but it's another topic.

## Tooling

John can generate hashes from various file types and crack a large range of formats, from hashed passwords to password-protected .zip and even .kbdx databases that only rely on a [weak] master password.

[Hashcat](https://hashcat.net/wiki/doku.php?id=example_hashes) is a bit more complex to use but still very efficient to recover passwords from hashes with valid assumptions and permutations:

```
hashcat -m7401 hash.txt -a3 -1?l?u?d ?1?1?1?1?1?1?1?1 --increment --increment-min 7
```

The above defines additional rules like uppercase, lowercase, special chars, minimum length, which can literally save days of work, comparing to the default options.
