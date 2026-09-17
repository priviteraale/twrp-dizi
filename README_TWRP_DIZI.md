# TWRP Device Tree per Redmi Pad Pro / POCO Pad (dizi) WiFi
Codename: dizi - Platform: parrot (Snapdragon 7s Gen 2) - Android 12 / HyperOS 3 (OS3.0.303.0.WNSEUXM)

Estratto da payload.bin Global (payload-dumper-go) su macOS 17/09/2026
- recovery.img 100M (header v4, ramdisk LZ4 22M -> 40M cpio)
- boot.img 96M kernel 46M (Image)
- dtbo.img 23M
- board parrot, density 320, 12.1" 2560x1600

## Fix applicati vs twrpdtgen auto:
- BOARD_USES_RECOVERY_AS_BOOT=false (dizi ha recovery dedicata, non recovery in boot)
- TARGET_RECOVERY_FSTAB + TW flags per tablet

## Come compilare su Linux (Ubuntu 22.04)
```bash
mkdir twrp && cd twrp
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp -b twrp-12.1
repo sync
git clone https://github.com/twrpdtgen/android_device_xiaomi_dizi.git device/xiaomi/dizi --depth=1
# sostituisci device/xiaomi/dizi con questa cartella patchata
cp -r /path/a/questa/cartella/* device/xiaomi/dizi/
. build/envsetup.sh
lunch omni_dizi-eng
mka recoveryimage -j$(nproc)
# output: out/target/product/dizi/recovery.img (~100M)
```

Oppure build via GitHub Actions: usa template https://github.com/twrpdtgen/twrpdtgen + push su tuo repo.

## Flash
```bash
adb reboot bootloader
fastboot flash recovery recovery.img
fastboot reboot recovery
# oppure test senza flash: fastboot boot recovery.img
# al primo avvio TWRP: Wipe -> Format Data (per cifratura FBE)
```

## File inclusi
- prebuilt/kernel (46M) estratto da boot.img
- prebuilt/dtbo.img (23M) estratto da dtbo
- recovery.img stock (100M) per confronto
