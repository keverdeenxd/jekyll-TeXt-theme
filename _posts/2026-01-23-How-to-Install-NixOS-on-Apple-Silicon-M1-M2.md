How to Install NixOS on Apple Silicon (M1/M2)

NixOS is a Linux distribution that uses the Nix package manager to handle packages and system configurations in a declarative way. Installing it on Apple Silicon devices (such as M1 and M2 Macs) requires a few additional steps compared to other platforms, due to the lack of official support for ARM-based Macs. Here’s how to do it.


---

What You’ll Need:

A Mac with an Apple Silicon (M1 or M2) processor.

A USB flash drive (8 GB or larger).

Another computer to prepare the installation USB.

A stable internet connection for downloading installation files.



---

Step 1: Prepare the Installation USB

Since NixOS doesn’t yet have official Apple Silicon support, we will use a custom approach to set up the installation media.

1. Download the NixOS ARM64 Image:

Go to the official NixOS download page: NixOS Downloads.

Download the ARM64 version of NixOS, specifically the minimal ISO for ARM architectures.



2. Create a Bootable USB:

Insert your USB drive into another computer.

On Linux, use the dd command to write the NixOS image to the USB drive:

sudo dd if=nixos-<version>-x86_64-linux.iso of=/dev/sdX bs=4M status=progress && sync

Replace /dev/sdX with the actual device name of your USB drive (be cautious when selecting this, as it will erase the drive).

On macOS, you can use balenaEtcher or dd via Terminal to create the bootable USB.





---

Step 2: Boot into NixOS Installer

Now, let’s boot your Apple Silicon Mac into the NixOS installer.

1. Power off your Mac, then hold the Power button to enter the boot options screen.


2. Select the bootable USB from the available options.


3. The NixOS installation environment will boot up. This is a minimal environment based on the NixOS installer, and you'll interact with it using the terminal.




---

Step 3: Set Up the Disk and Partitioning

Once you’re in the NixOS installer, you’ll need to partition your Mac’s disk for the installation.

1. Identify the Disk: In the terminal, list the available disks:

lsblk

Your internal storage should appear as something like /dev/nvme0n1.


2. Partition the Disk: We’ll be using parted or gdisk to create the necessary partitions. We need a EFI partition (for booting) and a root partition (for NixOS).

Here’s an example partitioning scheme:

EFI partition (500 MB)

Root partition (the remaining space)


Example using gdisk:

sudo gdisk /dev/nvme0n1

Follow the prompts to create the partitions:

Create a new GPT partition table (use o to create).

Create the EFI partition (1 for 500 MB).

Create the Linux filesystem partition for root.



3. Format the Partitions:

Format the EFI partition:

sudo mkfs.fat -F32 /dev/nvme0n1p1

Format the root partition:

sudo mkfs.ext4 /dev/nvme0n1p2



4. Mount the Partitions:

Mount the root partition:

sudo mount /dev/nvme0n1p2 /mnt

Mount the EFI partition:

sudo mount --mkdir /dev/nvme0n1p1 /mnt/boot





---

Step 4: Install NixOS

Now, we’ll install the NixOS system onto the disk.

1. Generate NixOS Configuration: The NixOS installer provides a simple way to generate an initial configuration. Run:

sudo nixos-generate-config --root /mnt


2. Edit the Configuration: Edit the generated NixOS configuration file located at /mnt/etc/nixos/configuration.nix:

sudo nano /mnt/etc/nixos/configuration.nix

In this file, you will need to:

Enable EFI boot:

boot.loader.grub.device = "/dev/nvme0n1p1";  # Adjust for your EFI partition
boot.loader.systemd-boot.enable = true;

Set up networking (replace yourhostname with your preferred name):

networking.hostName = "yourhostname";

Enable basic system services (like SSH, if desired):

services.openssh.enable = true;




3. Install NixOS: Once the configuration is edited, run:

sudo nixos-install

The installation will take some time to complete.




---

Step 5: Reboot into NixOS

Once NixOS is installed, you need to reboot your Mac.

1. Exit the NixOS installer:

exit


2. Reboot your Mac:

sudo reboot


3. Your Mac will reboot into NixOS. If the boot doesn’t work properly or you don’t see a boot menu, ensure that the System Preferences > Startup Disk is set to boot from the correct disk.




---

Step 6: Final Configuration

Once NixOS is running, you may need to adjust some additional settings or install specific drivers for Apple Silicon.

1. Check Hardware Compatibility: Apple Silicon devices require certain kernel modules for proper hardware support. You can check for these by consulting the NixOS Wiki or the NixOS community forums for specific patches.


2. Configure System Services and Software: Continue configuring your NixOS setup using the configuration.nix file. You can install packages, enable services, and fine-tune your system setup.




---

Conclusion

Congratulations! You’ve installed NixOS on your Apple Silicon device. While support for ARM-based Macs is still evolving, NixOS offers a flexible and highly configurable environment for users who are comfortable working with the terminal and a bit of troubleshooting.

Remember, NixOS is a very powerful and customizable OS, so continue exploring its features and packages to make the system work exactly as you need.
