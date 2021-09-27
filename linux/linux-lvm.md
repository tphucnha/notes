# LVM Resize – How to Decrease an LVM Partition
https://www.rootusers.com/lvm-resize-how-to-decrease-an-lvm-partition/

## Đang có:
PV: sda3

VG: fedora

LV: 

- fedora-swap - 8GB
- fedora-root (ext4) - 40 GB
- fedora-home (ext4) - **33 GB**

## Cần cắt `fedora-home` ra để cài thêm archlinux
PV: sda3

VG: fedora

LV: 

- fedora-swap - 8GB
- fedora-root (ext4) - 40 GB
- fedora-home (ext4) - **23 GB**

## Step by step
### Resize `fedora-home`
Umount: `umount /dev/mapper/fedora-home`

Checking command: `e2fsck -fy /dev/mapper/fedora-home`

Resize fs of LV fedora-home: `resize2fs /dev/mapper/fedora-home 23G`

Reduce LV fedora-home to 23G: `lvreduce -L 23G /dev/mapper/fedora-home`

Chưa hiểu sao phải chạy lại cái này cái nữa: `resize2fs /dev/mapper/fedora-home`
## Create new logical volume `arch-root`
