---
title: Alokasi volume kontainer dengan virtual drive
description: The easy way to presistent storage for docker container.
date: 2023-05-1
tags:
  - docker
  - vps
  - server
  - homelab
  - self-hosted
---

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Pada post kali ini saya ingin memberikan salah satu cara untuk mengalokasikan besaran penyimpanan yang dapat digunakan oleh kontainer docker kita.
Apabila anda terbiasa dengan tempfs yang bersifat *ephemeral* atau sementara selama kontainer berjalan, untuk kali ini saya akan mengalokasi penyimpanan yang bersifat *presistent*.
Ada banyak cara sebenarnya yang dapat dilakukan, namun saya lebih suka menggunakan cara ini karena sangat mudah dan tidak memerlukan perubahan dalam file docker-compose kita.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Membuat virtual drive
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

1. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Hal pertama yang dilakukan adalah masuk kedalam sesi terminal *server* Ubuntu kita. Saya mengunakan perintah berikut pada terminal Windows untuk menjalakan SSH.
```
ssh user@ip-server -i privatekey.key
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

2. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Arahkan terminal ke tempat docker-compose yang akan kita jalankan atau di tempat manapun yang ingin anda gunakan. Gunakan perintah berikut untuk membuat file ```drive1g.img``` dengan ukuran 1GB.
```
dd if=/dev/zero of=drive1g.img bs=1M count=1000 
```
Anda dapat mengubah ```count=1000``` dengan size yang anda inginkan. Contohnya ```count=10000``` untuk 10GB dan ```count=100000``` untuk 100GB.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

3. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah anda berhasil membuat file ```drive1g.img```, maka anda perlu menambahkan *file system* ext4 pada file tersebut dengan menggunakan perintah berikut.
```
mkfs -t ext4 drive1g.img
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Apabila anda sudah mengikuti semua perintah di atas, maka anda sudah berhasil membuat file *virtual drive* dengan *file system* ext4.
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

![Virtual drive](/public/vdrive.png) 
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

4. <sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Selanjutnya hanya anda tinggal perlu melakukan mounting file drive1g.img tersebut. Ada banyak cara, namun anda dapat menggunakan perintah berikut untuk melakukan *mounting* ```drive1g.img``` ke *working folder* saat ini.
```
sudo mount -t auto -o loop drive1g.img drive1g
```
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Selamat, anda sudah membuat folder ```drive1g``` dengan alokasi besar penyimpanan sebesar 1GB. Selanjutnya anda hanya tinggal mengarahkan volume kontainer anda ke folder tersebut dan kontainer hanya dapat menggunakan besar penyimpanan sebesar ukuran file *virtual drive* yang sudah dibuat tadi. Hal ini sangat cocok apabila anda ingin memberikan alokasi besar penyimpanan yang bersifat *presistent*.