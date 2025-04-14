**CyberSecurity HomeLab Setup**

This project documents the creation of a fully isolated virtual cybersecurity lab using VirtualBox. The lab includes an attacker machine (Parrot OS) and a target machine (Metasploitable 2), connected via an internal-only network. It is designed for safe, offline testing of cybersecurity tools, network monitoring, and system analysis without any risk to the host machine or the internet.


**Tools & Technologies**

- VirtualBox – Virtualization platform for hosting lab machines
- Parrot OS – Security-focused Linux distro used as the attacker/analyst VM
- Metasploitable 2 – Deliberately vulnerable Linux VM used as the target
- Wireshark – Network traffic analyzer for packet capture and inspection
- Nmap, Hydra, Metasploit – Installed tools within Parrot OS for future testing


**Project Goals**

- Set up a virtual cybersecurity lab with complete network isolation.
- Enable inter-VM communication using an internal network.
- Assign static IP addresses to ensure consistent network behavior.
- Prevent all internet and host system access from within the lab.


**Lab Configuration**

**1. VirtualBox Network Setup**
To ensure that the virtual machines (VMs) can communicate with each other but are completely isolated from the host system and the internet:

**Steps:**
- Open VirtualBox.
- Select a VM (e.g., Parrot OS) → Click Settings → Go to Network.
- Under Adapter 1:
      - Check Enable Network Adapter
      - Set Attached to: Internal Network
      - Set Name: cyberlab-net (use the same name for all lab VMs)
- Repeat this configuration for Metasploitable 2 (must use the same Internal Network name).
- Do not enable any other adapters (no NAT, no Bridged, no Host-Only).
- This creates an isolated, host-invisible LAN environment for your lab.


**2. VM Setup and Static IP Configuration**
Using static IPs ensures predictable connectivity and simplifies tool use like Nmap or Metasploit.

**Parrot OS (Attacker VM)**
- Boot Parrot OS in VirtualBox.
- Open a terminal and edit the network configuration file:
  
     `sudo nano /etc/network/interfaces`
- Add or modify the following configuration:
  
     `auto eth0`
  
     `iface eth0 inet static`

     `address 10.10.10.10`

     `netmask 255.255.255.0`

     `network 10.10.10.0`

- Restart networking service:
  
     `sudo systemctl restart networking`

 **Metasploitable 2 (Target VM)**
- Boot Metasploitable 2 in VirtualBox.
- Login with default credentials:

     `username: msfadmin`
  
     `password: msfadmin`
  
- Edit the network config file:
  
     `sudo nano /etc/network/interfaces`
  
- Modify or add:
  
     `auto eth0`

     `iface eth0 inet static`

     `address 10.10.10.20`

     `netmask 255.255.255.0`

     `network 10.10.10.0`
  
- Restart networking:
  
     `sudo /etc/init.d/networking restart`


**Lab Isolation Verification**
To ensure the lab is fully air-gapped:
- Only Internal Network is used — no NAT, no Bridged, no Host-only adapters are attached.
- VMs cannot ping external domains or your host system.
- Use the following tests:
  
     From Parrot OS:
  
     `ping 10.10.10.20   # ✅ Should succeed – communication with Metasploitable 2`

     `ping google.com    # ❌ Should fail – no internet access`
  
     From Metasploitable 2:
  
     `ping 10.10.10.10   # ✅ Should succeed`

     `ping 8.8.8.8       # ❌ Should fail`
  
- You can also run netstat -rn or ip route to confirm there is no default gateway defined.


**Key Takeaways**
- Created a fully isolated cybersecurity lab environment.
- Configured a secure, internal-only network for safe testing.
- Gained hands-on experience with network interfaces and virtual networking.
- Set the foundation for future security experiments with IDS, attack tools, and monitoring.


**Future Enhancements**
- Add SNORT or Suricata for real-time traffic monitoring and intrusion detection.
- Introduce Splunk or the ELK Stack for centralized logging.
     - Expand the lab with additional targets like:
            - DVWA (Damn Vulnerable Web App)
            - OWASP Juice Shop
- Automate lab provisioning with Vagrant or Ansible.
