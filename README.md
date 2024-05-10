# IP Shuffle Guide

If you're using the images provided, setting up IP Shuffle is straightforward. On the images labeled as "Computer 1," "Computer 2," and "Computer 3," follow these steps:

1. Execute `sudo crontab -e`.
2. Uncomment the line that looks like `#*/3 * * * * /usr/local/sbin/ip-shuffle`.
3. Save and exit the file.

This will configure the cron job to run every 3 minutes.

## Monitoring IP Shuffle Output

Each of these machines has a script located in `/root/` that will monitor the IP Shuffle output. You can see the logs by running the following command:

```bash
sudo ./execute-me.sh
```

This script will execute:

```bash
tail -f /var/log/ip-shuffle.log | ccze
```

to provide live updates.

## User Credentials

For each machine, including OPNsense, you can use the following credentials:

- **root**
  - Password: `password`
- **vbox**
  - Password: `password`
  - The `vbox` user has sudo privileges, allowing commands to be executed as root.

## Network Configuration

Ensure that each machine is configured as follows:

- The network adapter should be connected to the "Internal Lan" with the name `InternalLAN`.

For the OPNsense image, two interfaces should be configured:

- **Adapter 1:** Connect to "Internal Lan" with the name `InternalLAN`.
- **Adapter 2:** Connect to the network used for internet access.

This setup ensures that:

1. All machines on `InternalLAN` receive an IP address and can communicate with each other.
2. Each machine can access the internet.
