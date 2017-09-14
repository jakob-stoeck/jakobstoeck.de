---
layout: post
title:  "RAM disk for faster applications under macOS"
date:   2017-06-13 17:13:00 +0200
categories:
---

Theory:
A RAM disk might speed up IO-bound computations under macOS.

Experiment:
Find maximum speedup

Theoretical maximum. I use `pv` (`$ brew install pv`) to measure the throughput of writing data and then use the following options to get the average data througput for 450 MBs written: `pv --average-rate --stop-at-size --size450m` or in short `pv -aSs450m`.

## Contrast data throughput to the following sinks:

1. the void
2. SSD
3. RAM disk
4. RAM disk mounted with OS mount command
5. RAM disk folders mounted with bindfs

### the void

This should be the fastest possible data rate: Piping zeroes into the void. For bigger files than 450 MB the troughput would be even higher, maxing out at around 17GiB/s.

```shell
$ pv -aSs450m < /dev/zero > /dev/null
[13.5GiB/s]
```

### SSD

My MacBook Pro Late 2012 SSD has a little over 300MiB/s write throughput in this test.

```shell
$ for i in {1..10}; do pv -aSs450m < /dev/zero > test; rm -f test; done
[ 307MiB/s]
[ 300MiB/s]
[ 305MiB/s]
[ 305MiB/s]
[ 303MiB/s]
[ 299MiB/s]
[ 309MiB/s]
[ 303MiB/s]
[ 306MiB/s]
[ 304MiB/s]
```

### RAM disk

First setup the ram disk:

```shell
$ VOLUME_NAME=RamDisk
$ SIZE_IN_MB=1024
$ DEVICE=`hdiutil attach -nobrowse -nomount ram://$(($SIZE_IN_MB*2048))`
$ diskutil erasevolume HFS+ $VOLUME_NAME $DEVICE
```

The DDR3 RAM with 1600MHz in this setup has a throughput of ~1.38GiB/s. Nearly 5 times the SSD.

```shell
$ cd /Volumes/RamDisk
$ for i in {1..10}; do pv -aSs450m < /dev/zero > test; rm -f test; done
[ 1.3GiB/s]
[1.29GiB/s]
[1.37GiB/s]
[ 1.4GiB/s]
[1.35GiB/s]
[1.36GiB/s]
[1.39GiB/s]
[1.39GiB/s]
[1.38GiB/s]
[1.37GiB/s]
```

### RAM disk mounted with OS mount command

Applications write their cache inside files and folders. One way to move those folders to the RAM disk would be to replace them with symbolic links to the new destination on the RAM disk. This has the big disadvantage that the folders would stop working when the RAM disk is down (e.g. during/after a restart or when ejecting the ram disk).

A more robust solution is to bind the folder to a folder on the RAM disk. Under linux the `mount --bind` command can do this, under macOS this is not possible with single folders but only with whole devices: You can bind a folder to a device, i.e. the ram disk, but not to another folder on the ram disk.

A trick is to create the base folder structure of the future ram disk in a folder on the harddisk and mount the ram disk over this folder. This way, even when the mount point is down, folder access gracefully falls back to the hard disk folder structure. Which is very important for some folders like `/tmp` which are needed during startup.

```shell
$DEVICE=/dev/disk3 # return of hdiutil
$MOUNT_POINT="$HOME/mounts"

$DIRECTORIES='/private/tmp'
for f in $DIRECTORIES; do
	mv -v $f $MOUNT_POINT$f
	ln -vs $MOUNT_POINT$f $f
done

newfs_hfs -v `basename "$MOUNT_POINT"` $DEVICE
mount -o noatime -t hfs $DEVICE $MOUNT_POINT

for f in $DIRECTORIES; do
	mkdir -vp $MOUNT_POINT$f
done
```

```shell
cd $mount_point
for i in {1..10}; do pv -aSs450m < /dev/zero > test; rm -f test; done
[1.29GiB/s]
[1.43GiB/s]
[1.43GiB/s]
[1.43GiB/s]
[1.34GiB/s]
[1.37GiB/s]
[1.47GiB/s]
[1.46GiB/s]
[1.42GiB/s]
[1.36GiB/s]
```

### RAM disk folders mounted with bindfs

In the last chapter we looked at how Linux can `mount --bind` single folders. Under macOS we need to install `bindfs` to be able to emulate it:

```shell
$ brew cask install osxfuse && brew install bindfs
$ mkdir ~/testdir
$ bindfs /Volumes/RamDisk/testdir ~/testdir
```

With our usual test we see that, unfortunately bindfs is much slower than even using the hard disk:

```shell
$ cd ~/testdir
$ for i in {1..10}; do pv -aSs450m < /dev/zero > test; rm -f test; done
[ 143MiB/s]
[ 149MiB/s]
[ 148MiB/s]
[ 147MiB/s]
[ 145MiB/s]
[ 138MiB/s]
[ 145MiB/s]
[ 147MiB/s]
[ 146MiB/s]
[ 146MiB/s]
```

To unmount the bindfs volumes it’s easiest to go to `$ open /Volumes/` and click on the eject symbol.

## Conclusion

Binding folders to a RAM disk significantly speeds up the write throughput of the bound folders. You may use it for hot paths in your file system where many files are constantly read and written.

### Which folders may be sped up?

```shell
$ sudo fs_usage -w -t 5 -f filesys | tee fs_usage.log | egrep -o '(/.+?) {3}' | sed -e 's/\/dev\/disk[^ ]+  //' | sort | uniq -c | sort -nr
```
