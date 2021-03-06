# encrypt disk

*[linux](../README.md#linux)*

## initialize partition crypto luks mapping

```sh
cryptsetup -v -y luksFormat /dev/sdX1
cryptsetup luksOpen /dev/sdX1 mydrive
mkfs.my /dev/mapper/mydrive
```

## open disk

```sh
cryptsetup luksOpen /dev/sdX1 mydrive
mount /dev/mapper/mydrive /fld
```

## use keyfile for automount

suppose `/root` resides on a full encrypted system; you can place some key for automount of other encrypted devices ( `disk_4tb` attached on boot while `disk_4tb_bk` attached on demand )

- create keyfile ( may useful to install `haveged` to increase random entropy )

```sh
dd bs=512 count=4 if=/dev/random of=/root/.disk-4tb.key iflag=fullblock
dd bs=512 count=4 if=/dev/random of=/root/.disk-4tb-bk.key iflag=fullblock
chmod 400 /root/.disk-4tb*key
```

- add keys disks ( luksOpen required; double check device correspondence using `blkid` )

```sh
cryptsetup luksAddKey /dev/sda1 /root/.disk-4tb.key
cryptsetup luksAddKey /dev/sdc1 /root/.disk-4tb-bk.key
```

- setup `/etc/crypttab`

```
disk_4tb UUID=xxxxxxxx-yyyy-zzzz-wwww-aaaaaaaaaaaa /root/.disk-4tb.key luks
disk_4tb_bk UUID=xxxxxxxx-yyyy-zzzz-wwww-aaaaaaaaaaa2 /root/.disk-4tb-bk.key luks,noauto
```

- setup `/etc/fstab`

```
/dev/mapper/disk_4tb	/disk-4tb	btrfs defaults 0 2
/dev/mapper/disk_4tb_bk	/disk-4tb-bk	btrfs	defaults,noauto 0 2
```

## mount on demand using keyfile and crypttab info

```sh
cryptdisks_start disk_4tb_bk
mount /disk-4tb-bk
```

## duplicated uuid

typical scenario:
- have two backup disks ( short and long term )
- do not mount these at the same time
- want to mount one of backup disk in a specific folder through /etc/crypttab and /etc/fstab

```sh
cryptsetup luksUUID /dev/sdX1 --uuid "xxxxxxxx-yyyy-zzzz-wwww-aaaaaaaaaaaa"
```
