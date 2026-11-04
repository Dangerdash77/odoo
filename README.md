# Odoo

[![Build Status](https://runbot.odoo.com/runbot/badge/flat/1/master.svg)](https://runbot.odoo.com/runbot)
[![Tech Doc](https://img.shields.io/badge/master-docs-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/documentation/master)
[![Help](https://img.shields.io/badge/master-help-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/forum/help-1)
[![Nightly Builds](https://img.shields.io/badge/master-nightly-875A7B.svg?style=flat&colorA=8F8F8F)](https://nightly.odoo.com/)

Odoo is a suite of web based open source business apps.

The main Odoo Apps include an [Open Source CRM](https://www.odoo.com/page/crm),
[Website Builder](https://www.odoo.com/app/website),
[eCommerce](https://www.odoo.com/app/ecommerce),
[Warehouse Management](https://www.odoo.com/app/inventory),
[Project Management](https://www.odoo.com/app/project),
[Billing &amp; Accounting](https://www.odoo.com/app/accounting),
[Point of Sale](https://www.odoo.com/app/point-of-sale-shop),
[Human Resources](https://www.odoo.com/app/employees),
[Marketing](https://www.odoo.com/app/social-marketing),
[Manufacturing](https://www.odoo.com/app/manufacturing),
[...](https://www.odoo.com/)

Odoo Apps can be used as stand-alone applications, but they also integrate seamlessly so you get
a full-featured [Open Source ERP](https://www.odoo.com) when you install several Apps.

## Getting started with Odoo

For a standard installation please follow the [Setup instructions](https://www.odoo.com/documentation/master/administration/install/install.html)
from the documentation.

### How to Run Odoo Locally (Development)

#### 1. Prerequisites
- **Python**: Python 3.10 to 3.12 is recommended.
- **PostgreSQL**: PostgreSQL 12+ (e.g. PostgreSQL 16/17/18) installed and running locally on port 5432.
- **C/C++ Build Tools & Libraries**:
  - Windows: Visual Studio C++ Build Tools (required for compiling native wheels like `psycopg2`, `lxml`, etc.).
  - Linux/macOS: `build-essential`, `python3-dev`, `libxml2-dev`, `libxslt1-dev`, `libldap2-dev`, `libsasl2-dev`, `libpq-dev`.
- **wkhtmltopdf** (optional, for PDF report generation): version 0.12.5 with patched qt.

#### 2. PostgreSQL Setup
Ensure your PostgreSQL service is running and create a database user:
``bash
# Example creating user 'odoo' with password 'odoo' and superuser rights
createuser -s -U postgres odoo
# Set password if needed:
# psql -U postgres -c "ALTER USER odoo WITH PASSWORD 'odoo';"
``

#### 3. Set Up Python Virtual Environment
From the repository root (D:\Dine-IIn\odoo):

``bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows (PowerShell):
.\venv\Scripts\Activate.ps1
# Windows (cmd):
.\venv\Scripts\activate.bat
# Linux / macOS:
source venv/bin/activate

# Upgrade pip and setuptools
python -m pip install --upgrade pip setuptools wheel

# Install dependencies
pip install -r requirements.txt
``

#### 4. Running Odoo

Run Odoo with odoo-bin:

``bash
# Basic startup (creates/initializes a test database 'odoo_dev' with base addons)
python odoo-bin -d odoo_dev -r odoo -w odoo --db_host=localhost --db_port=5432 -i base --addons-path=addons

# For subsequent runs:
python odoo-bin -d odoo_dev -r odoo -w odoo --db_host=localhost --db_port=5432 --addons-path=addons
``

> **Note**: If your PostgreSQL user matches your OS user or uses peer/trust authentication on localhost, -r and -w flags can be omitted.

#### 5. Access the Web Interface
Once running, open your browser and navigate to:
`
http://localhost:8069
`


To learn the software, we recommend the [Odoo eLearning](https://www.odoo.com/slides),
or [Scale-up, the business game](https://www.odoo.com/page/scale-up-business-game).
Developers can start with [the developer tutorials](https://www.odoo.com/documentation/master/developer/howtos.html).

## Security

If you believe you have found a security issue, check our [Responsible Disclosure page](https://www.odoo.com/security-report)
for details and get in touch with us via email.

## Licensing & Costs: Is this Odoo ERP completely free?

**Yes! This repository is Odoo Community Edition, which is 100% free and open-source under the LGPLv3 (GNU Lesser General Public License v3).**

- **No User Limits**: You can create unlimited employee, client, and admin accounts without per-user licensing fees.
- **No Feature Expiry or Subscriptions**: You own the code, the database, and the infrastructure.
- **Commercial Use Allowed**: You are completely free to use it for your company, run production business workflows, modify it, add custom modules, and host it on your own servers without paying Odoo S.A.
- **Differences from Odoo Enterprise**: Odoo S.A. sells a proprietary "Enterprise" subscription that adds proprietary mobile wrappers, automated accounting bank sync, and full-featured studio. However, core CRM, Sales, Invoicing, Project, Stock, POS, and HR in this Community edition are fully functional and free forever.

---

## Initial Company Setup Guide

When accessing your Odoo database as `admin` for the first time:

### 1. Company Identity & Branding
1. Go to **Settings** (gear icon) -> **General Settings** -> **Companies** -> **Update Info**.
2. Enter your **Company Name**, **Address**, **Tax ID / GSTIN / VAT**, **Phone**, **Email**, and **Currency**.
3. Upload your company logo.

### 2. Document Layout (Invoices & Quotations)
1. In **Settings** -> **General Settings** -> **Document Layout**, click **Configure Document Layout**.
2. Select your layout style (*Light*, *Boxed*, *Bold*, or *Clean*) and your brand colors.
3. Add your company tagline and footer notes (e.g. payment terms, registration numbers).

### 3. Bank Account Configuration
1. Go to **Invoicing** -> **Configuration** -> **Add a Bank Account**.
2. Add your Bank Name, Account Number, and IFSC/SWIFT code so customers know where to send payments.

---

## Production Deployment on Google Cloud VM (e2-micro)

The Google Cloud `e2-micro` instance has 2 vCPUs and 1 GB RAM (eligible for GCP Free Tier). To run Odoo and PostgreSQL reliably in 1 GB RAM, follow these optimized steps:

### 1. Create the VM Instance
- **Machine type**: `e2-micro` (us-central1, us-east1, or us-west1)
- **OS**: Ubuntu 22.04 LTS or 24.04 LTS (30 GB Standard Persistent Disk)
- **Firewall**: Allow HTTP and HTTPS traffic.

### 2. Configure 4 GB Swap Space (Essential for 1 GB RAM)
``bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl vm.swappiness=10
``

### 3. Install System Dependencies & PostgreSQL
``bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-pip python3-dev python3-venv git build-essential \
    libxml2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev libssl-dev \
    libffi-dev libjpeg-dev libpq-dev liblcms2-dev libblas-dev libatlas-base-dev \
    postgresql postgresql-contrib nginx certbot python3-certbot-nginx
``

### 4. Configure PostgreSQL
``bash
sudo -u postgres createuser -s odoo
sudo -u postgres psql -c "ALTER USER odoo WITH PASSWORD 'strong_db_password';"
``

### 5. Deploy Odoo Source & Virtual Environment
``bash
sudo adduser --system --quiet --shell=/bin/bash --home=/opt/odoo --group odoo
sudo -u odoo git clone https://github.com/your-username/your-repo.git /opt/odoo/odoo-src
sudo -u odoo python3 -m venv /opt/odoo/venv
sudo /opt/odoo/venv/bin/pip install --upgrade pip wheel
sudo /opt/odoo/venv/bin/pip install -r /opt/odoo/odoo-src/requirements.txt
``

### 6. Create Production `/etc/odoo.conf`
``ini
[options]
admin_passwd = YourStrongMasterPassword
db_host = 127.0.0.1
db_port = 5432
db_user = odoo
db_password = strong_db_password
addons_path = /opt/odoo/odoo-src/addons,/opt/odoo/odoo-src/custom_addons
logfile = /var/log/odoo/odoo-server.log
http_port = 8069

; Memory optimization for e2-micro (1 GB RAM)
workers = 0
limit_memory_hard = 800000000
limit_memory_soft = 650000000
limit_time_cpu = 600
limit_time_real = 1200
proxy_mode = True
``
Set permissions:
``bash
sudo mkdir /var/log/odoo
sudo chown odoo:odoo /var/log/odoo
sudo chmod 640 /etc/odoo.conf
sudo chown odoo:odoo /etc/odoo.conf
``

### 7. Setup Systemd Service (Auto-Start on Boot)
Create `/etc/systemd/system/odoo.service`:
``ini
[Unit]
Description=Odoo ERP
After=network.target postgresql.service

[Service]
Type=simple
User=odoo
Group=odoo
ExecStart=/opt/odoo/venv/bin/python3 /opt/odoo/odoo-src/odoo-bin -c /etc/odoo.conf
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
``
Enable and start:
``bash
sudo systemctl daemon-reload
sudo systemctl enable --now odoo
``

### 8. Nginx Reverse Proxy & SSL (HTTPS)
Create `/etc/nginx/sites-available/odoo`:
``nginx
upstream odoo {
    server 127.0.0.1:8069;
}

server {
    listen 80;
    server_name erp.yourdomain.com;

    proxy_read_timeout 720s;
    proxy_connect_timeout 720s;
    proxy_send_timeout 720s;

    gzip on;
    gzip_types text/css text/plain text/xml application/xml application/json application/javascript;

    location / {
        proxy_pass http://odoo;
        proxy_set_header X-Forwarded-Host System.Management.Automation.Internal.Host.InternalHost;
        proxy_set_header X-Forwarded-For ;
        proxy_set_header X-Forwarded-Proto ;
        proxy_set_header X-Real-IP ;
    }

    client_max_body_size 64M;
}
``
Enable and reload:
``bash
sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t && sudo systemctl restart nginx
``

Generate free SSL certificate:
``bash
sudo certbot --nginx -d erp.yourdomain.com
``

### 9. Maintenance & Management Commands
| Task | Command |
|---|---|
| **View Live Logs** | `sudo journalctl -u odoo -f` or `sudo tail -f /var/log/odoo/odoo-server.log` |
| **Restart Service** | `sudo systemctl restart odoo` |
| **Upgrade Modules** | `/opt/odoo/venv/bin/python3 /opt/odoo/odoo-src/odoo-bin -c /etc/odoo.conf -d <dbname> -u <module_name>` |
| **Database Backup** | `sudo -u odoo pg_dump <dbname> > backup_.sql` |
## Licensing & Costs: Is this Odoo ERP completely free?

**Yes! This repository is Odoo Community Edition, which is 100% free and open-source under the LGPLv3 (GNU Lesser General Public License v3).**

- **No User Limits**: You can create unlimited employee, client, and admin accounts without per-user licensing fees.
- **No Feature Expiry or Subscriptions**: You own the code, the database, and the infrastructure.
- **Commercial Use Allowed**: You are completely free to use it for your company, run production business workflows, modify it, add custom modules, and host it on your own servers without paying Odoo S.A.
- **Differences from Odoo Enterprise**: Odoo S.A. sells a proprietary "Enterprise" subscription that adds proprietary mobile wrappers, automated accounting bank sync, and studio. However, core CRM, Sales, Invoicing, Project, Stock, POS, and HR in this Community edition are fully functional and free forever.

---

## Initial Company Setup Guide

When accessing your Odoo database as `admin` for the first time:

### 1. Company Identity & Branding
1. Go to **Settings** (gear icon) -> **General Settings** -> **Companies** -> **Update Info**.
2. Enter your **Company Name**, **Address**, **Tax ID / GSTIN / VAT**, **Phone**, **Email**, and **Currency**.
3. Upload your company logo.

### 2. Document Layout (Invoices & Quotations)
1. In **Settings** -> **General Settings** -> **Document Layout**, click **Configure Document Layout**.
2. Select your layout style (*Light*, *Boxed*, *Bold*, or *Clean*) and your brand colors.
3. Add your company tagline and footer notes (e.g. payment terms, registration numbers).

### 3. Bank Account Configuration
1. Go to **Invoicing** -> **Configuration** -> **Add a Bank Account**.
2. Add your Bank Name, Account Number, and IFSC/SWIFT code so customers know where to send payments.

---

## Production Deployment on Google Cloud VM (e2-micro)

The Google Cloud `e2-micro` instance has 2 vCPUs and 1 GB RAM (eligible for GCP Free Tier). To run Odoo and PostgreSQL reliably in 1 GB RAM, follow these optimized steps:

### 1. Create the VM Instance
- **Machine type**: `e2-micro` (us-central1, us-east1, or us-west1)
- **OS**: Ubuntu 22.04 LTS or 24.04 LTS (30 GB Standard Persistent Disk)
- **Firewall**: Allow HTTP and HTTPS traffic.

### 2. Configure 4 GB Swap Space (Essential for 1 GB RAM)
```bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl vm.swappiness=10
```

### 3. Install System Dependencies & PostgreSQL
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-pip python3-dev python3-venv git build-essential \
    libxml2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev libssl-dev \
    libffi-dev libjpeg-dev libpq-dev liblcms2-dev libblas-dev libatlas-base-dev \
    postgresql postgresql-contrib nginx certbot python3-certbot-nginx
```

### 4. Configure PostgreSQL
```bash
sudo -u postgres createuser -s odoo
sudo -u postgres psql -c "ALTER USER odoo WITH PASSWORD 'strong_db_password';"
```

### 5. Deploy Odoo Source & Virtual Environment
```bash
sudo adduser --system --quiet --shell=/bin/bash --home=/opt/odoo --group odoo
sudo -u odoo git clone https://github.com/your-username/your-repo.git /opt/odoo/odoo-src
sudo -u odoo python3 -m venv /opt/odoo/venv
sudo /opt/odoo/venv/bin/pip install --upgrade pip wheel
sudo /opt/odoo/venv/bin/pip install -r /opt/odoo/odoo-src/requirements.txt
```

### 6. Create Production `/etc/odoo.conf`
```ini
[options]
admin_passwd = YourStrongMasterPassword
db_host = 127.0.0.1
db_port = 5432
db_user = odoo
db_password = strong_db_password
addons_path = /opt/odoo/odoo-src/addons,/opt/odoo/odoo-src/custom_addons
logfile = /var/log/odoo/odoo-server.log
http_port = 8069

; Memory optimization for e2-micro (1 GB RAM)
workers = 0
limit_memory_hard = 800000000
limit_memory_soft = 650000000
limit_time_cpu = 600
limit_time_real = 1200
proxy_mode = True
```
Set permissions:
```bash
sudo mkdir /var/log/odoo
sudo chown odoo:odoo /var/log/odoo
sudo chmod 640 /etc/odoo.conf
sudo chown odoo:odoo /etc/odoo.conf
```

### 7. Setup Systemd Service (Auto-Start on Boot)
Create `/etc/systemd/system/odoo.service`:
```ini
[Unit]
Description=Odoo ERP
After=network.target postgresql.service

[Service]
Type=simple
User=odoo
Group=odoo
ExecStart=/opt/odoo/venv/bin/python3 /opt/odoo/odoo-src/odoo-bin -c /etc/odoo.conf
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```
Enable and start:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now odoo
```

### 8. Nginx Reverse Proxy & SSL (HTTPS)
Create `/etc/nginx/sites-available/odoo`:
```nginx
upstream odoo {
    server 127.0.0.1:8069;
}

server {
    listen 80;
    server_name erp.yourdomain.com;

    proxy_read_timeout 720s;
    proxy_connect_timeout 720s;
    proxy_send_timeout 720s;

    gzip on;
    gzip_types text/css text/plain text/xml application/xml application/json application/javascript;

    location / {
        proxy_pass http://odoo;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
    }

    client_max_body_size 64M;
}
```
Enable and reload:
```bash
sudo ln -s /etc/nginx/sites-available/odoo /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t && sudo systemctl restart nginx
```

Generate free SSL certificate:
```bash
sudo certbot --nginx -d erp.yourdomain.com
```

### 9. Maintenance & Management Commands
| Task | Command |
|---|---|
| **View Live Logs** | `sudo journalctl -u odoo -f` or `sudo tail -f /var/log/odoo/odoo-server.log` |
| **Restart Service** | `sudo systemctl restart odoo` |
| **Upgrade Modules** | `/opt/odoo/venv/bin/python3 /opt/odoo/odoo-src/odoo-bin -c /etc/odoo.conf -d <dbname> -u <module_name>` |
| **Database Backup** | `sudo -u odoo pg_dump <dbname> > backup_$(date +%F).sql` |