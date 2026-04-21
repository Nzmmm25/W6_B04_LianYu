# W6_TryHackMe_LianYu

Machine: https://tryhackme.com/room/lianyu

Target IP Address: 10.48.187.3

<img width="263" height="95" alt="image" src="https://github.com/user-attachments/assets/426a5d40-2908-4c99-b217-54e9ee1fcd46" />

---
First thing I did was look up the IP on Firefox. 

<img width="916" height="808" alt="image" src="https://github.com/user-attachments/assets/051b7b61-db34-494c-9af6-9971d28a2985" />
<br>
 
Next, I ran "nmap -sV 10.48.187.3" and discovered some open ports. 

<img width="736" height="380" alt="image" src="https://github.com/user-attachments/assets/bb41a5be-617e-4cfe-9664-1f5e9b8ba244" />

Open Ports Discovered: 

21/tcp open <br>
22/tcp open <br>
80/tcp open <br>
111/tcp open <br>

Next, I ran gobuster to see hidden directories. 
# gobuster dir -u 10.48.187.3 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
<img width="727" height="452" alt="image" src="https://github.com/user-attachments/assets/e0d6fbf9-d007-4f37-8e6f-c6182d452d34" />
Hidden Directories found: /island 
<br> 
<br>

Next I went back to Firefox to see the directory. <br>
<img width="633" height="325" alt="image" src="https://github.com/user-attachments/assets/e053f912-81ad-4007-95b6-2dc47c3c40c4" />
Highlighting the page reveals a hidden code "vigilante" 
<br> 
<br> 

I ran another dirbuster to see more in the subfolder. 
# gobuster dir -u 10.48.187.3/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
<img width="727" height="440" alt="image" src="https://github.com/user-attachments/assets/2233c9f8-afb7-47c4-8581-a1048093cf71" />
Found: /2100
<br> <br> 

Back to Firefox to see this directory
<img width="695" height="397" alt="image" src="https://github.com/user-attachments/assets/5b37f7bf-41b5-4406-9065-cec0e3bbcb1e" />
The html leaves a comment. Something about ".ticket". 

Back to dirbuster. 
# gobuster dir -u 10.48.187.3/island/2100 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket
<img width="723" height="489" alt="image" src="https://github.com/user-attachments/assets/58629a2a-18dd-4993-8742-365f3df907dc" />
Found: green_arrow.ticket
<br> <br> 

Back to Firefox. 
<img width="704" height="223" alt="image" src="https://github.com/user-attachments/assets/4f3c25cc-ad33-497f-b085-d70b8ad2c6fc" />
Ticket found: RTy8yhBQdscX
<br> <br>


