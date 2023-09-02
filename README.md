# Pfsense-OpenVPN-pfBlockerNG
Pfsense Version 2.7.0-RELEASE, on ESXI 8.0.1 with two vswitch, one for LAN and one for WAN.

Documentation to set up a VPN accessible from the internet with pfBlockerNG-devel enabled.

This a basic testing configuration not intended for production use.

- [Home page](#Home-page)
- [System > General Setup](#System--General-Setup)
- [Packets](#Packets)
- [OpenVPN server configuration](#OpenVPN-server-configuration)
- [Create the user that will connect with the VPN](#Create-the-user-that-will-connect-with-the-VPN)
- [Create an interface for the VPN](#Create-an-interface-for-the-VPN)
- [DNS resolver](#DNS-resolver)
- [pfBlockerNG](#pfBlockerNG)
  - [IP configurations](#IP-configurations)
  - [DNSBL configurations](#DNSBL-configurations)
- [Reboot](#Reboot)
- [OpenVPN / Client Export Utility](#OpenVPN--Client-Export-Utility)
  - [Editing the .ovpn config file](#Editing-the-.ovpn-config-file)
- [pfBlockerNG-devel testing addresses](#pfBlockerNG-devel-testing-addresses)

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

### Wizard
Type of Server: Local User Access
Generate new certificates
Set the IPv4 Local Network to the network you want the VPN to access (either only the LAN side of the Pfsense firewall or also the WAN side).
Check Redirect IPv4 Gateway for pfBlockerNG to work with the VPN later (disables split tunneling).

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/34adc30d-909c-4c17-b74a-d6a601ad6749)

Check that everyting is set up properly / set up a few more options such as Block Outside DNS and Force DNS cache update

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/2543296f-e731-4768-809b-d50a9af35ba1)

## Create the user that will connect with the VPN

A certificate must be created for the user, use the same encryption as the one used in the VPN server.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/c61ddb41-a2cb-40b1-9ee8-4702720203c1)

## Create an interface for the VPN

Just go to Interfaces, Assignements, create the interface and enable it.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/b91c33eb-7704-44b6-848a-e7a754f61d33)

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/1ec6dcbf-f783-4cfe-bdfe-d5a9fd69a588)

## DNS resolver

You need to check all the interfaces with ctrl+click or it won't work, checking only "All" doesn't seem to be enough.

DNS query forwarding is also enabled.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/86487083-95b6-449f-b740-3c256bce71f9)

## pfBlockerNG

Set up the basics with the wizard, make sure the defaults rules/feeds are downloaded and that the service is enabled.

### IP configurations

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/1b65b04f-40e5-46d9-adab-a7127b5a09b2)

Check WAN for the Inbound Firewall Rules and everything else for the Outbound Firewall Rules (ctrl+click).

### DNSBL configurations

LAN as Web Server Interface
Permit Firewall Rules enabled, ctrl+click on the interfaces

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/a52ce528-7529-4809-94dd-56ba400f2547)

## Reboot

It's better to reboot the firewall to apply the changes at this point.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/584d9483-1b11-4101-9ce6-91a38c1b5ab2)


## OpenVPN / Client Export Utility

Export the VPN configuration with the Client Export Utility.
You can check Block Outside DNS to avoid issues.

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/3b4f96bf-5ed4-4912-9752-3aa5564a362f)

### Editing the .ovpn config file

Just download the "Most Clients" inline configuration
![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/5cf79f6d-ad67-4586-9ce7-b315c64920e6)

The IP address will be set to your WAN interface, if you want to use the VPN over the internet, set you public ip address instead of the local ipv4 by directly editing the .ovpn file with a notepad.
The config is set to use the VPN over the internet and not locally, so the DNS resolution will have issues if you try to use it locally. If you want a local VPN set up an other server.
You'd have to also open the ports in your router (default 1194) to the WAN interface of the firewall.
If you use ESXI, using promiscious mode might resolve routing issues.
Firewall rules are left by default in this example.

## pfBlockerNG-devel testing addresses

Some addresses to test that pfBlockerNG-devel is working properly

www.google-analytics.com	StevenBlack_ADs
DNSBL_ADs_Basic

adservice.google.com	StevenBlack_ADs
DNSBL_ADs_Basic

tags.tiqcdn.com	StevenBlack_ADs
DNSBL_ADs_Basic

incoming.telemetry.mozilla.org	StevenBlack_ADs
DNSBL_ADs_Basic

Accessing one of these addresses will redirect you to this page:

![image](https://github.com/EMRD95/Pfsense-OpenVPN-pfBlockerNG/assets/114953576/de62e918-f47b-4ffd-99c4-41b3fd52811a)

pfBlockerNG doesn't MITM the connection so you might have an invalid certificate warning in your browser, there's no quick fix to this.
