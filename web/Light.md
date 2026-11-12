# Light TryHackMe

Starting out with this room, I notice in the intro it mentions it's a database application, so I'm already thinking about SQL injection. Something interesting is that ICMP packets are blocked, meaning I can't ping.
I then performed an Nmap scan with the -Pn flag to treat every port as if it's online so I can see a complete list of services. This scan brought up nothing very interesting: an SSH port, which is standard but could be 
used later in the room and a service running on 1337, which the room already tells us about and tells us to connect to.

<img width="1111" height="773" alt="image" src="https://github.com/user-attachments/assets/5fce2039-358c-4105-9463-a9684584811b" />

my next step was to follow what the room told me to do, and netcat into the service running on port 1337. Here I see it asks for a username, which the room provides for us, and upon entering the username I get a password,
likely belonging to the user. This is likely the database application the room is centered around. I tried admin as a quick check, but no results came up, so I moved on to thinking about SQL injections.

The SQL injection I used was ' OR '1'='1. The idea behind this injection is to take advantage of the query used to grab the password. That query likely looks something like
```text
SELECT password FROM users WHERE username = '<input>'
```
my injection turns that prompt into
```text
SELECT password FROM users WHERE username = '<input>' OR '1'='1
```

The statement WHERE 1 = 1 is almost always true, and more often than not, an admin account.

<img width="516" height="322" alt="image" src="https://github.com/user-attachments/assets/d936286b-14a5-4e87-aaad-bf2b229c8307" />

After trying a couple more injections and learning about a word filter, common as a protection against SQL injections, I tried some fingerprinting to try to identify any system-unique flaws that could be exploited.
This revealed that it doesn't like the character @, something that MySQL uses but SQLite does not. After trying a command involving a value that's included by default, sqlite_version, we get the password again, confirming
that this system is a SQLite database. 

<img width="452" height="205" alt="image" src="https://github.com/user-attachments/assets/147bace9-724e-4b3f-8d2e-d9a0f88a8f2e" />

Now that we know it's SQLite, we can continue to try some commands to get the username. We know there's a word filter, most likely on words like SELECT, WHERE, UNION, etc that can be used to extract information
But there are some workarounds we can use to avoid that. Most commonly, placing words with the capitalization of words that are likely banned is easiest. The words are likely in a list like:
```text
Banned_words = ["SELECT", 'WHERE", "FROM", "UNION", "OTHERS"]
```
So by playing with the spelling, like SELect or WHeRE, we can bypass this filter.

You'll see that primarily UNION statements are used; this is because UNION bolts on an independent SELECT statement which I can control, and by leaving the username section blank, the data returned is in my control.
Another important detail is the version of SQLite; in version 3.31.1, sqlite_master contains the schema for all tables; however, only one line is printed for the output of the program, so it doesn't dump all tables.
This is important for a prior assumption I made: there are separate tables for admins and general users; the password from earlier was not an admin password.

<img width="1067" height="856" alt="image" src="https://github.com/user-attachments/assets/2c2cf91b-75ec-4f21-9821-171a26545f2f" />




















