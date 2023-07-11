---
title: Setup server anda dengan cloud-Init
description: Cloud-Init
date: 2023-07-11
tags:
  - vps
  - server
  - homelab
  - self-hosted
  - keamanan
  - security
  - ssh
  - fail2ban
  - docker
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setup server anda langsung dari konfigurasi [cloud-init](https://cloud-init.io/).
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Cloud-init
PLACE HOLDER

::: details Cloud-init config
```
#cloud-config
groups:
  - docker
users:
  - name: amur
    ssh-authorized-keys:
      - ssh-rsa Np86zHMEEDuJfc= amur@Amur-Desktop
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    groups: sudo, docker
    shell: /bin/bash

package_update: true    # Update apt database
package_upgrade: true   # Upgrade apt packages
packages:
  - fail2ban
  - ca-certificates
  - curl
  - gnupg
  - lsb-release
  - unattended-upgrades

write_files:
  - path: /etc/fail2ban/jail.local
    content: |
         [sshd]
         #Atur waktu ban selama 30 menit
         bantime = 30m
         #Atur jumlah percobaan login gagal sebanyak 3x sebelum dilakukan ban
         maxretry=3

runcmd:
  - sed -i -e '/^PermitRootLogin/s/^.*$/PermitRootLogin no/' /etc/ssh/sshd_config
  - sed -i -e '/^#PasswordAuthentication/s/^.*$/PasswordAuthentication no/' /etc/ssh/sshd_config
  - sed -i -e '$aAllowUsers amur' /etc/ssh/sshd_config
  - systemctl restart ssh
  - systemctl restart fail2ban
  - mkdir -p /etc/apt/keyrings
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
  - echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
  - apt-get update
  - apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
  - systemctl enable docker
  - systemctl start docker

final_message: "The system is finally up with key pairing, fail2ban and docker installed. It took $UPTIME seconds"

```
::: 

![Cloud-init finish](/public/cloudinit-complete.png)