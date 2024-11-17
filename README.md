# Traefik Docker Deployment

## Overview
This project provides a dynamic configuration for a Traefik instance with the following features:
- HTTPS with Let's Encrypt via Cloudflare
- Dynamic subdomain certificates (SANs)
- Catch-all routing
- Support for Authelia or Basic Authentication
- Forwarding to an internal secondary Traefik instance
- Detailed logging and access logs

---

## **Prerequisites**

### **0. See Technotim's Video ###
[![Traefik 3 and FREE Wildcard Certificates with Docker](https://img.youtube.com/vi/n1vOfdz5Nm8/0.jpg)](https://youtu.be/n1vOfdz5Nm8) 

### **1. Cloudflare Account**
You need a Cloudflare account with the domains you want to manage already added and configured. Ensure:
- The domains have valid DNS records (e.g., A or CNAME records).
- The domains are active and using Cloudflare's nameservers.

### **2. API Token**
To use Cloudflare's DNS challenge for Let's Encrypt, you need an API token with permissions to manage DNS records across all zones.

#### **Steps to Create an API Token**
1. **Log in to Cloudflare**:
   - Go to [Cloudflare Dashboard](https://dash.cloudflare.com/).

2. **Navigate to API Tokens**:
   - Click on your profile in the top-right corner and select **API Tokens**.

3. **Create a New Token**:
   - Click **Create Token**.

4. **Choose Custom Token Template**:
   - Under **Custom Token**, click **Get Started**.

5. **Set Permissions**:
   - Select the following permissions:
     - **Account Resources**:
       - **Account:** All Accounts
       - **Permissions:** Read
     - **Zone Resources**:
       - **Zone:** All Zones
       - **Permissions:** DNS: Edit

6. **Review and Generate**:
   - Click **Continue to Summary**, review the settings, and click **Create Token**.

7. **Copy the Token**:
   - Save the token securely. You will need it for the configuration in `host_vars`.

---

## **Configuration Files**

### **host_vars/<hostname>.yml**
This file contains host-specific variables for the Traefik instance. Example:

```yaml
traefik_active: true
traefik_version: "2.10"
traefik_http_port: 80
traefik_https_port: 443
ddns_active: true
traefik_log_level: "INFO"
docker_exposed_by_default: false
traefik_folder: "/etc/traefik"
cloudflare_mail: "your-cloudflare-email@example.com"
cloudflare_token: "your-cloudflare-api-token"
acme_storage_path: "/etc/traefik/acme.json"
traefik_sans_0: "example.com"
traefik_sans_1: "another-example.com"
traefik_sans_2: "subdomain.example.com"
