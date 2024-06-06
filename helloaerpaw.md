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

