# LVM Resize – How to Decrease an LVM Partition
https://www.rootusers.com/lvm-resize-how-to-decrease-an-lvm-partition/

## What I have:
_PV: /dev/sda3_

Output of `lvs` command:
  LV          | VG     | Attr       | LSize
  ------------|--------|------------|------
  root        | fedora | -wi-ao---- | 40.00g
  home        | fedora | -wi-ao---- | **33.00g**
  swap        | fedora | -wi-ao---- | <7.79g

## What I want: get 10GB of space from `fedora-home` (for installing archlinux)
_PV: /dev/sda3_

Output of `lvs` command:
  LV          | VG     | Attr       | LSize
  ------------|--------|------------|------
  root        | fedora | -wi-ao---- | 40.00g
  home        | fedora | -wi-ao---- | **23.00g**
  swap        | fedora | -wi-ao---- | <7.79g
  **arch-root**   | **fedora** | **-wi-a-----** | **10.00g**

## Step by step
### Resize `fedora-home`
- `umount /dev/mapper/fedora-home`: Umount
- `e2fsck -fy /dev/mapper/fedora-home`: Checking command
- `resize2fs /dev/mapper/fedora-home 23G`: Resize fs of LV fedora-home
- `lvreduce -L 23G /dev/mapper/fedora-home`: Reduce LV fedora-home to 23G
- `resize2fs /dev/mapper/fedora-home`: No idea for the reason of doing this
### Create new logical volume `arch-root`
In one go `lvcreate -l +100%FREE fedora -n arch-root`
