---
title: Enkripsi Data Anda Dengan gocryptfs
description: Encrypt your data with gocryptfs
date: 2023-09-01
tags:
  - vps
  - serve
  - keamanan
  - security
  - enkripsi
  - encryption
  - gocryptfs
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Terkadang kita bertanya-tanya, apakah data kita yang ada didalam server yang kita sewa apakah aman atau tidak. Untuk kali ini saya akan memberikan cara peenggunaan [gocryptfs](https://github.com/rfjakob/gocryptfs) untuk meng-enkripsi data sensitif anda yang ada didalam server. 
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Saya memilih [gocryptfs](https://github.com/rfjakob/gocryptfs) karana performanya yang sangat baik dibandingkan dengan kopetitornya. Tautan ke [*benchmark*](https://nuetzlich.net/gocryptfs/comparison/#performance).

### Instalasi gocryptfs
1. Lakukan instalasi [gocryptfs](https://github.com/rfjakob/gocryptfs) sesuai dengan distro yang anda gunakan. Untuk Ubuntu anda dapat menggunakan perintah berikut.
```
apt install gocryptfs
```

### Membuat folder ter-enkripsi

1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Pertama kita perlu membuat folder yang akan kita gunakan sebagai folder yang ber-isi file-file ter-enkrpsi.

```
mkdir secure
```

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Kemudian kita membuat folder *mounting* yang akan kita akses selayaknya folder normal.
```
mkdir /mnt/unsecure
```

3. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Setelah itu kita dapat melakukan inisialisasi [gocryptfs](https://github.com/rfjakob/gocryptfs) dan melakukan *mounting* folder ter-enkripsi agak bisa di akses selayaknya folder normal dengan perintah berikut.
```
gocryptfs -init secure
gocryptfs secure /mnt/unsecure
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
![gocryptfs](/public/gocryptfs1.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Simpan *master key* di tempat yang aman dan tidak mudah hilang. Apabila anda lupa dengan kata kunci dan tidak menyimpan *master key* ini maka data anda tidak akan dapat di buka.

4. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Apabila berhasil maka anda dapat menaruh file didalam folder unsecure dan file anda akan secara *on-the-fly* ter-enkripsi pada folder secure. 
Untuk melepas mounting agar file tidak dapat di akses secara sembarangan maka dapat digunakan perintah berikut.
```
fusermount -u unsecure
```

5. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Apabila anda ingin mengakes isi folder secure maka anda hanya tinggal mounting folder secure ke folder unsecure dengan perintah berikut.
```
gocryptfs secure unsecure
```