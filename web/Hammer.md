# Hammer TryHackMe

This is an RCE room on TryHackMe. To start, I'll do a quick Nmap scan, using the aggressive scan and the all ports flag so I don't miss any important information.
<img width="1127" height="898" alt="image" src="https://github.com/user-attachments/assets/e2d7e0ad-202d-40c5-b899-320d9bb5edeb" />
We find a web server on port 1337; visiting this web server is a sensible next step.
<img width="677" height="382" alt="image" src="https://github.com/user-attachments/assets/67b53b13-6631-414c-9d49-46b37e414a74" />
When I visit the Apache server, I'm greeted with a login page, giving me some ideas for a next step. I visit the page source and see a big hint for a next step: the naming convention of the directories within this
web server. Using tools such as ffuf, we can discover directories within this web server. When I run through the fuzzing, I discover a couple of directories, most notably, logs, which may contain useful information
about login events.
<img width="772" height="476" alt="image" src="https://github.com/user-attachments/assets/364024a9-57e1-40bb-bfd5-a8ee65861c0a" />
Visiting logs show an error log directory:
<img width="583" height="245" alt="image" src="https://github.com/user-attachments/assets/95751c74-71ed-4367-98fe-f808d4b84aad" />
Visiting the error logs, I found an email that I can use for a login.
<img width="1917" height="316" alt="image" src="https://github.com/user-attachments/assets/bb0d3479-7701-4422-bdd9-846d7cad6e12" />

Because there's no visible password in the logs, I tried the forgot password option and entered the email I found; it shows a 180-second timer to enter a 4-digit code. I have no way to get this code because I don't have any access
to the email.
<img width="637" height="390" alt="image" src="https://github.com/user-attachments/assets/bbfa8fb7-7b92-4ae6-bce5-0f36363b6e8d" />

180 seconds is a long time, and being a 4-digit code, it can likely be brute-forced within that time frame. First, I use Burp Suite and navigate to the proxy tab where I open the web page in Burp Suite's browser; by doing 
this, im able to intercept the phpsessid token. The request also gives us everything we need to craft the brute force attempt.
<img width="1300" height="467" alt="image" src="https://github.com/user-attachments/assets/d38f3365-3868-4c32-8370-4e1b37f47931" />

Something important to note here is that there's a rate limit; this tells us how many attempts we have. To get around this, we're going to use a bunch of IPs so none of them ever hit their rate limits, and we can
continue to brute force. First thing I'm going to do is make .txt files of the codes we need 0-9999 and make a list of IPs we can use. 
<img width="1048" height="610" alt="image" src="https://github.com/user-attachments/assets/125926b5-cd15-4571-b2d8-1ca60dcc8ef8" />

I use the following brute-force command using ffuf:
```bash
ffuf -u http://10.65.170.162:1337/reset_password.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -H "Cookie: PHPSESSID=kctqcp19lung6dvkt1s2rqdoqj" -H "X-Forwarded-For: IP" -d "recovery_code=CODE" -w codes.txt:CODE -w ips.txt:IP -mode pitchfork -mc all -r -fs 321 -t 10 -s
```
Running it should verify the session for us, so no matter what code we put it in, it should work.

<img width="800" height="358" alt="ezgif-3502a9e8a7147dac" src="https://github.com/user-attachments/assets/d7319f95-8bf0-4fa5-87b8-52e001e8cba7" />


Doing this allows us to change the password, and then we can simply go back and log in with the email we found earlier and the new password that we just set.





















