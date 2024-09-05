![Screenshot (195)](https://github.com/user-attachments/assets/824c7df0-af1b-4a94-b640-ba54fe523795)

Scan for open ports using nmap:
![Screenshot (196)](https://github.com/user-attachments/assets/037d5e75-b9ec-4759-8b52-ab15055eb175)

- -p- - For all 65535 ports
- -sS - Stealth scan
- - sV - Service version detection

Ports 2222(ssh), 80(http) and 21(ftp) are open

Visiting the http page, we see apache's default page:
![image](https://github.com/user-attachments/assets/9dff2dc2-cc68-4f8c-922a-d55acddef4a2)

Scanning for hidden file and directories using gobuster:
![Screenshot (200)](https://github.com/user-attachments/assets/ec1fddb7-2abd-4577-9981-9021a4afc115)
We found some pages, <b>robots.txt and simple</b> 

Visiting /simple, we found a webpage:
![Screenshot (199)](https://github.com/user-attachments/assets/aea685f3-6aa2-4961-b3cd-4bcf5b216c87)
We realized that the page runs on cmsmadesimple which is a type of content management system. A cms is a software that helps users to create, modify and manage contents on a website.

We can check if cmsmadesimple has an exploit


Tried to logged in as an anonymous user using ftp. It worked but there's nothing muuch to do ther:
![Screenshot (198)](https://github.com/user-attachments/assets/d831b56c-7206-4918-9ce7-a2872b8e1e53)



