<h1 style="text-align:center;font-size:40px;">FH</h1>

## RASPBERRY PI

### Montar SO

```bash
Fdisk -l /dev/sdg
umount /dev/sdg1 mount | grep sdg
dd if=/ruta archivo of=/dev/sdg bs=4M
```

### Conf red

```bash
mount /dev/sdg2 /mnt
vim /mnt/etc/dhcpcd.conf

Interface eth0
Static ip_address=172.20.4.64/16
Static routers=172.20.2.1
Static domain_name_server=172.20.0.7 8.8.8.8

umount /mnt
mount /dev/sdg1 /mnt
touch /mnt/ssh
```

### Raspberry

```bash
ssh pi@[ip]
raspi-config
fsck /dev/sdx1-2 # reparar sist arch
```