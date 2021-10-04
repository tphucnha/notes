# LVM Resize – How to Decrease an LVM Partition
https://www.rootusers.com/lvm-resize-how-to-decrease-an-lvm-partition/

## What I have:
_PV: /dev/sda3_

Output of `lvs` command:
  LV          | VG     | LSize
  ------------|--------|------
  ----        | ------ | -----
  home        | fedora | _33.00g_
  ----        | ------ | -----

## What I want: 
- Get 10GB of space from `fedora-home` (for installing archlinux)
### Step by step
#### Resize `fedora-home`
- `umount /dev/mapper/fedora-home`: Umount
- `e2fsck -fy /dev/mapper/fedora-home`: Checking command
- `resize2fs /dev/mapper/fedora-home 23G`: Resize fs of LV fedora-home
- `lvreduce -L 23G /dev/mapper/fedora-home`: Reduce LV fedora-home to 23G
- `resize2fs /dev/mapper/fedora-home`: No idea for the reason of doing this
#### Create new logical volume `arch-root`
In one go `lvcreate -l +100%FREE fedora -n arch-root`
### Result:
_PV: /dev/sda3_

Output of `lvs` command:
  LV          | VG     | LSize
  ------------|--------|------
  _arch-root_   | _fedora_ | _10.00g_
  ----        | ------ | -----
  home        | fedora | _23.00g_
  ----        | ------ | -----
  
# Rename VG and LV without problems in THE NEXT BOOT =))
## What I have:
_PV: /dev/sda3_

Output of `lvs` command:
  LV          | VG     | LSize
  ------------|--------|------
  arch-root   | fedora | 10.00g
  root        | fedora | 40.00g
  home        | fedora | _23.00g_
  swap        | fedora | <7.79g
## What I want:
- Change VG name from `fedora` to `linux`
- Change LV name from `root` to `fedora-root`
### Step by step
- `vgrename fedora linux`: Change VG name
- `lvrename linux root fedora-root`: Change LV name
#### Fix grub config (about my environment: efi, grub2 bootloader), **3 files** need to be updated
1. /etc/default/grub
- `vim /etc/default/grub`: Using vim to change
  - Change value for `rd.lvm.lv` of root _(*)_

    type `:%s/rd.lvm.lv=fedora\/root/rd.lvm.lv=linux\/fedora-root/g` (find and replace syntax), hit ENTER in vim **NORMAL MODE**
2. /boot/efi/EFI/fedora/grubenv
- `vim /boot/efi/EFI/fedora/grubenv`
  - Change value for `rd.lvm.lv` of root, _do the same with (*)_

    type `:%s/rd.lvm.lv=fedora\/root/rd.lvm.lv=linux\/fedora-root/g` (find and replace syntax), hit ENTER in vim **NORMAL MODE**
  - Change value for `root` value (**)

    type `:%s/root=\/dev\/mapper\/fedora-root/root=\/dev\/mapper\/linux-fedora--root/g` (find and replace syntax), hit ENTER in vim **NORMAL MODE**
3. /boot/efi/EFI/fedora/grub.cfg
- `vim /boot/efi/EFI/fedora/grub.cfg`
  - Change value for `rd.lvm.lv` of root, _do the same with (*)_

    type `:%s/rd.lvm.lv=fedora\/root/rd.lvm.lv=linux\/fedora-root/g` (find and replace syntax), hit ENTER in vim **NORMAL MODE**
  - Change value for `root` value, _do the same with (**)_

    type `:%s/root=\/dev\/mapper\/fedora-root/root=\/dev\/mapper\/linux-fedora--root/g` (find and replace syntax), hit ENTER in vim **NORMAL MODE**
#### Fix fstab config
- `vim /etc/fstab`

  type `:%s/\/device\/mapper\/fedora-root/\/dev\/mapper\/linux-fedora--root/g` (find and replace syntax) , hit ENTER in vim **NORMAL MODE**
### Result:
_PV: /dev/sda3_

Output of `lvs` command:
  LV            | VG      | LSize
  ------------  |-------  |-------
  arch-root     | _linux_ | 10.00g
  _fedora-root_ | _linux_ | 40.00g
  home          | _linux_ | 23.00g
  swap          | _linux_ | <7.79g
