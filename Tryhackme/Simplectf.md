![Screenshot (195)](https://github.com/user-attachments/assets/824c7df0-af1b-4a94-b640-ba54fe523795)

Scan for open ports using nmap:
![Screenshot (196)](https://github.com/user-attachments/assets/037d5e75-b9ec-4759-8b52-ab15055eb175)

- -p- - For all 65535 ports
- -sS - Stealth scan
- - sV - Service version detection

Ports 2222(ssh), 80(http) and 21(ftp) are open

Tried to logged in as an anonymous user using ftp. It worked but there's nothing muuch to do ther:
![Screenshot (198)](https://github.com/user-attachments/assets/d831b56c-7206-4918-9ce7-a2872b8e1e53)

Visiting the http page, we see apache's default page:
![image](https://github.com/user-attachments/assets/9dff2dc2-cc68-4f8c-922a-d55acddef4a2)

Scanning for hidden file and directories using gobuster:
![Screenshot (200)](https://github.com/user-attachments/assets/ec1fddb7-2abd-4577-9981-9021a4afc115)
We found some pages, <b>robots.txt and simple</b> 

Visiting /simple, we found a webpage:
![Screenshot (199)](https://github.com/user-attachments/assets/aea685f3-6aa2-4961-b3cd-4bcf5b216c87)
We realized that the page runs on cmsmadesimple which is a type of content management system. A cms is a software that helps users to create, modify and manage contents on a website.

And we can see the version of cmsmadesimple to be v 2.2.8:
![Screenshot (202)](https://github.com/user-attachments/assets/63b099f0-9382-458f-8646-fcc447a2e5da)

we can check if cmsmadesimple has any exploit in metasploit or searchsploit:
![Screenshot (203)](https://github.com/user-attachments/assets/d957489a-a5bb-4a8a-a435-b086d90eaf01)

If we scroll down, we can see it is vulnerable to sql injection because the version is less that 2.2.10.
![Screenshot (204)](https://github.com/user-attachments/assets/ed2ca842-4d5a-49d1-95a0-959d41afaf54)

Download the exploit using <b>searchsploit -p 46635</b> to know the path to the exploit, then copy the exploit to our home directory.
![Screenshot (205)](https://github.com/user-attachments/assets/8ad67741-9950-473a-ac52-f3f66e086112)

It is a python file and we need to install some python modules to make it run.
![Screenshot (206)](https://github.com/user-attachments/assets/cdec6b72-68f1-4bd6-89b9-ce8029336ef5)

I have them installed already.

Then, use pip3 to install colored( a module).
![Screenshot (207)](https://github.com/user-attachments/assets/9ee64871-92df-42b2-af51-a593f4a10a7f)

and same for cprint:
![Screenshot (208)](https://github.com/user-attachments/assets/df62ea33-6081-4594-a04d-e843fd78f796)

We can start supply some options to the python script as seen below, then run it, which is going to perform an sqli
![Screenshot (209)](https://github.com/user-attachments/assets/45e42373-4e07-4d37-aa3a-335bca00ae43)

Injection Begins:
![Screenshot (210)](https://github.com/user-attachments/assets/05523168-b1ae-4cc9-91e2-00ac288b2cde)


After that, we found the password to be "secret". We will try to login to ssh on port 2222 with the username "mitch" and password "secret":
![Screenshot (211)](https://github.com/user-attachments/assets/73e989d2-dbb8-45e3-86eb-77a42e3df2a2)

<b>ssh mitch@ip p 2222</b>

We are in!

Use <b>python -c 'import os; os.system("/bin/bash")'</b> to break out of the restricted shell.

List the content of the file and cat to read:
![Screenshot (212)](https://github.com/user-attachments/assets/194eb91d-5314-4aa0-bcd2-a794e5987bde)

The other user on the system is "sunbath"
![Screenshot (213)](https://github.com/user-attachments/assets/afaf9cde-1565-43e7-b2a5-ee0c165fe98b)

Using sudo -l to see commands we can run as sudo without password:
![Screenshot (214)](https://github.com/user-attachments/assets/c3efa0aa-2bfa-4f32-8265-e5542fa0863f)

We can run vim without password prompt as sudo.

There's a command to escalate privileges as root using sudo vim on <a href="https://gtfobins.github.io/gtfobins/vim">gtfobin.vim</a>

sudo vim -c ':!/bin/sh'

Using "whoami" to see the user we are which is root, we can now read the content of the file root.txt:

![Screenshot (216)](https://github.com/user-attachments/assets/72721112-a265-4420-a530-e69cab8fe415)








