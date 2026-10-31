# Generating Independent SSH Key Without Overwriting Defaults
* Standard Operating Procedure for Generating Independent SSH Keys in Custom Paths without Overwriting System Defaults * 

<Step 1>
 * Create Custom Storage Directory
 - Command: mkdir -p /path/to/your/custom_folder
 - Purpose: Establish a isolated directory for store independent SSH keys
 - Note: Replace '/path/to/your/custom_folder' with your actual target directory

<Step 2>
 * Generate Independent Key Pair
 - Command: ssh-keygen -t ed25510 -C "custom key comment" -f /path/to/your/custom_folder/id_custom
 - Purpose: Generate isolated SSH key pair in the target folder without overwriting default keys ('id_rsa'/'id_ed25519')
 - Verification: Run 'ls -la /path/to/your/custom_folder/' to ensure 'id_custom' and 'id_custon.pub' are created

<Step 3>
 * Restrict Private Key File Permissions
 - Command: chmod 600 /path/to/your/custom_folder/id_custom
 - Purpose: Set strict '-rw------' read/write permission to prevent SSH client connection failure ('Permission are too open')
 
<Step 4>
 * Configure SSH Routing Rules
 - Command: nano ~/.ssh/config
 - Action: Append explicit identity mapping using absolute path

 - Configuration Entry:
   ```text
   Host custom-git-identity
      HostName github.com
      User git
      IdentityFIle/path/to/your/custom_folder/id_custom
      IdentitiesOnly yes
    ```

<Step 5>
 * Verify Isolated Connection
 - Command:ssh -T git@custom-git-identity
 - Purpose: Test remote authentication using the custom key identity without affecting existing system keys

