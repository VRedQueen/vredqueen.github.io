---
title: Easy Cryptography
published: 2025-09-26
description: The challenges involved decoding various cryptographic encodings, including alphanumeric substitution, ROT13 cipher, nested Base64, and Caesar cipher. Key techniques utilized online decoders, automated tools like CyberChef, hash cracking services, and pattern recognition to reverse weak encryption schemes and recover flags from encoded strings and password hashes.
image: ''
tags: [CTF, picoctf, Cryptography]
category: PicoCTF
draft: false
---

# The Numbers
*“The numbers... what do they mean?”*

<br>


This challenge provided the following image:

![](./thenumbers.png)

<br>

At first glance, it might look like a substitution cipher. However, if we look closely, the third number is 3. In alphanumeric code, the number 3 corresponds to the letter 'c'. This gives us a hint that it is a substitution cipher.

We can simply input the numbers into an online decoder.

![](./thenumbers2.png)

<br>

And then, we get our flag:
picoCTF{thenumbersmason}

<br>

# 13
*“Cryptography can be easy, do you know what ROT13 is? `cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}`”*

<br>


All the information we need is right here. We just need to decode the given string using ROT13.

![](./13.png)

<br>

# Mod 26
*“Cryptography can be easy, do you know what ROT13 is? `cvpbPGS{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_jdJBFOXJ}`”*

<br>

Again, we only need to apply ROT13 to decode the flag.

![](./mod26.png)

<br>

# interencdec
*“Can you get the real meaning from this file? Download the file here.”*

<br>

The downloaded file contains a Base64-encoded string. I decided to use [CyberChef](https://gchq.github.io/CyberChef/) to decode

![](./interencdec1.png)

<br>

Decoding this string gives us another Base64 string.

![](./interencdec2.png)

<br>

After the second Base64 decoding, we get what appears to be an encrypted flag. At first, CyberChef doesn’t automatically detect the cipher, but it looks like a substitution cipher. I decided to try a Caesar cipher decoder on [dcode.fr](https://www.dcode.fr/caesar-cipher), and found the flag:

![](./interencdec3.png)

<br>

# hashcrack
*“A company stored a secret message on a server which got breached due to the admin using weakly hashed passwords. Can you gain access to the secret stored within the server?”*

<br>

When we access the machine, we are given three sequential hashes. Each hash can be looked up directly using a search engine. Alternatively, you can use command-line, tools or websites like https://md5hashing.net/hash/sha256/ to crack them.

![](./hashcrack.png)

<br>
