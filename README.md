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
  <img width="600" src="images/gobuster2nd.png">
</p>
