# Cache Me Outside TryHackMe

The first question asks for a full name. In the provided conversation, we are given a link; by visiting the link, we can get the name easily.
<img width="775" height="557" alt="image" src="https://github.com/user-attachments/assets/f08fe5fd-76f9-422b-9b7e-f853e18cb039" />

There's a GitHub link provided that we can continue to investigate. There's a single repo with a single commit. Looking at this commit shows nothing, but adding a .patch at the end of the URL shows the patch file,
which often contains more details than are visible normally, such as metadata and line-by-line changes. This extension shows us the answer to the second question!
<img width="1217" height="570" alt="image" src="https://github.com/user-attachments/assets/62cb9c8f-abe4-4523-831d-2673e480c753" />

