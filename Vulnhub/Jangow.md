![Screenshot (234)](https://github.com/user-attachments/assets/5e3c22be-c1a5-4184-ac0d-e4c09ee9c1ee)<h1>Jangow!</h1>

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

![Screenshot (226)](https://github.com/user-attachments/assets/1e150926-ccaf-4798-8087-4e3c429f52c2)

After checking through the files and directories, we found "/site/wordpress/config.php":
![Screenshot (227)](https://github.com/user-attachments/assets/dd0b03d1-8a0c-407a-8bca-b3fc386341e2)

We see a username likely to be useful which is "desafio02".

Tried bruteforcing ftp with the username but it failed.

I also tried using nikto to check for vulnerability but failed
![Screenshot (228)](https://github.com/user-attachments/assets/c6db10fd-93e7-4696-b645-9806bbad014f)

Revisiting the /site directory, we see a page called "buscar" by the top right, clicking that page or link, we see an equal to or "=" sign
which is mostly likely vulnerable to <b>Command Injection</b>. Since we have an idea of the target Os, we can inject a linux command at the end of the equal to sign and see what it does. We can also use burpsuite to analyze this request.

We can type ls at the back of "=" to list the content of the directory:
![Screenshot (231)](https://github.com/user-attachments/assets/784c72da-a70f-4ae8-87c9-8b95c37fcd20)

Using pwd to print working directory:
![Screenshot (232)](https://github.com/user-attachments/assets/725ebadc-9c18-4ff1-a7c1-d1d4491c26b9)

I would use burpsuite for a good looking format

In burpsuite, I listed the files in the directory then, then listed the files in wordpress
![Screenshot (233)](https://github.com/user-attachments/assets/9981086c-2e10-45d3-a084-ebb585d5c547)

Found config.php file, we can read that too:
![Screenshot (234)](https://github.com/user-attachments/assets/ba10eddf-714b-460c-809b-d2b2d1a0618d)

And we can see some information such as username which is  desafio02 and password to be "abygurl69"

To confirm with the user really exists, we can read the /etc/passwd file using burpsuite:








