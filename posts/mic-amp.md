---
title: DIY Mic Amplifier
description: Membuat Mic Amplifier dari nol.
date: 2024-01-24
tags:
  - OP-AMP
  - Electronics
  - Rangkaian
  - Audio
---

### Post Pertama di Tahun 2024
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Setelah sekian lama tidak melakukan update di blog ini, saya akan mengawali tahun yang baru ini dengan membuat post yang agak berbeda dari biasanya. Untuk kali ini saya akan membuat sebuah rangkaian Mic Amplifier untuk Mic Condenser saya. Saya sendiri memiliki latar belakang enggineering sehingga rangkaian elektronik seperti ini tidaklah asing bagi saya.

### Latar Belakang

![Mic](/public/BM700.jpg)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Jadi selama ini saya menggunakan Mic Condenser BM700 yang banyak ditemukan di marketplace karena harganya yang sangat murah. Tapi mic BM700 ini memiliki banyak versi, dalam sepengetahuan saya untuk BM700 yang beredar saat ini  memiliki rangkaian internal yang jelek, sedangkan BM700 yang saya miliki sejak tahun 2016 ini masih mengunakan internal duplikat dari NW700.

::: details BM700 Internal
![Rangkaian](/public/BM700Internal.jpg)
:::

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Selama ini saya hanya mengandalkan soundcard bawaan PC dan menyuntikan phantom power (12v) ke BM700 ini. Untuk suaranya sendiri cukup ok sebenarnya, namun saya hanya menggunakan mic ini untuk voice chat saja. Saya juga perlu menaikan gain soundcard saya hingga +20 dB agar suara saya dapat terdengar. 

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Oleh karena itu untuk kali ini, saya ingin membuat rangkaian mic amplifier agar saya tidak perlu menaikan gain soundcard dan meningkatkan SNR dan Dynamic Range dari mic ini (harapannya).

### Desain Rangakain
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Untuk desain rangkaian Mic Amplifier ini, saya hanya menggunakan rangkaian yang sederhana saja. Untuk input mic saya akan mengubah sinyal balanced mic ke sinyal unbalanced. Rangkaian untuk sinyal unbalanced sendiri lebih mudah dibuat dan tidak perlu banyak komponen, namun sinyal unbalanced ini memiliki kelemahan dikemampuan penolakan derau atau noise rejection nya. Berikut rangkaian final yang saya buat setelah melewati berbagai macam eksperimen.

::: details Rangkaian Mic Amplifier
![Rangkaian](/public/Amplifier-overview.png)
:::

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Untuk postingan kali ini saya tidak akan menjelaskan secara satu persatu fungsi dari rangkaian ini dan alasan teknisnya (selain karena saya hanya punya nilai komponen tersebut), mungkin untuk penjelasan detail akan saya buat untuk postingan di lain waktu.


### Pembuatan Rangkaian
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Setelah desain rangkaian sudah saya buat, saatnya untuk membuat rangkaian tersebut. Saya menggunakan proto board single layer untuk papan rangkaian. Komponennya sendiri merupakan campuran komponen THT dan SMD (karena saya punyanya itu saja).

::: details Pembuatan Mic Amplifier
![PCB](/public/Proto-power.jpg)
![PCB](/public/Proto-op.jpg)
:::

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Mungkin untuk orang yang jeli dapat melihat penggunaan module buck converter MT3608. Modul ini sangatlah murah di marketplace, hanya 5000an rupiah saja. Modul ini digunakan untuk meningkatkan tegangan dari catu daya agar bisa mencapai setidaknya +24V, karena IC Regulator LM317 (Clone China) yang saya gunakan perlu memiliki perbedaan input dan output setidaknya 3V agar dapat bekerja dengan benar. Tentu saja, modul ini memiliki derau atau noise yang ternyata sangat mempengaruhi kinerja amplifier yang saya buat ini. Hal ini akan terlihat jelas pada hasil penggunaan rangkaian mic ini nantinya.

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Untuk IC Op-Ampnya sendiri saya menggunakan IC Texas Instrument TL072 untuk VCC/2 dan IC Japan Radio Company NJM4580D. Saya tidak mempunyai stok IC Texas Instrument NE5532 seperti yang ada di desain rangkaian awal.

### Hasil Mic Amplifier

![Amplifier](/public/Amplifier.jpg)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Saya cukup puas dengan hasil akhirnya. Hanya saja saya masih belum mebuat case dan socket untuk input/output mic amplifier ini. Untuk sementara input mic dan output amplifier akan terhubung secara langsung.

![Audacity](/public/Audacity.png)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Dengan menggunakan rangkaian mic amplifier ini, saya tidak perlu meningkatkan gain soundcard dan mic saya menjadi lebih sensitif dibandingkan dengan setup saya sebelumnya. Mic saya bisa menangkap tarikan nafas saya pada jarak sekitar 5cm dari mic, dari yang sebelumnya kurang sensitif untuk melakukan hal tersebut. Selain itu dynamic range dari mic saya meningkat dibandingkan dengan sebelumnya. 


### Noise Floor Dan Beberapa Percobaan Lainnya

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Its REW Time.

![REW](/public/AmplifierREWNoiseFloor.png)


<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Noise floor... modul buck converter MT3608 yang saya gunakan di awal pembuatan rangkaain ini menunjukan kejelekannya di sini. 
Dari 3 grafik tersebut dapat dilihat bahwa penggunaan buck converter saja pada rangkaian mic amplifier ini memiliki noise floor rata-rata sekitar -60 dBv. Dengan ditambahkan linier regulator LM317 (clone china) noise floor rata-rata turun menjadi -80 dBv. Ini merupakan peningkatan Signal to Noise Ratio (SNR) hingga 10x lipat. Hal ini sesuai dengan kemampuan dari linier regulator yang memilikki rejeksi derau atau noise pada frekuensi rendah, namun linier regulator memiliki kelemahan yaitu mereka hampir transparan terhadap derau atau noise frekuensi tinggi.

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Dengan menambahkan low pass filter pada input LM317 untuk mengurangi pengaruh frekuensi tinggi terjadi peningkatan SNR di frekuensi tinggi hingga menyentuh angka -110 dBv, yang berarti terjadi peningkatan SNR hingga 333x lipat apabila dibandingkan dengan penggunaan buck converter saja dan sekitar 33x lipat dengan penggunaan linier regulator tanpa low pass filter. Dengan mengunakan teknik ini, sinyal yang sebelumnya tertutup oleh derau atau noise dapat muncul. 
Terdapat kenaikan di frekuensi 4.5 kHz dan 9.5 kHz. Saya masih belum tau apakah ini frekuensi harmonik dari penambahan filter low pass, induksi dari linier regulator, modul buck converter, atau dari komputer saya. 

### Kesimpulan

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Pembuatan rangkaian Mic Amplifier ini berhasil dan memiliki performa yang saya rasa cukup bagus untuk rangkaian yang saya buat dengan komponen seadanya. Hal yang paling berpengaruh terhadap performa amplifier ini adalah pemilihan buck converter. Penggunaan buck converter sangat mempengaruhi Signal to Noise Ratio (SNR), dengan penggunaan filter dan linier regulator SNR dapat ditingkatkan hingga 333x yaitu sekitaran -110 dBv dari SNR awal yaitu sekitaran -60 dBv. Sebaiknya gunakan buck converter yang lebih bagus dan minim derau atau noise. 