---
title: Lindungi server anda dengan fail2ban!
description: Fail2ban for your server!
date: 2023-04-09
tags:
  - vps
  - server
  - homelab
  - self-hosted
  - keamanan
  - security
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Tentu saja kita tidak ingin server kita digunakan oleh orang yang tidak bertanggung jawab. Salah satu serangan yang umumnya terjadi adalah serangan ssh brute force. 
Serangan ssh brute force pasti terjadi pada server yang terhubung secara publik. 
Serangan ini mungkin jarang berhasil apabila kita menggunakan user dan password yang tidak umum atau penuh dengan karakter spesial. 
Tapi ada baiknya kita memperlambat atau bahkan mencegah oknum-oknum ini agar tidak dapat dengan mudah melakukan serangan tersebut pada server kita. 
Dengan memanfaatkan fail2ban kita dapat melarang ip address yang gagal melakukan login ssh pada kurun waktu dan jumlah kegagalan login tertentu.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Instalasi fail2ban
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Sebelum melakukan instalasi fail2ban, saya menggunakan perangkat berikut pada catatan kali ini.
- The Windows Terminal 
- Server ubuntu 22 yang akan diamankan
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
<br>
1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Hal pertama yang dilakukan adalah masuk kedalam sesi terminal server ubuntu kita. Saya mengunakan perintah berikut pada terminal windows untuk menjalakan ssh.
```
ssh user@ip-server -i privatekey.key
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Gunakan perintah berikut untuk melakukan instalasi fail2ban.
```
sudo apt install fail2ban
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

3. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah melakukan instalasi, kita dapat mengecek apakah fail2ban berjalan dengan baik dengan menggunakan perintah berikut.
```
sudo systemctl status fail2ban
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Apabila tidak terdapat error seperti gambar berikut makan fail2ban sudah berjalan dengan benar. <br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup><br>
![fail2ban - status](/public/fail2ban-status.png) 
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

4. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dengan menggunakan konfigurasi default dari fail2ban sebenarnya sudah cukup. Namun apabila kita ingin melakukan konfigurasi lebih dalam, kita dapat membuat konfigurasi jail tambahan. Secara default konfigurasi fail2ban ada di ```/etc/fail2ban/jail.conf```. Untuk menambah konfigurasi jail fail2ban kita dapat membuat file ```jail.local``` pada direktori konfigurasi fail2ban. Untuk kasus ini saya mengunakan nano sebagai text editor saya.
```
sudo nano /etc/fail2ban/jail.local
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Masukan konfigurasi seperti berikut dan kemudian disimpan.
```
[sshd]
#Atur waktu ban selama 30 menit
bantime = 30m
#Atur jumlah percobaan login gagal sebanyak 3x sebelum dilakukan ban
maxretry=3
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Kemudian restart fail2ban dengan perintah berikut.
```
sudo systemctl restart fail2ban
``` 
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

5. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah melakukan semua langkah di atas. Kita dapat melakukan pengecekan configurasi yang berjalan dengan mengunakan perintah berikut.
```
sudo fail2ban-client status
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Gunakan perintah berikut untuk melihat statistik sshd dari fail2ban.
```
sudo fail2ban-client status sshd
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

![fail2ban - status ban](/public/fail2ban-statusban.png) 
<br>
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Selamat! fail2ban sudah berjalan dengan baik di server anda. 
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Referensi
```
https://github.com/fail2ban/fail2ban
```
