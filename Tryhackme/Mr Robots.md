![Screenshot (307)](https://github.com/user-attachments/assets/c4aa6c13-c07c-4d19-994d-d3cbff6a9c7e)<h2>Mr Robots </h2>

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


  



  
