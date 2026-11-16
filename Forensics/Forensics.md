# Forensics TryHackMe

## Task 1
The first task of this room is a memory dump of an infected system. The title of the task is "Volatility forensics." Volatility is an open-source forensics tool, so we start with an idea of where we should go.
Now, before we boot up Volatility, there are still some things worth checking beforehand. Using strings can reveal interesting things about the memory dump; in this case, even with relevant filters, such as
setting the minimum string length and looking for only human-readable strings, there is simply too much junk to make out anything important. Something interesting, however, that can help us answer the first question
is that when looking for little-endian strings, I see system information, including information about the operating system, which is what the first question asks for. I used the command:
```bash
strings -e l file
```
to search for little-endian strings (UTF-16LE-encoded strings, to be precise). I won't paste the results that show me the OS, because there would be too much blurring to do, but sifting through the results should result
in a quick find.

The next question asks for the PID of a certain .exe file within the memory dump. I tried strings with a grep search for the process it asks about, but nothing comes, which makes sense because PID's are usually stored
as bytes, not strings to be read. Now that we know that this is where strings will stop being helpful, we can boot up Volatility to see more information. The question asks for the PID of a certain process, "SearchIndexer"
Using Volatility, we can add the pslist plugin for Windows, which lists processes present in the Windows memory image. 
After running the following command, containing the relevant plugin and process:
```bash
vol -f victim.raw windows.pslist | grep 'SearchIndexer'
```
I get the following results showing the PID:

<img width="1061" height="91" alt="image" src="https://github.com/user-attachments/assets/63d6e043-6673-4830-9d8c-6e1166df7595" />

The last question in task 1 asks which folder was accessed last; this one took some time because I wasn't familiar with how to find it. I originally tried to look at registry keys with Volatility but was able to find nothing. After viewing the hint, I dove into something called shellbags, which are Windows Registry keys that track a user's folder view preferences. First, I had to extract a hive file from Volatility, so I ran:
```bash
vol -f victim.raw -o dump/ windows.registry.hivelist --filter UsrClass --dump
```
This makes a hive list that I can use another tool, regripper, to pull shellbags out of. After installing regripper, I ran:
```bash
regripper -r dump/registry.UsrClass*.hive -p shellbags
```
to extract shellbags from the user class, where the user would be accessing their folders. Now, to find the last directory accessed by the user, we simply look at the dates and times accessed; an important note here is that all the most recent files come from general system use, not accessed by the user directly, so it takes some searching to find the correct file but eventually we come across:
<img width="1102" height="821" alt="image" src="https://github.com/user-attachments/assets/0e9041e5-28d1-4b3c-9873-94069aa28e55" />
giving us the answer to the final question of this task.


## Task 2
The first question of this task asks about open ports and protocols. To find these, I run:
```bash
vol -f victim.raw windows.netscan
```
This shows all the open ports and protocols from the memory dump. To find the suspicious ones, I sort through and find the ones that aren't open by default on Windows 7 machines. This leaves me with a few Windows Media Player ports that aren't necessarily malicious, but aren't open by default, making them suspicious. 
<img width="1016" height="211" alt="image" src="https://github.com/user-attachments/assets/7e3505a3-7bfd-487b-a6a6-95d9c107ebc3" />

I try these few inputs and got the answer to the first question of task 2.

Moving on, the last question of task 2 asks us to find malicious processes; this can be found using malfind, a Volatility tool. We have a hint already, because netscan gives us a suspicious PID; it's a good chance it may be malware. I run:
```bash
vol -f victim.raw windows.malfind
```

This gives me a result of 3 PID's that are likely malware, answering the final question of this task.
<img width="1921" height="677" alt="image" src="https://github.com/user-attachments/assets/3b74d979-0245-4e3f-a011-5a0cd21d5739" />

## Task 3
Here, it asks for more details about the malicious PIDs; this is mainly a strings task, and most of the answers can be solved with the strings command and grep commands that are crafted for the specific bits that they give us.
<img width="705" height="635" alt="image" src="https://github.com/user-attachments/assets/481ca617-aaae-49f0-a7a9-3d307cbb3263" />

For the very last question, it asks us to find a unique environmental variable of a malicious PID. Volatility has a plugin for this as well; running it shows us all the environment variables. Instead of researching which variables are normal for a process, I simply made 2 files and compared them to find the unique process.
<img width="1156" height="947" alt="image" src="https://github.com/user-attachments/assets/21924e51-fa20-4123-8403-5f4527254b8f" />

This wraps up the room!



