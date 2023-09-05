
## **8.3. Intro to Nmap:**

An open-source, extensible, and importantly free tool, Nmap is one of the industry standard's that everyone should have in their toolkit. Nmap is capable of a few things that are essential in the Information Gathering stages of a penetration testing methodology such as the [Penetration Testing Execution Standard](http://www.pentest-standard.org/index.php/Main_Page) (PTES), including:

- Host discovery
- Vulnerability discovery
- Service/Application discovery

## **8.4. Basic Nmap Functionality**

We'll quickly glaze over the basics of getting started with Nmap, the scan types, and the syntax for these types accordingly. We'll apply our networking knowledge learned yesterday in "Day 7 - The Grinch Really Did Steal Christmas" to help understand the differences between TCP and UDP scanning.  
  

### **8.4.1 TCP Scanning**

There are two common TCP scan types that you'll be using in Nmap. On the surface they seem to perform the same thing, however, they're very different. Before we break this down, let's illustrate TCP/IP's three-way-handshake again and recap the three stages of a "normal" three-way-handshake:

1. SYN
2. SYN/ACK
3. ACK

![](https://www.open.edu/openlearncreate/pluginfile.php/258370/mod_oucontent/oucontent/35227/9b0bce6c/594a9486/cn_black_fig3.jpg)  
_[_(The Open University., N.D)_](https://www.open.edu/openlearncreate/pluginfile.php/258370/mod_oucontent/oucontent/35227/9b0bce6c/594a9486/cn_black_fig3.jpg)_

- Connect Scan - `nmap -sT <ip>`
- SYN Scan - `nmap -sS <ip>`

#### 8.4.1.1 SYN Scan:

The most favourable type of scan, Nmap uses the TCP SYN scan (`-sS`) if the scan is run with both administrative privileges and a different type isn't provided. Unlike a connect scan, this scan type doesn't fulfil the "three-way-handshake" as what would normally take place. Instead, after the "SYN/ACK" is received from the remote host, a "RST" packet is sent by the host that we are scanning from (never completing the connection).

This scan type is the most favourable method as Nmap can use all the information gathered throughout the handshake to determine port status based on the response that is given by the IP address that is being scanned:

- SYN/ACK = open
- RST = Closed
- Multiple attempts = filtered

Not only this but also since fewer packets are sent across a network, there is less likelihood of being detected.  
  

#### 8.4.1.2. Connect Scan:

Unlike a SYN scan, administrative privileges aren't required for this scan to run. This is as a result of Nmap using the _Berkeley Sockets API_ which has quickly formed to be the standard method of how services like web applications communicate with an operating system. As a result of more packets being sent by Nmap, this scan is easier to detect and takes a longer time to complete. Moreover, as the "three-way-handshake" completes as if it were a normal connection, it can be logged a lot more conveniently.  
  

## 8.5. Nmap Timing Templates

Nmap allows the user to determine Nmap's performance. Measured in aggressiveness, a user can use one of six profiles [0 to 5] to change the rate at which Nmap scans at. With `-T0` being the stealthiest, this profile scans one port every 5 minutes, whereas `-T5` is considered both the most aggressive and potential to be inaccurate. This is because the `-T5` waits a mere 0.3 seconds for the remote device to respond to a handshake. Factors such as those listed below determine how accurate these scans are:

- The resource usage a remote server is under. The higher usage, the slower it is to send a response to Nmap.
- The quality of the connection. If you have a slow or unstable connection, you are likely to miss responses if you are sending many packets at once.

Generally speaking, you will want to use low-aggressive profiles for real-world scenarios, however, in a lab environment where noise doesn't matter - high-aggressive profiles prove to be the quickest. For perspective, Nmap uses `-T3` if no profile is provided. In a pentesting situation, you'd be inclined to use a lower value such as within in a lab environment, a higher value`-T4` will suffice as stealth is not as critical.  
  

## **8.6. An Introduction to Nmap's Scripting Engine**

A recent addition to Nmap is the **N**map **S**cripting **E**ngine or NSE for short. This feature introduces a "plug-in" nature to Nmap, where scripts can be used to automate various actions such as:

- Exploitation
- Fuzzing
- Bruteforcing

At the time of writing, the NSE comes with 603 scripts, which can be found [here.](https://nmap.org/nsedoc/scripts/)

![nsedoc](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/nsedoc.png)  
_Nmaps NSE Documentation Page_

Take for example the [FTP ProFTPD Backdoor](https://nmap.org/nsedoc/scripts/ftp-proftpd-backdoor.html) script. This script attempts to exploit an FTP service that is running ProFTPD version 1.3.3c, the version of which is fingerprinted by Nmap itself.

We can provide the script that we want to use by using the `--script` flag in Nmap like so:`nmap --script ftp-proftpd-backdoor -p 21 <ip_address>`  
  

## **8.7. Additional Scan Types:**

Not only can we use the Nmap's TCP Scan, but Nmap also boasts a combination of these types for various actions that are useful to us during the information gathering stage. I have assorted these into the table below, giving a brief explanation of their purpose.

(Where x.x.x.x == 10.10.5.190 )

|Flag|Usage Example|Description|
|---|---|---|
|-A|`nmap -A x.x.x.x`|Scan the host to identify services running by matching against Nmap's database with OS detection|
|-O|`nmap -O x.x.x.x`|Scan the host to retrieve and perform OS detection|
|-p|`nmap -p 22 x.x.x.x`|Scan a specific port number on the host. A range of ports can also be provided (i.e. 10-100) by using the first and last value of the range like so: `nmap -p 10-100 x.x.x.x`|
|-p-|`nmap -p- x.x.x.x`|Scan all ports (0-65535) on the host|
|-sV|`nmap -sV x.x.x.x`|Scan the host using TCP and perform version fingerprinting|

## **8.8. Defending against Nmap Scans**

The practice of security through obscurity doesn't work here. Whilst it may seem logical to attempt to hide a service by changing its port number to something other than the standard (such as changing SSH from port 22 to 2222), the service will still be fingerprinted during an Nmap scan (albeit slightly later on). Unfortunately, you cannot get the best of both worlds in having a service available yet hidden.

Fortunately, open-source **I**ntrusion **D**etection (IDS) & **P**revention **S**ystems (IPS) such as [Snort](https://www.snort.org/) and [Suricata](https://suricata-ids.org/) allows blue-teamers to protect their networks using network monitoring. For example, you would install these services on firewalls such as [pfSense](https://www.pfsense.org/):  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/pfsense_dash.png)  
_The dashboard of a pfSense Firewall._

Rulesets such as the [emerging threats](https://rules.emergingthreats.net/) for Snort and Suricata are capable of detecting and blocking a wide variety of potentially malicious traffic - including:

- Malware traffic
- Reverse shells
- Metasploit payloads
- Nmap scans

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/pfsense_rules.png)  
_A list of Snort rules installed on a pfSense firewall._

For example, detecting the Metasploit payload for [CVE 2013-3205](https://nvd.nist.gov/vuln/detail/CVE-2013-3205):  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/cve_2013.png)  
_The emerging threat rule to detect the Metasploit payload for CVE-2013-3205._

If properly configured, a majority of Nmap scans can be detected. This is especially true when using an aggressive timing template such as `-T4` or `-T5`. Let's take a look at the following Nmap scan being detected: `nmap -A 192.168.1.171`  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/nmap_scan.png)  
_Starting an Nmap scan to the pfSense firewall._

After returning to pfSense a few seconds later, we notice that alerts are being generated by Snort:  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/snort_list.png)  
_Viewing newly created alerts by Snort as a result of the Nmap scan._

Even with a timing template of`-T3`, Snort is capable of detecting the port scan, where after 6 alerts (in this case) the attacker is then blocked by the firewall.  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/snort_blocked.png)  
_After 6 alerts, Snort blocks the IP address running the Nmap scan from contacting the pfSense firewall._

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/nmap_blocked.png)  
_Confirming that the IP address running the Nmap scan can no longer contact the pfSense firewall._  
  

## **8.9. Challenge**

**Deploy** and use Nmap to scan the instance attached to this task. Take a note of the IP address to scan: `10.10.5.190` and enumerate it for Elf McEager!

_Optional bonus_: As a result of Elf McEager managing to recover Christmas in "_Day 7 - The Grinch Really Did Steal Christmas_", TBFC's website has been restored for all the elves to visit. Can you find it? I hear it's quite the read... You must add `10.10.5.190 tbfc.blog` to your **/etc/hosts** file before the application will load like below:  
![hosts](https://assets.tryhackme.com/additional/cmn-aoc2020/day-9/hosts.png)