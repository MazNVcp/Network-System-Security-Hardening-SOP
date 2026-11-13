# Windows & Linux OS Firewall
  - Configure firewall rules and traffic direction

## Core Concepts: Inbound vs Outbound
  - **Inbound**: An external party actively connects to your computer.
    - *Examples*: Others connecting via SSH, accessing your local website, hackers performing port scans.
  - **Outbound**: Your computer is connecting to an external source on its own.
    - *Examples*: Downloading files, uploading files, browsing the web, system updates. 

# Windows Firewall Configuration 
<Step 1>
 - Open Windows Firewall Application
 - Action: Press 'Win + R', Type 'wf.msc' and press Enter to open Windows Defender Firewall wuth Advanced Security.
 - Purpose: Access the core firewall management console.

<Step 2>
 - Create a New Inbound/Outbound Rule
   - Action: Click **Inbound Rules** (or Outbound Rules) on the left panel, then click **New Rule...** on the right panel.
- Wizard Steps:
  - **Rule Type**: Choose between Program, Port, Predefined, or Custom.
  - **Program**: Define Program.
  - **Protocol and Ports**: Define protocol type and specific ports.
  - **Scope**: Configure local and remote IP addresses.
  - **Action**: Choose whether to Allow or Block the connection.
  - **Profile**: Select network profiles (Domain, Private, Public).
  - **Name**: Give the rule a clear, recognizable name.

<Step 3>
 - Verify the Firewall setup & Turn on the web server in windows 
   - Open the Turn Windows features on or off
   - Activate The Internet Information Services (if need)
   - Open the Internet Information Services (IIS) Manager
   - Open the Browser and enter the localhost in URL bar
   - Virtual Box type the commant curl http://192.168.103.XX (Target IP)
   - *Note*: `curl` acts as a command-line web browser
   - **Test Result (Blocked)**: If the Firewall blocks the target IP on port 80, VirtualBox returns `Failed to connect`
   - **Test Result (Allowed)**: If the Firewall allows the connection, it returns the raw HTML code
  - Note: VirtualBox network must be Bridged or Host-only Adapter to reach Windows Host IP (Avoid NAT mode).

# Linux Firewall Configuration
<Step 1>
 - Setup The Linux Firewall by 'ufw'
   - Command: sudo ufw status to check ufw status.
      - **Check Basic Status**: `sudo ufw status`
         - Purpose: Quick check for active status and current rule table.
   - Command: sudo ufw allow 22/tcp
      - **Safety Warning**: Before running `sudo ufw enable`, always allow SSH (e.g., `sudo ufw allow 22/tcp`) if you are connected remotely, to avoid getting locked out!
   - show Rules updated & Rules updated (v6)
   - Command: sudo ufw enable
   - If remote the system will ask Command may disrupt existing ssh connections. Proceed with operation (y|n)?
   - Command: sudo ufw status to check the status
   ```text
   Status: active

   To				Action			From
   --           	        ------  	        ---- 
   22/tcp			ALLOW			Anywhere
   22/tcp (v6)			ALLOW			Anywhere (v6)
   ```
   - Command: sudo ufw status verbose
      - **Check Detailed Status**: `sudo ufw status verbose`
         - Purpose: View global default policies (Incoming/Outgoing), logging level, and full rules.
   ```text
   Status: active
   Logging: on (low)
   Default: deny (incoming), allow (outgoing), disable (routed)
   New profiles: skip
   
   To				Action			From
   --				------			----
   22/tcp			ALLOW IN		Anywhere
   22/tcp (v6)			ALLOW IN		Anywhere (v6)
   ```
   
   
