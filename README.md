# sno-labs
# 🧩 setup-dnsmasq-sno

Script to install and configure **dnsmasq** for a **Single Node OpenShift (SNO)** lab.

---

## 📌 Description

This script automates the installation and configuration of **dnsmasq** to resolve the main endpoints of a **Single Node OpenShift cluster**, including:

- `api.<cluster>.<basedomain>`
- `apps.<cluster>.<basedomain>`
- `api-int.<cluster>.<basedomain>`

It also adds external DNS servers (Cloudflare and Google by default) to forward non-cluster queries.

---

## 🛠️ Requirements

- Linux with support for `apt`, `yum`, `dnf`, or `zypper`
- **Root** privileges (`sudo`)
- Network access to install packages (if dnsmasq is not already installed)

---

## 📥 Installation

Clone this repository (GitHub Enterprise) or download the script directly.

**HTTPS:**

```bash
git clone https://github.ibm.com/Andre-Casagrande/sno-labs.git
cd sno-labs
chmod +x setup-dnsmasq-sno.sh
```

**SSH (alternative):**

```bash
git clone git@github.ibm.com:Andre-Casagrande/sno-labs.git
cd sno-labs
chmod +x setup-dnsmasq-sno.sh
```

---

## ▶️ Usage

Run the script as root:

```bash
sudo ./setup-dnsmasq-sno.sh
```

During execution, the script will ask the following **questions in English**:

- **Cluster name** (e.g., `snotest`)  
- **Host name** (e.g., `bastion`)  
- **Base domain** (e.g., `p1292.cecc.ihost.com`)  
- **Master IP** (e.g., `129.40.94.218`)  
- **Bastion IP** (e.g., `129.40.94.218`)  
- **External DNS servers** (default: `1.1.1.1,8.8.8.8`)  

---

## 📄 Example Generated Configuration

```ini
# /etc/dnsmasq.d/sno.conf
listen-address=129.40.94.218
bind-interfaces
domain-needed
bogus-priv
no-resolv
server=1.1.1.1
server=8.8.8.8

# SNO records
address=/api.snotest.p1292.cecc.ihost.com/129.40.94.218
address=/apps.snotest.p1292.cecc.ihost.com/129.40.94.218
address=/api-int.snotest.p1292.cecc.ihost.com/129.40.94.218

log-facility=/var/log/dnsmasq-sno.log
cache-size=1000
```

---

## 🔍 Verification

After running the script, test the DNS resolution:

```bash
dig @<BastionIP> api.<cluster>.<domain> +short
dig @129.40.94.218 api.snotest.p1292.cecc.ihost.com +short

nslookup apps.snotest.p1292.cecc.ihost.com 129.40.94.218
```

The result should return the configured **Master IP**.

---

## ⚠️ Notes

- The script creates backups of existing configs in `/root/dnsmasq-backups-<timestamp>`  
- To check detailed query logs:  

```bash
tail -f /var/log/dnsmasq-sno.log
```

- Ensure that clients point to the **Bastion IP** as their DNS server.

---

## 📅 Version

- **v1.0** – 2025-10-03

---

## 👤 Author

**André Casagrande**  
📧 [andre.casagrande@ibm.com](mailto:andre.casagrande@ibm.com)  
🔗 [LinkedIn](https://www.linkedin.com/in/andrebermudes/)  
