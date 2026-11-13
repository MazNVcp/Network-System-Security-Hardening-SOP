# Windows & Linux OS Firewall
  - Configure firewall rules and traffic direction

## CoreConcepts:Inbound vs Outbound
  - **Inbound**: An external party actively connects to your computer.
    - *Examples*: Others connecting via SSH, accessing your local website, hackers performing port scans.
  - **Outbound**: Your computer is connecting to an external source on its own.
    - *Examples*: Downloading files, uploading files, browsing the web, system updates. 

# Windows Firewall Configuration 
<Step 1>
 - Open Windows Firewall Application
  - Action: Press 'Win + R', Type 'wf.ms' and press Enter to open Windows Defender Firewall wuth Advanced Security.
  - Purpose: Access the core firewall management console.

<Step 2>
 - Create a New Inbound/Outbound Rule
  - Action: Click **Inbound Rules** (or Outbound Rules) on the left panel, then click **New Rule...** on the right panel.
- Wizard Steps:
  - **Rule Type**: Choose between Program, Port, Predefined, or Custom (Ex: Custom).
  - **Program**: Select which programs this applies to (Ex: All Programs).
  - **Protocol and Ports**: Define protocol type and specific ports.
  - **Scope**: Configure local and remote IP addresses.
  - **Action**: Choose whether to Allow or Block the connection.
  - **Profile**: Select network profiles (Domain, Private, Public).
  - **Name**: Give the rule a clear, recognizable name.
