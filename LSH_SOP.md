Configure Static IP for Cloned Linux VM (Host-Only Network)

<Step 1>
 - Open the VirtualBox
  - tools
  - network 
  - Host-only Networks
  - click on Configure Adapter Manually
  - setting the IPv4 Address:192.168.103.XX
  - setting the IPv4 Network Mask:255.255.255.0

<Step 2>
 - Settings the target Virtual Machine
   - Settings
   - Network
   - Adapter 2
   - Enable Network Adapter
   - Attached to (Host-only Adapter)

<Step 3>
 - Check the both VM MAC Address is different
 - After check then start the VM

<Step 4>  
 - Command: cd /etc/netplan
 - Command: sudo nano 00-installer-config.yaml
	
---------------------------------------------------------------------------		

(origin setting)
- network:
	```text
	ethernets:
	 enp0s3:
	   dhcp4 : true
	   dhcp6 : true
	   match :
	    macaddress:00:dd:aa:bb:cc(depends the macaddress id)
	   set-name: enp0s3
	version: 2
	```
---------------------------------------------------------------------------

(after Configure Static IP)
- network:
	```text
	ethernets:
	 enp0s3:
	   dhcp4: true
	   dhcp6: true
	   match:
	    macaddress:00:dd:aa:bb:cc
	   set-name: enp0s3
	 enp0s8
	   dhcp4: no
	   dhcp6: no
	   addresses:
	     - 192.168.103.XX/24
	version: 2
	```
----------------------------------------------------------------------------

if the virtual machine is <clone> must change the macaddress in different
 - ex: CloneVMv1 - 00:dd:aa:bb:cc then need to change the CloneVMv2 - 00:dd:aa:bb:ee
 - shutdown & restart both VM

<Step 5>
 - Verify network connectivity between virtual machines
   - Ex: Machine A (192.168.103.2) pings Machine B (192.168.103.3) to test the connection
   - Command: ping 192.168.103.3
   - Expected output on success:
     - 64 bytes from 192.168.103.3: icmp_seq=1 ttl=65 time=0.612ms
   - Note: Seeing "64 bytes" and "time=.." confirms successful network connectivity between the VMs.

<Step 6>
 - Command: ip address or ip a
 - purpose: Used to inspect active network interfaces,assigned IP addresses,and MAC addresses.

Key details checked from output:
 - Interface name(e.g., lo, enp0s3, enp0s8)
 - MAC address(e.g., link/ether 00:dd:aa:bb:cc) - Ensures unique hardware addressing for cloned VMs
 - IPv4 address(e.g., inet 192.168.XX/24) - Confirms static IP applied via Netplan

<Step 7>
 - Command: 'ssh example@192.168.103.XX'
 - Purpose: Establish a secure remote terminal session to the target machine

Troubleshooting - "Connection refused":
 - This error typically occurs if SSH service/package is missing or disabled on the target machine.

Solutions:
 - Install SSH Client (on host/source machine):
 - Command: 'sudo apt install ssh -y' or 'sudo apt install openssh-client -y'

 - Install & start SSH Server on target machine:
 - Command: 'sudo apt install openssh-server -y'
 - Command: 'sudo systemctl enable --now ssh'

<Step 8>
 - Configure SSH Hardening (Disable Password Authentication)
 - Purpose: Enhance Server security by enforcing SSH Key-based authentication and blocking brute-force password attacks. 
 - Command: 'cd /etc/ssh'
 - Command: 'sudo nano ssh_config

Configuration deep command:
- Command: sudo nano /etc/ssh/ssh_config
- Purpose: Configure SSH Client settings (controls outgoing SSH connections).
- Change: Set 'PasswordAuthentication no‘ restrict the client from using password auth when connecting to remote hosts. 

Configuration Change:
- Find the line: PasswordAuthentication yes
- Uncomment (remove '#') and change to: PasswordAuthentication yes/no
  - yes: Allows password-based login
  - no: Enforces SSH Key-based Authentication(Public-Key Cryptography)

Apply Changes:
 - sudo systemctl restart ssh

Security Note:
 - Ensure public key (id_rsa.pub or id_ed25519.pub) is added to '~/.ssh/authorized_keys' before restarting the SSH service to avoid lockout.

<Understanding Configuration Syntax( # ) vs Active Settings>
 - Commented Settings(#):Lines starting with # are treated as documentation or default templates by the system.They are inactive.
 - Active Settings:Removing the # (uncommenting) activates the rule and overrides the system default behavior.

