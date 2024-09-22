<h2>Mr Robots </h2>

![Screenshot (292)](https://github.com/user-attachments/assets/054ff140-ca69-4698-996f-a4d5a4c20d8e)

Discover running services with Nmap:
![Screenshot (294)](https://github.com/user-attachments/assets/d5863eea-bc9f-4e49-a45b-db3dd0808564)

- -sC - Enables nmap scripting engine
- -sV - Service version detection
- -sS - Half open scan
- --min-rate - Rate at which packet is sent

![Screenshot (306)](https://github.com/user-attachments/assets/98d36105-26e8-443d-a033-17a7ca2b11ea)

Discovered services are
- 22/ssh (closed)

- 80/http (Apache httpd)
  
- 443/https (Apache https)

<b>Note: My Ip changed. New Ip is 10.10.63.91</b>

Visiting http service on port 80, we see this:
![Screenshot (307)](https://github.com/user-attachments/assets/85ac0719-b717-4b96-a1df-f6e7fea92659)

![Screenshot (308)](https://github.com/user-attachments/assets/786abe0b-bd2d-40d4-8509-0ac776c0ca65)
Played around everything, no useful information at the moment.

Next is to search for files and directories on the server. I will be using gobuster:
![Screenshot (305)](https://github.com/user-attachments/assets/55ecc58a-fe67-4e4f-b6f8-3b695494bbc5)

Found some directories and files including the <b>robots</b> file.

We visit the page to see what we have:
![Screenshot (309)](https://github.com/user-attachments/assets/d514144a-c3d1-4741-979e-81765637a5e8)

We found some restricted pages including a file that looks like a key file. We can visit the page:
![Screenshot (310)](https://github.com/user-attachments/assets/df9d270f-3721-4c39-bc4e-77c08b771d8e)

We got the first key out 3!
![Screenshot (311)](https://github.com/user-attachments/assets/fcf7fbdd-00de-4eb9-aa60-ecd7052b1364)

We have 2 more keys to retrieve.

One of the restricted pages we found in the /robots page is the <b>fsocity.dic</b> file. We get to download the the dictionary file when we visit the page.
![Screenshot (312)](https://github.com/user-attachments/assets/2242682e-21ff-4ad0-8491-12e205fdf500)
Now we have a wordlist.

Found /login page among the file found with gobuster. It resolves to /wp-login.php page when visited.
![Screenshot (313)](https://github.com/user-attachments/assets/a0d933b1-ffb2-4880-9999-be1fb60e8f31)

We have no login information for that.

/license page was among the discovered pages with gobuster. When we visit, we see some texts:
![Screenshot (314)](https://github.com/user-attachments/assets/af46856e-65b8-419f-8d74-86fe34f51133)

When we scroll down, we see some encoded texts. Turns out to be in base64
![Screenshot (315)](https://github.com/user-attachments/assets/43c45640-0341-41b6-93bc-ef7caffdb57b)

We can use <a href="https://gchq.github.io/CyberChef">Cyberchef</a> to decode that:
![Screenshot (300)](https://github.com/user-attachments/assets/59399077-8563-4053-9e56-53000736619a)

After decoding, we get a username "elliot" and password "ER28-0652".

We now have a possible username and password. We can try to login to the previous /login page we found.
![Screenshot (316)](https://github.com/user-attachments/assets/c3deec1f-cec6-41c1-b915-eb27a5882fe0)
We are logged in successfully.

By selecting users, We can see elliot is an admin:
![Screenshot (317)](https://github.com/user-attachments/assets/64f264ca-479e-4e59-a527-35b3d0082f61)

While exploring, we found an editor page which we can use to leverage command injection.
![Screenshot (323)](https://github.com/user-attachments/assets/06d3a7ff-eaef-4691-897d-0af97d5ed533)

Next, I created a php payload with msfvenom:
![Screenshot (320)](https://github.com/user-attachments/assets/e6a66b9d-d8ac-487f-9b60-f7f67451b0af)

I selected the 404.php file(or any .php extension) by the right, cleared all texts in it and pasted my msfvenom payload to spawn a reverse shell.
![Screenshot (322)](https://github.com/user-attachments/assets/fe1eff43-3bad-4d17-b756-769f3e046dbb)

Next, I clicked on update file. 

I setup up my listener using the multi handler module in metasploit and set the required parameters.
![Screenshot (319)](https://github.com/user-attachments/assets/a0df070a-f9c3-4efa-872d-ea21e8157d11)

To execute the code, I visited the page 10.2.26.122/404.php
![Screenshot (324)](https://github.com/user-attachments/assets/4ef2fb30-0aeb-49d9-9446-98c048ed5efc)

We should now have a shell
![Screenshot (321)](https://github.com/user-attachments/assets/05f63ac8-dd62-4331-ad6a-b88757f2aecb)













