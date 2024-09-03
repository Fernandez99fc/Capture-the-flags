<h1>RootMe!</h1>
<b>Link: https://tryhackme.com/r/room/rrootme</b>


![Screenshot (134)](https://github.com/user-attachments/assets/70859579-a8b3-4992-b7db-9bd448f66076)

Using nmap to scan for open ports:
![Screenshot (135)](https://github.com/user-attachments/assets/2ed46ff1-7250-4763-83c9-5e5bfcee57d1)

Two ports are open: 

- ssh (port 22)
- http (port 80)

We can firstly check what is running on the web server on port 80 by visiting the ip address on the browser:
![Screenshot (136)](https://github.com/user-attachments/assets/ea18260a-66bd-4266-a2c8-804d29d02b7f)

We see a black page with a text "can you root me?"

After view the page source, we found nothing useful:
![Screenshot (137)](https://github.com/user-attachments/assets/ec864ef1-dba4-46bf-af22-16b30c09f53e)

Next is to target ssh. We see the version ssh uses, which is <b>openssh 7.6p1</b>. We can check if there's an exploit for that using metasploit or searchsploit.

Using "searchsploit openssh 7.6"  to search for an exploit for the version
![Screenshot (138)](https://github.com/user-attachments/assets/c2f37532-3343-410e-a11d-3ed477dc39e5)

We can see it is vulnerable to username enumeration

Open up metasploit using "msfconsole" command.
search for openssh
![Screenshot (139)](https://github.com/user-attachments/assets/6ad98380-52fe-4189-bfe6-4c69b2fc1757)

we will use option 3. type "info 3" to see information about module 3 which is username enumeration.
![Screenshot (140)](https://github.com/user-attachments/assets/cf1c7fd0-1bee-4f6d-a016-5ed3443ad67d)

By using "show options", we can see and set the parameters needed for the enumeration.
![Screenshot (141)](https://github.com/user-attachments/assets/93ff0624-cb51-4e6f-949b-460ab8982712)

After setting the required parameters, the enumeration failed. 

We can scan for hidden files on the previous web server we visited if we can find any useful info.

we can use gobuster to search for hidden file and directories:
![Screenshot (143)](https://github.com/user-attachments/assets/a6cf962c-5b69-43f5-95e5-5863adb0df5e)

We can see a /panel directory which allows us to upload files. We can try to upload a malicious script to get a reverse shell.
![image](https://github.com/user-attachments/assets/bc1ff669-7749-40aa-8e03-aa4aca2f2f55)

We can use msfvenom to generate a payload and output to a file:
![Screenshot (150)](https://github.com/user-attachments/assets/f322adbc-d2e4-42c1-a573-da165823b9f8)
add .phtml extension to the file.

set up listener using exploit/multi/handler module in metasploit:
![Screenshot (151)](https://github.com/user-attachments/assets/776ec039-673b-4bf2-b5e8-de421eecce10)

set the payload to "php/meterpreter/reverse_tcp" and the required options.
Type "run".

Next, upload to the server and execute file in /uploads directory.
We should have a shell.
![Screenshot (152)](https://github.com/user-attachments/assets/3b160b4f-b6bc-4749-ac41-500468702bfb)

<h2>Escalating Privilege to root!</h2>

Using a program having setuid bit, we can escalate privileges. Using the find command to find files with the setuid bit set.

Type "shell" to enter a shell environment and type "export TERM=xterm".

Using find:







