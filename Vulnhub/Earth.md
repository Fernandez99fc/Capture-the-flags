<h1>Earth!</h1>
<b>Link:</b> https://www.vulnhub.com/entry/the-planets-earth,755/

![Screenshot (251)](https://github.com/user-attachments/assets/96c9d164-1dfc-4e3e-96d4-91520304440b)

<b>We,ve not started, we already have an idea of the kernel version and the linux distro to be fedora.</b>

<h2><b> Enumeration </b></h2>

Find ip address of the machine using arp-scan:
![Screenshot (252)](https://github.com/user-attachments/assets/fcd96269-e97e-4126-931e-3cf529b52003)

The ip address appears to be 192.168.1.6.

Next is to scan for open ports using nmap:
![Screenshot (253)](https://github.com/user-attachments/assets/7d11501c-9fd9-4b4b-95f1-7174e0b7a697)

Port 22(ssh), 80(http) and 443(ssl/http) running apache web server are open. Good!



