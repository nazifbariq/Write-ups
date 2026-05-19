# 🚀 Task Writeups
## Task 1

✔️ **Download the file or using the provided AttackBox.** <br><br>

I'm using the provided AttackBox for this Room.<br><br>

### Q1: What is this user's avatar of?<br>
First i'm using exiftool. Exiftool is used to read the pictures metadata. <br><br>

`exiftool ./Rooms/OhSINT/WindowsXP.jpg` <br><br>
<p align="center">
  <img width="668" height="488" alt="image258" src="https://github.com/user-attachments/assets/389ac4b9-bde1-4347-b73a-5b675e09281d" />  
</p>

Here, i found something interesting. The author name is provided there, which is OWoodflint. <br>
I tried using Google and found a twitter account with name Owoodflint.
<p align="center">
<img width="605" height="285" alt="image236" src="https://github.com/user-attachments/assets/a8d99872-2ebc-426c-8519-705331b5d7a7" />
</p>

Ans: **cat** 

### Q2: What city is this person in?
Besides a twitter account, i also found a github user. There's a readme file which include the author location.
<p align="center">
  <img width="915" height="384" alt="image7" src="https://github.com/user-attachments/assets/b13b1b8e-6d1a-4fd9-aeab-916b3fc05c24" />
</p>

Ans: **London**

### Q3: What is the SSID of the WAP he connected to?
I begin my investigation on his twitter and i found a tweet about his Bssid.<br>
<p align="center">
  <img width="600" height="194" alt="image212" src="https://github.com/user-attachments/assets/95af2401-e016-4a8b-a96c-ab246dd7c48a" />
</p>
I'm using OSINT tool to find out his location using the Bssid i found.<br>

Ans: **UnileverWifi**

### Q4: What is his personal email address?
Searching through his github, is where i found it.<br>
Ans: **OWoodflint@gmail.com**

### Q5: What site did you find his email address on?
Ans: **Github**

### Q6: Where has he gone on holiday?
On his github there is a link that take us to his blog.
<p align="center">
  <img width="795" height="625" alt="image228" src="https://github.com/user-attachments/assets/3445d82d-7323-424c-b2b2-54bea8259323" />
</p>

Ans: **NewYork**

### Q7: What is this person's password?
Check the page source of his blog and i found his password.
<p align="center">
  <img width="280" height="64" alt="image278" src="https://github.com/user-attachments/assets/23381313-bd03-43be-a787-4605f55c710f" />
</p>

Ans: **pennYDr0pper.!**
