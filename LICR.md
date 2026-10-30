Linux Identity And Credential Recovery
- Troubleshooting and System Administration on Ubuntu OS
- Change Hostname, Username & Reset Root Password

<Step 1> 
Reset Root Password via Recovery Mode
 - Problem: Root password forgotten or locked out
 - Reboot machine and hold Shift/Esc to access GRUB menu
 - Navigation: Advanced options for Ubuntu -> (recovery mode) -> Drop to root shell prompt
 - Remount root filesysyem with write permissions:
 	Command: mount -o remount,rw /
 - Reset target user/root password:
	Command: passwd root
 - Restart system to verify login:
	Command: exec /sbin/init
 - Purpose: Gain full administrative control without data loss

<Step 2>
Hard-Rename Active Local User (Bypassing Process Locks)
 - Problem: "usermod: user is currently used by process XXX" error due to auto-respawning daemon/seesion
 - Cause: Systemd user session locks prevemt stamdard usermod excution
 - Solutuin: Directly modiffy core system configuration files via root shell

1.Modify User Name and Home Path:
 - Command: nano /etc/psswd
 - Action: Change target username and home directory path (e.g., User -> User2)

2.Synchronize Shadow Authentication Entry:
 - Command: nano /etc/shadow
 - Actioon: update username prefix on corresponding hash entry

3.Update Group Identity:
 - Command: nano /etc/group
 - Command: nano /etc/gshadow
 - Action: Rename target group from old username to new username

4.Rename Physical Home Directory:
 - Command: mv /home/OldUser /home/NewUser
 - Purpose: Cleanly re-identify a cloned VM user identity without process collision

<Step 3>
Change Hostname and Update Local Resolution (Vm -> Vm2)
 - Problem: Clone VM retains original hostname causing network resolution collision 
 - Cause: Hostname and /etc/hosts mapping must be re-identified post-cloning
 - SolutionL update system hostname via hostnamectl and align local loopback resolution

1.Set New System Hostname:
 - Command: sudo hostnamectl set-hostname Vm2
 - Action: Permanently changes the system identity to Vm2

2.Configure Local Resolution Mapping:
 - Command: sudo nano /etc/hosts
 - Action: Add basic loopback resolution to prevent sudo/network warnings
 - Entry:
   ```text
   127.0.0.1  localhost
   127.0.1.1  Vm2
   
   ::1     ip6-localhost ip6-loopback
   ff02::1 ip6-allnodes
   ff02::2 ip6-allrouters 
   ```

3.Refresh Current Shell Session:
 - Command: exec bash
 - Purpose: instantly reload current terminal prompt to reflect new identity (user@Vm2)


