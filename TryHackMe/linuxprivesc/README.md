# 🚀 Task Writeups
## Task 1 
### Q1: Deploy the machine and login to the "user" account using SSH.
Ans: **No answer needed**
### Q2: Run the "id" command. What is the result?
<p align="center">
  <img width="764" height="49" alt="image175" src="https://github.com/user-attachments/assets/9753b947-7b6a-4c2c-ad10-e540aa9fe940" />

Ans: **uid=1000(user) gid=1000(user) groups=1000(user),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev)**

## Task 2 Service Exploit
Since the MySQL service is running as "root" user for the servicee does not have password assigned. We will takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.
</p>
