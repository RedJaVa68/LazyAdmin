# LazyAdmin Write Up

![Image of THM](images/intro.png)

I first started by starting the machine. I copied the IP of the machine, and entered it into my browser.

I got a default Apache Ubuntu page. Now I know it's a Linux webserver.

<p align="center">
  <img width="600" src="images/apache_default.png">
</p>

I went onto my terminal and I ran: nmap -sV {IP}

I learned that the webserver's http runs on port 80.

<p align="center">
  <img width="600" src="images/nmap.png">
</p>

I decided to enumerate the http port of the webserver by using gobuster

I used gobuster dir -u {HTTP URL of IP} -w {wordlist.txt}

I found that there is a content directory. When I go to that URL, I find out that it uses SweetRice as the webserver's code.

<p align="center">
  <img width="600" src="images/gobuster1st.png">
</p>

I then enumerate the content directory to find anymore directories.

I used gobuster dir -u {HTTP URL of IP}/content -w {wordlist.txt}

I find more interesting directories...

<p align="center">
  <img width="600" src="images/gobuster2nd2.png">
</p>

I find the login page to the dashboard on the "as" directory

<p align="center">
  <img width="600" src="images/as_login.png">
</p>

I searched the other directories and found the "inc" directory to have a backup of the mysql database.

<p align="center">
  <img width="600" src="images/inc_dir.png">
</p>

I opened the sql file on a code editor and found a username and password...

<p align="center">
  <img width="600" src="images/code_in_sql.png">
</p>

I know its an MD5 hash. So I copied it and pasted it to hashes.com to decrypt it

Password was Password123. << A very secure password, right?


<p align="center">
  <img width="600" src="images/decrypt_pass.png">
</p>

I tried using the username "master" and the password "Password123" on the dashboard

It logged me right in. I went through each directory trying to find vulnerabilities in the site.

I found I could upload a reverse shell script to the themes...

It requires a zip file, so I put the script into a zip file and uploaded it.

<p align="center">
  <img width="600" src="images/reverse_shel1.png">
</p>

I remember that I can view the files to the themes in the directory /content/_themes

Woohoo! I found it!

<p align="center">
  <img width="600" src="images/reverse_shel2.png">
</p>

On my terminal I entered the command nc -lvp {port}

Netcat will be listening to the port for any reverse shell. 

Now I am in!

<p align="center">
  <img width="600" src="images/reverse_shel3.png">
</p>

First thing I did was look for user.txt

<p align="center">
  <img width="600" src="images/usertxt.png">
</p>

I also found that there is a perl script and a file that contains the login for the sql database

Next I did was check my sudo privileges... 

<p align="center">
  <img width="600" src="images/sudovuln.png">
</p>

I find out I can run the perl script as sudo, but I do not have write access to it

So I checked what the script does...

<p align="center">
  <img width="600" src="images/backup.png">
</p>

It runs a shell script in etc/copy.sh

I checked that script and found a reverse shell code in it. So i editted that code by putting my own IP and port number...

I ran another nc -lvp {different port number}

I ran the command sudo perl /home/itguy/backup.pl

Now I got a reverse script with root access!

<p align="center">
  <img width="600" src="images/success.png">
</p>

I ran cat root/root.txt and got the final code!

My suggestions are to make directory names random, do not leave the mysql backup on the same server, use hard to guess passwords, deny sudo access to scripts, and dont leave any reverse shell scripts on the webserver. :)

Here is the link to the tryhackme room if you want to try it out: https://tryhackme.com/room/lazyadmin
