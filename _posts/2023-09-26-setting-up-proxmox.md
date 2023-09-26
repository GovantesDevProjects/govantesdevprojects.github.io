---
title: A Comprehensive Guide to Setting Up a Proxmox Server
date: 2023-09-26 12:00:00 -500
categories: [proxmox,homelab]
tags: [proxmox,homelab,]
---

![Small Server](/assets/lib/small-server.jpg)

## Introduction

Setting up a Proxmox server can be a powerful solution for virtualization and container management. In this guide, we will walk you through the step-by-step process of setting up a Proxmox server, covering various aspects like updating repositories, configuring storage pools, enabling SMART monitoring, setting up PCI passthrough, making Proxmox VLAN aware, establishing NFS shares, and scheduling backups. By the end of this tutorial, you'll have a fully functional Proxmox server at your disposal.

### Step 1: Updating Repositories

Before we dive into the Proxmox setup, it's essential to ensure that your system is up-to-date, even without a subscription. Proxmox offers a free community edition, and updating repositories is a crucial first step. To do this, we first need to edit the repositories:

1. This can be done using the GUI
   * Disable the two `enterprise.proxmox.com/...` repositories
   * Add the `pve-no-subscription` repository
   * Your repositories page should now look like this:
    ![Repository Example](</assets/lib/repositories config.png>)
2. Now update the repositories

   ```bash
   apt-get update && apt upgrade -y
    ```

### Step 2: Configuring Proxmox ZFS Storage Pool

Proxmox supports various storage backends, and ZFS is a popular choice due to its robust features. Here's how to set it up:

  1. If you installed Proxmox on a disk and manually limited its installation size, we need to build another partition on the disk.
      To do so:

      ```shell
      fdisk /dev/DRIVE #enter DRIVE name (sda1, nvme0n1, etc)
      p #print partition table
      n #create a new partition
      press enter #accept default value of partition number
      press enter #accept default value of first cylinder
      press enter #accept default value of last cylinder
      p #print new partition table
      w #save new partition table
      ```

  2. Go to disks/ZFS and click `Create:ZFS`

      ![ZFS Pool](/assets/lib/nvme_pool.png)

  3. I now have a 430GB ZFS pool on my boot drive. In future posts, I'll cover how to create new directories within your ZFS pool.

### Step 3: Ensure SMART Monitoring is enabled

  1. Ensuring the health of your hard drives is critical. To ensure SMART Monitoring is enabled using the GUI:

      ![SMART Monitoring GUI](</assets/lib/smart monitoring.png>)

      * By default, SMART Monitoring is enabled
  2. Using the command line:

      * You can get the status of a disk by issuing the following command:

        ```bash
        smartctl -a /dev/sdX #where /dev/sdX is the path to one of your local disks.
        ```

        * If the output says:

        ```bash
        SMART support is: Disabled
        ```

        you can enable it with the command:

        ```bash
        smartctl -s on /dev/sdX
        ```

### Step 4: Enabling PCI Passthrough (IOMMU)

PCI passthrough allows you to assign physical devices to virtual machines. Ensure that your CPU supports VT-d (IOMMU) and enable it in BIOS.

1. Edit the GRUB configuration file:

   ```bash
      nano /etc/default/grub
      ```

2. Add the following line:

    ```bash
    GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
    #change intel for AMD if you are running an AMD CPU
    ```

3. Comment out:

    ```bash
    # GRUB_CMDLINE_LINUX_DEFAULT="quiet"
    ```

4. Update GRUB:

    ```bash
    update-grub
    ```

5. Update modules:

    ```bash
    nano /etc/modules
    ```

6. Add the following lines to `/etc/modules

    ```txt
    vfio
    vfio_iommu_type1
    vfio_pci
    vfio_virqfd
    ```

7. Save and exit, and reboot

    ```bash
    reboot
    ```

### Step 5: Making Proxmox VLAN Aware

To create VLANs in Proxmox, you can use the Linux bridge as a basis:

1. Go to networks, select Linux Bridge, and edit
  ![!\[VLAN aware\](vlan%20aware.png)](</assets/lib/vlan aware.png>)
2. Check VLAN aware and click OK

### Step 6: Scheduling Backups with Proxmox

Regular backups are crucial for data integrity:

1. Log in to the Proxmox web interface.

2. Navigate to the desired VM or container.

3. Click on "Backup" and configure your backup options, including storage location and schedule.

## Conclusion

Setting up a Proxmox server involves several essential steps, from updating repositories to configuring storage, enabling SMART monitoring, setting up PCI passthrough, VLANs, and scheduling backups. With this comprehensive guide, you can confidently build a robust Proxmox server tailored to your needs.
