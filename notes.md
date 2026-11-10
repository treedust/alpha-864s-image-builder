# inspect img file
```bash
# read the partitions
sudo losetup -Pf custom-lattepanda-alpha-864s-live.img
# list out the block device partition layout
lsblk | grep -E "loop[0-9]+"
mkdir -p /tmp/img
# mount the root file system
sudo mount -o ro /dev/loop0p3 /tmp/img

# cloud-init files
cat /tmp/img/var/lib/cloud/seed/nocloud/user-data
cat /tmp/img/etc/cloud/cloud.cfg

# unmount 
sudo umount /tmp/img
# stop reading partitions (detach img)
sudo losetup -d /dev/loop0
```