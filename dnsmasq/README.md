# Opnsense Dnsmasq Setup

## Just Dynamic DHCP, No DNS changes, No Static IP Reservation
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

4. Check Firewall port 67 & 68

Makesure rules for port 67. 68 is there
![image](https://github.com/user-attachments/assets/0136cf95-aff7-4a9e-af6e-4955db5ee0c7)

5. Check Interface to **not** Block Private Networks & bogon networks
![image](https://github.com/user-attachments/assets/60c58d24-970e-446b-8e39-03fdd83a91ef)

6. Apply (and optionally hit restart)

7. Now DHCP should work now
   
8. Check Dnsmasq --> Logs --> Level = Debug
![image](https://github.com/user-attachments/assets/3c8a13a3-6090-45d8-a1ed-b64789691d16)

