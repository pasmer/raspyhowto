# ZRAM

### How to install ZRAM on Raspberry Pi

From the terminal:

```
sudo apt update
sudo apt full-upgrade

git clone https://github.com/foundObjects/zram-swap.git

cd zram-swap
sudo ./install.sh
```

To check if everything is fine:

```
sudo cat /proc/swaps
```

Once installed, here are the lines you’ll want to add at the end of your **/etc/sysctl.conf** file:

```
vm.vfs_cache_pressure=500
vm.swappiness=100
vm.dirty_background_ratio=1
vm.dirty_ratio=50
```

Then **reboot**, or enable with:

```
sudo sysctl --system
```

Ref:

{% embed url="https://linuxhint.com/install-zram-raspberry-pi/" %}

{% embed url="https://linuxblog.io/raspberry-pi-performance-add-zram-kernel-parameters/" %}
