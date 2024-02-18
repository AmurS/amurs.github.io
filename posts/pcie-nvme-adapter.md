---
title: Adapter NVME PCIe x1
description: Testing adapter NVME
date: 2024-02-18
tags:
  - ssd
---

### Adapter NVME
<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Hanya sekedar postingan singkat. Baru baru ini saya habis membeli adapter M.2 NVME ke PCIe x1 karena port NVME di motherboard saya yang penuh. 

![PCIE Speed](/public/pcie1-nvme.jpg)<br>

### PCIe Lanes

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Pada umumnya, NVME memerlukan 4 jalur PCIe (*4 lanes*) untuk mendapatkan performa maksimal, tapi dengan adapter ini kita akan memanfaatkan slot PCIe x1 yang ada di motherboard. Tentu saja, performa SSD hanya akan berjalan dengan kecepatan 1 jalur PCIe (1 *lane*).

![PCIE](/public/gen.png)

### CrystalDiskMark

::: details Adata SX8200 Pro 256GB
![SX8200](/public/sx8200x1.png)
:::

::: details Western Digital SN730 512GB
![SN730](/public/sn730x1.png)
:::

::: details SATA A400 KW
![A400](/public/a400kwdiskmark.png)
:::

<sup class="watermark">Kunjungi https://note.rkgk.my.id </sup>Karena slot PCIe x1 yang tersisa merupakan PCIe x1 gen 2.0 dengan kecepatan teoritis maksimum sebesar 500MB/s maka hasil yang didapatkan terlihat cukup mirip. Dapat disimpulkan untuk PCIe x1 gen 2.0 memiliki kecepatan sedikit di bawah dari SATA 6.0 Gbps. Tentu saja apabila anda memiliki slot PCIe x1 yang tidak terpakai dapat dimanfaatkan untuk penyimpanan data, apalagi bila anda memiliki PCIe slot generasi ke 3 dengan kecepatan 2x lipat generasi ke 2.

### Referensi
```
https://www.crucial.com/support/articles-faq-ssd/pcie-speeds-limitations
```