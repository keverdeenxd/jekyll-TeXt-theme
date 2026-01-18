How to Install Arch Linux: A Beginner's Guide

Arch Linux is a popular, lightweight, and flexible Linux distribution known for its simplicity and control. It’s often chosen by advanced users who want to learn about the inner workings of Linux. However, its minimalistic design also makes it an excellent choice for anyone who wants a custom, streamlined system. While Arch is not as user-friendly as some other Linux distributions, with the right guidance, you can set it up with ease.

In this guide, we'll walk through the steps to install Arch Linux on your system, from downloading the installation image to configuring your desktop environment.


---

Prerequisites

Before you begin, make sure you have the following:

A stable internet connection

A USB drive (4GB or more recommended)

A machine capable of booting from a USB drive

Backup of your important data (just in case something goes wrong)



---

Step 1: Download Arch Linux ISO

1. Go to the official Arch Linux website and download the latest ISO image.


2. Verify the integrity of the ISO by checking its checksum to ensure it's not corrupted.




---

Step 2: Create a Bootable USB Drive

To install Arch Linux, you'll need to create a bootable USB stick from the ISO image.

On Linux:

sudo dd if=path_to_arch.iso of=/dev/sdX bs=4M status=progress && sync

Replace path_to_arch.iso with the location of your downloaded ISO file.

Replace /dev/sdX with your USB drive (e.g., /dev/sdb). Be very careful with this command—using the wrong drive can erase data!


On Windows:

Use a tool like Rufus to write the ISO to the USB drive.


---

Step 3: Boot from USB

1. Plug the bootable USB drive into your machine.


2. Restart your computer and enter the BIOS/UEFI settings. This is usually done by pressing a key like F2, F12, DEL, or ESC (depending on your system).


3. Set your system to boot from the USB drive and save the changes.


4. Once the system boots up, you'll see the Arch Linux boot prompt.




---

Step 4: Set Up the Internet Connection

Before proceeding with the installation, you need an active internet connection.

For wired connections: It should automatically connect.

For Wi-Fi:

Use iwctl to connect to Wi-Fi.


iwctl
station device_name connect your_wifi_network

Replace device_name with your wireless device (e.g., wlan0), and your_wifi_network with the network name.


---

Step 5: Update the System Clock

Next, update your system clock to ensure accurate timekeeping during the installation process.

timedatectl set-ntp true


---

Step 6: Partition the Disk

Use fdisk or parted to partition your disk. The number of partitions will depend on your preferences, but a simple layout could look like this:

1. EFI partition (for UEFI systems)


2. Root partition


3. Optionally, Home partition (if you want to separate system files and personal data)



Example with fdisk (assuming /dev/sda is your disk):

fdisk /dev/sda

Follow these steps to create your partitions:

Create a new partition table (if necessary).

Create a 512MB EFI partition (type EFI System).

Create a root partition (type Linux filesystem).

Optionally, create a home partition.


You can verify your partitions with:

lsblk


---

Step 7: Format the Partitions

Once the partitions are created, format them accordingly.

Format the EFI partition as FAT32:


mkfs.fat -F32 /dev/sda1

Format the root partition as ext4 (or another filesystem of your choice):


mkfs.ext4 /dev/sda2

If you created a home partition:

mkfs.ext4 /dev/sda3


---

Step 8: Mount the Partitions

Mount the root partition to /mnt:

mount /dev/sda2 /mnt

If you have a home partition, mount it as well:

mount /dev/sda3 /mnt/home

Mount the EFI partition:

mkdir /mnt/boot
mount /dev/sda1 /mnt/boot


---

Step 9: Install Base System

Use the pacstrap command to install the base system, which includes the core packages for Arch Linux.

pacstrap /mnt base linux linux-firmware vim

You can also add additional packages if you need them (e.g., nano for a text editor, or sudo for permissions).


---

Step 10: Configure the System

1. Generate fstab: This file contains information about disk partitions.



genfstab -U /mnt >> /mnt/etc/fstab

2. Chroot into the new system:



arch-chroot /mnt

3. Set the time zone:



ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc

Replace Region/City with your time zone (e.g., Europe/London).

4. Localization: Uncomment your locale in /etc/locale.gen, then generate the locales.



nano /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8
locale-gen

Create the locale configuration file:

echo "LANG=en_US.UTF-8" > /etc/locale.conf

5. Set the hostname:



echo "myhostname" > /etc/hostname

6. Set root password:



passwd

Enter and confirm your root password.


---

Step 11: Install Bootloader

For UEFI systems, you’ll need to install a bootloader. The recommended choice is systemd-boot.

bootctl --path=/boot install

Create a bootloader configuration file:

nano /boot/loader/loader.conf

Add the following lines:

default arch
timeout 3

Create the Arch Linux boot entry:

nano /boot/loader/entries/arch.conf

Add:

title   Arch Linux
linux   /vmlinuz-linux
initrd  /initramfs-linux.img
options root=/dev/sda2 rw


---

Step 12: Exit and Reboot

1. Exit from chroot:



exit

2. Unmount all partitions:



umount -R /mnt

3. Reboot your system:



reboot

Be sure to remove the USB drive before the system starts booting.


---

Step 13: Post-Installation

Once you boot into your new Arch Linux system, you can begin installing a desktop environment (like GNOME, KDE, or XFCE) and configuring other software packages.

Here are a few things you might want to do:

Install a display manager (e.g., lightdm).

Install your preferred desktop environment or window manager.

Install network tools, like NetworkManager.


For a detailed, up-to-date guide, visit the Arch Wiki.


---

Conclusion

Arch Linux offers a learning experience that many Linux enthusiasts love. By following these steps, you can set up Arch Linux from scratch and customize it to your needs. It might take a bit more effort than other distributions, but the rewards of having a lightweight, efficient, and tailored system are worth it. Happy installing!
