# Deploy_Harbor
### About:
Harbor is an open-source container image registry that provides security, identity management, and vulnerability scanning for container images.
### Key Features of Harbor: (WIP)
Role-Based Access Control (RBAC)
Users can be assigned roles (Admin, Developer, Guest) to restrict image access.
Supports integration with LDAP/Active Directory.
2️⃣ Vulnerability Scanning
Scans images using tools like Trivy or Clair.
Displays vulnerabilities and provides details on fixes.
Helps in securing containerized applications before deployment.
3️⃣ Image Signing & Security
Supports Notary for content trust and image signing.
Prevents running unsigned or tampered images.
4️⃣ Replication Across Registries
Synchronize images between Harbor and other registries (e.g., Docker Hub, AWS ECR, GCR).
Supports multi-cloud and hybrid deployments.
5️⃣ Web UI & API
Web-based dashboard for managing images and projects.
REST API for automation and integration.
6️⃣ Helm Chart & OCI Artifact Support
Stores and manages Helm charts and OCI artifacts.
Useful for Kubernetes deployments.
7️⃣ Authentication & Authorization
Supports authentication via:
Database (local users)
LDAP/Active Directory
OIDC (Keycloak, Okta, etc.)
8️⃣ Garbage Collection & Tag Retention
Automatically removes unused images to free up storage.
Configurable tag retention policies to keep the latest images.

### Deploy Harbor Using `Docker_Compose`:

**Prerequisites**:
- Docker Should be installed.
- 

**Download Harbour**
- Download the latest release of the Harbor registry release.
~~~bash
wget https://github.com/goharbor/harbor/releases/latest/download/harbor-online-installer.tgz
~~~
**Extract the file**
- Extract the downloaded file
~~~bash
tar xvf harbor-online-installer.tgz
~~~
**Install the Harbor**
~~~bash
cd harbor
~~~
There is a file with `harbor.yml.tmpl`, where all the setting will be there.
- Copy the file using `cp harbor.yml.tmpl harbor.yml`.
- Edit the `harbor.yml` to set the harbor password & dns certificates & dns name.

~~~bash
# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: reg.mydomain.com 

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 80

# https related config
https:
  # https port for harbor, default is 443
  port: 443
  # The path of cert and key files for nginx
  certificate: /your/certificate/path
  private_key: /your/private/key/path

# # Uncomment following will enable tls communication between all harbor components
# internal_tls:
#   # set enabled to true means internal tls is enabled
#   enabled: true
#   # put your cert and key files on dir
#   dir: /etc/harbor/tls/internal
#   # enable strong ssl ciphers (default: false)
#   strong_ssl_ciphers: false

# Uncomment external_url if you want to enable external proxy
# And when it enabled the hostname will no longer used
# external_url: https://reg.mydomain.com:8433

# The initial password of Harbor admin
# It only works in first time to install harbor
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: Harbor12345
~~~
**Install Harbor with `Trivy`**
~~~bash
./install.sh --with-trivy
~~~
**Map your IP with DNS**
- Map your IP with the DNS, create a A name record.
- Access the portal with User & password provided.
**Verify the scanner**
- ![image](https://github.com/user-attachments/assets/03948343-ed4c-442f-ac4d-4f8746a24d6b)


**Scane Images for Vulnerability**
- Tag your Images & push it the registry
- 
