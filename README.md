# DNS Sinkhole on Raspberry Pi 5 (Technitium DNS)
Network-wide ad and malware domain blocking using Technitium DNS Server as a sinkhole on a Raspberry Pi 5.

## Overview
Blocks malware as well as ads and trackers across entire home network, on every device. Block lists update automatically.

## Hardware & Software
- Raspberry Pi 5 (16GB RAM, storage, OS version)
- Technitium DNS Server (version)
- Network details (router model if relevant, static IP setup)

## Network Diagram
![Network diagram](images/network-diagram.png)

## Setup
The actual steps you took: OS install, static IP config,
Technitium installation, blocklist configuration, pointing
DHCP at the Pi. Include real commands in code blocks.

## Blocklists Used
Which lists, why you chose them, false positive handling.

## Results
Screenshots of the dashboard. Queries blocked per day,
top blocked domains. Numbers make it concrete.

## Problems I Ran Into
The most valuable section. What broke, how you diagnosed it,
how you fixed it.

## What I'd Do Differently
Shows growth. Maybe HA with a second Pi, DoH upstream, etc.
