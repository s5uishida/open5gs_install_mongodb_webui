# Install MongoDB 7.0 and Open5GS WebUI
The process for the following OS is shown here.

- Ubuntu 22.04
- Ubuntu 24.04

---

### [Sample Configurations and Miscellaneous for Mobile Network](https://github.com/s5uishida/sample_config_misc_for_mobile_network)

---

<a id="toc"></a>

## Table of Contents

- [Install MongoDB 7.0](#install_mongodb)
- [Install Open5GS WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<a id="install_mongodb"></a>

## Install MongoDB 7.0

**Note. MongoDB v4.4.19 and later will not run on CPUs that do not support AVX instruction.
In this case, it is necessary to downgrade it to v4.4.18.
For reference, I wrote the steps to install v4.4.18 on Ubuntu 20.04 on Raspberry Pi 4B [here](https://github.com/s5uishida/install_mongodb_on_ubuntu_for_rp4b).**
```
# apt update
# apt install gnupg wget
# wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-7.gpg
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-7.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<a id="install_webui"></a>

## Install Open5GS WebUI

It is assumed that MongoDB 7.0 has been installed already.  
First, install Node.js, see [here](https://github.com/nodesource/distributions).
```
# apt install -y ca-certificates curl gnupg
# mkdir -p /etc/apt/keyrings
# curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
# echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
# apt update
# apt install -y nodejs
```
Then, install Open5GS WebUI.
```
# wget https://raw.githubusercontent.com/open5gs/open5gs/refs/heads/main/docs/assets/webui/install
# bash install
```
If necessary, set the IP address and port to bind as follows (ex. `192.168.0.111:3000`).

`/lib/systemd/system/open5gs-webui.service`
```diff
--- open5gs-webui.service.orig  2024-05-12 00:04:58.407915513 +0900
+++ open5gs-webui.service       2024-05-12 00:05:29.518339202 +0900
@@ -7,6 +7,8 @@
 
 WorkingDirectory=/usr/lib/node_modules/open5gs
 Environment=NODE_ENV=production
+Environment=HOSTNAME=192.168.0.111
+Environment=PORT=3000
 ExecStart=/usr/bin/node server/index.js
 Restart=always
 RestartSec=2
```
Update system services.
```
# systemctl daemon-reload
# systemctl restart open5gs-webui
```
---
<a id="changelog"></a>

## Changelog (summary)

- [2024.05.11] Initial release.
