# Plesk, MikroTik, Apache, and MariaDB VLAN Lab

* Status: Baseline audit in progress
* Platform: Proxmox VE
* Scope: VLANs, routing, firewalling, web hosting, databases, backup, and recovery

## Purpose

I am building this project to practise technologies that I expect to use in an IT administration role.

The goal is not only to install Plesk, MikroTik, Apache, and MariaDB. I want to understand the traffic between them, restrict unnecessary access, troubleshoot failures, and prove that I can restore the services.

## Final design

```text
Existing LAN
     |
MikroTik CHR
     |
VLAN-aware Proxmox lab bridge
     |
     +-- Management VLAN
     +-- Plesk VLAN
     +-- Apache VLAN
     +-- MariaDB VLAN
     +-- Client VLAN
```

Plesk manages the Apache, nginx, PHP, and database services installed on the Plesk VM.

The standalone Apache VM is configured manually. It connects to MariaDB on another VLAN so I can practise a real cross-VLAN application dependency.

The main permitted application flow is:

```text
Apache VM -> MariaDB VM -> TCP 3306
```

Plesk and normal client systems do not receive access to the standalone MariaDB server by default.

## Main decisions

* The lab is isolated from my normal services.
* Proxmox does not receive an IP address on the lab bridge.
* MikroTik performs routing, DHCP, NAT, and inter-VLAN filtering.
* Administrative access is limited to a known management source.
* Inter-VLAN traffic is denied unless there is a documented reason to allow it.
* Plesk and manual Apache administration are treated as separate learning paths.
* Backups are not considered complete until I test a restore.
* Exact addresses, credentials, domains, and complete firewall exports are not published.

## Implementation stages

1. Audit Proxmox networking, repositories, storage, and available resources.
2. Create the isolated VLAN-aware bridge.
3. Deploy and secure MikroTik CHR.
4. Configure VLANs, DHCP, routing, NAT, and management access.
5. Deploy the client, Plesk, Apache, and MariaDB VMs.
6. Add firewall rules one dependency at a time.
7. Configure websites and the remote database connection.
8. Add monitoring and backup jobs.
9. Test failures and restores.
10. Publish sanitized configuration examples and results.

## Completion criteria

The project is complete when:

* every VLAN is isolated by default
* required application traffic works
* unauthorized database access fails
* Plesk hosts and manages its own test sites
* standalone Apache uses the remote MariaDB database
* management interfaces are restricted
* monitoring detects service failures
* at least one database restore and one VM or Plesk restore are tested
* the architecture and firewall decisions can be explained clearly

