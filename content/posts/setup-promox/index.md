+++
title = 'Setup Promox'
date = 2024-07-13T23:22:13+07:00
draft = true
tags = ["server", "promox", "selfhosted"] 
+++

## Setup new Promox env

### Post install script

Optimize OS and dismiss purchase popup

```sh
bash -c "$(wget -qLO - https://github.com/tteck/Proxmox/raw/main/misc/post-pve-install.sh)"
```

### Config fstab (if there're addition Hard drive)

- Check current drives
  - `lsblk`: list all blocks
  - `blkid`: get block's id
  - `fdisk -l | grep '^Disk'`

- format drive (ext4  or xfs):

  ```mkfs.ext4 /dev/sdb -f```

- mount the drive

  ```
  mkdir /mnt/nas/
  mount /dev/sdb /mnt/nas/
  ```

- add mount point to lxc
  
  `nano /etc/pve/lxc/102.conf`

  add this line: `mp0: /mnt/nas,mp=/media`

- auto mount on promox boot

  `vi /etc/fstab`
  
  Add this line:   `UUID=d50040a8-41f1-4624-bf0b-aeb81f7752e7 /mnt/nas xfs defaults 00`
  