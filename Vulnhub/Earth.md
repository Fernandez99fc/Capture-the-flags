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

Tried bruteforcing ssh with default usernames and a password list but didn't work.

Visiting the web server on port 80, we see a bad request with status code 400:
![Screenshot (254)](https://github.com/user-attachments/assets/020f3d1d-0e86-423d-83f5-4748e87eb55f)

Visiting port 443 running ssl/http, we see this message:
![Screenshot (255)](https://github.com/user-attachments/assets/db98e93a-40a3-40ea-834e-fd1eb8140517)

Stating that the browser sent a request that the server could not understand because we are speaking plain http to an ssl-enabled server port and we should use https to access the site instead of http.

So I will type https://192.168.1.6
![Screenshot (256)](https://github.com/user-attachments/assets/2e74e373-a503-4495-9029-db09b92efbd9)

Here we are at the web page and we can see the content of the page.






