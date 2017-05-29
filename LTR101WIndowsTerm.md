Windows is one of the most commonly used operating systems in the world, in comparison to Linux it is lesser used in security however still an OS of choice for many. The possibilities for usage are fairly large, the UI is usable and support of tools is varied however by default supports virtualisation via 3rd party applications and is easy to setup.

This post will discuss a small proportion of functionality built into windows, specifically the terminal environments available. 

### Powershell
Powershell is one of two ([three](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/) if you're on windows 10 and have the bash environment setup) terminal interfaces on Windows, it serves as a tool to facilitate automation & other exploitation on. 

Powershell can be used to create scripts similar to bash scripting on Unix, however the syntax and coding is very similar to that of perl. In recent years it has come out as a very powerful tool for hacking & post-exploitation as many anti-virus and anti-malware programs don't catch it due to the execution methods. Some brilliant frameworks and exploit tools have been built based upon powershell, feel free to check them out.

- [Empire](https://github.com/EmpireProject/Empire)
- [PoshC2](https://github.com/nettitude/PoshC2)
- [Powersploit](https://github.com/PowerShellMafia/PowerSploit)

In a security sense, post-exploitation techniques aren't the only use for powershell. It can also be used for legitimate system administration purposes to allow you to gain information about the system or systems on a network, it has endless potential for automating mundane tasks within Windows such as [deployment](https://github.com/PSAppDeployToolkit/PSAppDeployToolkit) and [GPO](https://technet.microsoft.com/en-gb/library/ee461027.aspx). 

It also acts as the main modern command line interface within windows facilitating the use of other programming languages such as python or ruby. It is accessible by doing the following `ctrl + r` then typing powershell, this will launch you into a powershell session with a prompt, this prompt will accept a certain degree of *nix commands such as `ls` and `echo`. You can find out all of the commands available within the powershell world by using the `man` or `help` commands against any command or module. However before you do this you'll want to run `update-help` to ensure all of the manual and help pages are up to date.

Accessing powershell is as simple as `windows key` + `r`, then typing `powershell.exe`.  This will pop a powershell window open:
![](/content/images/2017/03/ps.png)

From this interface you have direct access to the underlying operating system and powershell `cmdlets`. Which are modules built into powershell that have different functions.

### Command Line (CMD.exe)
Another terminal environment built into windows is the command line tool (cmd.exe), it acts as a more dated instance of powershell. It does not support the use of modules but still gives access to underlying system commands to find out about files and other aspects of the operating system.


![](/content/images/2017/04/cmd.png)

As can be seen above the prompt varies slightly in cmd vs ps, however the core basic commands work the same on both. Try it yourself, type `whoami`.

![](/content/images/2017/04/whoam.png)

Knowing your way around both powershell and cmd are very useful for post exploitation in testing but also for general usage. I'd suggest having a read into batch & powershell scripting to better understand both `ps` & `cmd`. 

A very basic batch script is shown below, it prints out some text to the screen and creates a folder then explains to the user what it just did. You can try too by saving this into a `.bat` file then double clicking on the `.bat` to run it.

```
@echo off

:: do stuff
echo Hello World
mkdir hello-world

:: Explain what the script just did
echo:
echo Created hello-world folder
echo:

endlocal

:: Pause allowing the user to read what shit does
pause
```

Here is a screenshot of it running and creating a folder successfully:

![](/content/images/2017/04/hw.png)

To do similar on powershell a script such as the following can be created:
```
# Powershell Hello World
Write-Host
Write-Host 'Hello World!'
mkdir hello-world-ps
```

Then to run it on our host we need to do a few minor things within powershell. First the execution policy needs to be set for the current user to allow us to execute the script. Then our script can be run.

```
PS C:\Desktop\PoC> Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
http://go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
PS C:\Desktop\PoC> cat .\ps.ps1
# Powershell Hello World
Write-Host 'Hello World!'
mkdir hello-world-ps
PS C:\Desktop\PoC> .\ps
Hello World!


    Directory: C:\Desktop\PoC


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       15/04/2017     10:56                hello-world-ps
```

![](/content/images/2017/04/ps_a.png)

From the output it can be seen that the folder was created successfully, this is evidenced from both the script output and the screenshot. Powershell has an execution policy in place by default to protect users from themselves, it prevents scripts from running in an attempt to stop malicious things happening.

### Bash on Windows 
What is this witchcraft you may wonder? Well as of Windows 10, it is now possible to run bash on windows from the inbuilt OS rather than install a 3rd party simulator such as Cygwin or PentestBox. To enable it the following steps can be taken:

 1. Turn Developer Mode on via Settings > Update & security > For developers
 2. Click the Start button , click Control Panel, click Programs, and then click   3Turn Windows features on or off.
 4. Enable Windows Subsystem for Linux (Beta)
 

Once this is done you should be able to open cmd or powershell and simply type "bash":

![](/content/images/2017/04/bash.png)

This will drop you into the bash environment which is running Ubuntu, it has all of the standard features you'd expect to see in Linux, allowing for package installation, bash scripting, built in ssh and more. The only things I've had minor trouble with is packages that require raw access to the network such as nmap and masscan, however there are windows variants available for these anyway!

Now to show a bash script running within windows I wrote the following quick script:

```
#!/bin/bash
whoami
id
pwd
echo "Hello World"
mkdir bashonwindows
```

As simple as that, show who I am, what my privs are echo a message to the terminal and create me a folder.

![](/content/images/2017/04/bashwin.png)

Boom headshot! Bash on windows my friends, native to W10 :-). 

Did you enjoy this? Check out the other #ltr101 posts [here](https://blog.zsec.uk/tag/ltr101/) or consider buying my [book](https://leanpub.com/ltr101-breaking-into-infosec).
