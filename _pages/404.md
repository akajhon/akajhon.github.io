---
title: "Malware Analysis - Agent Tesla"
classes: wide
header:
  teaser: /assets/images/2024-06-05-MalwareAnalysis_AgentTesla/agenttesla4.png
ribbon: navy
description: "Agent Tesla Analysis And Reverse Engineering"
categories:
  - Malware Analysis
tags:
  - Trojan
  - Bot
  - AgentTesla
toc: true
---
Sample:
```
e2f6a376216a6492d6fe3648a969608c
```

# Background
Agent Tesla is a highly advanced Remote Access Trojan (RAT) favored by cybercriminals and Advanced Persistent Threat (APT) groups for espionage. 
It first emerged in 2014 and is known for its ability to steal sensitive information like credentials and keystrokes, and to capture screenshots. 
Spread through malicious email attachments and software vulnerabilities, it is a potent tool for state-sponsored cyber espionage and data theft.

# Static Analysis - Stage 1
<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/malwarebazaar sample.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 1: Malware Bazaar Entry</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/VT detection on first exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 2: VirusTotal Detection</sub>
</div>
<br>

As seen in Figures 1 and 2, this malware is highly recognizable and detectable by EDRs and antivirus software.

Through the use of tools such as PEStudio and Detect It Easy, I was able to identify that this malware is packed. 
This observation highlights the sophistication of the malware, as packers are often used to obfuscate the underlying code.


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/Detect it easy on first exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 3: Detect It Easy on First Stage</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/pestudio first exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 4: PEStudio on First Stage</sub>
</div>
<br>

After analyzing the file with DNSpy, I identified the malware's unpacking function. 
It unpacks itself into memory and executes as a new process. 
As shown in Figure 5, I saved the unpacked content to a new file for further analysis.
In addition, I successfully dumped a DLL that is generated during the malware's execution.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/extracting from the memory the second stage.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 5: Extracting from the memory</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/dll from first stage.png" alt="Screenshot1" />
    <br>
    <sub>Figure 6: Extracting DLL</sub>
</div>
<br>


# Static Analysis - Stage 2

After extracting the dll and the unpacked malware, we have files that lookes like this:

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/after extracting tyronedll + 2nd stage.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 7: 3 Files that were extracted</sub>
</div>
<br>

Analyzing its hash on VirusTotal revealed that it's a new variant that has not been previously analyzed or uploaded.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/VT no detection on second exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 8: No detection on 2nd Stage</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/VT no detection on dll.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 9: No Detection on the dll</sub>
</div>
<br>

For quick and precise analysis of both files, I once again utilized Detect It Easy and PEStudio as shown in the next Figures.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/Detect it easy on second exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 10: Detect It Easy 2nd Stage</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/pestudio second exe.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 11: PEStudio 2nd Stage</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/Detect it easy on tyronedll.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 12: Detect It Easy on the dll</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/pestudio tyrone.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 13: PEStudio the dll</sub>
</div>
<br>

Using CAPA, I was also able to identify the specific capabilities and behaviors of the malware, providing deeper insights into its functionality.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/capa stage2.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 14: CAPA 2nd Stage</sub>
</div>
<br>


# Dynamic Analysis

Analyzing the second stage in DNSpy revealed the malware's functionality. 
I identified keylogger and screen logger capabilities, password harvesting, and data extraction from browsers, databases, and more.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/keylogger function.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 15: KeyLogger Functions</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/retrieve db from the specific browser.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 16: Checks For Browser</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/retrieve credentials from db and try to decrypt.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 17: Retrieve data from DB</sub>
</div>
<br>

At this point, I decided to run the malware and observe its effects on the system. 
To do this, I used Regshot to capture the system's registry before and after running the malware. 
This allowed me to analyze the changes made to the registry by the malware.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/regshot 1.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 18: Regshot Change</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/regshot 2.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 19: Regshot Change</sub>
</div>
<br>

The registry modification indicates that the malware is attempting to disguise itself as a legitimate system process, which could complicate detection and removal efforts.


# Malware Configuration

After searching through the Assembly Explorer, I was able to extract the malware configuration, as shown in Figure 17.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/malware configuration.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 20: Extracted Malware Configuration</sub>
</div>
<br>

# Extracting IOC From The SMTP Server

Using the SMTP credentials, I successfully logged into the attacker's SMTP server and extracted additional IOCs. 
I wrote a Python script to extract the logs, but for privacy reasons, I won't provide the script here.

Then, I developed an additional Python script to specifically extract emails from the logs.

```python
import csv
import re

# Regular expression to match email addresses
email_regex = r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,6}\b'

# Path to the CSV file
csv_file_path = 'emails2.csv'

# Set to store unique email addresses
unique_emails = set()

# Open the CSV file for reading with utf-8 encoding
with open(csv_file_path, mode='r', newline='', encoding='utf-8') as file:
    # Create a CSV reader object
    csv_reader = csv.reader(file)

    # Loop over each row in the CSV file
    for row in csv_reader:
        # Join all the columns in the row into a single string
        row_str = ' '.join(row)
        
        # Use regular expression to find email addresses in the row string
        emails = re.findall(email_regex, row_str)
        
        # Add unique email addresses to the set
        unique_emails.update(emails)

# Print the unique email addresses
for email in unique_emails:
    print(email)
```

