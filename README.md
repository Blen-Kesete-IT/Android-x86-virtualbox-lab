# Android-x86-virtualbox-lab
# Android-x86 and Kali Linux Networking Lab

I’m building a virtual networking lab to practice installing operating systems, configuring static IP addresses, and testing connectivity.

The goal is to install Android-x86 9.0 in VirtualBox and connect it to the same NAT Network as my Kali Linux VM.

Through this project, I’ll practice:

- Creating and configuring a virtual machine
- Installing Android-x86 from an ISO
- Connecting two VMs through a shared NAT Network
- Configuring IP addresses, a gateway, and DNS
- Using ping to test communication and internet access

I’ll document my setup, challenges, and results here, including screenshots of the connectivity tests.

## Step 1: Download the Android-x86 ISO

Download `android-x86_64-9.0-r2.iso` from the official Android-x86 website. This file serves as the installation disc for the Android virtual machine.
Download the Android-x86 9.0 ISO from the [official Android-x86 download page] or its linked SourceForge release page. Follow the Release 9.0 link and select `android-x86_64-9.0-r2.iso` for a 64-bit VM.

<img width="1224" height="1346" alt="image" src="https://github.com/user-attachments/assets/16e5652b-a29e-4dfe-8aa5-a6b9c60d1855" />


## Step 2: Create a New Virtual Machine

Open VirtualBox and click **New**.
Name the virtual machine **Android9-Lab**.
Set the operating system type to **Linux** and version to **Other Linux (64-bit)**.
If prompted for an ISO, select the downloaded Android-x86 ISO and choose **Skip Unattended Installation**.
Allocate **2048 MB (2 GB)** of RAM.
Create a **VDI virtual hard disk** with dynamically allocated storage and a size of **at least 10 GB**.
Finish creating the virtual machine.

The VM is now ready for the Android-x86 installation.

<img width="683" height="476" alt="image" src="https://github.com/user-attachments/assets/bdd321ee-d3d2-4a18-9769-383f74a97120" />

## Step 3: Attach the ISO and Install Android-x86

1. Open the VM’s **Settings → Storage**.
2. Select the **Empty** optical drive under the storage controller.
3. Click the disk icon and choose the Android-x86 9.0 ISO you downloaded.
4. Go to **Settings → Display**, allocate at least **64 MB** of video memory, and enable **3D Acceleration** as instructed in the lab.
5. Start the VM and select **Installation – Install Android-x86 to harddisk**.
6. Select **Create/Modify partitions** and create a partition on the Android VM’s virtual disk. Write the changes and exit the partition tool.
7. Select the new partition, choose **ext4**, and confirm formatting. This erases the selected partition, so make sure it is the Android VM’s disk.
8. Choose **Yes** to install GRUB and **Yes** when asked to make the `/system` directory writable.
9. Once installation finishes, eject the ISO from the virtual optical drive and reboot.

### Graphics Troubleshooting

If Android’s graphical interface does not appear, shut down the VM and open **Settings → Display → Graphics Controller**. Try **VBoxVGA** and **VMSVGA**, testing each separately. Record which configuration works for your setup.

<img width="686" height="471" alt="image" src="https://github.com/user-attachments/assets/82cba3f0-69bb-4f45-89cf-00c749136d1d" />


## Step 4: Connect Android to the NAT Network

1. Shut down the Android VM.
2. In VirtualBox, select **Android9-Lab → Settings → Network**.
3. Under **Adapter 1**, ensure **Enable Network Adapter** is checked.
4. Set **Attached to** to **NAT Network**.
5. In **Name**, select the same NAT Network used by your Kali Linux VM.
6. Ensure **Cable Connected** is checked, then click **OK**.
7. Start the Android VM again.

Both VMs must use the same named **NATNetwork** so they can communicate with each other. Select **NATNetwork**, not the separate **NAT Network** option.

<img width="728" height="488" alt="image" src="https://github.com/user-attachments/assets/b6abc000-b4d0-4922-ae70-a5df1c060cfd" />

## Step 5: Set a Static IP Address Inside Android

These settings are configured **inside the running Android VM**.

1. Start **Android9-Lab** in VirtualBox and sign in or unlock Android to reach the home screen.
2. Open Android’s **Settings → Network & Internet → Ethernet**. The menu path may vary by build.
3. Open the connection’s IP settings and change **DHCP** to **Static**.
4. Enter the following values:
   - **IP address:** `10.0.0.9`
   - **Subnet mask:** `255.255.255.0`
   - **Network prefix length:** `24` (if requested instead of a subnet mask)
   - **Gateway:** `10.0.0.1`
   - **DNS 1:** `8.8.8.8`
5. Save or apply the settings.

**Troubleshooting note:** I initially forgot to switch from DHCP to Static, so Android kept receiving a different IP address. Make sure **Static** is selected before saving.









