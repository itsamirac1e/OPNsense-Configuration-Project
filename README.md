<h1><b>OPNsense Configuration Project</b></h1>

<h2>Description</h2>
In this project, I downloaded and configured OPNsense firewall/routing software onto a VM in my Oracle Virtualbox hypervisor. This served as my final project for my UT Austin cybersecurity bootcamp in order to demonstrate my skills in networking, system administration, hypervisor configuration, pentesting, and many more!

<h2>Security Tools/Techniques & Utilities Used</h2>

- <b>OPNsense</b>
- <b>Ubuntu</b>
- <b>Filezilla</b>
- <b>Suricata IDS/IPS</b> 


<h2>Environments Used </h2>
<ul>
 <li>Oracle VirtualBox</li>
 <li>Ubuntu VM</li>
 <li>OPNsense web GUI</li>
</ul>

<h2>Key Takeaways</h2>
<ul>
 <li>Learned how to configure networks, firewalls, servers and systems.</li>
 <li>How to administrate Windows and Linux clients and servers.</li>
 <li>Harden devices and networks.</li>
 <li>Various penetration tools and techniques.</li>
 <li>Monitor and defend against attacks.</li>
</ul>


<h2>Lab Topology (So far)</h2>

<p align="center">
<br/>
<img src="https://i.imgur.com/bhHIwaR.png" height="80%" width="80%" alt="Network Topology"/>
</p>

<h2>OPNsense Firewall Installation</h2>
<ul>
 <li>Downloaded OPNsense iso image from https://opnsense.org/download/</li>
 <li>Created VM in Virtualbox (already pre-installed) and configured device requirements to boot and run OPNsense. Requirements set:</li>
  <ul>
   <li>Base Memory: 4096 MB</li>
   <li>OS: FreeBSD 64-bit</li>
   <li>Video Memory: 16 MB</li>
   <li>Network Adapters: For OPNsense to work correctly, two interfaces are needed. 
    One for your WAN that is connected to your internet and another that is connected to your LAN.
   </li>
    <ul>
     <li>Adapter 1: Internal Network (LAN) em0 - Creates a virtual internal local network.</li>
     <li>Adapter 2: Bridged Adapter (WAN) em1</li>
    </ul>
   <li>Booted iso image and configured LAN and WAN networks accordingly:</li>
  </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/LygjPqw.png" height="80%" width="80%" alt="OPNsense_Terminal"/>
</p>

<li>Logged into pre-installed Ubuntu VM in my hypervisor and logged into 192.168.1.1 (OPNsense web GUI):</li>

