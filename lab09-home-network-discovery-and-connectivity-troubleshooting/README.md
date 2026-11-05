# Lab 09 – Home Network Discovery and Connectivity Troubleshooting

## Lab Type

Independent Home Lab – Windows and macOS Network Troubleshooting

---

# Objective

The objective of this lab was to investigate a small home network containing a Windows 11 laptop and a macOS laptop. The exercise involved identifying network configuration details, understanding the roles of DHCP, DNS and the default gateway, testing connectivity between endpoints, and investigating failed ICMP communication.

The lab provided practical experience using Windows PowerShell and macOS Terminal networking commands while applying a structured troubleshooting methodology.

---

# Devices Used

**Windows Endpoint:** EDU-LAB-CL02 (Lenovo)

**Operating System:** Windows 11 Pro

**Mac Endpoint:** EDU-LAB-CL01 (MacBook Air)

**Operating System:** macOS

**Network Equipment:** Home wireless router

---

# Step 1 – Network Discovery

## Windows Network Configuration

### Tools and Commands

```powershell
ipconfig
ipconfig /all
```

These commands were used to inspect the Windows endpoint's network adapters and identify its IPv4 configuration, subnet mask, default gateway, DNS server and DHCP settings.

### Findings

The Windows endpoint was connected through its wireless network adapter.

| Property | Lab Finding |
|---|---|
| IPv4 Address | 192.0.2.40 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.0.2.1 |
| DNS Server | 192.0.2.1 |
| DHCP Enabled | Yes |
| DHCP Server | 192.0.2.1 |

*Addresses above are documentation-only examples replacing the actual home network addresses.*

## Mac Network Configuration

### Tools and Commands

```bash
ipconfig getifaddr en0
route -n get default
```

These commands were used to identify the MacBook's Wi-Fi IPv4 address and default gateway.

### Findings

The MacBook was initially connected to the 2.4 GHz Wi-Fi network and was subsequently connected to the 5 GHz network.

Its IP configuration changed following the reconnection.

Both endpoints were ultimately identified within the same IPv4 subnet.

---

# Step 2 – Understanding DHCP, DNS and the Default Gateway

During the investigation, the Windows endpoint reported the same IPv4 address for its DHCP server, DNS server and default gateway.

This demonstrated that a home router can provide several network services simultaneously.

### DHCP

Dynamic Host Configuration Protocol (DHCP) automatically provides network configuration to clients, including IP addresses, subnet masks, default gateways and DNS server addresses.

### DNS

Domain Name System (DNS) translates domain names into IP addresses, allowing devices to locate network resources using names rather than numerical addresses.

### Default Gateway

The default gateway provides a route from the local network to other networks, including the Internet.

In this lab, the home router provided all three services using the same local IP address.

---

# Step 3 – Network Connectivity Testing

## Initial Connectivity Tests

The following command was executed on Windows:

```powershell
ping 192.0.2.49
```

The initial test returned:

`Destination host unreachable`

The response originated from the Windows endpoint.

A second test was performed from macOS:

```bash
ping -c 4 192.0.2.40
```

The result was 100% packet loss.

These results indicated that the endpoints were not successfully exchanging ICMP Echo Requests and Replies.

---

# Step 4 – ARP Investigation

## Tool and Command

```powershell
arp -a
```

The ARP table was inspected to identify whether Windows had resolved the MacBook's IPv4 address to a physical MAC address.

Initially, no entry for the MacBook's IP address was found.

After reconnecting the MacBook to the same Wi-Fi frequency band as the Windows endpoint, the MacBook received a new IPv4 address.

A subsequent ARP inspection showed a dynamic entry for the MacBook.

### What is ARP?

Address Resolution Protocol (ARP) is used in IPv4 networks to discover the MAC address associated with an IPv4 address on the local network.

This investigation demonstrated the importance of checking address resolution when troubleshooting local network connectivity.

---

# Step 5 – macOS Firewall Investigation

The macOS Firewall settings were inspected through:

**System Settings → Network → Firewall → Options**

The following configuration was observed:

- macOS Firewall: Enabled
- Enable Stealth Mode: Enabled

### What is Stealth Mode?

Stealth Mode is a macOS security feature that makes the computer less responsive to certain network discovery attempts, helping reduce its visibility to other devices.

It can affect responses to ICMP ping requests.

---

# Step 6 – Controlled Connectivity Test

To investigate the possible impact of Stealth Mode, a temporary configuration change was performed.

1. Opened macOS Firewall Options.
2. Temporarily disabled Enable Stealth Mode.
3. Kept the macOS Firewall enabled.
4. Returned to the Windows endpoint.
5. Repeated the connectivity test.

### Command

```powershell
ping 192.0.2.23
```

### Result

The Windows endpoint successfully received ICMP Echo Replies from the MacBook.

This demonstrated that the change in Stealth Mode configuration was associated with the successful ping test.

Stealth Mode was subsequently re-enabled to restore the original security configuration.

---

# Step 7 – Final Network Configuration

The following diagram represents the logical network discovered during the investigation.

```text
             Internet
                |
          Home Router
           192.0.2.1
       Gateway / DHCP / DNS
                |
          Home Wi-Fi LAN
                |
        +-------+-------+
        |               |
    Windows 11        macOS
    EDU-LAB-CL02      EDU-LAB-CL01
    192.0.2.40       192.0.2.23
        |               |
       5 GHz           5 GHz
```

*The IP addresses in this diagram are sanitised documentation examples.*

---

# Final Reflection

This lab provided practical experience investigating a local network containing two different operating systems.

I used Windows PowerShell and macOS Terminal to inspect network configurations, identify IP addresses, examine DHCP and DNS settings, and determine the default gateway.

The connectivity investigation introduced practical troubleshooting techniques involving ICMP ping, ARP address resolution and endpoint firewall settings.

I also explored how macOS Stealth Mode can affect network visibility and demonstrated the importance of making controlled configuration changes while maintaining endpoint security.

The exercise reinforced the importance of following a structured troubleshooting methodology: gathering information, identifying possible causes, testing hypotheses, evaluating results and restoring security configurations.

These skills are relevant to everyday IT Support and Help Desk responsibilities, particularly when diagnosing local network connectivity problems.

---

# Tools and Commands Checklist

- [x] `ipconfig`
- [x] `ipconfig /all`
- [x] `ipconfig getifaddr en0`
- [x] `route -n get default`
- [x] `ping`
- [x] `arp -a`
- [x] Windows PowerShell
- [x] macOS Terminal
- [x] macOS Firewall settings

---
