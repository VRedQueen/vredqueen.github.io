---
title: Easy Forensics
published: 2025-09-30
description: The challenges involved analyzing various file formats including images, disk images, and network captures to uncover hidden data. Key techniques included examining file metadata and hex data for embedded strings, extracting and decoding Base64 from multiple sources (EXIF data, TCP payloads, steganography tools), working with polyglot files, verifying files through SHA-256 hashing, and using specialized tools like zsteg for steganography and tshark for network forensics to reconstruct fragmented payloads.
image: ''
tags: [CTF, picoctf, forensics]
category: PicoCTF
draft: false
---

# Glory of the Garden
*“This garden contains more than it seems.”*

<br>

For this challenge, I started with the provided image. With no immediate clues, I used `exiftool` and `file`, but found nothing unusual—it appeared to be a normal picture.

![](./garden1.png)

<br>

![](./garden2.png)

<br>

I then checked the challenge hint:

![](./garden3.png)

<br>

This pointed me in the right direction. Using Bless Hex Editor, I examined the raw binary data of the image. I searched for the flag and found it directly in the hex data.

![](./garden4.png)

<br>

# Information
“Files can always be changed in a secret way. Can you find the flag? cat.jpg”

<br>

Opening the image revealed only a cute kitten.

![](./information1.png)

<br>

The `file` command confirmed it was a JPEG. Checking the metadata, I noticed a suspicious Base64 string in the Rights field.

![](./information2.png)

<br>

Decoding this string in [CyberChef](https://gchq.github.io/CyberChef/) revealed the flag.

![](./information3.png)

<br>

# CanYouSee
*“How about some hide and seek? Download this file here.”*

I inspected the image metadata and found a Base64 string.

![](./CanYouSee1.png)

<br>

Decoding it gave the flag.

![](./CanYouSee2.png)

<br>

# Secret of the Polyglot
*“The Network Operations Center (NOC) of your local institution picked up a suspicious file, they're getting conflicting information on what type of file it is. They've brought you in as an external expert to examine the file. Can you extract all the information from this strange file?”*

<br>

Opening the PDF revealed the second part of the flag.

![](./polyglot1.png)

<br>

Since a polyglot file can have multiple formats, I changed the extension to PNG and found the first half of the flag.

![](./polyglot2.png)

<br>


# Scan Surprise
*“I've gotten bored of handing out flags as text. Wouldn't it be cool if they were an image instead?“*

<br>

The challenge provided a ZIP file containing multiple nested folders. In the deepest folder, I found a QR code. Scanning it revealed the flag.

![](./scansurprise1.png)

<br>


# Verify
*“People keep trying to trick my players with imitation flags. I want to make sure they get the real thing! I'm going to provide the SHA-256 hash and a decrypt script to help you know that my flags are legitimate.”*

<br>

Connecting to the challenge provided a checksum and a decrypt program.

![](./verify1.png)

<br>

Running the program with the checksum initially failed:

![](./verify2.png)

<br>


Looking inside the `files` folder, there are many files without obvious clues. However, when we check the fingerprints using `sha256sum`, one file has the exact same hash as the one in the text file.

![](./verify3.png)

<br>


Using the decrypt program with this file revealed the flag.

![](./verify4.png)

<br>


# Ph4nt0m 1ntrud3r
*“A digital ghost has breached my defenses, and my sensitive data has been stolen! 😱💻 Your mission is to uncover how this phantom intruder infiltrated my system and retrieve the hidden flag. To solve this challenge, you'll need to analyze the provided PCAP file and track down the attack method. The attacker has cleverly concealed his moves in well timely manner. Dive into the network traffic, apply the right filters and show off your forensic prowess and unmask the digital intruder! “*

<br>

Opening the PCAP in Wireshark revealed 22 TCP streams. In the hex dump, I noticed suspicious Base64 strings appearing across multiple packets.

![](./pcap1.png)

<br>

First, we use tshark with -T fields and -e tcp.payload to locate where the Base64 strings are stored:
`tshark -r myNetworkTraffic.pcap -T fields -e tcp.payload`

![](./pcap2.png)

<br>

With positive results from the previous command, we use: `tshark -r myNetworkTraffic.pcap -T fields -e tcp.payload | xxd -r -p | grep -o '[A-Za-z0-9+/=]\{4,\}' > base64_parts.txt` to extract all Base64 strings into a text file.

![](./pcap3.png)

<br>

Not all strings decoded properly, but valid flag fragments were present.

![](./pcap4.png)

<br>

Filtering for strings ending with "==" and arranging them in order revealed the complete flag.

![](./pcap5.png)

<br>

# RED
*“RED, RED, RED, RED Download the image: red.png”*

<br>

The red square image had nothing unusual in its metadata, but contained a poem, suggesting steganography.

![](./red1.png)

<br>

![](./red2.png)

<br>

Using `zsteg` revealed a Base64 string in the initial analysis.

![](./red3.png)

<br>

Decoding it provided the flag.

![](./red4.png)

<br>

# DISKO 1
*“Can you find the flag in this disk image?”*

First, I used file to identify the image type. Running `strings` produced extensive output which I interpreted as a hint.

![](./disko1.png)

<br>

Filtering for "pico" using `grep` directly revealed the flag.

![](./disko2.png)

<br>

