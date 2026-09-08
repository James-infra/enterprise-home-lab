# Enterprise Home Lab

A personal lab for developing and documenting practical skills in Windows infrastructure, networking, identity, and systems administration.

This repository records configuration decisions, verification steps, and troubleshooting. All work described here was performed in a lab environment.

## Project Objectives

- Build an internal lab network with a pfSense gateway.
- Deploy Windows Server 2025 and Active Directory Domain Services.
- Configure DNS and Windows DHCP.
- Validate client connectivity and domain services.
- Document repeatable procedures and troubleshooting findings.
- Extend the environment into Group Policy, hybrid identity, and endpoint management.

## Lab Environment

| Component | Role | Configuration |
|---|---|---|
| Ubuntu host | Virtualization host | VMware Workstation |
| VMware vmnet2 | Internal lab network | 192.168.50.0/24 |
| Host virtual adapter | Host access to lab network | 192.168.50.1 |
| fw01 | pfSense firewall and gateway | LAN: 192.168.50.254 |
| DC01 | Windows Server 2025; AD DS, DNS, and DHCP | 192.168.50.10 |
| Active Directory domain | Internal directory namespace | corp.lab.example |
| CLIENT01 | Planned client validation system | DHCP lease testing pending |

## Architecture

The internal lab uses VMware's host-only network, vmnet2.

- pfSense provides the internal network gateway.
- DC01 hosts Active Directory and internal DNS.
- Windows DHCP on DC01 is the designated DHCP service for the internal subnet.
- VMware DHCP on vmnet2 and pfSense LAN DHCP are disabled to prevent competing DHCP services.

### Intended Client Network Settings

| Setting | Value |
|---|---|
| Subnet | 192.168.50.0/24 |
| Default gateway | 192.168.50.254 |
| DNS server | 192.168.50.10 |
| AD domain | corp.lab.example |

## Milestone Status

| Milestone | Status |
|---|---|
| VMware internal network configured | Completed |
| pfSense deployed and web interface accessible | Completed |
| pfSense security baseline checked | Completed |
| Windows Server 2025 installed | Completed |
| DC01 promoted to a domain controller | Completed |
| AD DS and DNS verification | Completed |
| Windows DHCP role installed and service running | Completed |
| DHCP authorization command executed | Completed |
| DHCP configuration and client lease validation | In progress |
| CLIENT01 domain join | Planned |
| Group Policy implementation | Planned |
| Hybrid identity and Intune integration | Planned |

## Completed Verification

### Network and Firewall

- Confirmed the vmnet2 host adapter uses 192.168.50.1/24.
- Confirmed VMware network services were running.
- Corrected pfSense WAN and LAN interface assignments.
- Confirmed access to the pfSense web interface.
- Checked that WAN web administration and UPnP were disabled.
- Confirmed no WAN port-forwarding rules were configured.

### Windows Server and Active Directory

- Created installation and post-promotion snapshots.
- Verified domain controller health using dcdiag.
- Confirmed SYSVOL and NETLOGON shares were present.
- Verified AD DS, DNS, Netlogon, and KDC services.
- Verified forward and reverse DNS zones.
- Confirmed DNS forwarding through pfSense.

### DHCP

- Confirmed the DHCP Server role was installed.
- Confirmed the DHCPServer service was running.
- Executed the DHCP authorization command for DC01.

Successful client lease assignment remains a separate validation milestone.

## Troubleshooting Highlights

### VMware Kernel Modules

Encountered a "Key was rejected by service" error while loading VMware kernel modules.

Later checks confirmed VMware network services were operational. A detailed runbook will record the verified remediation steps.

### pfSense Interface Assignment

Identified incorrect WAN and LAN assignments during deployment.

Corrected the assignments and verified access to the firewall management interface.

### DHCP Service Ownership

Reviewed DHCP services on the internal network before using Windows DHCP.

Disabled competing DHCP services on the internal segment so DC01 could provide client addressing.

## Next Steps

- Verify DHCP authorization, scope activation, exclusions, and options.
- Obtain a DHCP lease on CLIENT01.
- Verify the assigned gateway and DNS server.
- Test internal DNS resolution and network connectivity.
- Join CLIENT01 to corp.lab.example.
- Implement and test initial Group Policy settings.
- Add screenshots, command outputs, and detailed runbooks.

## Documentation Standards

Each milestone will document:

1. Objective
2. Configuration
3. Verification evidence
4. Problems encountered
5. Resolution and lessons learned

Passwords, tokens, private keys, and sensitive configuration exports are excluded from this repository.
