**link**: https://play.picoctf.org/practice/challenge/109
**challenge:** web exploitation

![Screenshot (606)](https://github.com/user-attachments/assets/8998dfd6-4f34-4b82-bbd8-a2d4fce4f9a2)

**description**:
i sent out 2 invitations to all my friends for my birthday! i'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different!. you wouldn't believe how long it took me to find a collision. anyway, see if you're invited by submitting 2 pdfs to my website. http://mercury.picoctf.net:50970/

step 1: visit the link:
![Screenshot (607)](https://github.com/user-attachments/assets/05a68e61-4309-4e0a-b475-08999201c1be)

we are asked to upload two files to the website , but in a pdf format. remember, the description says "two invites that are similar with the same md5 hash". therefore, we will be using two similar files in a pdf format having the same md5 hash. 

after making a research of what the problem was, it was tagged to an md5 collision. md5 is vulnerable to collision, where two input create the same hash value. this poses a security risk particularly in applications like digital signatures.  here is a link to read more about it https://www.mscs.dal.ca/~selinger/md5collision/.
![Screenshot (610)](https://github.com/user-attachments/assets/5a6c18b5-ebc3-4b75-8bb1-44ee58c1e3c8)
we found two similar programs in the link above, you can download for your preferred workstation. after downloading the two programs, you should rename with a ".pdf" extension because that is what the format the website wants.

note: if you upload two pdf files that are not having the same md5 hash, you'll get an error saying "md5 hashes do not match" or if you upload files in another format different from pdf, you'll get a "not a pdf" error.
![Screenshot (609)](https://github.com/user-attachments/assets/33e02ce8-d104-40f5-8c1d-336300ce4766)

after renaming with a .pdf extenion, click on upload and we will get the flag:
![Screenshot (608)](https://github.com/user-attachments/assets/1ef3c7c6-b901-454b-a787-f7b9caa7228c)

