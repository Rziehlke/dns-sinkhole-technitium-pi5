# DNS Sinkhole on Raspberry Pi 5 (Technitium DNS)
Network-wide ad and malware domain blocking using Technitium DNS Server as a sinkhole on a Raspberry Pi 5.

## Overview
Blocks malware, ads, and trackers across entire home network, on every device. Block lists update automatically.

## Hardware & Software
- Raspberry Pi 5 (16GB RAM, storage, OS version)
- Technitium DNS Server (version)
- Network details (router model if relevant, static IP setup)

## Network Diagram
![Network diagram](images/network-diagram.png)

## Setup
Step 1: Download Raspberry Pi Imager on your Windows desktop from raspberrypi.com/software. Insert your microSD card (or USB SSD). Choose Raspberry Pi 5 as the device, Raspberry Pi OS (other) > Raspberry Pi OS Lite (64-bit) as the OS since you don't need a desktop for a headless server, and your card as storage.

![Raspberry Pi OS Lite](images/screenshot-raspberry-pi-imager.png)

Step 2: When Imager asks 'Would you like to apply OS customisation settings?', click Edit Settings. Set a hostname (e.g. dns-pi), a username and strong password, and your locale. On the Services tab, enable SSH with password authentication. This is what lets you go headless from the first boot. Write the image, then screenshot these settings for your repo.

![NRaspberry Pi Imager Customization Settings](images/screenshot-imager-customization.png)

Step 3: Insert the card, connect Ethernet (strongly preferred over Wi-Fi for a DNS server), and power on. Wait about a minute, then from Windows Terminal: ssh username@dns-pi.local (or find the IP in your router's client list). Accept the host key, log in, then run sudo apt update && sudo apt full-upgrade -y.

![SSH](images/screenshot-ssh.png)

Step 4: A DNS server needs a fixed address. Two options: reserve the Pi's IP in your router's DHCP settings (easier, survives OS reinstalls), or set it on the Pi with nmcli. Router reservation is the cleaner choice; note the MAC address with 'ip link' and reserve something like 192.168.1.53. Reboot the Pi Server (sudo reboot) and confirm the Pi comes up at that address.

![DHCP Reservation](images/screenshot-DHCP-reservation.png)

Step 5: Run the official installer: curl -sSL https://download.technitium.com/dns/install.sh | sudo bash. It installs the .NET runtime and Technitium as a systemd service. When it finishes, browse from your desktop to http://192.168.1.53:5380, create the admin account, and screenshot the dashboard for the repo.

![Technitium Install](images/screenshot-technitium-install.png)

Step 6: In the Technitium web console go to Settings > Blocking. Add blocklist URLs; a solid starter set is Hagezi Multi Pro or the OISD Big list, both maintained and low on false positives. Set the blocklist auto-update interval.

![Technitium Block List](images/screenshot-technitium-blocklist.png)

Step 7: Under Settings > Proxy & Forwarders, set upstream resolvers. I used Cloudflare and Quad9 over HTTPS.

![Technitium Forwarders](images/screenshot-technitium-forwarders.png)

Step 8: From your desktop, test before committing the network: nslookup doubleclick.net 192.168.1.53 should return 0.0.0.0 or NXDOMAIN (blocked), while nslookup example.com 192.168.1.53 resolves normally. Check the Technitium dashboard logs to see your queries arriving.

![SSH](images/screenshot-ssh.png)

Step 9: In your router's DHCP settings, set the primary DNS server to the Pi's IP so every device picks it up on next lease renewal. Leave the secondary DNS blank if you want everything sinkholed, or accept that a public secondary lets devices bypass blocking when the Pi is slow. Renew a device's lease, browse for a while, and watch blocked queries climb on the dashboard.

![SSH](images/screenshot-ssh.png)

## Blocklists Used
Which lists, why you chose them, false positive handling.

## Results
Screenshots of the dashboard. Queries blocked per day,
top blocked domains. Numbers make it concrete.

## Problems I Ran Into
1) Accidentally installed the full Raspberry Pi OS on the flash drive, so had to re-install the Lite version. Make sure to select 'Raspberry Pi OS (other)' to access the Lite version using the Raspberry Pi Imager software.

2) Couldn't find the Pi Server on my Google Wifi router app device list, to locate the IP address. Fixed by unplugging Raspberry Pi power cable for 10 seconds and replugging, to reset server and also reset entire network from Google WiFi app.

3) Couldn't open query logs to verify settings in Technitium dashboard. Fixed by installing 'Query Logs (Sqlite)' in app store in the Technitium dashboard.

## What I'd Do Differently
Shows growth. Maybe HA with a second Pi, DoH upstream, etc.
