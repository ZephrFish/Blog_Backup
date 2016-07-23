

So, for some of you who follow my blog, you may have noticed it was down this weekend. This is due to me smartly attempting to update to installation of ghost but getting it wrong several times. As a result decided to flatten the server and spin up a new VPS, now all that is out of the way the blog is brand new and back up :D. 

Now the reason for this post is a quick and easy deployment + upgrade script I've created for ease of setup. The main and only requirements being a VPS + Debian, the rest is taken care of in the following script. I've split this into two posts as it was getting huge!

###Requirements
 1. A Debian or Ubuntu based Server, can be a VPS too
 2. Common Sense
 3. Some minor reading skills
 4. You also need to have root privs on the server you're running for this to work

###Setting up the server
#### Change the default root password
Usually when setting up a server the VPS provider will give you a default password that has been randomly generated. It is **highly recommended** that you change this to something more secure >14 characters in length and memorable. This can simply be achieved with the following:

    passwd root
    Enter new UNIX password:
    Retype new UNIX password:

Once done you should be good to continue :-).

####Adding a User
This set of steps is assuming you've literally just spun up a VPS and it has nothing installed, firstly you'll want to create a user other than root to run with, in this example I have chosen the username **tux** however this can be anything you want. This can be achieved by using the following command:

     adduser tux

which will give the following output and questions:

    adding user `tux' ...
    Adding new group `tux' (1002) ...
    Adding new user `tux' (1002) with group `tux' ...
    Creating home directory `/home/tux' ...
    Copying files from `/etc/skel' ...
    Enter new UNIX password:
    Retype new UNIX password:
    passwd: password updated successfully
    Changing the user information for tux
    Enter the new value, or press ENTER for the default
            Full Name []:
            Room Number []:
            Work Phone []:
            Home Phone []:
            Other []:
    Is the information correct? [Y/n]


If all of the information is correct, you can populate the fields or just hit return for each then simply hit Y to accept, if all is well you will have added a new user, this can be checked by running the following:

    root@localhost:/var/www# cat /etc/passwd | grep tux

Which should give the following output if adding the user is successful

    tux:x:1002:1002::/home/tux:/bin/bash

Great! We now have a lower priv user, which is important for security.

####Setting Up Sources.list
For most of you this might not be important as you will have a fresh clean image of Debian 8 however for the unlucky few who's VPS provider does not give up to date sources.list files, this is for you, simply run the following code snippet to update your sources file with Jessie default URLs. It will also perform an update within apt.

    rm  /etc/apt/sources.list
    touch  /etc/apt/sources.list
    echo "deb http://ftp.de.debian.org/debian/ jessie main contrib non-free" >>  /etc/apt/sources.list
    echo "deb-src http://ftp.de.debian.org/debian/ jessie main contrib non-free" >>  /etc/apt/sources.list
    echo "deb http://security.debian.org/ jessie/updates main contrib non-free" >>  /etc/apt/sources.list
    echo "deb-src http://security.debian.org/ jessie/updates main contrib non-free" >>  /etc/apt/sources.list
    echo "deb http://ftp.de.debian.org/debian/ jessie-updates main contrib non-free" >>  /etc/apt/sources.list
    echo "deb-src http://ftp.de.debian.org/debian/ jessie-updates main contrib non-free" >>  /etc/apt/sources.list
    echo "deb http://ftp.de.debian.org/debian jessie-backports main contrib non-free" >>  /etc/apt/sources.list
    echo "deb-src http://ftp.de.debian.org/debian jessie-backports main contrib non-free" >>  /etc/apt/sources.list
    apt-get update

####Installing the Basics

once this is done, it would be good to install some basic packages then perform an upgrade using the following:

    apt-get install sudo wget curl git zip ccze byobu -y
    apt-get update
    apt-get upgrade -y && apt-get dist-upgrade -y

