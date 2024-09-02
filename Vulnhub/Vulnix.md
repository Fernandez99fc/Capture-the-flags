![VirtualBox_LINUX SERVER_22_04_2024_15_27_15](https://github.com/user-attachments/assets/7a682568-5d2f-45f6-ad0c-fe6371429a5a)**link:** https://www.vulnhub.com/entry/hacklab-vulnix,48/
**title**: vulnix


description
--
here we have a vulnerable linux host with configuration weaknesses rather than purposely vulnerable software versions (well at the time of the release anyway!)

the host is based upon ubuntu server 12.04 and is fully patched as of early september 2012. the details are as follows:

- architecture: x86
- format: vmware (vmx & vmdk) compatibility with version 4 onwards
- ram: 512mb
- network: nat
- extracted size: 820mb
- compressed (download size): 194mb – 7zip format – 7zip can be obtained from here
- md5 hash of vulnix: 0bf19d11836f72d22f30bf52cd585757

the goal: boot up, find the ip, hack away and obtain the trophy hidden away in /root by any means you wish- excluding the actual hacking of the vmdk.
enumeration
--
the goal is to find a way into the root account of the target machine. we have only just the ip address of the target, nothing else. let's dive in!
the first thing we should do is to gather information about the target using it's ip address, i'll be using nmap in this case to scan for open ports.
![VirtualBox_LINUX SERVER_22_04_2024_10_32_34](https://github.com/user-attachments/assets/09384f37-dc18-48b7-8f49-01dadad65613)
![VirtualBox_LINUX SERVER_22_04_2024_11_12_36](https://github.com/user-attachments/assets/14cf1b70-c1e6-458f-87eb-a48f0cf9f2e5)

we can see we have more than 7 open ports and running services. i'll begin by trying to exploit the most common ports which is ssh,smtp,nfs,imap before touching the rest. we noticed the ssh version is openssh 5.9p1 running on ubuntu server 12.04 as it was stated in the description.

i'll check if openssh 5.9p has any known vulnerability using metasploit framework, you can also use searchsploit. 

![VirtualBox_LINUX SERVER_22_04_2024_13_12_54](https://github.com/user-attachments/assets/ed8f2898-61f0-4b51-89ef-42e591f360eb)

i tried using auxiliary/scanner/ssh/ssh_enumusers module against the ssh service and i found out openssh 5.9p was vulnerable to it. this would enable us to enumerate usernames by sending a malformed packet using public key authentication. you can use "info" command to read more about the module selected. 
![VirtualBox_LINUX SERVER_22_04_2024_13_14_42](https://github.com/user-attachments/assets/6ea5efad-7721-4db8-b8aa-c9f45636dcbc)

set required parameters, you can leave the user_file option empty as metasploit will use it's default wordlist which is also the /usr/share/wordlists/metasploit/unix_users.txt file i set. after, run the "exploit" or "run" command to begin the attack.

![VirtualBox_LINUX SERVER_22_04_2024_13_23_50](https://github.com/user-attachments/assets/e3c4163b-26ad-4a5b-bb38-394be77ab984)
i was able to find some usernames present on the target system. normally, i would copy usernames  user and root into a file and run and run a bruteforce attack against them as the remaining usernames tend to be system users and aren't accessible. if you read the /etc/passwd file on your linux system, you'll see their shell set to "nologin", meaning, it has no shell and no access.

![VirtualBox_LINUX SERVER_22_04_2024_13_30_31](https://github.com/user-attachments/assets/7070d8ff-09e8-451d-8588-c1354da2002f)
using hyrda to bruteforce passwords against the accounts "user " and "root". hydra is an online password cracking tool than runs against more than 40 supported protocols. i would write the two usernames to a file and name it "unixnames". 

![VirtualBox_LINUX SERVER_22_04_2024_13_37_43](https://github.com/user-attachments/assets/e90c1141-9b0f-4c07-bfce-5a199a149f4e)

to cut the long story short, i found out the password for the username "user" which was "letmein" and included it in a password file. running a bruteforce using a wordlist like rockyou.txt would take so long.
**arguments:**
-t - number of threads to run
-l - specifiies a file containing usernames
-p -  specifies a file containing passwords
-u - try passwords against every user(u)
-f -  stop on the first valid attempt.

after that, i logged into ssh using the credentials found and there was pretty much nothing interesting to do.
our next step is to target the nfs service to see if there's a share being hosted. nfs(network file system) runs on port 2049 and it is commonly used to share files,directories, drivers e.t.c to other computers on the network.

 we will be using the showmount command: "showmount -e ip address"
![VirtualBox_LINUX SERVER_22_04_2024_15_08_012](https://github.com/user-attachments/assets/40205725-d9b2-4883-a4e9-702de9c864b9)

/home/vulnix is a share found on the nfs server. we will create a directory to mount the shares. i created a direcroty in /mnt filesystem, using "sudo mkdir /mnt/exportdir".   directory named "exportdir"
![VirtualBox_LINUX SERVER_22_04_2024_15_08_01](https://github.com/user-attachments/assets/45e7948d-d3d5-4629-8fae-fb792c1c03d1)

after creating a directory, we then mounted the nfs shares in the directory we created using "mount -t nfs target-ip: shares /mnt/exportdir/"
note: you can create your mount point anywhere you like.

we didn't get access to the share if we change to the mount point where we mounted the shares. most likely because we don't have the permission to access the share on the network.  if we check /etc/exports file of the system when you login via ssh, you'll notice that no one on the network has a permission to the share denoted by the asterik * asterik symbol.

![VirtualBox_LINUX SERVER_22_04_2024_15_19_58](https://github.com/user-attachments/assets/82024202-6407-4078-b143-5540acdccbf8)
what we can do is to create a username "vulnix" same as the target username on our system and change it's uuid to the same uuid of the vulnix account on the target system.  ssh back into the target system and type "id vulnix" to view the uuid of the vulnix account.

![VirtualBox_LINUX SERVER_22_04_2024_15_25_29](https://github.com/user-attachments/assets/78f4951b-4ff1-4167-8177-1be58f9adba2)
uid 2008 and gid 2008 in this case. 

create a new user as stated previously and it's home directory. i've created one before.

![VirtualBox_LINUX SERVER_22_04_2024_15_26_30](https://github.com/user-attachments/assets/6bbc5172-020a-4af5-9658-82a3213ecf95)
edit /etc/passwd file to change the uid and gid to 2008 and save, you can use nano or vim to edit the file.

![VirtualBox_LINUX SERVER_22_04_2024_15_27_15](https://github.com/user-attachments/assets/ca682bf4-f484-47de-85c6-4b42e1e6dc63)

![VirtualBox_LINUX SERVER_22_04_2024_15_28_19](https://github.com/user-attachments/assets/87229446-5785-4ee2-8542-10f88eb50b1a)
save the file and change directory(cd) to the mount point again, we should have access.

![VirtualBox_LINUX SERVER_22_04_2024_15_28_19](https://github.com/user-attachments/assets/3573b2e1-791c-4248-af81-1d5cbfcceb7c)
the directory is empty, to gain full access to the system using the vulnix account, we can create an ssh key pair which we will use to login and won't require password.

![VirtualBox_LINUX SERVER_24_04_2024_06_19_06](https://github.com/user-attachments/assets/cecfcd76-ec1a-4f09-ab4f-0b64a46f8ff3)
we got an access into vulnix account!
we used "sudo -l " to list which commands the user has permission to execute and we discovered it only has permission of the superuser to edit the /etc/exports file. we will edit the file and add the root account to the nfs share.
![VirtualBox_LINUX SERVER_24_04_2024_06_38_01](https://github.com/user-attachments/assets/88928de5-cc0e-4cc3-82ad-4882f5f90dd7)
![VirtualBox_LINUX SERVER_24_04_2024_06_38_37](https://github.com/user-attachments/assets/89287073-28b4-4304-b899-b273636afca3)
![VirtualBox_LINUX SERVER_24_04_2024_06_45_07](https://github.com/user-attachments/assets/45ea6d7a-194c-42f5-8819-171e5b2be793)

mount /root nfs share on your local system and switch user to root to access it.

![VirtualBox_LINUX SERVER_24_04_2024_06_44_42](https://github.com/user-attachments/assets/9142860b-cb78-48e0-a9b4-0fbb2a04f775)
we gained access to the root share and we can now access the flag. lets gain full access to the root account.
![VirtualBox_LINUX SERVER_24_04_2024_06_49_01](https://github.com/user-attachments/assets/51c23a51-f3b4-4835-8d32-5ec1e5c3cbb1)
we generated an ssh key for the local root account and copied it to the .ssh directory of the root share.
we will noe login using ssh

![VirtualBox_LINUX SERVER_24_04_2024_06_52_20](https://github.com/user-attachments/assets/0fd29d50-9cca-4fe0-aaa3-fe5e26f93330)
finally!
