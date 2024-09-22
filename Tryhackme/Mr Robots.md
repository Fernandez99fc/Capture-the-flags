![Screenshot (315)](https://github.com/user-attachments/assets/8d3dbfaa-f1ab-4dee-8694-82b2cea1b153)<h2>Mr Robots </h2>

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

When we scroll down, we see some encoded texts:
![Screenshot (315)](https://github.com/user-attachments/assets/43c45640-0341-41b6-93bc-ef7caffdb57b)

We can use <a href="https://gchq.github.io/CyberChef">Cyberchef</a> to decode that:




