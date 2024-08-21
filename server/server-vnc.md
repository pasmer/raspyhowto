# Server VNC

## Real VNC

First things first, activate VNC from Raspy-config.

<mark style="color:red;">UPDATE: Real VNC is not still free of charge.</mark>

#### Disable wayvnc

1. Open Terminal on the Pi, or connect to it using SSH
2. Run the command: `sudo raspi-config`
3. Select Advanced Options, then select Wayland
4. Select X11 and confirm
5. Reboot the Pi when prompted

#### Install Real VNC

```
Install
sudo apt-get install realvnc-vnc-server
-----

check:

sudo service vncserver-x11-serviced status

-----------------


Avvio
    To start VNC Server now: sudo systemctl start vncserver-x11-serviced.service
    To start VNC Server at next boot, and every subsequent boot: sudo systemctl enable vncserver-x11-serviced.service
    To stop VNC Server: sudo systemctl stop vncserver-x11-serviced.service
    To prevent VNC Server starting at boot: sudo systemctl disable vncserver-x11-serviced.service
```



## TightVNC

As alternative to RealVNC, you can use TightVNC for Raspberry Pi.

### How to install

As the `tightvncserver` package is available through the Raspbian repository, all we need to do is run the following command.

```bash
sudo apt install tightvncserver
```

**1.** With the VNC server now installed to our Pi, let us now proceed to configure it.

To start the configuration process, we need to run the command below.

```bash
vncserver :1
```

**2.** You will now need to set up a password for when you connect to the Raspberry Pi remotely via a VNC client.

Make sure the password that you set is no longer than 8 characters long. Any password that is longer than 8 characters will be truncated (Meaning any letters after the eighth will be removed).

```bash
You will require a password to access your desktops.

Password:
```

As this password will give someone access to your Raspberry Pi’s desktop, make sure you set this to something secure.

**3.** You will next be asked to re-enter the password again to verify it.

```bash
Verify:
```

**4.** Next, you will be asked if you want to set up a separate view-only password. Press **N**.



### Creating the service for VNC <a href="#creating-the-service-for-vnc" id="creating-the-service-for-vnc"></a>

As the VNC server software we are using does not have a systemctl compatible service, we will need to write our own.

Luckily for us, the process of writing a service file is a very straightforward one.

**1.** To get our VNC software to start on boot, we will need to write a service file for it on our Raspberry Pi.

To begin creating our service, run the following command.

```bash
sudo nano /etc/systemd/system/vncserver.service
```

We will be calling this service `vncserver.service`.

**2.** Within this file, we need to enter the following lines.

Ensure you replace “`<USER>`” with your username. For example, pi, for Raspberry Pi.

```systemd
[Unit]
Description=TightVNC remote desktop server
After=network.target
 
[Service]
User=<USER>
Type=forking
ExecStart=/usr/bin/vncserver :1
ExecStop=/usr/bin/vncserver -kill :1
 
[Install]
WantedBy=multi-user.target
```

These lines tell the service manager how to handle the VNC server on our Raspberry Pi.

### Starting the VNC Server on the Raspberry Pi <a href="#starting-the-vnc-server-on-the-raspberry-pi" id="starting-the-vnc-server-on-the-raspberry-pi"></a>

**4.** We can now test to see if our new VNC service is working by running the following command to start it.

```bash
sudo systemctl start vncserver
```

If the service says it’s failed, try checking the status of it to find out where.

### Telling the VNC Server to Start at Boot <a href="#telling-the-vnc-server-to-start-at-boot" id="telling-the-vnc-server-to-start-at-boot"></a>

**6.** Once we have verified that the VNC service is now functioning, we can tell it to start at boot.

To get our new service to start at boot is to enable the service by using the command below.

```bash
sudo systemctl enable vncserver
```



### How to solve the Copy-Paste issue

The copy-and-paste may not work on TightVNC. You need to install **autocutsel**.

To enable copy and paste functionality between the host and TightVNC server, you can use a program called autocutsel. This tool helps to synchronize the clipboard between VNC server and client. Here’s how you can configure it on your Raspberry Pi: Install autocutsel: First of all, you need to install autocutsel on your Raspberry Pi. Open the terminal and type the following command:

```
sudo apt-get install autocutsel
```

Configure autocutsel: After installation, you must configure autocutsel to run every time you start the VNC server. Edit the VNC xstartup file that is in the directory. vnc in your home directory:

```
nano ~/.vnc/xstartup
```

Add autocutsel to xstartup file: Add the following line to xstartup file to start autocutsel automatically.

The xstartup file might look like this:

```
#!/bin/sh
xrdb $HOME/.Xresources
#startlxde &
xsetroot -solid grey
autocutsel -fork
# startlxde &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#x-window-manager &
# Fix to make GNOME work
export XKL_XMODMAP_DISABLE=1
/etc/X11/Xsession
```

#### Source:

{% embed url="https://pimylifeup.com/raspberry-pi-vnc-server/" %}

