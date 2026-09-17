# Build TWRP per Redmi Pad Pro WiFi (dizi) via GitHub Actions

## 1. Crea repo GitHub
- Vai su https://github.com/new -> nome `android_device_xiaomi_dizi` o `twrp-dizi` -> Create (vuoto, senza README)

## 2. Push del device tree (hai già tutto in /tmp/twrpdtgen_dizi)
Da Terminale macOS:
```bash
cd /tmp/twrpdtgen_dizi
git init
git add .
git commit -m "dizi: initial TWRP device tree for Redmi Pad Pro WiFi (parrot, Android 12, HyperOS 3) - prebuilt kernel 45M + dtbo 23M + fixed recovery_as_boot"
git branch -M main
git remote add origin https://github.com/TUO_USERNAME/twrp-dizi.git
git push -u origin main
```

## 3. Lancia build
- Apri il repo su GitHub -> tab Actions -> workflow "Build TWRP dizi" -> Run workflow -> Run
- Attendi ~30-45min (sync 20GB + compilazione)
- Scarica artifact `twrp-dizi-recovery` -> dentro `recovery.img` 100M

## 4. Flash
```bash
adb reboot bootloader
fastboot flash recovery recovery.img
# oppure test: fastboot boot recovery.img
fastboot reboot recovery
# Wipe -> Format Data al primo avvio (FBE)
```

## File inclusi
- prebuilt/kernel (46M da boot.img OS3.0.303.0.WNSEUXM)
- prebuilt/dtbo.img (23M)
- recovery.img stock per confronto in payload.bin originale (non incluso nello zip GitHub, è sul tuo Desktop)
- BoardConfig.mk fix: BOARD_USES_RECOVERY_AS_BOOT=false

## Note
- Workflow usa ubuntu-22.04 + openjdk-11 + minimal-manifest twrp-12.1
- Se build fallisce per `dtbo`, rimuovi `BOARD_INCLUDE_RECOVERY_DTBO` da BoardConfig.mk e ripusha
