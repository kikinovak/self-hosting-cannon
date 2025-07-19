Self-hosting with Docker & Linux
================================

Ubuntu Bare Metal Installation
------------------------------

Hardware: HP ProDesk 600 G2

Download the current LTS version of Ubuntu:

  * https://ubuntu.com/download/server

Create a bootable flash drive:

```console
# dd if=ubuntu-24.04.2-live-server-amd64.iso of=/dev/sdX
```

Insert the USB drive and press `Esc` > `Boot Menu` > `UEFI` > `Try or Install
Ubuntu Server`.

- Language: **English**

- Keyboard layout: **Switzerland - French**

- Type of installation: **Ubuntu Server**

- Storage configuration: **Use an entire disk**

- Set up this disk as an LVM group: **no**

- Your name: **OS Admin**

- Your server's name: **dockerhost**

- Pick a username: **osadmin**

- Password: **Self-Hosted!**

- Install OpenSSH server: **yes**

- Reboot

Log into the newly installed system:

```console
$ ssh-copy-id -i /home/kikinovak/.ssh/id_rsa.pub osadmin@dockerhost
$ ssh osadmin@dockerhost
```

