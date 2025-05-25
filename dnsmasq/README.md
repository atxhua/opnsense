# Opnsense Dnsmasq Setup
## TOC
1. [Just Dynamic DHCP, No DNS changes, No Static IP Reservation](#just-dynamic-dhcp-no-dns-changes-no-static-ip-reservation)
2. [Static IPV4 Reservation](#Static-IPV4-Reservation)
3. [Using DNSMASQ as main DNS and Forward Query to Unbound](#Using-DNSMASQ-as-main-DNS-and-Forward-Query-to-Unbound)
    
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

7. DHCP should work now
No static reservation just yet
   
8. Check Dnsmasq --> Logs --> Level = Debug

![image](https://github.com/user-attachments/assets/3c8a13a3-6090-45d8-a1ed-b64789691d16)


## Static IPV4 Reservation
1. Create a hosts.csv file with the following content

```
host,ip,hwaddr
device-a,192.168.1.11,"aa:bb:cc:dd:ee:11"
device-b,192.168.2.22,"aa:bb:cc:dd:02:22,aa:bb:cc:dd:ee:33"
device-b,192.168.3.22,"aa:bb:cc:dd:02:22,aa:bb:cc:dd:ee:33"
```

2. Semi Automate export ISC Static Mapping to hosts.csv
    * System --> Configuration --> Backup --> Download Configuration

![image](https://github.com/user-attachments/assets/17cdb83c-fa9b-4946-9698-bc81f733d68f)

    * Get all `<staticmap></staticmap>` section in each interfaces

![image](https://github.com/user-attachments/assets/d09ec6c2-607d-4270-be34-d0126fa11a8e)

    * Copy and paste to any AI engine to convert to the csv for you.

    
3. Import hosts.csv file

Hosts --> Import

![image](https://github.com/user-attachments/assets/390c042a-a609-4e08-a734-fc23823b3bb7)
![image](https://github.com/user-attachments/assets/4dff2bd0-3e91-4882-9b35-ea46cfd1046b)

Select the hosts.csv file, then hit the tick button

![image](https://github.com/user-attachments/assets/c3e3249c-1697-47ea-8087-c806045e1374)

4. After Import

![image](https://github.com/user-attachments/assets/145d3202-ce8c-4b90-9e39-fa449acfaa8e)

5. No duplicate IP Address

    * You can have same mac address in different line (e.g. using same mac address connect to different subnet)
    * You *can not* have same IP Address appear in multiple different line
  
## Using DNSMASQ as main DNS and Forward Query to Unbound 
1. Change Unbound DNS to other port

![image](https://github.com/user-attachments/assets/32f9b7db-80eb-4703-8d6b-5a1f369111d1)


2. Set Dnsmasq DNS port to 53

Service --> Dnsmasq --> General --> DNS Port = 53

![image](https://github.com/user-attachments/assets/abbd264e-279d-4d1f-b7ae-708a0e604f23)

3. Dnsmasq Forward Query to Unbound

Service --> Dnsmasq --> Domains --> Add

```
Domain = *
IP address = 127.0.0.1
Port = 5053 ( or what ever your Unbound Port is on)
```

![image](https://github.com/user-attachments/assets/151d5976-c06b-4209-9589-f85f1f281785)

