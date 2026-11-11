# Sakura TryHackMe

This challenge first gives you a link to a photo that an attacker left behind, a message of "You've been Pwned!" and some binary in the background that serves as a red herring.

By inspecting the page source, you can find the file path the image is from and, from this, the username of the attacker, giving us the answer to the first question.

<img width="702" height="355" alt="image" src="https://github.com/user-attachments/assets/a9100353-83d8-4c96-907f-caad5507b7e8" />

A quick Google search brings up a lot of different things, likely because of the popularity of this room; what we're actually looking for gets buried. After a little scrolling, I find a GitHub account associated 
with the username, so I click to investigate. Going to the repos, I find a PGP file; here we have a public key that we can try to decode.

<img width="640" height="837" alt="image" src="https://github.com/user-attachments/assets/7fdcabbf-c576-4282-b5f4-d923f7290542" />

We can copy the key over to a .asc file and use GPG to show what the key is; here we find the attacker's email and the answer to the second question.

<img width="565" height="300" alt="image" src="https://github.com/user-attachments/assets/fa7af259-403a-4ae3-b9a4-6c0468f360db" />

In the search results, we also find an X account; here we have a picture of the attacker and a post that introduces them as another account, containing their full name in the handle and the answer to question 3.

<img width="585" height="125" alt="image" src="https://github.com/user-attachments/assets/18553e4a-b022-4104-977b-417577c473b2" />

Going back to scouring GitHub, we see 2 repos: one labeled ETH and one labeled ethminer. ETH is the handle for Ethereum, and using this we can assume the attacker is involved in some sort of ETH mining, which answers question 4.

The question gives a hint about what we were supposed to be looking at for the next questions. GitHub saves the commit history of public repos, so we can look back at info that has since been removed.
Going to the ETH repo, we see a single script for mining, when I open it, it looks like a part of a link to a crypto wallet. By visiting the history, I find what it looked like at its first commit, and I can see that an address was originally included, answering question 5.
<img width="1870" height="847" alt="image" src="https://github.com/user-attachments/assets/a5a3f8a6-0c81-4989-bb00-2332cd1064bf" />


We can further investigate this address further by visiting a website called blockchain.com, which tracks everything crypto, including different wallets. Inputting this address here shows all transactions made. For question 6, we can navigate to the time it asks and view the transaction that took place. Here we find the answer and see where they received crypto from.

<img width="956" height="57" alt="image" src="https://github.com/user-attachments/assets/86eae42f-98bb-4398-a13f-981ed1c42cea" />

For question 7, we can continue to view the wallet history; here we find another exchange where crypto is going in and out, and the answer to the question.

<img width="952" height="72" alt="image" src="https://github.com/user-attachments/assets/1b8e7d33-07bb-431e-99d4-a80d7fadd727" />

The next question asks about their Twitter account; we already found this earlier, so it probably showed up early because of the popularity of this room and how similar the usernames are. We can confirm this 
is the right account by seeing that the account the message in the picture was sent from is the same as the one linked to the attacker's full name.

Question 9 asks about a BSSID for the attacker's home Wi-Fi. Logging into Twitter and scrolling through more of their posts I find something very suspicous worth checking out, a screenshot of some sort of code
and a strangely worded reply that likely serves as a hint.

<img width="592" height="405" alt="image" src="https://github.com/user-attachments/assets/141c7cd2-913d-4a8b-883a-b804982d0236" />

Searching for Deep Paste, I quickly found that it's a Tor site. Unfortunately, the site is considered defunct, and it's not possible to do this step manually anymore, as this is an older room. We can find the info we need by navigating to OSINT Dojo's GitHub and viewing the full screenshot, which shows the info we need to move on.

With the network values we have, we can use wigle.net, a network mapping tool that's super helpful for finding out information about networks. You'll need to make an account, but once you do, you can go to advanced search and enter the home wifi value and see the BSSID value needed to answer question 10.

<img width="1917" height="812" alt="image" src="https://github.com/user-attachments/assets/556c8ab5-5d59-4fbd-b59c-1b3f3380a492" />

Moving on, we can answer question 14 right away. In the same place we found the home wifi name, we find the city Wi-Fi as well, which includes the name of the city that the attacker likely lives in.

<img width="1262" height="346" alt="image" src="https://github.com/user-attachments/assets/f93923ca-486e-4149-a84c-eabfbc808eeb" />

The other questions require taking a close look at the posts the attacker made. In the picture they took before heading home, there's a prominent monument in the background which reveals where they are. A quick Google search reveals the airport that is closest, answering question 11.

Question 12 asks where the layover back home was. This one is a little trickier, but with tools like Google Lens or Yandex we can figure it out. We can also search for the lounge the attacker stayed out and find out where their first-class locations are; in this case, there are only 3 first-class locations, easily found with a quick Google search, and only 2 that make sense for a layover. Simply trying both gets us the answer for where the layover is.

<img width="647" height="717" alt="image" src="https://github.com/user-attachments/assets/1dd3d8e6-5bda-416a-83b8-18c83af6366c" />

Finally, it asks what the lake is in the aerial picture they took. We know it's between their layover and their home city, so I load up Google Maps and line up the shape of the shorelines to find what lake is visible, and then simply zoom in to find the name of the lake and the final answer of this room!

<img width="815" height="535" alt="image" src="https://github.com/user-attachments/assets/63bab826-ca4d-4a94-a11e-9e34fce6a8a8" />

















