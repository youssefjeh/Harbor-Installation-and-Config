
# 🐳 Harbor Registry Documentation

Welcome to the **Harbor Registry Docs** repository — a curated collection of technical notes, setup guides, and solutions gathered during real-world deployment and operation of [Harbor](https://goharbor.io/), a cloud-native registry for storing, signing, and scanning container images.

This repository is maintained for internal use, knowledge sharing, and continuous improvement of DevOps practices.


## 📁 Documentation Structure

| File | Description |
|------|-------------|
| [`installation-config.md`](./installation-config.md) | Step-by-step guide to install and configure Harbor (with or without Docker Compose). |
| [`integration-with-Trivy.md`](./integration-wit-Trivy.md) | Common issues encountered during installation and configuration and integration with **Trivy** — with fixes. |
| [`securing-harbor-https.md`](./securing-harbor-https.md) | How to secure Harbor with HTTPS using an internal CA certificate. |
| *(More coming soon)* | Additional guides like enabling [**Cosign**](https://github.com/sigstore/cosign), configuring Notary, LDAP integration, etc. |



## ✅ Goals of This Repo

- Serve as a reference during Harbor deployments and upgrades
- Record lessons learned and recurring issues
- Share DevOps practices and command-line recipes
- Facilitate collaboration between team members



## 🧠 How to Use

- Browse by filename or topic
- Apply steps carefully in your environment (customize domains, certs, paths, etc.)
- Suggest improvements or PRs if used in a team setting



## 📌 Notes

- This repo **does not** include sensitive information like real domain names, passwords, or production secrets.
- Replace all placeholder values (e.g. `example.com`, `internal-CA.pfx`) with your real environment-specific data.



## 📆 What's Next?

- [ ] Add guide for enabling **Cosign** for image signing
- [ ] Add Harbor **upgrade steps**
- [ ] Add reverse proxy configuration (e.g., NGINX or HAProxy)



## 🤝 Contributions

If you're collaborating on this internally or with a team:
- Feel free to fork and suggest PRs
- Keep the style consistent
- Respect the placeholder values and avoid leaking real infrastructure info

## 📜 License

This documentation is intended for internal/informational use. Not officially affiliated with the Harbor project.



