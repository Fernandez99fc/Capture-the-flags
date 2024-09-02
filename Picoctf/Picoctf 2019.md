Title: Picoctf 2019 Web Exploitation
link: https://play.picoctf.org/challenge/4ask 8)

Today, i will be solving the "where are the robots" challenge. it is a web exploitation based challenge. let's dive into it. 
![Screenshot (601) 1](https://github.com/user-attachments/assets/4710783e-0933-4464-b2b5-a437e7471a38)
we have been provided a link above to visit. we get a blank page having the below message when we click the link:
![Screenshot (602)](https://github.com/user-attachments/assets/a615e303-e419-4f28-adc5-9597d692b700)
the hint could tell us that the admin does'nt want us to see a particular file which is the robot.txt file. the robot.txt file tells the search engnines which part of the websites users shouldn't see or have access to. it could be a file or directory.

![Screenshot (603)](https://github.com/user-attachments/assets/fbbf11f6-16f6-4bca-88ed-c9acb1ab8332)
when we visit the robot.txt file, we can see that the admin doesn't want us to read the /477ce.html file. we should try viewing the file and see what we have there. 

![Screenshot (605)](https://github.com/user-attachments/assets/c711fb3c-655b-406c-a9e2-6cf2c63365f5)
here we are, we got the root flag!

