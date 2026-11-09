# W1seguy TryHackMe
The first thing this room asks you to do is download a file containing source code. Upon downloading it and using a quick cat to see the contents, we can see the idea behind this room:
<img width="858" height="832" alt="image" src="https://github.com/user-attachments/assets/e05e7a61-f1ee-4a2d-8891-d7529fd6779b" />

The idea is that it listens on a TCP server on port 1337 and calls start; from here, it makes a key that's 5 characters long and then passes it to setup. Setup starts with a fake flag(hint?), then each character
of the flag is XOR'd with a character of the key, constantly cycling through the key such that character[0] uses key[0] and character[5] uses key[0], and so on.

Now that we know what the code's doing, we can open permissions and try running it. Open the permissions with
```bash
chmod 777 file.py
```

Now we need to run the code and use
```bash
nc MACHINE_IP 1337
```
to connect to the script where we are prompted with an XOR text for flag 1.

The idea behind finding the answer is that we know part of the flag because we know the format; we know the flag must contain THM{}, and XOR can undo itself because we know some of the characters of the flag and the
length of the key. This works because:
ciphertext = plaintext XOR key
So knowing part of the plaintext (the flag) lets us rearrange it:
key = plaintext XOR ciphertext

So we take the first 4 bytes of the XOR-encoded text and XOR them with the characters from the plaintext that we know, THM{. This gets us the first 4 characters of the encryption key. 
To get the last character, you can calculate the length of the encoded text using key[(length - 1) % 5] and find out if it lands in key[4], giving us the final character. In the case of this room, it does, and we were able
to calculate the final character of our key. If this didnt workout, the solution would be fairly simple because there are so few characters it could be; a brute force would get us the last character quickly.

Inputting the key, I get flag2:
<img width="945" height="101" alt="image" src="https://github.com/user-attachments/assets/8e39d58b-72b4-4867-9469-f1d83fcc8607" />

And then, by putting the actual encoded text through CyberChef with the key from hex, I get flag 1:
<img width="1536" height="882" alt="image" src="https://github.com/user-attachments/assets/bc3aeaac-5bf0-4352-933c-fe34da12c6fb" />






