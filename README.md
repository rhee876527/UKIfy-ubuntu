# UKIfy-ubuntu

The config files generate an `Ubuntu noble Xfce4 desktop efi` that you can boot a live system off. 

This uses your system RAM as the filesystem storage thanks to `systemd.volatile=overlay`. No external drives or USB is required to boot into the live system. 

Even though it is possible to directly boot the generated `EFI` image from `UEFI`.

#### Requirements:
- Mkosi
- Systemd-boot
- An `EFI` boot partition with at least `~600MB` free.
- 8GB RAM or more. Much lower if you want a headless live system.

#### Get started
###### testing with qemu

In `mkosi.conf` comment out `virtio` under `KernelModulesInclude=`
and do the same in `mkosi.conf.d/ubuntu.conf`
```
[Runtime]
RAM=8G
Console=gui # need qemu-desktop
```
then `mkosi -f qemu`

###### build final USI image

`mkosi -f --image-version=$(date --utc +%Y%m%d%H%M%S)`

###### boot the EFI binary
copy the generated efi to `/boot/EFI/Linux` and select it (labeled Ubuntu 24.04) as a boot option in `systemd-boot`.

That's it!

###### Note on USI image size
```
[Output]
Format=uki
CompressLevel=17
CompressOutput=zstd
```
ZSTD compression level of `17` brings the `efi` image size to `528MB` which is reasonably compact for a `~1.7GB` filesystem size (check screenshots folder). 

Higher compression levels can lead to even smaller `efi` binaries at the cost of taking more time to decompress and load the initial `initramfs` on boot.
