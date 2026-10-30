Remote_Access_Custom_keys
 Using Windows OS Remote to Linux OS

<Step 1>
Verify network connectivity target Machines
 - Windows OS
 - Open the cmd<Command Prompt>
 - Command ping the target Machines's IP
 - Command: ping 192.168.103.XX
 - Expected output on success:

   ```text
   Replay from 192.168.102.XX: bytes=32 time=Xms TTL=64
   =
   =

   Ping statistics for 192.168.103.XX:
       Packets: Sent = 3, Received = 3, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = Xms, Maximum = Xms , Average = Xms
   ```

<Step 2>
 - SSH Client software on the Windows OS connection to Linux OS
   - Command: ssh TargetMachine@192.168.103.XX
   - Purpose: Establish a secure remote termianl session to the target machine
   - 

<Step 3>
 - Handle Host Key Conflict (Host Identification Changed)
   - Cause: Target machine host key changed or IP resued (Cloned VM / OS re-install)
   - Windows OS (CMD)
   - Command: ssh-keygen -R 192.168.103.XX
   - Purpose: Re,ove stale host key record from C:\Users\<Username>\.ssh\know_hosts
   - Expected output on success:
     - # Host 192.168.103.XX updated: C:\Users\<Username>\.ssh\know_hosts updated.

<Step 4> 
 - First-Time Host Authenticity Verification
   - Command: ssh TargetMachine@192.168.103.XX
 - Expected prompt:
     ```text
     The authenticity of host '192.168.103.XX' can't be established.
     ED25519 key fingerprint is SHA256: XXXXX...
     Are you sure you want to continue connectiong (yes/on/[fingerprint])?
     ```
 - Action: type "yes' and press Enter
 - Purpose: Save target machine host fingerprint to local known_hosts file

<Step 5>
 - Deploy Public Key to Target Machine (Authorized Keys Setup)
   - Target Linux OS
   - Concept: A single Linux host can accept multiple public keys in authorized_keys
   - Open targer user's SSH config file:
     - Command: nano ~/.ssh/authorized_keys
   - Action: Paste client's public key (e.g., contents of id_ed25519_custom.pub)
   - Set strict permissions:
     - Command: chmod 700 ~/.ssh
     - Command: chomd 600 ~/.ssh/authorized_keys
 - Purpose: Grant key-based passwordless access without overwriting default keys

<Step 6>
- Verify Key-Based Authentication
 - Windows OS (CMD)
 - Command: ssh -i C:\Users\<Username>\.ssh\id_ed25519_custom TargetMachine@192.168.103.XX
 - Expected output on success:
   ```text
      Welcome to Ubuntu 26.04.1 LTS ...
   ```
