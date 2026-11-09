### NET-001 - Network Interface Investigation

1. Investigation Objective

The objective of this investigation was to identify the workstation's network interface, examine its IP configuration, determine its default IPv4 route, and verify basic connectivity with local and external destinations.

As part of an initial security assessment, understanding how an endpoint connects to a network is important for identifying it's expected communication paths and establishing a foundation for future traffic monitoring.

# Lab Environment #

- Workstation: Mac mini M4
- Operating system: macOS
- Environments: Home network
- Investigation type: Network discovery and initial connectivity assessment

2. Tools and Methodology

The following native macOS commands were used to collect information without modifying the network configuration.

Command                         Purpose
ifconfig                        Inspect network interfaces, assigned IP addresses, subnet masks, and interface status
route -n get default            Identify the default IPv4 gateway and associated outgoing interface.
route -n get 2.2.2.2            Inspect the route selected for a specific external IPv4 destination.
ping -c 4 <gateway-ip>          Test ICMP connectivity with the local default gateway.
ping -c 4 2.2.2.2               Test ICMP connectivity with an external IPv4 destination.

3. Observations and Evidence

3.1 Network Interface Discovery

the ifconfig output revealed multiple network interfaces.

- en1 reported an active link and had an assigned private IPv4 address in the 192.168.x.x range.
- en0 and en3 were present but reported inactive.
- The subnet mask associated with en1 was 0xffffff00, equivalent to 255.255.255.0 or /24.
- The interface also had IPv6 link-local and global addresses configured.

Analysis:

The results indicates that en1 was an active network interface with both IPv4 and IPv6 addressing.

The /24 subnet mask identifies the first 24 bits of the IPv4 address as the network portion. For example, an address of 192.168.1.20/24 belongs to the 192.168.1.0/24 subnet.

The persistence of multiple interfaces does not necessarily indicate multiple active network connections.
Interfaces may be inactive, virtual, or reserved for different networking functions.

Security Relevances:

Identifying active interfaces helps security analysts understand and endpoint's potential network communication paths. It also establishes which interfaces should be considered during future traffic monitoring and configurations reviews.

3.2 Default IPv4 Route Investigation
The route -n get default command was used to identify the workstation's default IPv4 route.

Observations:
The default route used interface en1.
The configured gateway belonged to the workstation's local IPv4 subnet.
A route lookup for 2.2.2.2 also selected the same gateway and outgoing interface.

Analysis:

The routing information supports the conclusion that en1 was the selected interface for the default IPv4 route during the investigation.
When a destination does not match a more specific route, the operating system uses the default route to determine the next hop.
For an external IPv4 destination such as 2.2.2.2, the workstation was configured to forward traffic through its local gateway.

Security relevance:

Understanding routing decisions helps analysts identify expected network paths and investigate potential misconfigurations or unexpected traffic redirection.
The routing table provides evidence of the operating system's configured forwarding decisions but does not independently prove successful delivery.

3.3 Connectivity Testing

ICMP echo requests were sent to the default gateway and an external IPv4 destination.

Results:

Destination                                 Request Sent            Replies         Result
Local default gateway                       4                       4               Successful
External IPv4 address (2.2.2.2)             4                       4               Successful

Analysis:

The successful responses from the default gateway demonstrated that the workstation could exchange ICMP echo traffic with the local router during testing.

The successful responses from 2.2.2.2 demonstrated that ICMP communication with at least one external IPv4 destination was functional at the time of the investigation.

These results support the routing observations but do not prove that all applications or internet services are reachable.

Security relevance:

Basic connectivity testing helps distinguish certain network reachability problems from application-level failures.

For example, a workstation may successfully exchange ICMP traffic with an external host while experiencing problems with DNS resolution, TCP connectivity, or application services.

4. Key Findings

The investigation established the following:

1- en1 was an active interface with IPv4 and IPv6 addresses configured.
2- The interface used a /24 IPv4 subnet mask.
3- The default IPv4 route selected en1 and a gateway on the local subnet.
4- The route to the tested external IPv4 destination used the same interface and gateway.
5- ICMP connectivity to both the local gateway and the selected external destination was successful.
6- IPv6 was configured and should be included in subsequent network investigations.

5. Investigation Limitations

This investigation was limited to network configuration discovery, routing inspection, and basic ICMP connectivity tests.

- Successful ICMP responses do not establish that DNS, HTTPS, or other application services are functioning correctly.
- A four-request ping test represents a brief observation rather than continuous connectivity monitoring.
- The routing results do not independently confirm the path taken by every transmitted packet.
- IPv6 routing and connectivity were not evaluated.
- No comprehensive application-level traffic baseline was established.
- The results do not establish whether network activity is benign or malicious.

6. Conclusion

The workstation had an active network interface, en1, configured with IPv4 and IPv6 addresses. The IPv4 routing configuration identified en1 as the outgoing interface for the default route and the tested external destination.

Successful ICMP tests demonstrated basic reachability to the local gateway and one external IPv4 destination.

These findings provide an initial understanding of the workstation's network configuration and connectivity. Additional investigation is required to identify active network services, application communication patterns, and potential security concerns.

7. Next Investigation

The next stage will focus on identifying network sockets and the processes associated with them.

The objective will be to determine which applications are communicating over the network, identify their local and remote endpoints, and collect evidence that can contribute to an initial network activity baseline.
