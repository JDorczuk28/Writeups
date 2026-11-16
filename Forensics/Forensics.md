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
after running the following command, containing the relevant plugin and process:
```bash
vol -f victim.raw windows.pslist | grep 'SearchIndexer'
```
I get the following results showing the PID:

<img width="1061" height="91" alt="image" src="https://github.com/user-attachments/assets/63d6e043-6673-4830-9d8c-6e1166df7595" />

