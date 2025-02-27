# IP Shuffle

[![Project Tracker](https://img.shields.io/badge/repo%20status-Project%20Tracker-lightgrey)](https://wiki.hthompson.dev/en/project-tracker)

IP Shuffle is a mid-term project for my Cyber Defense class at EWU. The project instructions was to implement a Moving Target Defense (MTD) technique, to which Chelsea Edwards and I chose IP Shuffle. This MTD is a technique that changes the IP address of a machine every few minutes to make it harder for an adversary to target the machine.

In this project, we have three virtual machines that will change their IP addresses every 3 minutes, and an OPNsense firewall that will route traffic between the machines and the internet. A single script is placed on each machine, called `ip-shuffle`, that performs the IP address change. A cron job is then set up to run this script every 3 minutes.

For a more in depth look and understanding of the project, please refer to the [IP Shuffle](/IP%20Shuffle.pdf) PDF. Here we explain the threat model, system design, and evaluation of the technique.

## Setup Guide

If you are interested in observing the IP Shuffle technique in action within our tested environment outlined in the PDF, you can use the provided OVA images. These images are pre-configured with the necessary software and scripts to run IP Shuffle. All that you need to do is import the images into VirtualBox, configure the network settings, and modify the cron job to run the `ip-shuffle` script every 3 minutes.

### OVA Images

There are four OVA images provided that you will need to download and import:

- [Computer 1 OVA](https://vms3.sfo3.cdn.digitaloceanspaces.com/ova/ip-shuffle/Computer%201.ova)
- [Computer 2 OVA](https://vms3.sfo3.cdn.digitaloceanspaces.com/ova/ip-shuffle/Computer%202.ova)
- [Computer 3 OVA](https://vms3.sfo3.cdn.digitaloceanspaces.com/ova/ip-shuffle/Computer%203.ova)
- [OPNsense OVA](https://vms3.sfo3.cdn.digitaloceanspaces.com/ova/ip-shuffle/OPNsense.ova)

### Network Configuration

Ensure that each machine is configured as follows:

- The network adapter should be connected to the "Internal Lan" with the name `InternalLAN`.

For the OPNsense image, two interfaces should be configured:

- **Adapter 1:** Connect to "Internal Lan" with the name `InternalLAN`.
- **Adapter 2:** Connect to the network interface used for internet access.

This setup ensures that:

1. All machines on `InternalLAN` receive an IP address and can communicate with each other.
2. Each machine can access the internet.

### Cron Job Configuration

If you're using the images provided, setting up IP Shuffle is straightforward. On the images labeled as "Computer 1," "Computer 2," and "Computer 3," follow these steps:

1. Execute `sudo crontab -e`.
2. Uncomment the line that looks like `#*/3 * * * * /usr/local/sbin/ip-shuffle`.
3. Save and exit the file.

This will configure the cron job to run every 3 minutes.

### Monitoring IP Shuffle Output

Each of these machines has a script located in `/root/` that will monitor the IP Shuffle output. You can see the logs by running the following command:

```bash
sudo ./execute-me.sh
```

This script will execute

```bash
tail -f /var/log/ip-shuffle.log | ccze
```

to provide live updates.

### User Credentials

For each machine, including OPNsense, you can use the following credentials:

- **root**
  - Password: `password`
- **vbox**
  - Password: `password`
  - The `vbox` user has sudo privileges, allowing commands to be executed as root.
  - OPNsense does not include the `vbox` user.
