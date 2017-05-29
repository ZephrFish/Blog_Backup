This post talks of what virtualisation is, why it is important and how it works. I will also take you through setting up your first virtual machine (VM). 

## What is Virtualisation
Virtualisation put briefly is the process of creating a container of resources to run an operating system within the context of another. Think of it like building a shed in your house and setting it up like another house or living environment.  

## What it is Used For
Virtualisation is all about separating traditional IT resources into more easily managed and centralised solutions. This separation often increases scalability, improves resource utilisation, and reduces administrative resources. Specifically it works by utilising hardware to create a virtual environment similar to that of a physical machine, taking an example as you have a desktop that has a quad core processor, 4 GB of ram & a 500GB Hard drive. In the conventional way you could install windows on this and roll with a single operating system however say you need to do something in linux or another OS, you could dual boot systems but the quicker way to do so would be to virtualise. 

## Setting Up Your First Virtual Machine
For this section you'll need to download either VMWare player OR VirtualBox, both work just the same. My personal preference is VMWare Workstation(paid for version) as it has some neat features such as export and snapshots plus other little tweaks here and there.

**Note**: I'm going to show you how to setup on windows, however to do so on Mac & Linux is more or less the same with both VMWare & Virtualbox.

### Steps to Create a VM
- **Step 1**: Obtain from  [VMWare](https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/12_0) or [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- **Step 2**: Obtain an ISO for an OS you want to virtualise; for this tutorial I'll be using [Debian](https://www.debian.org/CD/http-ftp/#stable), however you can use whatever OS you want. 
####Create a new VM (VMWare)

   - *VMWare*: Select File>New Virtual Machine
   - *VMWare*: This will spawn a new vm wizard, from here select "Installer disk image file" (ISO) and choose the ISO you want to use to setup a VM.
   
![](/content/images/2017/03/step3b-1.png)

   - *VMWare*: Follow the wizard though select the appropriate operating system to match the ISO you've selected. 
![](/content/images/2017/03/3c.png)
   - *VMWare*: Give it a name and a location on your local machine.
![](/content/images/2017/03/3d.png)
   - *VMWare*: Select the specs you require, depending on what purpose you want for the machine you're creating.
![](/content/images/2017/03/3e.png)
![](/content/images/2017/03/3f.png)
   - *VMWare*: Once this is done it's just a case of starting your VM by clicking the green start button, then it's just a case of setting up like you would an OS normally.
![](/content/images/2017/03/3g.png)

####Create a new VM (VirtualBox)
*(The screenshots in this example have been taken on a Mac however the process is exactly the same on Windows)*

- Once you have Virtualbox open, creating a VM is fairly simple. Simply click New, to create a new VM.

![](/content/images/2017/03/virtualbox_new.png)

- Next select `Expert Mode` which should give a window similar to that shown below.
 
![](/content/images/2017/03/step2VirtualBox.png)

This allows you to name your new VM, select the OS you're installing, the version & specify the amount of RAM  you want to give it too. In this example I'm going to install Debian with 2GB of RAM.

![](/content/images/2017/03/HDD_VM.png)

  - Click Create 
  - Select the size & location you want to store the virtual drive file. 
  - Then select Create again.

Once this is done VirtualBox will create an instance of your VM, the final step is to click start, and give the VM an ISO to install. 
![](/content/images/2017/03/start.png)

![](/content/images/2017/03/VM-Started.png)


Other Platforms
-------------

There are many other platforms too for virtualisation of operating systems, I've simply demonstrated the two most common applications used on consumer devices. Some examples worth checking out can be seen in the list below.

- ESXi
- HyperV
- Xen
- KVM
- QEMU

Did you enjoy this? Check out the other #ltr101 posts [here](https://blog.zsec.uk/tag/ltr101/) or consider buying my [book](https://leanpub.com/ltr101-breaking-into-infosec).
