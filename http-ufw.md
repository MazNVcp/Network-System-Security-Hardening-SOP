# HTTP Test Server & UFW Configuration (Linux VM)
  - Configure http (Python 3) rules in linux os <Virtual Machine>

## http Configuration
 
<Step 1>
**Machine 1 (Web Server)**
- Prepare Test Page (`~/www`)
  - Create a folder name (www) on user (User@PCName~:)
  - Command: cd ~ 
  - Command: mkdir www
  - Command: cd www
  - Command: touch index.html
  - Option 1 Command: `sudo nano index.html` (key in = `<html><body><h1>My Test Site</h1></body></html>`)
  - Option 2 Command: `cat <<< <html><body><h1>My Test Site</h1></body></html> >index.html`
  - Option 3 Command: `echo <h1>My Test Site</h1> > index.html`
 
<Step 2>
**Machine 1 (Web Server)**
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
**Machine 2 (CA Server)**
- Configuring HTTPS/TLS for a Web Server on an internal network `Internal CA`
- Create an Internal Root CA :
  - Command: cd ~
  - Command: mkdir -p internal-ca
  - Command: cd internal-ca
  - Command: openssl genrsa -out ca.key 4096
  - Command: `cat ca.key` (Verify `-----BEGIN PRIVATE KEY-----` / `-----END PRIVATE KEY-----` header & footer)
  - Generate Root CA certificate (`ca.crt`):

```bash 
openssl req -x509 -new -nodes \
  -key ca.key \
  -sha256 \
  -days 3650 \
  -out ca.crt \
  -subj "/CN=Internal Network Root CA"
```

  - Command: `cat ca.crt` (Verify `-----BEGIN CERTIFICATE-----` / `-----END CERTIFICATE-----` header & footer)

<Step 4>
**Machine 1 (Web Server)**
- Generate a Server Certificate
  - Command: cd ~
  - Command: mkdir https
  - Command: cd https
  - Command: openssl genrsa -out server.key 2048
  - Command: `cat server.key` (Verify `-----BEGIN PRIVATE KEY-----` / `-----END PRIVATE KEY-----` header & footer)
  - Generate Certificate Request (`csr`)

```bash
openssl req -new \
  -key server.key \
  -out server.csr \
  -subj "/CN=web.internal.local"
```

  - Command: `cat server.csr` (Verify `-----BEGIN CERTIFICATE REQUEST-----` / `-----END CERTIFICATE REQUEST-----` header & footer)
  - Command: nano server.ext

```INI
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage=digitalSignature,keyEncipherment
extendedKeyUsage=serverAuth
SubjectAltName=@alt_names

[alt_names]
DNS.1=web.internal.local
IP.1=192.168.103.XX
```

<Step 5>
- Copy the server.csr and server.ext to Target Server (`Machine 1`, `Web Server`).
- On `Machine 1` 
  - Command:scp server.ext server.csr Machine2@192.168.103.XX:~/internal-ca/ 
  - 

```bash
openssl x509 -req \
  -in server.csr \
  -CA ca.crt \
  -CAkey ca.key \
  -CAcreateserial \
  -out server.crt \
  -days 365 \
  -sha256 \
  -extfile server.ext
```


