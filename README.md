# AIHydra
## Exploit Writeup and POC for AI Hydra 26 (and probably 52) series lights 

## Usage: 
AI-Exploit.py
-t targetIP
-p Password to Set if the no password login attempt bounces
-c Command to run on the lights. 
-h Help 

# Background: 

Current Version: 2.5.1
API endpoints are at /api.

!(https://github.com/Laransec/AIHydra/blob/master/Pics/1.png)

Scanned /api with the default dirbuster directory-list-2.3-small.txt and found endpoints

!(https://github.com/Laransec/AIHydra/blob/master/Pics/2.png)

I investigated /api/command and guessed the JSON format of {"command" : "commandhere"} which allows for command execution as root.

!(https://github.com/Laransec/AIHydra/blob/master/Pics/3.png)
Cat /etc/ shadow returns 

!(https://github.com/Laransec/AIHydra/blob/master/Pics/4.png)

Cracking root password hash with John The Ripper and the rockyou.txt wordlist returned
 3l3v3n           (root)
 
 !(https://github.com/Laransec/AIHydra/blob/master/Pics/5.png)


The Dropbear SSH client was available. I started it though the command API 

!(https://github.com/Laransec/AIHydra/blob/master/Pics/rootshell.png)

Setting a password on the web interface does enforce a login cookie requirement before accessing the API endpoint. As with many IOT devices this doesn’t matter because they rolled their own weird webserver into the binary that runs everything. 
An unauthenticated POST request to /setpassword works to set a new password. Responds with a valid cookie. You can use this cookie to go back and run commands as root. 

!(https://github.com/Laransec/AIHydra/blob/master/Pics/6.png)