To explain the package selection and the commands above here is a short summary of each requirement and why I've chosen to install it.

 - **wget** - extremely useful for pulling down files when building things or wanting to aquire items from the internet.
 - **sudo** - for secuirty, allows a user to execute certain things with root privs without actually having the danger of running as root all the time, **NEVER** run a server as root...
 - **curl** - Similar to wget, however has other powerful uses too which we'll see later.
 - **git** - for cloning down git repositories and general usefulness
 - **zip** - for unzipping source packages and the likes
 - **ccze** - for making log files pretty with syntaxing and other niceties
 - **byobu** - a very powerful terminal manager, utilizes screen or tmux(up to you) to allow for multiple terminal sessions in one ssh login, very useful for server management over ssh!

Finally in this section an upgrade and dist-upgrade will insure all of our packages are up to date and patched.

####Configuring Sudo + SSH
Now that we have a user, basic tools installed and our system is up to date, the next stage would be to start protecting it by locking down login, please note: this is not an all inclusive guide on how to lock down ssh, full guides can be found on other sites.

To avoid having to log out of our normal user and log back in as the root account, we can set up what is known as "super user" or root privileges for our normal account. This will allow our normal user to run commands with administrative privileges by putting the word sudo before each command.

To do this it can be achieved by simply running the following command(where tux is your username you've created):

    gpasswd -a tux sudo

Once complete, the user will now have sudo/root privs, meaning they can execute root commands with sudo prefixed before a command.

Moving on from sudo, now it's time to start securing ssh. There are several options here, however first and foremost setting up public key authentication is a good starting point. Setting this up will increase the security of your server by requiring a private SSH key to log in.

#####Generate a Public Private Key Pair

If you already have an SSH key pair, you can skip this step and move on, but for the moment we're going to assume you have not got a pair.
To generate a new key pair, enter the following command at the terminal of your local machine(machine you're using to connect to your server with).

if you're using linux or unix use the following command(where tux is replaced with your username):
 

     ssh-keygen -t rsa -b 4096 -f /home/tux/.ssh/id_rsa

Assuming your local user is called "tux", you will see output that looks like the following:

    ssh-keygen output
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/tux/.ssh/id_rsa):
    Hit return to accept this file name and path (or enter a new name).


Next, you will be prompted for a passphrase to secure the key with. You may either enter a passphrase or leave the passphrase blank.

**Note**: If you leave the passphrase blank, you will be able to use the private key for authentication without entering a passphrase. If you enter a passphrase, you will need both the private key and the passphrase to log in. Securing your keys with passphrases is more secure, but both methods have their uses and are more secure than basic password authentication.

This generates a private key, **`id_rsa`**, and a public key, **`id_rsa.pub`**, in the **`.ssh`** directory of tux's home directory. Remember that the private key should not be shared with anyone!

    Your identification has been saved in /home/tux/.ssh/id_rsa.
    Your public key has been saved in /home/tux/.ssh/id_rsa.pub.
    The key fingerprint is:
    a9:49:2e:2a:5e:33:3e:a9:de:4e:77:11:58:b6:90:26 tux@remote_host
    The key's randomart image is:
    +--[ RSA 2048]----+
    |     ..o         |
    |   E o= .        |
    |    o. o         |
    |        ..       |
    |      ..S        |
    |     o o.        |
    |   =o.+.         |
    |. =++..          |
    |o=++.            |
    +-----------------+
 
Alternatively if you're on windows you'll need to generate a key, I prefer to use PuTTYgen however you may use any tool of your choice. In order to do this from within puttygen the following can be done:

 1. Launch the program, and then click the Generate button. The program
    generates the keys for you.
 2. Enter a unique key passphrase in the Key passphrase and Confirm passphrase fields. 
 3. Save the public and private keys by clicking the Save public key and Save private key buttons.
 4. From the Public key for pasting into OpenSSH `authorized_keys` file field at the top of the window, copy all the text (starting with ssh-rsa) to your clipboard by pressing Ctrl-C. You need this key available on your clipboard to paste either into the public key tool in the Control Panel or directly into the `authorized_keys` on your server.

Regardless of OS, after generating an SSH key pair, you will want to copy your public key to your new server. This can be achieved by doing the following:

echo the contents of the public key to the terminal OR if you're on windows simply copy the contents of the public key.

    cat ~/.ssh/id_rsa.pub

The key should look like the string shown below note the value will be different however the layout should be similar.
   
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDNqqi1mHLnryb1FdbePrSZQdmXRZxGZbo0gTfglysq6KMNUNY2VhzmYN9JYW39yNtjhVxqfW6ewc+eHiL+IRRM1P5ecDAaL3V0ou6ecSurU+t9DR4114mzNJ5SqNxMgiJzbXdhR+j55GjfXdk0FyzxM3a5qpVcGZEXiAzGzhHytUV51+YGnuLGaZ37nebh3UlYC+KJev4MYIVww0tWmY+9GniRSQlgLLUQZ+FcBUjaqhwqVqsHe4F/woW1IHe7mfm63GXyBavVc+llrEzRbMO111MogZUcoWDI9w7UIm8ZOTnhJsk7jhJzG2GpSXZHmly/a/buFaaFnmfZ4MYPkgJD tux@example.com

Take this value and copy/paste it into `/home/tux/.ssh/authorized_keys` on your server. Now that you have the keys setup the next step is to alter your ssh configuration to only allow key based authentication. There is a quicker way of doing this however, you can also execute the following:

    echo public_key_string >> ~/.ssh/authorized_keys

Assuming you have the public key file on your server already, if not the manual copy and paste works fine too. 


#####Altering SSH Configuration
Now that we have keys setup we'll need to alter the ssh configuration of our server, this can be achieved by opening sshd_condig as a user with root privs (this can be root too if you'd prefer) in your favorite text editor:

    sudo nano /etc/ssh/sshd_config

