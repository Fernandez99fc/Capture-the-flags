**link**: https://ctflearn/challenges/109

**title**: don't bump the header

**difficulty**: medium

**objective:**
try to bypass my security measure on this site! http://165.227.106.113/header.php![VirtualBox_LINUX SERVER_17_04_2024_14_25_32](https://github.com/user-attachments/assets/1a57e353-9ecc-47bf-8f45-dd118ae6d6de)

the first step is visiting the website http://165.227.106.113/header.php
![VirtualBox_LINUX SERVER_17_04_2024_14_29_04](https://github.com/user-attachments/assets/9be31138-0af5-4db9-8693-a49ad3ef7bcd)

when we visited the url, we saw an error message displayed indicating that our user agent is not correct and we can't access the website, also telling us the version of the user-agent that is in use, which is mozilla 5.0 running on linux.

by viewing the page source, we get this:
![VirtualBox_LINUX SERVER_17_04_2024_15_34_00](https://github.com/user-attachments/assets/cba42e24-7d70-42d6-aa88-4253530b6d85)

seems the website does'nt want us to have access because we are not using it's preffered user-agent "sup3rs3cr3tag3nt". what we can do now is try to intercept the http traffic and modify the http request with burpsuite and changing the user-agent to sup3rs3cr3tag3nt to see if we can gain access.

![VirtualBox_LINUX SERVER_17_04_2024_15_48_19](https://github.com/user-attachments/assets/4d46838e-a2a5-4c72-b261-52ccb6855c53)
![VirtualBox_LINUX SERVER_17_04_2024_15_48_39](https://github.com/user-attachments/assets/3fe6580a-7880-463d-bf24-27941e0b8ae3)
user-agent has been changed to sup3rs3cr3tag3nt, we can now forward the traffic to the target website.
![VirtualBox_LINUX SERVER_17_04_2024_15_49_05](https://github.com/user-attachments/assets/3cb59cec-f6dc-4d80-b380-2e60684f7db5)
we can see a message aboce saying we didn't come from the site "awesomesauce.com", which means our traffic isn't coming from "awesomesauce.com". we need to reload the page and forward the traffic to repeater to modify the request, then what we need to do is change our user-agent to " sup3rs3cr3tag3nt" again and act like the traffic is being forwarded from "awesomsauce.com" by adding a "referer" line in the header of the request pointing to that website.  adding "referer: awesomesauce.com" to the http request header. 
![VirtualBox_LINUX SERVER_17_04_2024_17_00_12](https://github.com/user-attachments/assets/9a3b8ac3-d0f5-4901-8a07-92138adfa8a8)

we can see we got a flag in the response header
flag: flag{did_this_m3ss_with_y0ur_h34d}




