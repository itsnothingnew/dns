This guide walks you through setting up a public DNS resolver using Unbound and dnsdist.

---

## 🛠 Requirements

- Ubuntu 24.04 LTS
- Root access
- SSL/TLS certificate and private key
- Basic Linux Knowledge

---

### 1. Install Unbound (Ubuntu-maintained)

```bash
sudo apt-get update
sudo apt-get install unbound -y
```

---

### 2. Install Latest dnsdist from PowerDNS

Follow instructions from the official dnsdist (PowerDNS) repository: [repo.powerdns.com](https://repo.powerdns.com/)

---

### 3. Update sysctl.conf

Edit the system control config:

```bash
sudo nano /etc/sysctl.conf
```

Then paste the contents from my github repo's sysctl.conf file **at the end of the file**, save, and exit.

Apply changes:

```bash
sudo sysctl -p
```

---

### 4. Configure Unbound

Create a new config file:

```bash
sudo nano /etc/unbound/unbound.conf.d/unbound.conf
```

Paste the contents from my github repo's unbound.conf file into it.

Remove default remote control config:

```bash
sudo rm /etc/unbound/unbound.conf.d/remote-control.conf
```

Restart Unbound:

```bash
sudo systemctl restart unbound
```

---

### 5. Configure dnsdist

Open the default config:

```bash
sudo nano /etc/dnsdist/dnsdist.conf
```

Delete all existing content and paste my github repo's dnsdist.conf content there.

---

### 6. Setup SSL/TLS Certificates for dnsdist

dnsdist may not work correctly if SSL/TLS certificates are stored in directories other than the default. Therefore, create a directory for storing the SSL/TLS certificates within the dnsdist root directory:

```bash
sudo mkdir -p /etc/dnsdist/certs
# Created the certs directory

sudo nano /etc/dnsdist/certs/cert.pem
# Paste your SSL certificate here

sudo nano /etc/dnsdist/certs/key.pem
# Paste your private key here
```

Update permissions (only if you encounter errors while restarting):

```bash
sudo chmod 600 /etc/dnsdist/certs/*
sudo chown _dnsdist:_dnsdist /etc/dnsdist/certs/*
```

Restart dnsdist:

```bash
sudo systemctl restart dnsdist
```

---

### Test the functionality of DoH and DoT

```bash
curl -v google.com --doh-url https://your-dns-domain.com/dns-query &> /dev/stdout | grep "* DoH"
```

```bash
kdig @your-dns-domain.com -p 853 +tls google.com
```

---
