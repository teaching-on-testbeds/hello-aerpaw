AERPAW is the first wireless research platform envisioned and built to allow studying the convergence of advanced wireless technologies (such as 5G) and autonomous drones. <br><br>
In this tutorial we will learn how to use AERPAW to run experiments on a Virtual Environment. Installation of Q ground control and Open VPN control is required and guided process is mentioned. <br>
>[!NOTE] 
>This process has a “human in the loop” approval stage - students will need to wait for an instructor or research advisor to approve their request to join their project. They should be prepared to start the tutorial, wait for this approval, and then continue.
### Preparing Your Workstation <br>
#### QGroundControl <br>
##### Windows <br>
QGroundControl can be installed on 64 bit versions of Windows: <br>
Download [QGroundControl-installer.exe](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl-installer.exe) <br>
Double click the executable to launch the installer. <br> <br>
The Windows installer creates 3 shortcuts: QGroundControl, GPU Compatibility Mode, GPU Safe Mode. Use the first shortcut unless you experience startup or video rendering issues. For more information see [Troubleshooting QGC Setup > Windows: UI Rendering/Video Driver Issues.](https://docs.qgroundcontrol.com/Stable_V4.3/en/qgc-user-guide/troubleshooting/qgc_setup.html#opengl_troubleshooting) <br>
Prebuilt QGroundControl versions from 4.0 onwards are 64-bit only. It is possible to manually build 32 bit versions (this is not supported by the dev team). <br> <br>
##### MacOS <br>
Download [QGroundControl.dmg](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.dmg). <br>
Double-click the .dmg file to mount it, then drag the QGroundControl application to your Application folder.<br> <br>
QGroundControl continues to not be signed which causes problem on Catalina. To open QGC app for the first time: <br>
Right-click the QGC app icon, select Open from the menu. You will only be presented with an option to Cancel. Select Cancel. <br>
Right-click the QGC app icon again, Open from the menu. This time you will be presented with the option to Open. <br> <br>

##### Ubuntu Linux <br>
QGroundControl can be installed/run on Ubuntu LTS 20.04 (and later). <br> <br>

Ubuntu comes with a serial modem manager that interferes with any robotics related use of a serial port (or USB serial). Before installing QGroundControl you should remove the modem manager and grant yourself permissions to access the serial port. You also need to install GStreamer in order to support video streaming. <br> <br>

Before installing QGroundControl for the first time: <br>
On the command prompt enter:
>[!TIP]
>sudo usermod -a -G dialout $USER <br>
sudo apt-get remove modemmanager -y <br>
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y <br>
sudo apt install libqt5gui5 -y <br>
sudo apt install libfuse2 -y <br>

Logout and login again to enable the change to user permissions. 
  To install QGroundControl:

Download [QGroundControl.AppImage](https://d176tv9ibo4jno.cloudfront.net/latest/QGroundControl.AppImage). <br>
Install (and run) using the terminal commands: <br>
>[!TIP]
>chmod +x ./QGroundControl.AppImage
./QGroundControl.AppImage  (or double click)

#### QGroundControl <br>
##### On Linux: <br>
Use openvpn.  Linux is by far the recommended platform from which to access AERPAW. In our experience, the VPN connection from Linux is stable. Make sure to use openvpn Version 2. openvpn Version 3 will NOT work

Use _sudo apt-get install openvpn_ if you do not already have it installed.  <br> 
For running the software provide the OVPN profile file you have saved above. Run the following at a command prompt (e.g. terminal window): <br>
_sudo openvpn --config aerpaw_exp.ovpn_ <br>
This creates a new logical network interface, likely named tap0 or similar, in your OS.  You can check on this with:
_ifconfig -a_ <br>
The interface is created in a DOWN state, and Linux does not automatically request a Layer-3 address on it.  This requires two further commands to be executed: <br>
_sudo ifconfig tap0 up_ <br>
_sudo dhclient tap0 -v_ <br>

##### On Mac OS: <br>
Use Tunnelblick.  This free software can be downloaded from <br>[https://tunnelblick.net/downloads.html](https://tunnelblick.net/downloads.html) ; we have tested connectivity using the 3.8.6a stable build, on MacOS 10.15.7 .   Follow the instructions provided in the Tunnelblick documentation to provide Tunnelblick with the _aerpaw_exp.ovpn_ file.  
>[!Note]
> in our testing, we find that after Tunnelblick establishes the connection, and it turns green, there is nevertheless a delay of several seconds before an IP address is assigned to the tap interface, and the interface becomes usable.
>The OpenVPN Connect software on MacOS will NOT work - it does not support Layer-2 VPNs. <br> <br>
##### On Windows: <br>
OpenVPN client for Windows should work fine. Dowload from here  https://openvpn.net/community-downloads/ .<br>
This should create an icon in the system tray for OpenVPN.  The openvpn profile file you received as part of the Manifest can be imported by selecting the menu item "Import -> Import file".  After the file is imported click "connect" and this should create a new logical network interface in your OS with IP address 192.168.108.xxx for the example provided above.   <br>
If OpenVPN does not work, you can also try Windows Subsystem for Linux (WSL), a compatibility layer that enables Linux binary executables to be run on a Windows OS.  You must install WSL 2 (WSL 1 provides a thinner compatibility, which will not suffice). <br>

#### Exercise: Create SSH keys <br>
Once you are part of a project with an active allocation, you can set up SSH keys.

Note: If you already have an SSH key pair, you can use it with AERPAW - copy the contents of the public key, then skip to the “Profile: Upload SSH keys to AERPAW” section and continue there. If you don’t already have an SSH key pair, continue with the rest of this section. <br><br>

AERPAW users access resources using public key authentication. Using SSH public-key authentication to connect to a remote system is a more secure alternative to logging in with an account password. <br><br>

SSH public-key authentication uses a pair of separate keys (i.e., a key pair): one “private” key, which you keep a secret, and the other “public”. A key pair has a special property: any message that is encrypted with your private key can only be decrypted with your public key, and any message that is encrypted with your public key can only be decrypted with your private key. <br><br>

This property can be exploited for authenticating login to a remote machine. First, you upload the public key to a special location on the remote machine. Then, when you want to log in to the machine: <br><br>

You use a special argument with your SSH command to let your SSH application know that you are going to use a key, and the location of your private key. If the private key is protected by a passphrase, you may be prompted to enter the passphrase (this is not a password for the remote machine, though).<br><br>
The machine you are logging in to will ask your SSH client to “prove” that it owns the (secret) private key that matches an authorized public key. To do this, the machine will send a random message to you.<br><br>
Your SSH client will encrypt the random message with the private key and send it back to the remote machine.
The remote machine will decrypt the message with your public key. If the decrypted message matches the message it sent you, it has “proof” that you are in possession of the private key for that key pair, and will grant you access (without using an account password on the remote machine.)<br>
(Of course, this relies on you keeping your private key a secret.)<br><br>

We’re going to generate a key pair on our laptop, then upload it to the AERPAW sites we are likely to use.<br>

Open a terminal, and generate a key named id_rsa_aerpaw:<br>

ssh-keygen -t rsa -f ~/.ssh/id_rsa_aerpaw<br>
Follow the prompts to generate and save the key pair. The output should look something like this: <br><br>

$ ssh-keygen -t rsa<br>
Generating public/private rsa key pair.<br>
Enter file in which to save the key (/users/ffund01/.ssh/id_rsa_aerpaw): <br>
Enter passphrase (empty for no passphrase): <br>
Enter same passphrase again: <br>
Your identification has been saved in /users/ffund01/.ssh/id_rsa_aerpaw.<br>
Your public key has been saved in /users/ffund01/.ssh/id_rsa_aerpaw.pub.<br>
The key fingerprint is:<br>
SHA256:z1W/psy05g1kyOTL37HzYimECvOtzYdtZcK+8jEGirA ffund01@example.com<br>
The key's randomart image is:<br>
+---[RSA 2048]----+<br>
|                 |<br>
|                 |<br>
|           .  .  |<br>
|          + .. . |<br>
|    .   S .*.o  .|<br>
|     oo. +ooB o .|<br>
|    E .+.ooB+* = |<br>
|        oo+.@+@\.\o|<br>
|        ..o==@ =+|<br>
+----[SHA256]-----+<br>
If you use a passphrase, make a note of it somewhere safe! (You don’t have to use a passphrase, though - feel free to leave that empty for no passphrase.)<br>

