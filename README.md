# valheim-server
This is a WIP collection of things needed to host a Valheim server

## Initial thoughts
My goal is to host a Valheim server on my Ubuntu 24 home server. I understand that SteamCMD makes that pretty easy, so this repo will mostly be a collection of notes and config (so that I can re-discover how it works after a few months have passed and I've forgotten everything).
**Note - this will install everything directly on your machine under your active user.** Docker images for hosting Valheim exist and might be more suitable for your needs.

## SteamCMD installation

Per [this article](https://pimylifeup.com/linux-steamcmd/)
```bash
sudo apt update && \
sudo apt upgrade -y && \
sudo apt install software-properties-common && \
sudo dpkg --add-architecture i386 && \
sudo apt-add-repository multiverse && \
sudo apt update && \
sudo apt install steamcmd
```

Test it out with `steamcmd`

## Valheim server installation

### Create a User for the Server
```bash
sudo useradd -m valheim
sudo passwd valheim
sudo -u valheim -s
cd ~
```

### Install the Valheim Server
```bash
/usr/games/steamcmd +@sSteamCmdForcePlatformType linux +force_install_dir /home/valheim/valheimserver +login anonymous +app_update 896660 +quit
```

### Set up the Server as a Service
Exit the `valheim` user session so that your service files are managed by your standard user.
Alternatively, make the `valheim` user a sudoer.
```bash
exit
```

Create a service file under the standard user:
```bash
sudo nano /etc/systemd/system/valheimserver.service
```

Add the contents from `valheimserver.service` file in this repository. Replace servername, worldname, password, and ispublic (0, 1) values as needed.

### Start and Test the Server
Enable and start the service:
```bash
sudo systemctl enable valheimserver.service
sudo systemctl start valheimserver.service
```

Check the server status:
```bash
sudo systemctl status valheimserver.service
```

### Additional Configuration
- Ensure port `2456` is open on your firewall.
- Consider port forwarding if on a home network.

### Open Port 2456 on Ubuntu Firewall

To ensure that port `2456` is open on your Ubuntu server, follow these steps:

1. **Enable UFW:**

   If `ufw` is not already active, enable it with:
   ```bash
   sudo ufw enable
   ```

   Note: Enabling `ufw` may disrupt existing SSH connections. Proceed with caution.

2. **Allow Port 2456:**

   Add a rule to allow traffic on port `2456`:
   ```bash
   sudo ufw allow 2456
   ```

3. **Verify the Rule:**

   Check the status to ensure the rule has been added:
   ```bash
   sudo ufw status
   ```

   You should see entries indicating that port `2456` is allowed for both IPv4 and IPv6.

By following these steps, you ensure that your Valheim server can communicate through port `2456`.