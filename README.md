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

<li>Next, open filezilla in a terminal on Ubuntu. This will establish an SFTP connection to the OPNsense machine so we can access the file structure and upload our own custom rules.</li>


































