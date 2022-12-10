# Easy Cipher/Decipher

CTFs (easy to medium levels) usually involve some decipher. The creators left encoded messages to help players solve the CTF. 

It can be fun, but some of them are like chasing treasures. The most basic ones will only use base64-encoded strings, but it's rarely that simple. A typical case will involve several layers of various encoding.

## Typical "nooby" mistakes

It's true you don't need any tool to encode/decode base64 strings, but make sure you use something like that in your terminal:

```
echo -n bangerbanger | base64
```

The `-n` option is important to prevent any unwanted additional line during the echo that could mess up everything. The terminal is fine, but ensure you don't make such mistake that can cause headaches.

## Best resources

I recommend [CyberChef](https://gchq.github.io/CyberChef/), even if it's not perfect, as it allows combining various ciphers at will with a very convenient interface.

The Burp Suite Community Edition also includes a module to decode/encode strings, but it hasn't the same level of customization.
