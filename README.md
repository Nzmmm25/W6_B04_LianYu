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

Then translate this ticket to base58 and getting this result: 
<img width="126" height="273" alt="image" src="https://github.com/user-attachments/assets/4269b5af-f50f-42b9-b7e9-dfe0b329ebac" />
Result = !#th3h00d
<br> <br> 

IP Address changed from TryHackMe. 
New IP Address = 10.48.153.88

Next I tried to ftp since there's an open port. (proven from nmap -sV scan)
<img width="393" height="200" alt="image" src="https://github.com/user-attachments/assets/162f4784-f3e7-4ee8-9b76-f8dbd5a4009e" />

<img width="686" height="136" alt="image" src="https://github.com/user-attachments/assets/2fb27ff9-a1fa-4281-8654-65d548cff565" />
After downloading all files in the directory, I tried to view the images and one of them is not a png as proven: 
<img width="545" height="168" alt="image" src="https://github.com/user-attachments/assets/5c5a2c68-e42d-4920-a46d-d7fd184dbe10" />

To extract aa.jpg using steghide, i need a password first. It could be stored in Leave_me_alone.png
<img width="537" height="158" alt="image" src="https://github.com/user-attachments/assets/446fb5a4-3553-4220-93aa-f88d325d5d9a" />

Next, the hex bits in Leave_me_alone.png needs to be changed for it to be accessible. 
<img width="536" height="79" alt="image" src="https://github.com/user-attachments/assets/563e54d7-07f1-4d7f-a8d6-0a66c76299aa" />
<br> 

When it's finally accesible, it reveals the password to be "password". 
<img width="844" height="358" alt="image" src="https://github.com/user-attachments/assets/517f4c11-cdaa-4612-8ff2-6ffdb43219b0" />
<br>

Extracting from aa.png gets me ss.zip
<img width="474" height="134" alt="image" src="https://github.com/user-attachments/assets/d048847e-78e6-4096-b4d5-d859be0e4880" />
<br>

Extracting ss.zip
<img width="722" height="435" alt="image" src="https://github.com/user-attachments/assets/afb978fb-fa52-4479-a9d1-b8096447d55a" />
cat shado reveals a password. 
<br>
<br>

Logging in using ssh as user slade. 
<img width="720" height="455" alt="image" src="https://github.com/user-attachments/assets/6015bda6-3871-4fdb-a9a3-d038a50d4a45" />
<br>

Immediately getting user.txt
<img width="407" height="150" alt="image" src="https://github.com/user-attachments/assets/14dd6746-28fa-4099-b197-5179c8db40c6" />
<br>

Next, try to access root.
<img width="767" height="187" alt="image" src="https://github.com/user-attachments/assets/daa94e04-6929-4373-971f-7d4e600a5160" />
<br>
<img width="432" height="74" alt="image" src="https://github.com/user-attachments/assets/9848952b-a958-43c6-9e79-fc974251c2bb" />
<br>

Next started a netcat listener 
<img width="741" height="493" alt="image" src="https://github.com/user-attachments/assets/fdfb05cb-f93b-4bac-ba07-7b9feace7acc" />
<br>

Successful root access. Read root.txt to get the final flag. 
<img width="842" height="283" alt="image" src="https://github.com/user-attachments/assets/81f1c550-e58b-4c2c-9bd0-65d2e1fb6ae0" />
<br>





