
10.3. What is Samba & where is it Used?  
  
Whilst we learnt about one of the most commonplace protocols that are used for file-sharing on Day 10, we'll be covering an alternative technology for file-sharing that is most used within organisation/company networks. Offering encryption as standard, this technology consists of two protocols:

- SMB (Server Message Block) - Natively supported by Windows and not Linux
- NFS (Network File System) - Natively supported by Linux and not Windows

Protocols such as SMB send "requests" and "responses" when communicating with each other, as illustrated below:

![](https://cdn.ttgtmedia.com/rms/onlineImages/networking-smb_mobile.jpg)

_[(TechTarget., 2017)](https://searchnetworking.techtarget.com/definition/Server-Message-Block-Protocol)  
_  
What makes Samba so popular and useful is that it removes the differences between these two protocols, meaning that the two operating systems can now share resources including files amongst each other. Simply, Samba connects to a "**share**" (think of this as a virtual folder) and is capable of day-to-day activities like deleting, moving or uploading files.

Samba is flexible in the sense it can be useful for both you and me or businesses with thousands of employees. For example, employees can access documents from a central computer rather than each employee storing their own copy. As previously mentioned, this technology is encrypted enabling sensitive data like username and passwords used in the authentication process (and the data itself)  to be communicated between client/server securely.

Unlike FTP, other IT devices such as network printers can also be shared between client/server.

---

10.4. Searching for Samba Shares  
We're going to be using the _enum4linux_ tool that is already provided to you on the THM AttackBox. Let's get our hands dirty!  
  

1. Open a terminal prompt and navigate to enum4linux: `cd /root/Desktop/Tools/Miscellaneous`
2. Run enum4linux and list all the possible options we could use, take time to study these for anything  
    interesting:  `./enum4linux.pl -h`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-11/img1.png)

Note how we can use options like `-S` to list shares or `-U` (note the uppercase) to list possible users. In  
my example, I want to find out who can be used to access the server through Samba: `./enum4linux.pl -U MACHINE_IP`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-11/img2.png)  
Note how _enum4linux_ has discovered four users in my example...One of these users may have a weak password such as "password123" that we can log in with and access sensitive data as.  
**1.** jjohns  
**2.** lbutton  
**3.** jfrost  
**4.** cmnatic

And as a result of further enumeration with _enum4linux_, we've discovered the following three shares!  
**1.** homes  
**2.** share1  
**3.** IPC$

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-11/img3.png)  
_Now it's your turn, scan your Instance (**MACHINE_IP**) to answer **Question #1** and **Question #2**. Remember the options that_  
we can use with `enum4linux.pl` _!_

---

10.5. Connecting to a Share

We've already learnt two key pieces of information from the previous section:  

- Usernames to authenticate as
- Shares that we can access (remembering that shares most likely contain data)

However, a very common and easy to cause vulnerability by administrators is wrong permissions. You may be able to access a share and its data without logging in at all, such as we will demonstrate below:

**1.** Remember that the IP address of the Samba server is that of the Instance  you deployed (**MACHINE_IP**)

**2.** Use the _smbclient_ tool to begin accessing the Samba server and its shares, replacing "**sharename**" with the name of the share you wish to access: `smbclient //REPLACE_INSTANCE_IP_ADDRESS/**sharename**`

**3.** You will be asked for a password, the easiest password is no password! **We can just press** "**Enter**" to test this theory. If successful, this means that the share requires no authentication and we are now logged in.

_For example, accessing **"share1"** on another device:_

 ![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-11/img4.png) |

You can use the `help` command to list some of the commands you can run whilst connected to the Samba share. Here's a quick rundown of the fundamentals:

|||
|---|---|
|Command|Description|
|ls|List files and directories in the current location|
|cd <directory>|Change our working directory|
|pwd|Output the full path to our working directory|
|more <filename>|Find out **more** about the contents of a file. To close the open file, you press `:q`|
|get <filename>|Download a file from a share|
|put <filename>|Upload a file from a share|

  

You can now proceed to answer **Question #3** and **Question #4**

---

10.6. Conclusion, where to go from here and additional Material:  
  
You've learned the fundamentals of how a very commonplace protocol used by computing devices works, and ultimately, can be leveraged through the use of enumeration and misconfiguration. With this said, you might be surprised to learn that even printers can use the protocols behind Samba. [Swafox](https://tryhackme.com/p/Swafox) has created a lovely room on [Printer Hacking 101](https://tryhackme.com/room/printerhacking101).

There's no truer statement in pentesting that practice makes perfect.  Not only can you use the tools within this room, why not give a few others a try and apply your knowledge in the "[Kenobi](https://tryhackme.com/room/kenobi)" Walkthrough room or the "[Anonymous](https://tryhackme.com/room/anonymous)"Challenge room (CTF)