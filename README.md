# Pfsense-OpenVPN-pfBlockerNG
Pfsense Version 2.7.0-RELEASE, on ESXI 8.0.1 with two vswitch, one for LAN and one for WAN.

Documentation to set up a VPN accessible from the internet with pfBlockerNG enabled.

This a basic testing configuration not intended for production use.

## Home page

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/4e1de2fd-05a8-45a7-8d8b-4fc0a61e1936)


## System > General Setup

Set some DNS servers, here's it's Google and Cloudflare, with the WAN interface for gateway.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/78254d65-5461-4595-868c-22e805595680)


## Packets

Install the needed packets

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/2439828c-c1af-46bd-96b7-337890e1c210)

## OpenVPN server configuration

Create the OpenVPN configuration with the wizard, check the boxes to automatically create the firewall rules at the end, and then set up the last configuration similarly to the one bellow.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/2543296f-e731-4768-809b-d50a9af35ba1)

## Create the user that will connect with the VPN

A certificate must be created for the user, use the same encryption as the one used in the VPN server.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/6a2d478c-6514-4a95-9ac6-3b69ef21dde1)

## Create an interface for the VPN

Just go to Interfaces, Assignements, create the interface and enable it.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/b91c33eb-7704-44b6-848a-e7a754f61d33)

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/1ec6dcbf-f783-4cfe-bdfe-d5a9fd69a588)

## DNS resolver

You need to check all the interfaces with ctrl+click or it won't work, checking only "All" doesn't seem to be enough.

DNS query forwarding is also enabled.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/86487083-95b6-449f-b740-3c256bce71f9)

## pfBlockerNG

Set up the basics with the wizard, make sure the basic rules are downloaded and that the service is enabled.




