---
title: SSD KW, abal, nan palsu?
description: Fake Kingston A400 SSD from the local marketplace 
date: 2023-04-12
tags:
  - SSD
  - KW
  - Counterfeit
  - Fake
---

<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup> 

> Lah loh, SSD KW? Emang ada? 


Tentu saja ada.

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Beberapa hari yang lalu saya menemukan SSD Kingston A400 dengan harga yang sangat waw. Murah banget. Sehingga mata saya terbutakan dan langsung saya *checkout* karena penasaran. SSD ini saya dapatkan dari toko hijau dengan harga yang sangat miring, yaitu cuma 190 ribu rupiah saja. Yang dimana untuk harga SSD yang asli dangan ukuran yang sama itu sekitar 2 kali lipat dari yang KW ini. 

> Kok tau ini KW?

Dari pencarian dan hasil riset online yang telah saya lakukan. Memang benar SSD kali ini merupakan SSD KW dari SSD Kingston seri A400. Mulai dari *packaging* yang berbeda dari *packaging* Kingston A400 yang asli, dan nomor seri yang tidak dapat di cek di web official Kingston. Jadi ternyata SSD KW seperti ini banyak ditemukan di *marketplace* China seperti Alicepat.

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Pada catatan kali ini saya akan membahas tentang SSD KW ini. Apakah ini SSD beneran (walaupun KW) atau SSD yang aslinya microSD/USB flash drive seperti *scam-scam* wi*h dot com.


### *Packaging*
::: details Kingston A400 KW
![Box](/public/a400-1.jpg)
![a400](/public/a400-2.jpg)
:::
::: details Kingston A400 Original
![Box](/public/a400-3.jpg)
:::
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup><br>
### CrystalDiskInfo

![CrystalDiskInfo](/public/a400kwinfo.png)
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
### CrystalDiskMark

![CrystalDiskMark](/public/a400kwdiskmark.png)
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Pengetesan ukuran asli SSD

![H2testw](/public/a400h2test.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dari hasil test H2testw SSD KW ini lolos verifikasi full disk dan memiliki kapasitas asli.

![H2testw - Write](/public/a400kwh2write.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dengan kecepatan *write* sekuensial seperti di atas. SSD KW ini memiliki SLC Caching sekitar 30% dari total ukuran SSD atau sekitar 80GB. Apabila SLC Cache ini habis maka kecepatan *write* akan turun drastis disekitaran 100 MB/s dan turun lagi menjadi hanya 50MB/s. Dari kecepatan yang didapat, saya memiliki kecurigaan NAND yang digunakan merupakan NAND QLC. Tentu saja, apabila anda melakukan *write* sekuensial di bawah 80GB dalam satu waktu, maka kecepatan *write* anda tetap akan berada dikecepatan 400MB/s seperti di gambar.
<br><sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

![H2testw - Read](/public/a400kwh2read.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Untuk hasil kecepatan *read*, SSD KW ini memiliki kemampuan yang bagus, hampir selalu berada dikecepatan 400MB/s dengan sedikit fluktuasi.
<br><br>
### Pengetesan real-world


![Apex Move from HDD to SSD](/public/a400kwsteam.png)

![Apex Move from HDD to SSD](/public/a400kwapexwrite.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dalam pengujian ini, dilakukan perpindahan instalasi Apex Legend dengan ukuran 62GB dari HDD ke SSD KW. Dibutuhkan waktu sekitar 13 menit 40 detik dengan kecepatan *write* random seperti pada gambar di atas.

### Hasil benchmark akhir


![CrystalDiskMark](/public/a400kwdiskmark-after.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Dapat dilihat dari hasil tersebut, tidak ada perubahan yang signifikan pada kecepatan *read* dan *write* SSD walaupun sudah melewati berbagai macam pengujian dan SSD dalam keadaan tidak kosong.    

### Kesimpulan

1. Harga yang sangat menggiurkan. Rp. 740,-/ GB. Murah banget.
2. SLC Cache besar. 80GB SLC Cache untuk ukuran SSD 256GB termasuk besar.
3. Kecepatan *read* bagus. Bisa sustain 400MB/s untuk *read* sekuensial.
4. *Endurance* tidak jelas. Dari indikasi kecepatan *write* yang turun hingga 50MB/s, mungkin saja ini menggunakan NAND QLC (TBW tipikal 150TB untuk 256GB di brand ternama).
5. Barang KW. Memakai nama Kingston tapi ini bukan keluaran Kingston.  
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>

### Referensi
```
https://linustechtips.com/topic/992444-kingston-a400-from-aliexpress/#comment-14524508
https://www.kingston.com/id/support/product-verification
```