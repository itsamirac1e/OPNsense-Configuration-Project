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
<img src="https://i.imgur.com/LygjPqw.png" height="80%" width="80%" alt="Network Topology"/>
</p>

<li>Logged into pre-installed Ubuntu VM in my hypervisor and logged into 192.168.1.1 (OPNsense web GUI):</li>
