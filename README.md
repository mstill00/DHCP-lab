# DHCP lab

## Objective

This DHCP lab seeks to emulate the setup, configuration, and management of a DHCP server using a Windows Server 2022 core virtual machine (VM).

### Project Overview/Skills Learned

- Installed the Dynamic Host Configuration Protocol (DHCP) server role on the DHCP server using PowerShell.
- Joined the DHCP server to the domain.

### Tools Used

- PowerShell for managing server roles, DHCP, and the local "DHCP Administrators" group.
- SConfig for manually configuring the server's hostname, IP address, subnet mask, default gateway, and preferred DNS server.
- VirtualBox for VM deployment and resource provisioning.

## Prerequisites

This section's steps are out of scope for this project. However, this section provides important context behind the underlying infrastructure of the lab and some of the rationale behind my decision making.

Here are some of the steps I took before starting the project:

- I set up a domain controller VM as a part of the <a href="https://github.com/mstill00/Active-Directory-Lab/blob/main/README.md">Active Directory lab</a>.
  - Domain-joining the DHCP server will allow me to centrally manage the server using a domain user account that is a part of a security group for DHCP admins.
    - This setup is beneficial for 2 reasons.
      - Scalability: If the administrator of the DHCP server leaves the company, their user account can be removed from the DHCP-Admins security group and quickly replaced by another user account.
      - Security: As opposed to the break glass local administrator account on the DHCP server, a domain user account can be audited for non-repudiation purposes and assigned only the permissions required to administer the DHCP service on the server.

- Within the Active Directory Administrative Center on the domain controller, I created a computer object for the DHCP server (NC-DHCP-01), created a security group for DHCP administrators (DHCP-Admins), and added a privileged domain user account (mstil01-admin) to the security group.<br><img width="269" height="128" alt="DHCP_Lab_IMG_3" src="https://github.com/user-attachments/assets/d80a8740-eee3-4a76-9abd-f753a7440ea6" />
  - The OU hierarchies containing the DHCP server computer object and the security group can be viewed in the <a href="https://github.com/mstill00/Active-Directory-Lab/blob/main/README.md#prerequisites">Prerequisites</a> section of the Active Directory lab.

- I planned out and documented a DHCP scope for my Lab network (192.168.40.64/28).

DHCP scope: 192.168.40.70-192.168.40.78

- I installed Windows Server 2022 core to run the DHCP service. Here are the reasons that I used core over desktop for the DHCP server:
  - I needed practice managing server roles in PowerShell.
  - I needed practice using the server configuration tool (SConfig).
  - The device hosting the VMs only has 32 GB of RAM (roughly 11 GB is used by other applications, effectively leaving me with ~21 GB of RAM for VMs).
    - Using Windows Server core, I was able to allocate 2 GB of RAM to the DHCP server VM without stability issues.

## Steps

