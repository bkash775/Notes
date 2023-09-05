
  
12.3. Vulnerability...reveal yourself!  
As an application's lifecycle continues, so does its version numbering. Applications contain seemingly innocent hallmarks of information such as version numbering. Known as information disclosure, these nuggets of information are handed to us by the server through error messages such as in the following screenshot, HTTP headers or even on the website itself.

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img1.png)

An attacker can use knowledgebases such as [Rapid7](http://rapid7.com/), [AttackerKB](https://attackerkb.com/), [MITRE](https://cve.mitre.org/cve/) or [Exploit-DB](http://exploit-db.com/) to look for vulnerabilities associated with the version number of that application. Vulnerabilities are attributed by a CVE number. You can learn more about these in [MuirlandOracle's Intro to Research room](https://tryhackme.com/room/introtoresearch).

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img2.png)  
  
12.4. Everything CGI (And no, not the movie kind...)  
As you may have discovered throughout the "Web" portion of the event, webservers don't just display websites...They are capable of interacting with the operating system directly. The Common Gateway Interface or CGI for short is a standard means of communicating and processing data between a client such as a web browser to a web server.  

Simply, this technology facilitates interaction with programmes such as Python script files, C++ and Java application, or system commands all within the browser - as if you were executing it on the command line.

![](https://www.tcl.tk/man/aolserver3.0/cgi.gif)  
([America Online., 1999](https://www.tcl.tk/man/aolserver3.0/cgi-ch1.htm))

Despite their age, CGI scripts are still relied upon from devices such as embedded computers to IoT devices, Routers, and the likes, who can't run complex frameworks like PHP or Node.

  

12.5. The Nitty Gritty  
Whilst CGI has the right intentions and use cases, this technology can quickly be abused by people like us! The commonplace for CGI scripts to be stored is within the **/cgi-bin/** folder on a webserver. Take, for example, this **systeminfo.sh** file that displays the date, time and the user the webserver is running as:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img3.png)

When navigating to the location of this script using our browser, the script is executed on the web server, the resulting output of this is then displayed to us. How could we use this?

12.6. As We've Demonstrated...  
We could, perhaps, parse our own commands through to this script that will be executed. Because we know that this is a Ubuntu machine,  we can try some Linux commands like `ls` to list the contents of the working directory:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img4.png)  

Or on a Windows machine, the `systeminfo` command reveals some useful information:

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img5.png)

This is achieved by parsing the command as an argument with `?&` **i.e.** `?&ls`. As this is a web server, any spaces or special characters will need to be [URL encoded](https://www.techopedia.com/definition/10346/url-encoding).

12.7. There are tools for this! Practical Metasploit  
Now we understand the application that's running, tools such as Metasploit can be used to confirm suspicions and hopefully leverage them! After some independent research, this application is vulnerable to the [ShellShock attack](https://securityintelligence.com/articles/shellshock-vulnerability-in-depth/) ([CVE 2014-6271](https://nvd.nist.gov/vuln/detail/CVE-2014-6271))

Let's start Metasploit's console and use the ShellShock payload. (TryHackMe's [room](https://tryhackme.com/room/rpmetasploit) and [blog post](https://blog.tryhackme.com/metasploit/) on Metasploit will be useful here)

At the minimum, when using an exploit, Metasploit needs to know two things:

- Your machine (such as the TryHackMe AttackBox) that you're attacking _from_ (**LHOST**)
- The target that you're attacking (**RHOST(S)**)

Exploits will have their own individual settings that you will need to configure. We can list these by using the `options` command, then using `set OPTION VALUE` accordingly. In our example, the exploit involves CGI scripts and as such, we must specify the location of the script on the webserver that we're attacking. In the example so far, this was at [http://10.0.0.1/cgi-bin/systeminfo.sh](http://10.0.0.1/cgi-bin/systeminfo.sh)

In order for the attack used as the example in this task to work, the options would be set like so:

- **LHOST** - _10.0.0.10_ (our PC)
- **RHOST** - _10.0.0.1_ (the remote PC)
- **TARGETURI** _/cgi-bin/systeminfo.sh_ (the location of the script)

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/options.png)  
_Please note that these options are for the exploit used as an example, you will have to set these values accordingly for the challenge._

After ensuring our options are `set` right, Let's run the exploit to get a Meterpreter connection...Success!

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img6.png)

To run system commands on the host, we will use `shell`. By creating a shell on the remote host, we can run system commands as if it were our own PC.

![](https://assets.tryhackme.com/additional/cmn-aoc2020/day-13/img7.png)

I highly recommend the [RP: Metasploit](https://tryhackme.com/room/rpmetasploit) room if you wish to delve into this wonderful framework further.

CVE-2019-0232 apache tomcat 9.0.17 cgi cmmdlineargs vulnerability

