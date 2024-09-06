<h1>Jangow!</h1>

<b>Link:</b> https://www.vulnhub.com/entry/jangow-101,754/

<b>Overview:</b>

![Screenshot (217)](https://github.com/user-attachments/assets/545a933c-55ff-4627-9a5d-c1bffdef8338)

There's no goal to hacking the box, and we can see the description says "The secret to the box is enumeration". So let's begin.

<b>Enumeration</b>

We have the Ip address ready:
![Screenshot (218)](https://github.com/user-attachments/assets/dac57ffa-fb5b-489c-bb12-fa2ccb93f14c)

Using nmap to scan for open ports:
![Screenshot (219)](https://github.com/user-attachments/assets/db6dca07-7ad9-4a5c-a61c-5df8a364e912)

Just 2 ports open, which is ftp(port 21) and http(port 80) running apache web server.Good!

No anonymous login for ftp. Next we visit the web server to see what it's running.

![Screenshot (220)](https://github.com/user-attachments/assets/6de217ae-a1b4-42fd-87d4-d49462e4e0b8)
We see this page. We can see the version of apache to be 2.4.18. 

Visiting the /site directory, we see this webpage hosting a site called "grayscale"
![Screenshot (221)](https://github.com/user-attachments/assets/715c2ccc-3947-4047-862f-71ca5d2d054f)

There might be some hidden files and directories. I'll be sticking with dirbuster to scan for files and directories. And i'm using ths because it can also perform a recursive search within subdirectories.
![Screenshot (222)](https://github.com/user-attachments/assets/ad9e0bbb-70b4-44ad-a081-07cd7363b8bf)

![Screenshot (223)](https://github.com/user-attachments/assets/dc73a5fa-f80f-4455-bef8-8b72a7fc0209)

In the result section, we can see files and directories that have been successfully found having a status code of 200, 301 and 302.

After the scan was completed, I generated a report:
![Screenshot (225)](https://github.com/user-attachments/assets/f90a3340-3446-4e36-b4bb-d5a3f12cc36e)




