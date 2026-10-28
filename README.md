# kernel_xiaomi_generic

Prebuilt GKI kernel and boot images for the Xiaomi 15 Ultra (`xuanyuan`).

| File | Size | Description |
|---|---|---|
| `kernel` | 36 MB | GKI `Image` (Linux 6.6.118, `android15-6.6`, `ab15863337`) |
| `prebuilt_kernel_headers.tar.gz` | 1.7 MB | Kernel headers |
| `dtb.img` | 4.1 MB | Device tree blob |
| `dtbo.img` | 18 MB | Device tree overlays (signed stock image, AVB footer included) |
| `vendor_boot.img` | 96 MB | vendor_boot (stock ramdisk, kernel modules, dtb) |
| `vendor_dlkm.img` | 83 MB | Vendor kernel modules |
| `system_dlkm.img` | 15 MB | System kernel modules |

## Origin

The images were read directly from the device partitions
(`adb exec-out dd if=/dev/block/by-name/<part>`), so they match the firmware
running on the phone.

Current version: HyperOS **OS4.0.0.6.XOAEUXM** (EEA), slot `_a`. `kernel` comes
from `boot_a` and `dtb.img` from `vendor_boot_a` (both via `unpack_bootimg`);
`vendor_dlkm` and `system_dlkm` were read from `/dev/block/mapper/*_a`. The
camera, display and GPU modules match the vendor blobs of the same release.

The `dtbo` partition is 24 MB, but the stock image is 18 MB with its AVB footer
at its own end; only the signed image is kept.

## Usage

`BoardConfig.mk` sets `TARGET_FORCE_PREBUILT_KERNEL := true`, so the kernel is
not built from source and this repository is used directly:

```make
TARGET_PREBUILT_KERNEL          := $(KERNEL_DIR)/kernel
TARGET_PREBUILT_KERNEL_HEADERS  := $(KERNEL_DIR)/prebuilt_kernel_headers.tar.gz
TARGET_PREBUILT_DTB             := $(KERNEL_DIR)/dtb.img
BOARD_PREBUILT_DTBOIMAGE        := $(KERNEL_DIR)/dtbo.img
BOARD_PREBUILT_VENDOR_BOOTIMAGE := $(KERNEL_DIR)/vendor_boot.img
```

## Source code

- `MiCode/Xiaomi_Kernel_OpenSource`, branch `xuanyuan-v-oss`
  (`release-v-qcom-sm8750-O1`, Linux 6.6, GKI 2.0)
- `MiCode/kernel_devicetree`, branch `xuanyuan-v-oss`
- CodeLinaro BSP manifests: `xiaomi-8750/kernel_manifest`
  (`kernel.lnx.6.6.r1-rel`, `pakala`)

This repository only holds the built output.

Used by [device_xiaomi_xuanyuan](https://github.com/krmmyvz/device_xiaomi_xuanyuan).
