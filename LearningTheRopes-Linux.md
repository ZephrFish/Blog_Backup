# Learning the Ropes 101: Operating Systems - Linux
When learning about information security, software development, computer science or "insert other relevant topic here" it is likely that you will come up against a variety of different operating systems. Most new folks to this area will be primarily windows users and as a result will have had little or no exposure to the world of Linux. Or you might be on the other side of the fence, having grown up with *nix by your side the world of windows is an alien planet to you. You might also fall into the Mac camp, having grown up with macs, you're used to things looking pretty, but have you ever tinkered with what goes on under the hood in the OS?

Do not worry if you fall into either of these three! We've all been at the same point one way or another, this next post will explain in depth some of the operating systems you might come across and it will also show you example scenarios.

 1. Linux
 2. Windows (Coming Soon)
 3. Mac OSX (Coming Soon)

##Linux
Linux is one of the most commonly used operating systems in servers & web applications to date. In 2015, Linux & Unix-like operating systems made up 98% of the servers on the Internet. From this statistic alone it can be seen that it's likely to be popular for a reason?

When learning about Linux, if it's new to you and you've come from a mainly windows background, you'll be used to using a graphical user interface(GUI). 

While Linux has a large variety of GUIs, it excels most with it's use of the Terminal. The command-line interface, sometimes referred to as the CLI or terminal, is a tool into which you can type text commands to perform specific tasks in contrast to the mouse's pointing and clicking on menus and buttons. You may have come across the command line within windows before(cmd.exe). If this is something you've never come across you're still probably scratching your head thinking:

###What the heck is a terminal or command line?
Allow me to explain some more, since you can directly control the computer by typing, many tasks can be performed more quickly. As a result of this, some tasks can be automated with special commands that loop through and perform the same action on many files saving you, potentially, loads of time in the process.

The application or user interface that accepts your typed responses and displays the data on the screen is called a shell, and there are many different varieties that you can choose from, but the most common these days is the Bash shell, which is the default on Linux and Mac systems in the Terminal application. 

###Getting started with the command line interface
A lot of the folk I've spoken to have been really interested in command line but haven't really known where to get started. I'd usually say just fire up Linux and play. However for some people this might be a mentally difficult task, for those of you who want a little hand on your way, I suggest you take a [Command Line Crash Course](https://www.codecademy.com/learn/learn-the-command-line) on Code Academy.

For those of you who are as far as knowing the basics, I suggest you start to look at bash scripting to enable you to do things. The most simple bash script would be a hello world application as shown in the code below:
    

>  #!/bin/bash
>
> echo "Hello World"

Open this in a text editor such as nano, vim, leafpad(if you prefer a GUI) and save as myscript.sh. Now some of you will see the above code and think WHAT EVEN IS THAT, I DON'T UNDERSTAND, HEEEELLLLLPPPPPPP. And others might see it and think I sort of know what that does but tell me more. Don't worry if you're either of these two situations, I'm about to explain what it does, what else you can do. 

For the first camp of people who see the code and are running about confused and crying. Do not worry, it's going to be alright. Firstly explaining that the lines of code do, the first line #!/bin/bash explains the environment that the script is running which is the shell environment in linux = [bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)). The next line is a simple print Hello World on screen. 

Now this may be too simple for some of you, for those of you who want to get more out of things, pick a project and write it in bash, check out http://www.commandlinefu.com/ for tips and tricks of things to do. If you're stuck for ideas, check out my [Github](https://github.com/ZephrFish/RandomScripts) scripts.

This should be enough to get you started on linux, there will be a future more in depth writeup on advanced topics to check out. Onwards! 

  
