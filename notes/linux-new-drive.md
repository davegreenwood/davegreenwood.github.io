# Format and install a new drive for linux

This guide is for drives only for use on a linux system, starting from a
brand new drive installed internally.

## Block devices

Hard drives read and write data in fixed-size blocks. The easy way to list the
block devices attached to your Linux system is to use the lsblk (list block devices) command:

    $ lsblk
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   5.5T  0 disk
    sdb      8:16   0 232.9G  0 disk
    ├─sdb1   8:17   0 224.9G  0 part /
    ├─sdb2   8:18   0     1K  0 part
    └─sdb5   8:21   0     8G  0 part [SWAP]

The device identifiers are listed in the left column, each beginning with sd, and ending with a letter, starting with a.
Each partition of each drive is assigned a number, starting with 1.
Here the new drive is sda and does not have any partitions.

Another useful command is `fdisk`, which requires `sudo` to run:

    $ sudo fdisk -l
    Disk /dev/sda: 5.5 TiB, 6001175126016 bytes, 11721045168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes


    Disk /dev/sdb: 232.9 GiB, 250059350016 bytes, 488397168 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0x1b472a4f

    Device     Boot     Start       End   Sectors   Size Id Type
    /dev/sdb1  *         2048 471687167 471685120 224.9G 83 Linux
    /dev/sdb2       471689214 488396799  16707586     8G  5 Extended
    /dev/sdb5       471689216 488396799  16707584     8G 82 Linux swap / Solaris

## Partitions

Create partitions using `parted`, first labeling scheme:

    $ sudo parted /dev/sda mklabel gpt

then the partition, one partition using all of the drive.

    $ sudo parted -a opt /dev/sda mkpart primary ext4 0% 100%

check with `lsblk`

    lsblk
    NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda      8:0    0   5.5T  0 disk
    └─sda1   8:1    0   5.5T  0 part
    sdb      8:16   0 232.9G  0 disk
    ├─sdb1   8:17   0 224.9G  0 part /
    ├─sdb2   8:18   0     1K  0 part
    └─sdb5   8:21   0     8G  0 part [SWAP]

Now that we have a partition available, we can format it as an Ext4 filesystem.
To do this, pass the partition to the `mkfs.ext4` utility.

We can add a partition label by passing the `-L` flag,
using a name that will help you identify this particular drive:

    $ sudo mkfs.ext4 -L toshiba6tb /dev/sda1

This can take a few seconds on a large drive.

## Mount drive

Create a mount point under `/mnt` :

    sudo mkdir /mnt/parity0

find the drive identifier:

    ls /dev/disk/by-id

and create an entry in `/etc/fstab`:

    /dev/disk/by-id/ata-TOSHIBA_HDWD260_9061S19US5FH-part1 /mnt/parity0   ext4   defaults   0   0

finally, `sudo mount -a`, then `df -h `should show the new drive mounted.
