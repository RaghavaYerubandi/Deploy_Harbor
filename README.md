# Deploy_Harbor
We will explore how to deploy a private registry to store the contaainer images using `Harbor`.
### About:
Harbor is an open-source container image registry that provides security, identity management, and vulnerability scanning for container images.
It is often used in Kubernetes env's as a private registry to store, scan and manage container images.
Harbor adds security, compliance, and management features on top of a basic registry like `Dockerhub` etc.
- Learn more about harbor here `https://goharbor.io/`.
### Key Features of Harbor: (WIP)
- Vulnerability Scanning
  - Scans images using tools like Trivy or Clair.
  - Displays vulnerabilities and provides details on fixes.
  - Helps in securing containerized applications before deployment.
- Image Signing & Security
  - Supports Notary for content trust and image signing.
  - Prevents running unsigned or tampered images.
- Replication Across Registries
  - Synchronize images between Harbor and other registries (e.g., Docker Hub, AWS ECR, GCR).
- Web UI & API
  - Web-based dashboard for managing images and projects.
  - REST API for automation and integration.
- Helm Chart & OCI Artifact Support
  - Stores and manages Helm charts and OCI artifacts.
  - Useful for Kubernetes deployments.
- Garbage Collection & Tag Retention
  - Automatically removes unused images to free up storage.
  - Configurable tag retention policies to keep the latest images.
- Role-Based Access Control (RBAC).
  - Users can be assigned roles to restrict image access.
  - Supports integration with LDAP/Active Directory.

### Deploy Harbor Using `Docker_Compose`:

**Prerequisites**:
- Docker must be installed on the system.
- Docker Compose is required for running Harbor as a standalone registry.

**Download Harbour**
- Download the latest release of Harbor.
~~~bash
wget https://github.com/goharbor/harbor/releases/latest/download/harbor-online-installer.tgz
~~~
**Extract the archive file**
- Extract the downloaded archive file
~~~bash
tar xvf harbor-online-installer.tgz
~~~
**Install the Harbor**
- Navigate to the Harbor directory.
~~~bash
cd harbor
~~~
**Note:** Inside the directory, you'll find a configuration file with name `harbor.yml.tmpl`. This file contains all the necessary configurations for Harbor.
- Copy the configuration file to create the actual configuration file.
~~~bash
cp harbor.yml.tmpl harbor.yml
~~~
- Edit the `harbor.yml` and modify the following changes.
  - Configure the DNS name for the Harbor registry.
  - Specify SSL certificates if using HTTPS.
  - Set the admin password for Harbor.

- Below is the harbor.yml file. Update the necessary fields as per your requirement.
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
**Install Harbor with Trivy Scanner`**
~~~bash
./install.sh --with-trivy
~~~
**Configure DNS**
- Map your server’s IP address to a DNS hostname by creating an A record in your DNS settings.
- This ensures the Harbor portal is accessible via a domain name.
**Access Harbor**
- Open the Harbor UI in a browser using the configured hostname.
- Log in with the admin username and password specified in `harbor.yml`.
**Verify the scanner**
- Navigate to Administration > Interrogation Services in the Harbor UI.
- Ensure that Trivy is listed as the default scanner as shown below.

![image](https://github.com/user-attachments/assets/03948343-ed4c-442f-ac4d-4f8746a24d6b)

**Scan Images for Vulnerabilities**
- Tag and push your images to the registry.
- Scan the Image to check for vulnerabilities.

![image](https://github.com/user-attachments/assets/4122615f-d21c-4f20-bc4d-adf4d53be9ec)

