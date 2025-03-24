# Harbor Installation and Configuration
### **How to Install and Configure Harbor**

Harbor is an open-source container image registry that provides security, identity, and compliance features. Below are the steps to install and configure Harbor.

---

## **1. Prerequisites**
Before installing Harbor, ensure you have the following:

- **OS**: Linux-based system (Ubuntu 20.04, CentOS 7+, or similar)
- **Docker**: Installed and running (`Docker 19.03+`)
- **Docker Compose**: Installed (`Docker Compose v1.29+`)
- **Helm (Optional)**: If deploying on Kubernetes

**Install Docker & Docker Compose:**
```bash
# Install Docker
curl -fsSL https://get.docker.com | bash
sudo systemctl enable docker
sudo systemctl start docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verify installation
docker --version
docker-compose --version
```

---

## **2. Download and Install Harbor**
1. **Download the latest Harbor release:**
   ```bash
   wget https://github.com/goharbor/harbor/releases/download/v2.9.0/harbor-offline-installer-v2.9.0.tgz
   ```
   *(Replace `v2.9.0` with the latest version available.)*

2. **Extract the package:**
   ```bash
   tar -xvzf harbor-offline-installer-*.tgz
   cd harbor
   ```

---

## **3. Configure Harbor**
Modify the **Harbor configuration file (`harbor.yml`)**:

```bash
cp harbor.yml.tmpl harbor.yml
nano harbor.yml
```

### **Key Configurations in `harbor.yml`:**
- **Hostname:** Change it to your domain or IP address:
  ```yaml
  hostname: myregistry.example.com
  ```
- **HTTPS (Optional):** If using SSL, provide paths to certificates:
  ```yaml
  https:
    port: 443
    certificate: /your/path/to/cert.crt
    private_key: /your/path/to/key.key
  ```
- **Harbor Admin Password:** Set an admin password:
  ```yaml
  harbor_admin_password: StrongPassword123
  ```
- **Database Password:** Customize database credentials (optional).

---

## **4. Install and Start Harbor**
After modifying the configuration, install Harbor:

```bash
sudo ./install.sh
```

### **Check Running Containers:**
```bash
docker ps
```
You should see multiple Harbor containers running (e.g., `harbor-core`, `harbor-db`, `harbor-portal`).

---

## **5. Access and Use Harbor**
- Open a web browser and go to `http://your-hostname`
- Login with:
  - **Username**: `admin`
  - **Password**: (as set in `harbor.yml`)

---

## **6. Pushing Images to Harbor**
1. **Login to Harbor:**
   ```bash
   docker login myregistry.example.com
   ```
2. **Tag an Image:**
   ```bash
   docker tag nginx:latest myregistry.example.com/library/nginx:latest
   ```
3. **Push the Image:**
   ```bash
   docker push myregistry.example.com/library/nginx:latest
   ```

---

## **7. Enable Notary (Optional)**
To enable **content trust** with Notary:
- Modify `harbor.yml`:
  ```yaml
  notary:
    enabled: true
  ```
- Restart Harbor:
  ```bash
  docker-compose down
  docker-compose up -d
  ```

---

## **8. Configure Garbage Collection (Optional)**
```bash
docker exec -it harbor-core bash
harbor_gc
```

---

### **Conclusion**
Harbor is now installed and configured as a private container registry. You can integrate it with Kubernetes, set up image replication, or enforce security policies.
