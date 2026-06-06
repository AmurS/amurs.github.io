---
title: Membangun Server Lokal
description: Membangun server lokal di rumah.
date: 2026-06-06
tags:
  - Server
  - VM
  - LXC
  - Proxmox
  - NAS
---

### Membangun NAS Server Rumahan dengan Hardware Bekas
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Post Pertama di Tahun 2026

Setelah sekian lama hanya mengandalkan cloud storage dan hard drive eksternal yang berserakan, akhirnya saya memutuskan untuk membangun NAS server sendiri. Tujuannya sederhana: saya ingin self-hosted storage server di rumah yang bisa saya akses kapan saja, tanpa bergantung pada layanan pihak ketiga. Ditambah lagi, saya ingin belajar lebih dalam tentang administrasi server dan virtualisasi.

Saya sendiri memiliki latar belakang teknis, jadi membangun server dari komponen bekas bukanlah hal yang asing. Tantangannya adalah melakukannya dengan budget seminimal mungkin.

### Latar Belakang

Jadi selama ini saya menggunakan hard drive eksternal yang di-colok ke PC dan dibagikan sebagai network share. Solusi ini tentu saja tidak ideal — PC harus menyala 24/7, konsumsi listrik boros, dan tidak ada redundancy jika hard drive rusak.

Saya juga sempat menggunakan layanan cloud seperti Google Drive dan OneDrive, tapi untuk file-file besar seperti backup VM, ISO installer, dan koleksi media, biaya langganan dan keterbatasan bandwidth upload/download menjadi masalah.

Oleh karena itu untuk kali ini, saya ingin membangun sebuah NAS server dedicated yang hemat listrik, mungil, tapi mumpuni untuk kebutuhan storage rumah tangga. Dengan budget utama hanya untuk komponen baru seperti case dan SAS card, sisanya memanfaatkan hardware bekas yang ada.

###Spesifikasi Hardware

Berikut spesifikasi NAS server yang saya bangun:

| Komponen | Spesifikasi |
|---|---|
| **CPU** | Intel Core i3-4170 (3.7 GHz, 2C/4T) |
| **HSF** | JONSBO CR-1300 EVO FRGB |
| **Motherboard** | Lenovo OEM (Socket LGA1150) |
| **RAM** | 2x 8GB DDR3 12800 + 2x 2GB DDR3 12800 = **20GB total** |
| **Boot Drive** | 256GB SATA SSD |
| **Storage** | Beberapa HDD 2TB dan 4TB |
| **SAS Controller** | Supermicro LSI 9211-8i (IT Mode) |
| **Case** | FSP S140 Lite (Dual Chamber) |
| **Fan** | Ocypus 120mm ARGB |
| **Thermal Paste** | Arctic MX-7 |
| **HDD Cage** | 3D Printed custom (5 HDD per cage) |

Saya memilih i3-4170 karena cukup untuk menjalankan Proxmox + beberapa container dan VM ringan. Dengan RAM 20GB, saya masih bisa menjalankan beberapa layanan tambahan seperti AdGuard Home, OPNSense, atau container Linux ringan lainnya.

### Pemilihan Case: FSP S140 Lite

![PCCase1](/public/PCCase1.jpg)

Case yang saya gunakan adalah FSP S140 Lite. Ini adalah case dual chamber yang cukup unik — desainnya mirip dengan case ITX modern tapi dalam bentuk yang lebih besar dan murah. Harganya sangat terjangkau untuk case baru dengan fitur dual chamber.

Yang menarik dari case ini adalah pemisahan ruang antara komponen utama dan ruang PSU/kabel. Ini sangat membantu manajemen kabel dan airflow. Sayangnya, case ini hanya mendukung maksimal **2 buah 3.5 inch HDD** secara bawaan — jelas tidak cukup untuk NAS.

![PCCase2](/public/PCCase2.jpg)

Tapi case ini memiliki ruang kosong yang cukup lega di chamber depan. Dengan sedikit modifikasi, saya bisa menjejalkan lebih banyak hard drive. Solusinya adalah dengan menggunakan 3D printed HDD cage custom.

### 3D Printed HDD Cage

![3DPrintHDDstack](/public/3DPrintHDDstack.jpg)

