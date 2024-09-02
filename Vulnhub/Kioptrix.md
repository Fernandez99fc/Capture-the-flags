
link: https://www.vulnhub.com/entry/kioptrix-level-12-3%2c24/
name: kioptrix
web application

note: (phymyadmin is used for managing mysql database and should be protected or limited to authorized hosts.)

enumeration
--
we will be using nmap to scan for open ports on the target ip.
![VirtualBox_LINUX SERVER_23_04_2024_20_24_59](https://github.com/user-attachments/assets/c81be95e-4103-4e43-9c45-49b15dd20bca)
two ports found: 
ssh(22) and http(80).

i have tried logging in via ssh, it didn't work. i'll have to try visiting apache server running on port 80 http. firstlt, i'll add the target ip to my /etc/hosts configuration file to be able to load the webpage quickly.
![VirtualBox_LINUX SERVER_23_04_2024_20_44_06](https://github.com/user-attachments/assets/6eabb01c-1533-4ec2-b228-2b98d769bf2b)
i went to the blog page and read the documentation, i realized that the page is being hosted on http://kioptrix3.com and it has a gallery page. 
![VirtualBox_LINUX SERVER_23_04_2024_20_51_32](https://github.com/user-attachments/assets/6145a670-fdae-47a7-8d4c-588a5691e41d)
i visited the link provided http://kioptrix3.com/gallery which shows the gallery page of the website
![VirtualBox_LINUX SERVER_23_04_2024_20_53_42](https://github.com/user-attachments/assets/440ab96b-e890-4122-94a6-778c8011d334)
nothing much to do here.
i found a login page on the website
![VirtualBox_LINUX SERVER_23_04_2024_20_56_08](https://github.com/user-attachments/assets/1f89deb3-e592-4964-b4ed-48d6c9eb09f1)
i found the name of the cms, which is lotuscms, let's check if it has known exploit using metasploit.

![VirtualBox_LINUX SERVER_23_04_2024_21_02_09](https://github.com/user-attachments/assets/f6a70493-4819-4769-a692-f3d1850a5c52)
using show options command to view the available options. i set my rhost,uri and changed the payload to generic/shell_bind_tcp.
![VirtualBox_LINUX SERVER_23_04_2024_21_09_50](https://github.com/user-attachments/assets/465eadcd-0632-4a67-90c8-01d9561d2bdc)
![VirtualBox_LINUX SERVER_23_04_2024_21_12_58 1](https://github.com/user-attachments/assets/138f62c9-8a2d-4c96-b280-0bd1f1d8ec3b)

i was able to get a reverse shell!
![VirtualBox_LINUX SERVER_23_04_2024_22_02_40](https://github.com/user-attachments/assets/3593959b-af8c-439c-9f08-cd644daf860c)
![VirtualBox_LINUX SERVER_23_04_2024_22_00_41](https://github.com/user-attachments/assets/7e9090a9-9aab-4169-ac4a-742407c6780b)
i logged in as root with the credentials found.
![VirtualBox_LINUX SERVER_23_04_2024_22_06_35](https://github.com/user-attachments/assets/f8cb794f-25e7-4952-8e0f-e3b0e82b1700)

browse to gallery and then to dev_account
![VirtualBox_LINUX SERVER_23_04_2024_22_07_33](https://github.com/user-attachments/assets/44c27223-985f-408b-94ca-16dad62e99c6)
navigating through the dev_accounts diretory, i found some usernames and passwords which included was "loneferret". the name of the new employee.
![VirtualBox_LINUX SERVER_23_04_2024_22_09_24](https://github.com/user-attachments/assets/b5ab8566-6bdd-4e0c-8aa1-09d76f265748)

we see the passowrd hash for the user loneferret. we will crack it using john the ripper, a password cracking tool i'll copy the password into a file.
 in order to identify the hash type in use, i'll use hash-identifier command.
 ![VirtualBox_LINUX SERVER_23_04_2024_22_35_41](https://github.com/user-attachments/assets/7cb80271-1c6f-4738-b767-c6c6c776788f)

 it appears to be an md5 hash.
 we will use the md5 syntax with john the ripper.
 ![VirtualBox_LINUX SERVER_23_04_2024_22_37_15](https://github.com/user-attachments/assets/0accf7f0-0dcf-4e21-bc84-ad4fe72a6276)

 john cracked the password and we now know the password is "starwars".
 --format=raw-md5 - indicated that it is an md5 hash
since we have an idea of the username and password, i will try logging in via ssh with the user "loneferret" and password "starwars".
![VirtualBox_LINUX SERVER_23_04_2024_22_46_33](https://github.com/user-attachments/assets/4ef6e713-5e46-4e4b-805a-ad5cd5abf6f9)

found a file in the directory , i'll read using cat.
![VirtualBox_LINUX SERVER_23_04_2024_22_48_07](https://github.com/user-attachments/assets/66c0e19e-7cf6-4e48-849e-438ae1d85c26)
it is asking us to use the command "sudo ht". it throws up an error. i'll view the commands the user can run as root by running "sudo -l".
![VirtualBox_LINUX SERVER_23_04_2024_22_50_28](https://github.com/user-attachments/assets/309fb0b3-3c52-45a2-ab98-4920402d74d5)
we can see the user is able to run /usr/bin/su and /usr/local/bin/ht as the root user. if we run "sudo ht" again, we will get the same error, we can resolve this by doing an "export term=xterm".
![VirtualBox_LINUX SERVER_23_04_2024_22_56_33](https://github.com/user-attachments/assets/0c4639ac-0596-48ec-ad48-a2c2702794d5)

![VirtualBox_LINUX SERVER_23_04_2024_22_56_01](https://github.com/user-attachments/assets/7620c802-3130-4599-a440-00430c9c3a06)
we are greeted with the ht text editor. the goal is make the loneferret user have superuser permissions over all commands

i will press alt+f and go to open:
![VirtualBox_LINUX SERVER_23_04_2024_22_59_08](https://github.com/user-attachments/assets/ec4820f1-4c5a-485d-8d3b-eca1ccfdfaca)
then enter the /etc/sudoers file
![VirtualBox_LINUX SERVER_23_04_2024_23_00_12](https://github.com/user-attachments/assets/889f3dca-ad05-4f46-a2be-86143774a631)

we are now in the /etc/sudoers file, where we will grante loneferret with root privileges.
![VirtualBox_LINUX SERVER_23_04_2024_23_00_34](https://github.com/user-attachments/assets/330450d0-faf4-4e08-9f3a-8b1152eb691e)
i will remove everything on the loneferret line and replace it with the same options as root.
![VirtualBox_LINUX SERVER_23_04_2024_23_03_54](https://github.com/user-attachments/assets/2f231f08-78c0-404f-a3d1-10da1a07fa81)
from here, i will save using alt+f again and close.
![VirtualBox_LINUX SERVER_23_04_2024_23_10_31](https://github.com/user-attachments/assets/e8970204-d51d-41f4-9f25-241f1a17601c)

we can now run sudo su and we finally got into the root account.

