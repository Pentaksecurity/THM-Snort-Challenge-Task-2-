# THM-Snort-Challenge-Task-2

TryHackMe Snort Challenge - Live Attacks Writeup:



# 💻 Task 2 Reverse-Shell:

# Scenario

In this scenario we got the alert that there is persistent outbound traffic detected. Possibly a reverse shell. 



Our goal is to use Snort in sniffer mode and try to figure out the attack source, service and port.

Then, write an IPS rule and run Snort in IPS mode to stop the brute-force attack.



# 🔍 Action 1:



- Run Snort in sniffer mode and write the output to log file.

'sudo snort -l.'

- After running snort and logging traffic lets inspect the log that was created.

'sudo snort -r <snort log file> -X'

<img width="793" alt="Screen Shot 2025-03-24 at 11 45 06 AM" src="https://github.com/user-attachments/assets/7c0c3bcc-a094-4a21-a239-3ff5d7f26f1f" />

- Upon inspection of the log traffic, I discovered outbound traffic going out to a suspicious IP with port 4444, which is commonly used for reverse shell and Metasploit.

<img width="789" alt="Screen Shot 2025-03-24 at 11 45 16 AM" src="https://github.com/user-attachments/assets/6dafa9f8-488f-454d-9e92-748350c78133" />

# 🚫 Action 2:



- Lets go ahead and write some rules to block the traffic going out to port 4444 and the suspicious IP address.

- I have created 2 local rules.



Rule 1: reject tcp any any <> any 4444 (msg: "Outbound Traffic to Port 4444 Blocked"; sid:1000001; rev:1;)

Rule 2: reject tcp any any <> "<suspicous IP>" any (msg: "Traffic to suspicous IP Blocked"; sid:1000002; rev:1;)

<img width="783" alt="Screen Shot 2025-03-24 at 11 45 43 AM" src="https://github.com/user-attachments/assets/693d667c-4cf1-4543-b833-fd6c0c09ecfd" />


# 💻 Action 3:



- Now lets test our new rules!

<img width="677" alt="Screen Shot 2025-03-24 at 11 46 00 AM" src="https://github.com/user-attachments/assets/41090850-cc73-4c33-abec-7dfbfe1b788e" />


'sudo snort -c local.rules -A console'



-Once you see your rules working lets run Snort in IPS mode and get the flag!

<img width="669" alt="Screen Shot 2025-03-24 at 11 46 09 AM" src="https://github.com/user-attachments/assets/ebaffa4f-d708-4bd7-9727-d44556e3c312" />


'sudo snort -c local.rules -A full'

<img width="790" alt="Screen Shot 2025-03-24 at 11 46 23 AM" src="https://github.com/user-attachments/assets/1aa84caa-0313-4655-99c1-2d78ac292035" />


And we stopped the attack and got the flag!




