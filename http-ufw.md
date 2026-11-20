# HTTP Test Server & UFW Configuration (Linux VM)
  - Configure http (Python 3) rules in linux os <Virtual Machine>

# http Configuration
 
<Step 1>
- Prepare Test Page (`~/www`)
  - Create a folder name (www) on user (User@PCName~:)
  - Command: cd ~ 
  - Command: mkdir www
  - Command: cd www
  - Command: touch index.html
  - Option 1 Command: `sudo nano index.html` (key in = `<html><body><h1>My Test Site</h1></body></html>`)
  - Option 2 Command: `cat <<< `<html><body><h1>My Test Site</h1></body></html>` >index.html`
  - Option 3 Command: `echo "<h1>My Test Site</h1>" > index.html`
 
<Step 2>
- Start the web server & Allow Subnet via UFW
  - Start HTTP server (run inside '~/www'):
  - Command: sudo python3 -m http.server 80
    - Show Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
  - Using the Browser enter the 192.168.103.XX( Target IP ) in URL bar will show "My Test Site"
  - If ufw firewall is active & didn't ALLOW the port 80 then need to ALLOW the port
  - Command: sudo ufw status (Quick Check the ufw firewall status)
  - Command: sudo ufw allow from 192.168.103.0/24 to any port 80 proto tcp
    - 192.168.103.0/24: Entire subnet can access.
    - 192.168.103.1: Only this single IP host can access.
  - Verify firewall & browser access ('http://192.168.103.XX'):
  - Command: sudo ufw status

```text
Status: active

To                    Action        From
--                    ------        ----
80/tcp                ALLOW         192.168.103.0/24
```

<Step 3>