Inside the file, search for the section called `PasswordAuthentication`. This might be commented out. Uncomment the line and set the value to "no". This will disable your ability to log in through SSH using account passwords:

    PasswordAuthentication no

 Save and close the file when you are finished. To actually implement the changes we just made, you must restart the service.
You  can issue this command:

    sudo service ssh restart

You can now test ssh connectivity to your server by issuing the following from your local machine:

    ssh tux@machine

Which if successful will display a message similar to that shown:

    The authenticity of host '1.2.3.4 (1.2.3.4)' can't be established.
    ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
    Are you sure you want to continue connecting (yes/no)? yes
    tux@1.2.3.4's password:

If successful you will have accessed your server successfully, time to do some more ssh hardening.

Below are the recommended options. Note if the line is commented out remove the # from the start of the line to bring it into effect. Also test all your changes before logging out of the server. If you’ve broken SSH you’ll need to access your server over console which is usually a last resort but available with most VPS and Server providers.

    Protocol 2 
    PermitRootLogin no
    PasswordAuthentication no
    PermitEmptyPasswords no

These are the main settings to alter, the descriptions below explain why this is done.

- Protocol 2 – Enforce use of this protocol only. If it says 1,2 then remove “1,” from it. Version 1 allows you to SSH using insecure methods. There is no excuse to permit this type of authentication any more.
- PermitRootLogin no – Where possible, disable root login over ssh and use sudo if needed to gain root privs, this allows for a better security model.
- AllowUsers tux, zephr, panda – Limit the users that can connect  to SSH in the actual SSH configuration. There are many ways of doing this, but why not double up?
- PasswordAuthentication no – Once again if there are limited persons accessing the box and they all have keys, why leave the option to authenticate with a password on? If people are still authenticating with passwords, encourage a migration to SSH keys. Only use this option if your keys are setup, tested and working perfectly.
- PermitEmptyPasswords no – If there is no password set on a user then they can’t login to SSH. Simple as that.

After you’ve made some changes you need to restart SSHD:

    service sshd restart

This has been a basic outline of settings to configure to lock down and harden a new server, there are several other settings you can setup however I might do a further post on how to do this at a later date. 

Now that you have a server setup, you might be wanting to setup a blog, this can be achieved by following the steps in [part 2]()
