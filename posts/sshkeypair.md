---
title: Tinggalkan password, gunakan key pairs!
description: SSH Key-pair based login.
date: 2023-04-11
tags:
  - vps
  - server
  - homelab
  - self-hosted
  - keamanan
  - security
  - ssh
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup> 
Mungkin apabila ada yang memperhatikan catatan saya, kerap kali untuk masuk kedalam sesi terminal saya menggunakan file ```keypair.key``` untuk autentikasi. 
Ini merupakan [*key-based authentication*](https://en.wikipedia.org/wiki/Key_authentication). *Key-based authentication* merupakan metode autentikasi yang lebih aman dari pada *password* yang kerap kita gunakan.
Dengan menggunakan pasangan kunci publik dan privat, *server* dan *client* dapat melakukan proses autentikasi secara aman dan terhindar dari [*keylogger*](https://en.wikipedia.org/wiki/Keystroke_logging) dan [CSRF *attacks*](https://en.wikipedia.org/wiki/Cross-site_request_forgery).
Berikut akan saya berikan langkah-langkah yang perlu dilakukan untuk menggunakan *key-based authentication* pada *server* kita.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Pembuatan *key pairs*
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Sebelum kita mengubah konfigurasi server agar dapat melakukan autentikasi dengan pasangan kunci, kita perlu membuat pasangan kunci yang akan digunakan.
Karena saya menggunakan OS Windows maka pada catatan ini saya akan membuat pasangan kunci dengan aplikasi yang ada pada Windows.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Pada windows kita dapat menggunakan fitur bawaan windows yaitu [OpenSSH](https://en.wikipedia.org/wiki/OpenSSH). Untuk mengecek apakah OpenSSH sudah terinstall pada Windows, kita dapat melakukan pencarian ```Optional Features``` di start menu dan mencari ```OpenSSH``` seperti pada gambar berikut. Apabila belum ada, maka kita dapat menambahnya dengan ```Add a feature```.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![OpenSSH Windows](/public/openssh-win.png)

<br>
Untuk membuat pasangan kunci kita dapat mengikuti langkah-langkah berikut.

1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Buka terminal dengan *administrator privileges* (run as admin) dan masukan perintah berikut untuk membuat pasangan kunci.
```
ssh-keygen -t ECDSA
```
Perintah ```-t ECDSA``` akan membuat pasangan kunci dengan algoritma ```ECDSA```, ssh-keygen dapat membuat pasangan kunci dengan algoritma  ```DSA, RSA, ECDSA, atau Ed25519```.
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah memasukan perintah di atas maka anda akan diminta untuk memasukan *passphrase*. Ada baiknya untuk menyimpan atau mengingat *passphrase* ini apabila anda memasukan *passphrase* pada saat pembuatan pasangan kunci. Pasangan kunci akan tersimpan di folder ```C:\Users\$user\.ssh``` dengan nama ```id_ecdsa``` untuk *private key* dan ```id_ecdsa.pub``` untuk *publik key*.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![OpenSSH Windows](/public/openssh-win2.png)
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
::: danger PERHATIAN
Simpanlah *private key* di tempat yang aman, karena *private key* ini setara dengan *password* anda.
:::
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Konfigurasi SSH *server*
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah kita memiliki pasangan kunci privat dan publik, kita dapat memlakukan konfigurasi pada *server* agar dapat melakukan autentikasi dengan pasangan kunci. Berikut langkah-langkah yang perlu dilakukan.

1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Hal pertama yang dilakukan adalah masuk kedalam sesi terminal *server* anda. Setelah masuk kita akan membuat folder untuk *public key* yang telah kita buat.
```
mkdir ~/.ssh
chmod 700 ~/.ssh
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Ada banyak cara untuk memindahkan *public key* ke *server kita*. Pada catatan ini saya akan meng-*copy* isi *public key* ke file ```authorized_keys``` secara manual karena menurut saya ini lebih mudah dilakukan. Saya akan membuat file ```authorized_keys``` dengan menggunakan perintah berikut.
```
nano ~/.ssh/authorized_keys
```
Buka file *public key* dengan mengunakan notepad dan *copy* seluruh isinya kedalam terminal seperti gambar berikut kemudian simpan.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![Public Key](/public/publickey.png)
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup><br>
Ubah izin file dengan perintah berikut.
```
chmod 600 ~/.ssh/authorized_keys
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

3. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah membuat file  ```authorized_keys```, kita akan mengubah konfigurasi SSH agar hanya dapat melakukan autentikasi dengan pasangan kunci. Gunakan perintah berikut untuk mengubah konfigurasi SSH.
```
sudo nano /etc/ssh/sshd_config
```
Kemudian cari ```#PasswordAuthentication no``` dan ubah menjadi ```PasswordAuthentication no``` seperti gambar berikut dan simpan.
::: danger PERHATIAN
Dengan mengubah konfigurasi ini, maka anda bisa saja terkunci dari *server* apabila ada kesalahan dalam mengikuti langkah-langkah sebelumnya. 
:::
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![SSH Config](/public/sshconfig.png)
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

4.  <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
*Restart service* SSH dengan mengunakan perintah berikut untuk mengaktifkan konfigurasi yang baru.
```
sudo systemctl restart ssh
```
Selamat, server anda sudah dikonfigurasi untuk hanya dapat melakukan autentikasi dengan mengunakan pasangan kunci.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Untuk masuk ke dalam *server*, buka terminal di lokasi *private key* tersimpan dan masukan perintah berikut.
```
ssh -i id_ecdsa user@host
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![Public Key](/public/sshkeylogin.png)

### Referensi
```
https://www.redhat.com/sysadmin/key-based-authentication-ssh
https://phoenixnap.com/kb/ssh-with-key
https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
```