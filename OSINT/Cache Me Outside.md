# Cache Me Outside TryHackMe

The first question asks for a full name. In the provided conversation, we are given a link; by visiting the link, we can get the name easily.
<img width="775" height="557" alt="image" src="https://github.com/user-attachments/assets/f08fe5fd-76f9-422b-9b7e-f853e18cb039" />

There's a GitHub link provided that we can continue to investigate. There's a single repo with a single commit. Looking at this commit shows nothing, but adding a .patch at the end of the URL shows the patch file,
which often contains more details than are visible normally, such as metadata and line-by-line changes. This extension shows us the answer to the second question!
<img width="1217" height="570" alt="image" src="https://github.com/user-attachments/assets/62cb9c8f-abe4-4523-831d-2673e480c753" />

Normally, I wouldn't do this for TryHackMe challenges, but the directions suggest that I get the phone number for question 3 by emailing the user: "The interaction and response are part of the controlled challenge setup". By emailing the user, I get an auto-reply containing the phone number.

Moving on to the next question, it asks what city he is located in. By searching up his name, we come across his Threads account where he has a picture posted with a store name in it. A quick google search confirms the location and the answer to this question.
<img width="710" height="346" alt="image" src="https://github.com/user-attachments/assets/627d3f28-2bc2-4b7d-95fc-051b8ec31da7" />

The last question asks which tram he got off. Going to maps and finding the location, a search for French supermarkets reveals one right down the road, and right next to it, a tram station, giving us the final answer! 
<img width="1232" height="757" alt="image" src="https://github.com/user-attachments/assets/21a16a5d-9b1a-493b-9914-2faef3a78978" />