Karena case tidak mendukung banyak HDD, saya mendesain dan mencetak 3D cage khusus untuk menumpuk hard drive. Satu cage 3.5 inch bisa menampung **5 buah HDD** secara vertikal. Dengan ruang yang ada, saya bisa memasang setidaknya satu cage ini, dan masih ada ruang untuk cage tambahan jika saya ingin menambah storage di masa depan.

Cage ini didesain dengan celah ventilasi di setiap slot-nya dan menggunakan baut standar 6-32 untuk pemasangan HDD. Hasil cetakan menggunakan filament PLA+ dengan infill 40% — cukup kuat untuk menahan berat 5 HDD.

### Supermicro LSI 9211-8i SAS Card

![SASCard+Cable](/public/SASCard+Cable.jpg)

Untuk menghubungkan banyak hard drive ke motherboard, saya menggunakan **Supermicro LSI 9211-8i** dalam mode IT (Initiator Target). Kartu ini menggunakan chip LSI SAS2008 yang sudah terkenal stabil dan banyak digunakan di server enterprise.

Kartu ini memiliki 2 port internal yang masing-masing mendukung koneksi SFF-8087 (mini SAS). Dengan kabel breakout SFF-8087 ke 4x SATA, satu kartu ini bisa mendukung hingga **8 hard drive** — lebih dari cukup untuk kebutuhan awal saya.

![SASCable](/public/SASCable.jpg)

Kabel SFF-8087 yang saya gunakan adalah kabel bekas server yang dibeli murah di marketplace. Pastikan membeli kabel yang masih berfungsi baik, karena kabel SAS berkualitas rendah bisa menyebabkan intermittent connection dan corruption data.

### Repaste LSI Card

![SASCardrepaste](/public/SASCardrepaste.jpg)

LSI 9211-8i terkenal dengan masalah **overheating** pada chip-nya. Chip LSI SAS2008 bisa mencapai suhu 90-100°C tanpa heatsink yang baik, apalagi di dalam case dengan airflow terbatas.

Saya melakukan repaste thermal paste pada heatsink kartu LSI ini menggunakan **Arctic MX-7**. Thermal paste bawaan dari kartu bekas biasanya sudah mengering dan tidak efektif lagi. Dengan repaste, suhu chip bisa turun 10-15°C.

### Assembly

![Assembly](/public/Assembly.jpg)

Proses perakitan cukup straightforward:

1. **Pasang CPU, HSF JONSBO CR-1300 EVO FRGB, dan RAM** ke motherboard Lenovo.
2. **Flash LSI 9211-8i** ke mode IT (jika belum). Mode IT membuat kartu bertindak sebagai HBA biasa, bukan RAID controller — penting untuk direct disk management.
3. **Pasang LSI card** ke slot PCIe x16 atau x8.
4. **Pasang SSD boot** (SATA) dan hubungkan kabel power.
5. **Pasang HDD** ke 3D printed cage dan kencangkan dengan baut.
6. **Hubungkan kabel SFF-8087** dari LSI card ke hard drive menggunakan kabel breakout.
7. **Manajemen kabel** — salah satu bagian paling memakan waktu. Case FSP S140 Lite dengan desain dual chamber sangat membantu menyembunyikan kabel-kabel yang tidak terpakai.

Sistem operasi yang saya gunakan adalah **Proxmox VE** yang diinstall di SSD 256GB. Proxmox dipilih karena berbasis Debian dan memiliki web interface yang memudahkan manajemen VM dan container.

Rencananya, di atas Proxmox saya akan menjalankan:
- **OPNSense** sebagai VM firewall/router utama untuk menggantikan router ISP.
- **AdGuard Home** untuk DNS-level ad blocking dan filtering di seluruh jaringan.
- **Windows Server 2025** sebagai VM untuk manajemen storage dan layanan Windows.
- Beberapa LXC ringan lainnya untuk kebutuhan spesifik.

### Menyalakan Server

![NASTurnOn](/public/NASTurnOn.jpg)

Setelah semua terpasang, saatnya momen yang menegangkan — menekan tombol power untuk pertama kalinya.

