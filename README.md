# kernel_xiaomi_generic

Xiaomi 15 Ultra (`xuanyuan`) için **prebuilt** GKI kernel ve boot görüntüleri.

| Dosya | Boyut | Açıklama |
|---|---|---|
| `kernel` | 36 MB | GKI `Image` (Linux 6.6.118, `android15-6.6`, `ab15863337`) |
| `prebuilt_kernel_headers.tar.gz` | 1.7 MB | kernel başlıkları |
| `dtb.img` | 4.1 MB | device tree blob |
| `dtbo.img` | 18 MB | device tree overlay'leri |
| `vendor_boot.img` | 96 MB | vendor_boot (stok ramdisk + kernel modülleri + dtb) |
| `vendor_dlkm.img` | 83 MB | vendor kernel modülleri |
| `system_dlkm.img` | 15 MB | system kernel modülleri |

## Kaynak

Bu görseller cihazın kendi partition'larından **doğrudan** okunmuştur
(`adb exec-out dd if=/dev/block/by-name/<part>`) — cihazda çalışan firmware
ile birebir aynıdır.

Güncel sürüm: HyperOS **OS4.0.0.6.XOAEUXM** (EEA), slot `_a`. `kernel` `boot_a`'dan,
`dtb.img` `vendor_boot_a`'dan `unpack_bootimg` ile çıkarıldı; `vendor_dlkm`/`system_dlkm`
`/dev/block/mapper/*_a`'dan okundu. Kamera/ekran/GPU modülleri vendor blob'larıyla aynı sürümdedir.

`BoardConfig.mk` içinde `TARGET_FORCE_PREBUILT_KERNEL := true` olduğu için
kernel kaynaktan derlenmez; bu repo doğrudan kullanılır:

```make
TARGET_PREBUILT_KERNEL          := $(KERNEL_DIR)/kernel
TARGET_PREBUILT_KERNEL_HEADERS  := $(KERNEL_DIR)/prebuilt_kernel_headers.tar.gz
TARGET_PREBUILT_DTB             := $(KERNEL_DIR)/dtb.img
BOARD_PREBUILT_DTBOIMAGE        := $(KERNEL_DIR)/dtbo.img
BOARD_PREBUILT_VENDOR_BOOTIMAGE := $(KERNEL_DIR)/vendor_boot.img
```

## Kaynak kod

- `MiCode/Xiaomi_Kernel_OpenSource` → dal `xuanyuan-v-oss`
  (`release-v-qcom-sm8750-O1`, Linux 6.6, GKI 2.0)
- `MiCode/kernel_devicetree` → dal `xuanyuan-v-oss`
- CodeLinaro BSP manifest'leri: `xiaomi-8750/kernel_manifest`
  (`kernel.lnx.6.6.r1-rel`, `pakala`)

Bu repo yalnızca **derlenmiş** çıktıyı içerir.
