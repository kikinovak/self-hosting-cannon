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

Update the system:

```console
$ sudo apt update
$ sudo apt upgrade -y
$ sudo reboot
```


Install Tailscale on Ubuntu
---------------------------

Tailscale creates a secure private mesh network that allows you to access your
self-hosted services from anywhere without exposing them to the public
Internet.

Before you install Tailscale on your system you'll need a Tailscale account:

  * https://tailscale.com

> Choose the *Sign up with GitHub* login method.

Install Tailscale on the Docker host:

```console
$ curl -fsSL https://tailscale.com/install.sh | sh
Installing Tailscale for ubuntu noble, using method apt
+ sudo mkdir -p --mode=0755 /usr/share/keyrings
[sudo] password for osadmin: **********
...
Installation complete! Log in to start using Tailscale by running:

sudo tailscale up
```

Start Tailscale:

```console
$ sudo tailscale up

To authenticate, visit:

        https://login.tailscale.com/a/148b87b701b839
```

Copy that link, open it in a web browser and sign in.

Tailscale lists the devices on a page called *Machines*. We currently only have
one device connected to our tailnet: `dockerhost`

Disable key expiry for the Docker host: click the `...` menu and then **Disable
key expiry**.


Install Tailscale on RHEL 9
---------------------------

Add the Tailscale repository:

```console
# dnf config-manager --add-repo https://pkgs.tailscale.com/stable/rhel/9/tailscale.repo
```

Install Tailscale:

```console
# dnf install tailscale
```

Enable and start the service:

```console
# systemctl enable tailscaled --now
```

Connect your machine to your Tailscale network and authenticate in your
browser:

```console
# tailscale up
```

Find your Tailscale IPv4 address:

```console
# tailscale ip -4
```

> If the device you added is a server or remotely-accessed device, you may want
> to consider disabling key expiry to prevent the need to periodically
> re-authenticate.

