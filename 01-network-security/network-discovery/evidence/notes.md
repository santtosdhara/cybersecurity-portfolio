### Network Interface Investiation 

Tool and purpose:
- ifconfig: Inspect interface, assigned IP addresses, subnet masks, and link status.
- rount -n get default: Identify the default IPv4 gateway and associated interface.
- ping -c 4 <destination>: Test whether a selected destination responds to ICMP echo requests.

Observations: 
- end1 reported an active link.
- en0 and en 3 were present but reported inactive.
- en1 had a private address in 192.168.x.x.
- The subnet mask was 0xffffff00, equivalent to 255.255.255.0 or /24.
- en1 also had a link-local IPv6 address and global IPv6 addresses. 
- The default IPv4 route used en1.
The default gateway belonged to the workstation's IPv4 subnet. 
- Pinging the gateway returned four replies from four requests. 
- Pinging the external address 2.2.2.2 also returned four replies from four requests.


Initial interpretation:
- The workstation's IPv4 subnet is 192.168.1.0/24.
- Under this mask, addresses with the same first three groups belong to the same IPv4 subnet.
- The default route provides a next hop when no more specific route mathces a destination.
- The workstation could exchange ICMP with its gateway and one external destination during testing.
- IPv6 configured and must also be cosidered in future investigation.

Limits:

- Successful pings do not prove that DNS, websites, or every internet service works.
- An IP address or an acive connection alone does not establish whether activity is safe or malicious. 
- I have not yet identifies which applications are communicating, their destinations, or their ports. 

