**Hello!**

Welcome to Nathan (Pang)'s guide to firewalls and backends on Archlinux.

Firewalls are meant to protect your computer by limiting data that goes into your computer while allowing data to be sent from your computer.

**Updating Linux**

Before doing anything, we must first ensure our Linux distribution is up to date. To update Archlinux, use the following command:

    `sudo pacman -Syu`

Using *-Syu* will also update your kernel. To ensure kernel updates are applied, reboot your Linux. Use the following command to do so:

    `sudo reboot`

**Installing a Firewall**

The firewall we will be using is ufw. Ufw stands for Uncomplicated Firewall. To install ufw, use the following command:

    `sudo pacman -S ufw`

We can use the following code to check the status of our ufw:

    `sudo ufw s~tatus verbose`

It should return inactive. We haven't turned it on yet.

**Configuring the Firewall**

Now that we have our firewall, we need to configure what the firewall will actually do. We can choose what our firewall will deny and allow. The following command are common settings, as they allow for a decent amount of protection:

    `sudo ufw default deny incoming
    sudo ufw default allow outgoing`
