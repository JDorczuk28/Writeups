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
<img width="1661" height="592" alt="image" src="https://github.com/user-attachments/assets/dc30e673-5b53-4e5a-b42d-b9a5a44f35cc" />

After running a few commands, I learn I dont have very much power, viewing the page source, I find a JWTtoken that I can take a look at and maybe manipulate.
<img width="1883" height="458" alt="image" src="https://github.com/user-attachments/assets/37a82ff1-fde2-4906-9a7f-233d688afc54" />

Decoding the token, it shows info about the user and most notably, our role and the kid variable in the header.
<img width="1637" height="737" alt="image" src="https://github.com/user-attachments/assets/7fde6663-1fdf-4517-babc-199eea15f778" />

If we run ls in the command bar we get when logging in, we can see another .key file, theres a chance that this can go in the kid variable and allow us to change our role to admin.
<img width="1090" height="435" alt="image" src="https://github.com/user-attachments/assets/bc2a83c1-d277-4af0-891e-3528f41d7738" />
We also need to contents of this file to be manipulate the JWT token correctly, using curl gets me them even though cat doesnt work:
<img width="477" height="112" alt="image" src="https://github.com/user-attachments/assets/cf6eaaa4-3826-4bc1-aaea-a409f54f403b" />


Adding this information gets me my updated key. 
<img width="1495" height="812" alt="image" src="https://github.com/user-attachments/assets/cdaecb14-a7b8-4732-a540-70f948c3696c" />


To use this key, we can fire up Burp Suite again, this time using the Repeater tab. Insert my altered JWT token into the Authorization header and put in the command I want; the flag path is specified in the questions so we dont need to search for it, and that gives us the final flag of this room.
<img width="1591" height="640" alt="Screenshot 2026-11-19 172542" src="https://github.com/user-attachments/assets/d00f559b-b484-4b5f-921a-16afb9715a91" />



























