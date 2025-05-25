# Opnsense Dnsmasq Setup

## Step 1: Just Dynamic DHCP, No DNS changes, No Static IP Researvation
This assume that you have DNS service runnning on Opnsense (eg, port 53)

1. Disable existing ISC / Kea DHCP
   * Services --> ISC DHCPv4 --> only uncheck Enable, then apply. No other changes needed.
   * Repeat for ISC DHCPv6 / Kea DHCPv4 / Kea DHCPv6

2. Enable Dnsmasq DHCP
   Set DNS port to any unused port or set it to 0 to disable it entirely
![image](https://github.com/user-attachments/assets/bffec432-86ad-4c23-8812-e5c2a3ee5100)

3. Setup DHCP Address Range for each Interface
   Click Add --> Select Interface --> Fill up Start Address & End Address
![image](https://github.com/user-attachments/assets/2925b378-503f-4764-846b-ab080e4b053d)

4. Check Firewall
Makesure files for port 67. 68 is there
![image](https://github.com/user-attachments/assets/0136cf95-aff7-4a9e-af6e-4955db5ee0c7)

6. 
