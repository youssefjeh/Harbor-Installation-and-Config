## 🔐 Securing Harbor Registry with HTTPS Using Internal CA Certificate

### 🎯 Goal

Configure Harbor Registry to use HTTPS with a TLS certificate signed by an internal **Certificate Authority (CA)**, ensuring secure and trusted connections.

---

### 🧾 Prerequisites

* A `.pfx` file provided by your infrastructure/security team, containing the internal CA certificate and private key.
* Access to the Harbor host machine.
* OpenSSL installed on the system.

---

### 1. 📦 Extract Certificate and Private Key from `.pfx`

```bash
# Extract certificate
openssl pkcs12 -in internal-ca.pfx -out internal-CA.crt -clcerts -nokeys

# Extract private key
openssl pkcs12 -in internal-ca.pfx -out internal-CA.key -nocerts -nodes
```

**Output files:**

```
/usr/local/share/ca-certificates/
├── internal-CA.crt
└── internal-CA.key
```

---

### 2. 🔐 Generate a Private Key for Your Harbor Domain

```bash
openssl genrsa -out registry.example.com.key 2048
```

---

### 3. 📝 Create OpenSSL Config File for SAN (Subject Alternative Name)

Create a file called `san.conf`:

```ini
[ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = req_ext
prompt             = no

[ req_distinguished_name ]
C  = XX
ST = State
L  = City
O  = Organization
OU = Department
CN = registry.example.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = registry.example.com
DNS.2 = *.example.com
```

> Adjust SANs and distinguished name fields (`C`, `ST`, `L`, etc.) to match your environment.

---

### 4. 📨 Generate a Certificate Signing Request (CSR)

```bash
openssl req -new \
  -key registry.example.com.key \
  -out registry.example.com.csr \
  -config san.conf
```

---

### 5. ✅ Sign the CSR with Your Internal CA

```bash
openssl x509 -req \
  -in registry.example.com.csr \
  -CA /usr/local/share/ca-certificates/internal-CA.crt \
  -CAkey /usr/local/share/ca-certificates/internal-CA.key \
  -CAcreateserial \
  -out registry.example.com.crt \
  -days 365 \
  -sha256 \
  -extfile san.conf \
  -extensions req_ext
```

> ⚠️ Don't forget to include the `-extfile san.conf` and `-extensions req_ext` flags — these ensure SANs are embedded in the final certificate.

---

### 6. 🔎 Verify Subject Alternative Names (SAN)

```bash
openssl x509 -in registry.example.com.crt -noout -text | grep -A1 "Subject Alternative Name"
```

Expected output should include:

```
X509v3 Subject Alternative Name:
    DNS:registry.example.com, DNS:*.example.com
```

---

### 🚀 Next Steps

* Deploy `registry.example.com.crt` and `registry.example.com.key` to the Harbor host.
* Update Harbor configuration (`harbor.yml`) to enable HTTPS with the new certificate.
* Reconfigure Harbor or restart the services to apply changes.

---

### ✅  Harbor Config (HTTPS)

```yaml
https:
  port: 443
  certificate: /path/to/registry.example.com.crt
  private_key: /path/to/registry.example.com.key
```