Using this script, I was able to extract more than 80 emails that may be compromised or related to the attacker.

In addition, running the malware revealed the newly generated processes. Using Wireshark, I captured network IOCs.

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/new proccess.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 21: The New Proccess</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/2024-06-05-MalwareAnalysis_AgentTesla/wireshark.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 22: Using WireShark</sub>
</div>
<br>


# IOCs

- Hash:
```
e2f6a376216a6492d6fe3648a969608c
5e8a624d975b4d471b6145ae628a2f97
bc19b0a9d487ac8e5d22b40bd6176d2d
```
- URL:
```
hxxps://account.dyn[.]com
api[.]ipify[.]org
```
- Emails:
```
mabouamayem@ta***d[.]ae
imran@alb******lic[.]com
shakim@t***d[.]ae
sivaraj.t@tho*****ast[.]com
Suneesh_KS@s**[.]ae
Vibin.Davis@a*****air[.]com
sabu@t***ae[.]com
QRWorkshopauh@****[.]ae
reem.albedwawi@e****up[.]com
chiragjoshi@gra*******ves[.]com
anuvind@albu*******ic[.]com
vinodkumar@g*****lf[.]com
jessa@ki******ings[.]com
madhav@ge*****lf[.]com
abdul.samad@e*****up[.]com
jbayhon@t****ed[.]ae
Raman.Jha@a****ir[.]com
Purchase3[.]spme@su*****en[.]com
chandrajith@t****e[.]com
simpson.d@****c[.]ae
saheer.m@en****up[.]com
Workshopauh@h***c[.]ae
kausarali@a*****ic[.]com
purchase3.spme@su****en[.]com
coordinator@t***e[.]com
ameen_aziz@go*******lc[.]com
mmerchant52@****o[.]com
Deepanshu.Gupta@****r[.]com
stanveer@t****d[.]ae
ibrah.esad@****[.]com
muhammedsigma786@*****[.]com
Dinakaran.Umamaheswaran@a*****ir[.]com
hameed@alb*****lic[.]com
ismailpt@t****e[.]com
Serviceauh@h***c[.]ae
news@ncx[.]ni****e[.]ae
Thameem.Mohammed@a****ir[.]com
saeed9797seead@****[.]com
Mohamed.Abdhul@a****ir[.]com
thiemokho.doucoure@e****up[.]com
Muhammed.Shihabuddin@s***[.]ae
Sreenidhi.Gadihalli@a****ir[.]com
lahiru.r@****c[.]ae
omajdalawi@t****d[.]ae
Naushad.Ahmad@e****p[.]com
Syed.Oli@a****ir[.]com
wahmed@t***ed[.]ae
Purchaseauh@h***c[.]ae
nabdulsalam@t****d[.]ae
purchaseauh@h****c[.]ae
krisanth.c@g*****f[.]com
qc1@g****em[.]com
serviceauh@h***c[.]ae
anita.singh@c*****st[.]com
Jules.v@h***c[.]ae
Saadiyat.DCP1@a****r[.]com
dinakaran.umamaheswaran@a*****r[.]com
mohamed.halan@g****lf[.]com
Mailer-Daemon@box2229[.]b****t[.]com
info@a*****c[.]com
sales@p**st[.]com
mustafa@a****ic[.]com
Amrou.Askar@a****ir[.]com
ummer.mohammed@****[.]com
ashiq.mehmood@e****up[.]com
ramesh@a****ic[.]com
anup.p@h***c[.]ae
vinup.s@g****f[.]com
mohan@g****f[.]com
Inderjeet.Arora@a****ir[.]com
Bheemrao.Kumar@a***ir[.]com
ttaylor@t****cy[.]us
arul@g****f[.]com
QRworkshopauh@h****c[.]ae
anand.bodas@******[.]com
gulfe@g****c[.]ae
info@a****ms[.]com
purchase@p***t[.]com
khozem@a*****lic[.]com
dwi.endah@en****p[.]com
joyson.lobo@ca****st[.]com
ravindrarao@t****e[.]com
```

# Yara Rule

```yar
rule AgentTeslaRule {
    meta:
        description = "Searches for AgetTesla variant"
        author = "0xMrMagnezi"
        date = "2024-06-05"
    
    strings:
        $hex_sequence = { 24 61 38 31 37 33 65 61 33 2D 38 36 32 64 2D 34 62 37 65 2D 62 36 36 62 2D 65 30 37 63 66 64 32 65 62 36 34 37 }
	$ascii = "PY718E785ZXFG4844GPE4Z"
        $wide = "PY718E785ZXFG4844GPE4Z" wide
        $nocase = "PY718E785ZXFG4844GPE4Z" nocase
        $wide_nocase = "PY718E785ZXFG4844GPE4Z" wide nocase
    
    condition:
        $hex_sequence or $ascii or $wide or $nocase or $wide_nocase
}
```
