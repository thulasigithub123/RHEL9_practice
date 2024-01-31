# RHEL 9 Administration
## Day 1

### 1. Installation of Virtual Box and Configuring VMs
- Download RHEL 9.3 ISO: `rhel-9.3-x86_64_dvd.iso`
- VM Configuration:
  - Linux Server Instance: 
    - IP: `10.0.2.5/24`
    - RAM: `4GB`, HDD: `20GB`, CPUs: `2`
  - Linux Client Instance:
    - IP: `10.0.2.6/24`

### 2. Setting up Virtual Network Adapter
- Host-only and NAT Network for Internet access.

### 3. Boot VMs with Optical Disk and Installation
- Boot VMs with ISO for installation.

### 4. Clone and Snapshots
- Clone VMs with new MAC addresses.
- Take snapshots of initial setups.

### 5. User Profiles
- Root user: `admin123`
- User1: `12345`

### 6. Networking
- Network Manager Text User Interface: `nmtui`
- Network Manager Command-Line Interface

### 7. Configuring Network Profiles
- `nmcli device status` (view adapters and status)
- `nmcli connection show` (list connections)
- Setup new connection as automatic for DHCP.

 ![image](https://github.com/thulasigithub123/RHEL9_practice/assets/87015668/3afe66a0-1f27-4a44-87d6-c1e5d969692c)

- Check IP: `ip addr show`
- List gateways: `ip route`
- DNS addresses: `cat /etc/resolv.conf`
- Network configurations: `cat /etc/NetworkManager/system-connection/con-name.nmconnection`
- Reload and restart: `nmcli connection reload && systemctl restart NetworkManager`

### 8. Computer Naming
- Short/alias name: `hostname`
- Detailed description: `hostnamectl`
- Set hostname: `hostnamectl set-hostname xxx`
- Verify hostname: `cat /etc/hostname`

### 9. Name Resolution Entries
- Check entries: `cat /etc/hosts`
- Add entries for client and server (IP, fullname, alias)
- Verify internal connection with Ping utility.
 