Server menyala tanpa masalah. Semua komponen terdeteksi — 20GB RAM, SSD boot drive, dan hard drive yang terhubung ke LSI card semuanya muncul di BIOS. Lanjut ke instalasi Proxmox, proses berjalan mulus.

Setelah Proxmox terinstall, saya melakukan **passthrough** HDD langsung ke VM Windows Server. Karena HDD masih berformat NTFS dan mengandung data existing, pendekatan ini paling praktis — tidak perlu backup-restore data. Nantinya, ketika harga HDD turun, drive-drive ini akan dipindahkan ke konfigurasi RAID Z1 di TrueNAS Scale atau OMV untuk redundancy yang lebih baik.

### Cooling LSI SAS Card


![SASCardCoolingInsideCase](/public/SASCardCoolingInsideCase.jpg)

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Ini adalah bagian yang paling krusial. LSI 9211-8i adalah kartu yang **panas**. Tanpa pendinginan yang memadai, kartu ini bisa menyebabkan system hang, hard drive disconnect, atau bahkan kerusakan permanen pada chip.

Di case FSP S140 Lite, airflow standard mungkin tidak cukup untuk mendinginkan LSI card, terutama karena letaknya yang dekat dengan GPU atau komponen panas lainnya. Solusi saya:

1. **Memasang fan Ocypus 120mm ARGB** di bawah LSI card.
2. Memastikan ada **exhaust fan** di belakang case untuk mengeluarkan udara panas.

Chip LSI SAS2008 memiliki TDP sekitar 8-10 Watt, tapi karena ukurannya kecil, density panasnya sangat tinggi. Suhu operasi ideal adalah di bawah **70°C**. Dengan repaste MX-7 dan fan langsung, suhu chip terjaga di kisaran **55-60°C** saat idle dan **65-70°C** saat semua HDD sedang diakses — cukup aman.

Ocypus fan ini sebenarnya ARGB, tapi untuk server yang diletakkan di pojok ruangan, efek RGB-nya lebih sebagai bonus estetika daripada fungsional :D

### Hasil dan Performa Awal

Setelah beberapa hari berjalan, NAS server ini menunjukkan performa yang memuaskan:

- **Transfer speed via SMB**: ~100-110 MB/s (terbatas Gigabit Ethernet)
- **Suhu CPU**: 40-45°C idle, 55-60°C load
- **Suhu LSI Card**: 55-60°C idle, 65-70°C load
- **Konsumsi daya**: ~45-55 Watt (tergantung jumlah HDD yang aktif)


Konsumsi daya ~50 Watt mungkin terdengar besar, tapi untuk server yang menyala 24/7, biaya listriknya sekitar Rp 50.000 - 70.000 per bulan (dengan asumsi tarif Rp 1.440/kWh). Masih jauh lebih murah dibandingkan langganan cloud storage.

### Kesimpulan
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>
Pembuatan NAS server rumahan ini berhasil dan memiliki performa yang cukup bagus untuk hardware seadanya. Hal yang paling krusial dalam build ini adalah:

1. **Pendinginan LSI SAS Card** — jangan pernah sepelekan ini. LSI 9211-8i adalah kartu yang sangat panas dan butuh airflow langsung. Repaste thermal paste sangat dianjurkan untuk kartu bekas.
2. **3D printed HDD cage** — solusi murah dan efektif untuk case yang tidak mendukung banyak HDD. Dengan desain yang tepat, Anda bisa menjejalkan banyak hard drive ke dalam case kecil.
3. **Pemilihan case dual chamber** — FSP S140 Lite membuktikan bahwa case murah sekalipun bisa menjadi pilihan bagus untuk NAS dengan sedikit modifikasi.
4. **HDD passthrough ke VM Windows Server** — karena HDD masih NTFS dan berisi data, passthrough langsung ke VM Windows Server adalah solusi paling praktis tanpa memformat ulang. Rencana ke depan akan migrasi ke RAID Z1 untuk redundancy.

Ke depannya, saya berencana menambah HDD cage kedua untuk ekspansi storage, dan mungkin mengganti fan bawaan case dengan yang lebih senyap. Tapi untuk sekarang, NAS ini sudah cukup melayani kebutuhan storage di rumah.