<p align="center">
<br/>
<img src="https://i.imgur.com/zcqlkYr.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<h2>Suricata IDS/IPS Installation on OPNsense</h2>
<ul>
 <li>IDS/IPS can be used to detect and prevent any malicious signatures and patterns on one’s network. Suricata is the IDS/IPS service that OPNsense uses for this process.</li>
 <li>Navigate to Services > Intrusion Detection > Administration</li>
  <ul>
   <li>Enable IDS, IPS mode, Promiscuous mode, and syslog alerts.</li>
   <li>Change pattern matcher setting to ‘Hyperscan’. Hyperscan is a pattern matching library in Suricata designed for identifying specific signatures or patterns in network traffic that are indicative of malicious activity.</li>
   <li>Enable LAN and WAN interfaces to be monitored and specify the home subnet: 192.168.1.0/24</li>
  </ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/8wcNfhA.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>
 
 <li>Next I decided to create a custom rule to detect basic nmap SYN scanning activities:</li>
 <ul>
  <li>Ran <i>‘touch customnmaprule.rules’</i> to create a rule file.</li>
  <li>Used Nano to edit the file and add the following rule:</li>
  <li><i>alert tcp $HOME_NET any -> 192.168.1.1/24 any (msg:"POSSIBLE NMAP SYNSTEALTH SCAN DETECTED"; flow:stateless; flags:S; priority:5; threshold:type threshold, track by_src, count 50, seconds 1; classtype:attempted-recon; sid:1234;)</i></li>
 </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/dU2rAKs.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<ul>
 <li>In order to implement this custom rule I created, I needed to SSH into OPNsense via terminal in Ubuntu. I then used a platform called FileZilla to establish the connection and view the file hierarchy of OPNsense. The following steps show how I enabled OPNsense to allow SSH connection on the LAN from my VM.</li>
 <ul>
  <li>Switch protocol to HTTP.</li>
  <li>Enable ‘permit root user login’ and ‘permit password login’. Set SSH port to ‘22’.</li>
  <ul>
   <li>If you do not enable password login, filezilla will try to establish connection via certificates.</li>
  </ul>
  <li>Set Listen Interfaces to LAN.</li>
  <li>Enable SSH, permit root user login. Generally in a production environment you create a user that is not root if you want to login and create/upload your own custom rules.</li>
 </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/jFg92fD.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<ul>
 <li>Next, open filezilla in a terminal on Ubuntu. This will establish an SFTP connection to the OPNsense machine so we can access the file structure and upload our own custom rules.</li>
 <ul>
  <li>Host: sftp://192.168.1.1</li>
  <li>Username: root</li>
  <li>Password: opnsense</li>
  <li>Port: 22</li>
 </ul>
 <li>Once the connection had been established, in the right-hand window indicating the OPNsense file structure, I navigated to this directory:</li>
 <ul>
  <li>/usr/local/opnsense/scripts/suricata/metadata/rules</li>
  <li>This directory holds the rules OPNsense uses. I then pasted my customnmap.xml file into the ‘rules’ folder.</li>
 </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/W4Wt4uD.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<ul>
 <li>Next I set up an HTTP server on my Ubuntu VM to serve the customnmap.rules file to OPNsense. The .xml file I placed earlier in OPNsense will then query for those set of rules to be downloaded to the server. In a new terminal, I ran the following command:</li>
 <ul>
  <li><i>sudo python3 -m http.server 80</i></li>
 </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/qndDO9Q.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<li>I then navigated to the OPNsense IDS administration dashboard and restarted the service. In the ‘Download’ tab, we can see that the custom rule I created has been populated.</li>

<p align="center">
<br/>
<img src="https://i.imgur.com/IwQFnWO.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<li>Once I confirmed the rule had populated, I then enabled the custom Nmap ruleset, and clicked on the “Download & Update Rules” button which allows OPNsense to grab the .rules file through the HTTP server I established.</li>
<li>Navigate to ‘Rules’ tab and view the custom rule that has been populated and has been enabled:</li>

<p align="center">
<br/>
<img src="https://i.imgur.com/XpzAl0P.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<ul>
 <li>Now we will test if this rule actually works! Navigate to a new terminal in your Ubuntu VM and run the following command:</li>
 <ul>
  <li><i>‘Sudo nmap -sS -Pn --top-ports 500 192.168.1.1’</i></li>
  <li>This command uses the Nmap tool to perform a TCP SYN scan on the OPNsense firewall, and will scan the first 500 ports.</li>
  <ul>
   <li>'-sS': Specifies a TCP SYN scan, where Nmap sends a TCP SYN packet to the target and listens for a response.</li>
   <li>'-Pn': Treats all hosts as online and does not perform host discovery. This is useful when you already know the target is responsive, and you want to skip the host discovery phase.</li>
   <li>'--top-ports 500': Specifies that Nmap should scan the top 500 most commonly used ports. This is a convenience option to speed up the scan by focusing on the ports that are more likely to be interesting or commonly used.</li>
  </ul>
 </ul>
</ul>

<p align="center">
<br/>
<img src="https://i.imgur.com/tsu3PZm.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>

<li>Navigate back to the OPNsense firewall and go to the ‘Alerts’ Tab. Results were displayed indicating the rule was successful and the IDS/IPS works!</li>

<p align="center">
<br/>
<img src="https://i.imgur.com/72B3xJv.png" height="80%" width="80%" alt="OPNsense_Dashboard"/>
</p>


































