### NET-002 - Outbound Network Connection Investigation

Project: Network Security - NET002
Environment: macOS Home Lab
Investigation Type: Network Connection Analysis 
Status: In Progress
Date: November 9th, 2026

1. Investigation Scenario

A macOS workstation has been reported as potentially establishing unexpected outbound network connections.

As a junior SOC analyst, my responsibility is to investigate the workstation's network activity, identify the process responsible for observed connections, and determine whether any activity requires further investigation.

Simulated SOC investigation conducted in my home lab.

2. Investigation Objectives

The objective of this investigation is to:

- Identify visible network connections on the workstation.
- Determine which processes are associated with those connections.
- Examine local and remote endpoints, protocols, ports, and socket states.
- Investigate the executable responsible for a selected connection.
- Assess the available evidence for potentially suspicious behavior.
- Document findings, limitations, and recommended next steps.

3. Investigation Scope

System: Mac mini, macOS.
Tools: Terminal
Investigation boundaries: Read-only investigation of my own workstation and its network activity.
Out of scope: Exploitation, unauthorized network scanning, and changes to system of firewall configuration.

4. Investigation Methodology

Document each investigation phase as it happens.
For each phase, answer:
- What question am I trying to answer?
- Which tool or command did I use?
- Why did I choose this method?
- What evidence did I collect?
- What does the evidence suggest?
- What remains unknown?

    1. Network Connection Discovery: Udes lsof to identify active TCP connections, listening ports, associated process, and connections states.
    2. Process Investigation: Udes ps to examine process IDs, executable paths, parents processes, and command-line arguments.
    3. Executable Verification: Used codesign to verify the integrity and code signature of the identified macOS application.
    4. Network Exposure Assessment: Tested TCP ports 5000 and 7000 from another device on the local network to assess reachability. 
    5. Outbouind Connection Analysis: Examined an established IPv6 connection associated with ChatGPT, identifying its local and remote addresses and ports.
    6. Destination Research: USed whois to investigate the ownership of the remote IP address and identify its registered network provider.
    7. Behavioral Validation: Reopen ChatGPT and compared process ID and network endpoints to assess whether the observed connection was consistent with application activity.

### Phase 1 — Initial Network Connection Triage
     
**Investigation question:**

What network connections and listening services are active on the macOS workstation, and are there any connections that require further investigation?

**Command or tool used:**

```bash
lsof -nP -i
```

**Why I used this tool:**

To identify processes using network sockets and examine their associated IP addresses, ports, protocols, and connection state.

**Observations:**

The initial investigation identified a process named ControlCe, associated with PID 624 at the time of observation. The process had TCP sockets in the LISTEN state on ports 5000 and 7000.

The listening sockets used wildcard addresses (*), indicating that they were not restricted to the IPv4 loopback address.

**Evidence:**

... add reference ...

**Analysis:**

The LISTEN state indicates that the process is waiting for incoming connection attempts. The wildcard binding suggests that the services may accept connections multiple local networks interfaces.
However, listening sockets do not establish that an external connection is active or that the service is malicious.

**Uncertainties or limitations:**

The initial output did not establish the executable's legitimacy, whether the listening ports were reachable from another device, or whether the process was performing suspicious activity.

**Next investigative action:**

Investigate the process associated with the listening sockets to identify it's executable path and determine whether its behavior is consistent with an expected macOS service.