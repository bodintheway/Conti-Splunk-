# Conti-Splunk-
An Exchange server was compromised with ransomware. Use Splunk to investigate how the attackers compromised the server.


Some employees from your company reported that they can’t log into Outlook. The Exchange system admin also reported that he can’t log in to the Exchange Admin Center. After initial triage, they discovered some weird readme files settled on the Exchange server.  

Below is a copy of the ransomware note.

![image](https://github.com/user-attachments/assets/87a72057-2bd8-418f-bb81-68133afa7d8c)


Warning: Do NOT attempt to visit and/or interact with any URLs displayed in the ransom note. 

Read the latest on the Conti ransomware here. 

### Can you identify the location of the ransomware?

The first thing I did in Splunk was run index=* to get a sense of what data we're working with. After that, I focused on the Image field to investigate which executables were running and where they were located—looking for any unusual paths that might indicate the presence of ransomware.
![image](https://github.com/user-attachments/assets/be6ee2d0-9375-4db3-b896-97c27436d1bc)


![image](https://github.com/user-attachments/assets/bf1ce272-e39f-4da3-a3a5-da971ef416ec)

![image](https://github.com/user-attachments/assets/16b38c4f-efd0-47aa-9348-1292f2da454c)
then i discovered C:\Users\Administrator\Documents\cmd.exe a little sispicious



### What is the Sysmon event ID for the related file creation event?

its 11. if you dont know something like this just google it.


### Can you find the MD5 hash of the ransomware?

To find the MD5 hash of the ransomware, I clicked on C:\Users\Administrator\Documents\cmd.exe, then selected View Events to examine related logs. After that, I added md5 to the query to display the hash value associated with the executable.
![image](https://github.com/user-attachments/assets/9c980a2f-a4ac-4efb-86fb-fd41800d814b)

### What file was saved to multiple folder locations?

To find what file was saved to multiple folder locations, I searched:
```
spl
Copy
Edit
index=* EventCode="11" Image="c:\\Users\\Administrator\\Documents\\cmd.exe"
```
Then I clicked on the TargetFilename field and noticed that multiple README files were being created across different directories.

![image](https://github.com/user-attachments/assets/d0bd943f-528d-4059-aa61-6cbcb9cbdb1d)


### What was the command the attacker used to add a new user to the compromised system?
```
index=* user /add
```

![image](https://github.com/user-attachments/assets/2336cbd1-7b6b-4985-aa7d-c30cb3584daa)
![image](https://github.com/user-attachments/assets/bbfcdfc2-6dc2-4573-ab9e-6fcbc72053ad)


### The attacker migrated the process for better persistence. What is the migrated process image (executable), and what is the original process image (executable) when the attacker got on the system?


Discovered that the source image C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe is related to the target image C:\Windows\System32\wbem\unsecapp.exe.

Discovered that the source image C:\Windows\System32\wbem\unsecapp.exe is related to the target image C:\Windows\System32\lsass.exe.


### The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?

lsass.exe (Local Security Authority Subsystem Service) is responsible for managing security policies and handling authentication on Windows. Attackers often target lsass.exe to extract password hashes or credentials from memory, enabling them to move laterally or escalate privileges.
![image](https://github.com/user-attachments/assets/119199fa-1128-4588-b3df-15420098657e)

C:\Windows\System32\lsass.exe













