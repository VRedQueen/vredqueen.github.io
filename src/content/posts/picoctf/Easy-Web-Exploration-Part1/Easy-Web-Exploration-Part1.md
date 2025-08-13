# where are the robots

*“Can you find the robots? https://jupiter.challenges.picoctf.org/problem/56830/ (link) or [http://jupiter.challenges.picoctf.org:56830](http://jupiter.challenges.picoctf.org:56830/)”*

When we acess the site, its a simple page with nothing but a title and a phrase. 

So, just like the title of the challege said, let’s acess the robots directory. 

There’s no hints for what to do after this, but we can check the `/1bb4c.html` file.

There it is, the flag.




# Insp3ct0r

*“Kishor Balan tipped us off that the following code may need inspection: https://jupiter.challenges.picoctf.org/problem/9670/ (link) or [http://jupiter.challenges.picoctf.org:9670](http://jupiter.challenges.picoctf.org:9670/)”*

When we acess the link that the challenge propose, it show a simple page with two interactive bottoms, but theres nothing there. 

So, we can start seeing the page source. 

How you can see in the image, part of the flag its there. But where its the rest? In the phrase they that “html is neat” and in the site they said that was maded using html, css and java. 

In beggining of the code, we can see reference to css: `href=”mycss.css”` and another going to `src=“myjs.js”`. 

If we acess to then we have the part two and three of the flag.





# **dont-use-client-side**

*“Can you break into this super secure portal? https://jupiter.challenges.picoctf.org/problem/37821/ (link) or [http://jupiter.challenges.picoctf.org:37821](http://jupiter.challenges.picoctf.org:37821/)”*

When we enter the site, theres only a box that requers a valid credential.

At firts, i thought it will need some client-side vulnerability exploration. But i decided to check the source code.


As much as it seems right, the flag its not in order. You can just you can follow the variable split: 0, split; split, split*2; split*2, split*3; etc.

pico*CTF{no_clients_plz_1*a3c89}




# GET aHEAD

*“Find the flag being held on this server to get ahead of the competition http://mercury.picoctf.net:28916/”*

When we enter the site, there’s only two interactive box that changes the background color. 

I cheked the source code, but there was nothing there. So i decided to check the requests on burpsuite. As we can see, when we choose the red color we receive a GET request, and the blue one get a POST request.

The name of the challege is GET aHEAD and that its a big hint. I send a GET request to the repeater function and change the GET method to a HEAD. When we send the HEAD request, there’s the flag. 




# logon

*“The factory is hiding things from all of its users. Can you login as Joe and find what they've been looking at? https://jupiter.challenges.picoctf.org/problem/13594/ (link) or [http://jupiter.challenges.picoctf.org:13594](http://jupiter.challenges.picoctf.org:13594/)”*

When we acess the site theres only a login box.

Checking the source code, there’s nothing revealing. Let’s send this to the proxy on burpsuite and trying to log with basic credentials, in this case i’ll use *admin:12345*.

That’s the answer on the login page. And the response in the burpsuite it’s interesting.

The administrator cookie parameter is set to false. Maybe we can change it?

Going back to the page, we can go to the inspect mode. In the appication session we can acess the cookies and see the admin parameter. 

Now, we change the False for True and refresh the page. And there’s the flag. 




# Inspect HTML

*“Can you get the flag? Go to this website and see what you can discover.”*
Going to the page, its a simple page with a text. 
However, the name of the challege just said what we have to do. Going to the source code of the page, we have the flag right there. 



# Bookmarklet

*“Why search for the flag when I can make a bookmarklet to print it for me?”*

After lauching the instance, we receive a link to the site. In the site, we have a text and one box with a code. 

At first, i thought we wil need to run some xss script or explore another web vuilnerabilitie. But, i decided to look the code.

This JavaScript code decrypts an encrypted flag using a simple XOR-like decryption algorithm with a predefined key. The code decrypts the **`encryptedFlag`** using the key **`"picoctf"`**. The final outputwill be the flag displayed in a browser alert. 

In this case, we just have to run it.

Opening the console in the inspector mode, we have to type “allow pasting” before put the code. Running the code, opens an alert pop-up containing the flag.




# Cookies

*“Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:29649/”*

Acessing the site, we just have a text with a inout box. 

The source code doesnt give us nothing. So, i decided to put the sugestion that was in the input box and we have this result:

If we try to type another thing in the box, we receive the message: “That doesn't appear to be a valid cookie.”. When we send the site to the proxy on burpsuite, we have this view:

Looking the “snickerdoodle” request, we can see that value of the name changes.

I tried type another cookie’s name and random things in the box, but nothing changes. If the flag was tied to the name value, the easiest way to check its with a code. 

This Python script brute-forces values from 1 to 20 against the name value, checks each response for the string "pico", and prints the successful cookie value along with the flag when found.
When we run the script, we discovery that the right value for name it’s 18, the flag is located at the end of the page's HTML source code.
