# How to Install Ubuntu Server: A Step-by-Step Guide

Ubuntu Server is a powerful, easy-to-use, and versatile operating system that’s widely used for running web servers, database servers, and other services. It’s a popular choice for system administrators and developers due to its stability, security, and extensive community support. Installing Ubuntu Server is straightforward, and in this guide, we'll walk you through the steps to get Ubuntu Server up and running on your machine.


---

Prerequisites

Before starting the installation process, ensure you have the following:

A machine to install Ubuntu Server on (either a physical machine or a virtual machine).

A stable internet connection.

A USB flash drive (at least 4GB in size).

Backup your data (if applicable).

Ubuntu Server ISO image file (you can download it from the official Ubuntu website).



---

Step 1: Download the Ubuntu Server ISO

1. Go to the Ubuntu Downloads page and download the latest stable version of Ubuntu Server. The ISO file will be around 1-2 GB in size.


2. If you want to verify the integrity of the ISO, you can check its checksum on the Ubuntu website.




---

Step 2: Create a Bootable USB Drive

You’ll need to create a bootable USB drive to install Ubuntu Server.

On Linux:

If you're using a Linux machine, you can use the dd command to write the ISO to your USB drive:

sudo dd if=/path/to/ubuntu-server.iso of=/dev/sdX bs=4M status=progress && sync

Replace /path/to/ubuntu-server.iso with the location of the downloaded Ubuntu Server ISO file.

Replace /dev/sdX with the device name of your USB stick (e.g., /dev/sdb). Be cautious, as selecting the wrong device could erase your data.


On Windows:

You can use a tool like Rufus to create the bootable USB. Here’s how:

1. Open Rufus and select your USB drive.


2. In the "Boot selection" section, choose the Ubuntu Server ISO file you downloaded.


3. Leave the partition scheme as "MBR" or "GPT" based on your system’s configuration.


4. Click "Start" to write the image to your USB drive.




---

Step 3: Boot from USB

1. Insert the bootable USB drive into your machine and restart it.


2. Enter the BIOS/UEFI settings (usually by pressing a key like F2, F12, DEL, or ESC during startup).


3. Set the USB drive as the primary boot device.


4. Save changes and exit BIOS/UEFI settings.


5. Your system should now boot from the USB, and you'll see the Ubuntu Server installation menu.




---

Step 4: Select Language and Region

Once the Ubuntu Server installer starts, you’ll be prompted to select the language and region.

1. Select your preferred language (e.g., English).


2. Choose your location and keyboard layout.




---

Step 5: Configure Network

During the installation, the system will attempt to configure your network settings automatically.

1. For wired connections (Ethernet): The installer should automatically detect the network interface and connect you to the internet.


2. For wireless connections: If you're using Wi-Fi, you’ll be prompted to select a network and enter the Wi-Fi password.




---

Step 6: Set Up User and Password

1. The installer will prompt you to create a user account. Choose a name and password for your system.


2. Set a strong password for the root user (optional, but recommended for security purposes).


3. You can choose to skip the root password if you want to use sudo instead (recommended for most users).




---

Step 7: Partition the Disk

The installer will prompt you to select how to partition your hard drive. You have several options:

1. Use entire disk (Recommended for beginners): This will automatically partition the disk, and the installer will handle everything for you.


2. Manual partitioning: If you want more control over your disk layout, you can manually partition the disk. This option is ideal for advanced users or those who want to create multiple partitions (e.g., separate partitions for /home, /var, etc.).



After choosing your preferred partition scheme, the installer will display a summary of the partitions. If everything looks good, confirm the partition changes.


---

Step 8: Choose Software to Install

The installer will ask you to select additional software packages. For a basic Ubuntu Server setup, you can choose:

1. OpenSSH server (recommended): This allows you to connect to your server remotely via SSH.


2. Snap server: If you want to use snaps (Ubuntu's universal package format), you can install this.


3. LAMP stack: If you're setting up a web server, you can install the Linux, Apache, MySQL, and PHP stack.


4. Other options: You can choose to install specific software like a DNS server, Samba server, etc.



For most users, enabling the OpenSSH server is a good starting point.


---

Step 9: Install the GRUB Bootloader

The installer will ask you to install the GRUB bootloader. GRUB is responsible for booting your system.

1. Select "Yes" to install the GRUB bootloader to your disk.


2. If you're using a UEFI system, the installer will automatically handle the EFI partition and bootloader configuration.




---

Step 10: Finalize Installation and Reboot

Once all the packages are installed, the installer will prompt you to remove the installation media (the USB drive) and press Enter to reboot.


---

Step 11: Post-Installation Setup

After the server reboots, you’ll be greeted with a login prompt. Log in with the user account you created earlier.

1. Update the system: Once logged in, it’s important to update your system to ensure you have the latest patches and software.



sudo apt update
sudo apt upgrade

2. Install additional packages: You can install additional software as needed. For example, if you want to install a web server:



sudo apt install apache2

3. Set up SSH: If you selected OpenSSH during installation, you can now connect to your server remotely using SSH. From another machine, you can run:



ssh your_username@your_server_ip


---

Step 12: Configure Networking (Optional)

For a more robust network setup, you may want to configure static IP addresses, configure DNS, or set up firewall rules. You can edit the network configuration file as follows:

sudo nano /etc/netplan/00-installer-config.yaml

Adjust your network settings to suit your needs, then apply the changes:

sudo netplan apply


---
Conclusion

Congratulations! You've successfully installed Ubuntu Server on your machine. Now, you can begin configuring it to suit your needs, whether it’s setting up a web server, file server, or anything else.

Ubuntu Server is a flexible, powerful OS with a large community and extensive documentation, making it an excellent choice for both beginners and experienced users. Happy server managing!

For more details on Ubuntu Server configurations, be sure to check the official Ubuntu Server documentation.
