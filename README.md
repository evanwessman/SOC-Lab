<h1>SOC Analyst Home Lab</h1>


<h2>Description</h2>
Security Operations Center (SOC) analysts play a crucial role in maintaining the security posture of organizations by monitoring, analyzing, and responding to security incidents. While looking for projects to gain hands on experience outside of my academic labs I heard about this project from Eric Capuano (Recon InfoSec) via Gerald Auger (SimplyCyber) . Eric's substack blog "So you want to be a SOC Analyst" was a great resource; explaining the scope, process and execution of creating a home SOC lab where I accomplished the following :
<br/><br/>
- Configured VMware with Windows and Ubuntu VMs<br/>
- Managed Windows VM: disabled Defender, prevented standby, installed Sysmon, and deployed LimaCharlie EDR<br/>
- Set up Sliver C2 server on Ubuntu VM, executed payload on Windows, and analyzed EDR telemetry<br/>
- Developed & implemented blocking rule in LimaCharlie, proficient in crafting detection rules, and used baselining to reduce false positives<br/>
- Automated YARA scanning in LimaCharlie, developed rules for scanning files and processes, and created D&R rules for new .exe files in Downloads<br/>

<br />


<h2>Environments and Utilities Used</h2>

- <b>VMWare WorkStation</b>
- <b>Windows 11 DevEval</b>
- <b>Ubuntu Server 22.04.1</b>
- <b>LimaCharlie EDR</b>
- <b>Sliver Command & Control (C2) Server</b>
- <b>PowerShell</b> 
- <b>Windows Command Prompt & Linux Terminal</b>


<h2>Project walk-through:</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
