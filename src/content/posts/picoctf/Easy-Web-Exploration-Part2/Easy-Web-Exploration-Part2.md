---
title: Easy Web Exploration Part 2
published: 2025-09-26
description: The challenges involved analyzing web pages, inspecting source and hidden resources (JS/CSS), unminifying code, and decoding Base64 to uncover hidden flags. Key techniques included exploiting SSTI, retrieving credentials from client-side files, manipulating cookies, and intercepting/modifying HTTP requests to bypass authentication.
image: ''
tags: [CTF, picoctf, web, webexploration]
category: PicoCTF
draft: false
---

# Includes

*“Can you get the flag? Go to this website and see what you can discover.”*

<br>

When we accessed the website provided to us, there was a simple page with some text and a button.

![](./includes1.png)

<br>

When we clicked the button, a pop-up appeared telling us that some code is in a separate file.

![](./includes2.png)

<br>

At first, I thought we should check whether any directories existed. But fuzzing didn’t give us anything.

![](./includes3.png)

<br>

Checking the page source showed nothing special either.

![](./includes4.png)

<br>

However, as in previous challenges they placed flags inside JavaScript and CSS files, so it was worth checking those. And sure enough — there was the flag.

![](./includes5.png)

<br>

![](./includes6.png)

<br>

# Local Authority

*“Can you get the flag? Go to this website and see what you can discover.”*

<br>

The challenge gave us a website that only contains username and password input fields.

![](./local_authority1.png)

<br>

Looking at the page source, we didn’t find anything suspicious. However, as we’ve seen in other picoCTF challenges, the authors like to hide things in other files.

![](./local_authority2.png)

<br>

Inspecting login.php, we found some strange code that could point to the path for the username/password or possibly the flag.

![](./local_authority3.png)

<br>

I continued looking through the other files, and in `secure.js` I found the username and password.

![](./local_authority4.png)

<br>

Entering those credentials on the site revealed the flag.

![](./local_authority5.png)

<br>

# Unminify

*“I don't like scrolling down to read the code of my website, so I've squished it. As a bonus, my pages load faster! Browse here, and find the flag!”*

The challenge gave us a site containing just a simple text.

![](./unminify1.png)

<br>

When we opened the source code, it was all on one line. Scrolling horizontally revealed the flag.

![](./unminify2.png)

<br>

# SSTI1

*“I made a cool website where you can announce whatever you want! Try it out! I heard templating is a cool and modular way to build web apps! Check out my website here!”*

The challenge gave us a site that contains a short message and an input field. The challenge name hinted at what needed to be done.

![](./ssti1.png)

<br>

We submitted `{{7*7}}` just to confirm it was SSTI. The result 49 confirmed it:

![](./ssti2.png)

<br>

Next, we needed to determine which templating framework the site was using. Submitting `{{ self.environment.__class__.__module__ }}` lets us check whether it is using Jinja2 (which is more common) or another engine.

![](./ssti3.png)

After confirming the engine, we searched for a Python template RCE payload in [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Python.md?ref=blog.qz.sg#jinja2---remote-command-execution). Using a modified template, we grepped for files containing the known picoCTF flag format:

`{{ namespace.__init__.__globals__.os.popen('grep picoCTF . -rnw').read() }}`

![](./ssti4.png)

<br>

# WebDecode

*“Do you know how to use the web inspector? Start searching here to find the flag.”*

The site we received starts with a joke for us, but also has About and Contact pages.

![](./webdecode1.png)

<br>

On the About page, there was a hint that the flag is there but we needed to inspect.

![](./webdecode2.png)

<br>

I decided to check the page source and found a Base64 string.

![](./webdecode3.png)

<br>

Decoding the Base64 in [CyberChef](https://gchq.github.io/CyberChef/) revealed the flag.

![](./webdecode4.png)

<br>

# Cookie Monster Secret Recipe

*“Cookie Monster has hidden his top-secret cookie recipe somewhere on his website. As an aspiring cookie detective, your mission is to uncover this delectable secret. Can you outsmart Cookie Monster and find the hidden recipe? You can access the Cookie Monster here and good luck.”*

The site contains username and password fields.

![](./cookie_monster1.png)

<br>

After entering random credentials, we received “access denied” along with a hint. Let’s check the cookies.

![](./cookie_monster2.png)

<br>

Inspecting the cookies revealed the “secret recipe.”

![](./cookie_monster3.png)

<br>

Submitting the secret recipe (after removing the trailing `%3D`) in [CyberChef](https://gchq.github.io/CyberChef/) gave us the flag.

![](./cookie_monster4.png)

<br>

# IntroToBurp

*“Try here to find the flag.”*

<br>

The site looked like a registration form.

![](./introtoburp1.png)

<br>

After entering some random data, we were redirected to a 2FA authentication page that required an OTP we didn’t have.

![](./introtoburp2.png)

<br>

In Burp Suite, I attempted to brute-force the OTP.

![](./introtoburp3.png)

<br>


Using the configured tests showed that brute-forcing wasn’t practical — it would take a long time and all requests were returning status `200`.

![](./introtoburp4.png)

<br>

So I reverted and sent the request to Repeater. In Repeater, I removed the OTP parameter from the request. When I sent the modified request, the server returned the flag.

![](./introtoburp5.png)

<br>
