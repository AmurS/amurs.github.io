---
title: Docker dan Instalasinya
description: Post pertama di note rkgk!
date: 2023-03-29
tags:
  - docker
  - vps
  - server
  - homelab
  - self-hosted
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup> 
Sepertinya pada saat ini kita selalu mendengar tentang kontainer. Hampir semua *self-hosted apps* yang biasa dijalankan pada *baremetal* atau di dalam *Virtual Machine* (VM) memiliki pilihan untuk dapat dijalankan juga sebagai sebuah kontainer. [Docker](https://www.docker.com/) merupakan salah satu *platform* kontainer yang sering digunakan karena mudah dalam penggunaannya. Dalam catatan kali ini, saya akan membahas tentang bagaimana kita dapat menginstall dan menggunakan docker pada server linux kita. Tentu saja docker tidak hanya dapat digunakan pada server linux, namun saya lebih familiar dengan docker pada server linux jadi ya... 
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Instalasi Docker
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Sebelum melakukan instalasi, berikut komponen dan perangkat lunak yang saya gunakan pada catatan ini.
- *Virtual Private Server* (VPS) Ubuntu 22
- The Windows Terminal 
<br>
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Hal pertama yang perlu dilakukan adalah untuk masuk kedalam sesi terminal server linux kita. Dalam kasus ini, saya akan menginstall docker pada VPS Ubuntu 22 milik saya. Saya menggunakan The Windows Terminal dan ssh untuk masuk kedalam server linux tersebut.

```
ssh user@ip-vps 
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dengan perintah di atas, kita dapat melakukan SSH kedalam VPS kita. Karena saya menggunakan private key untuk masuk ke VPS maka saya perlu menggunakan perintah berikut.
```
ssh user@ip-vps -i privatekey.key
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah masuk kedalam sesi terminal server, kita perlu melakukan update. Berikut perintah yang perlu dilakukan.
```
sudo apt-get update
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

3. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Selanjutnya kita akan melakukan instalasi docker secara manual dengan menggunakan perintah berikut.
```
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah menambahkan source package docker untuk ubuntu, maka kita perlu melakukan update dan menginstall docker.
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah melakukan semua perintah di atas, maka docker sudah terinstall pada server linux. Namun docker pada saat ini bekerja dengan mengunakan user dengan akses root. Hal ini tentu saja kurang aman karena kontainer pada docker akan berjalan dengan akses penuh pada server kita. Oleh karena itu kita sebaiknya  melakukan tahap selanjutnya agar kontainer tidak berjalan sebagai root.

4. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Pada tahap ini, kita akan membuat group kusus untuk docker. 
```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```
Lakukan restart pada server agar group yang baru saja dibuat dapat diaplikasikan.  Untuk memastikan docker berjalan dengan baik, kita dapat melakukan perintah berikut untuk membuat kontainer docker yang berisi test *image* hello-world.
```
docker run hello-world
```
Apabila tidak terdapat *error logs* maka instalasi docker kita telah berhasil. 

### Referensi
```
https://docs.docker.com/engine/install/ubuntu/
https://docs.docker.com/engine/install/linux-postinstall/
```