# Real VNC

First things first, activate VNC from Raspy-config.

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

\


