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
