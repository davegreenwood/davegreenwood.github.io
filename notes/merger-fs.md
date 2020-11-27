# MergerFS - replace a drive

MergerFS is a great utility for combining JBOD into one volume.
    <https://github.com/trapexit/mergerfs>

Used with snapraid, we have a reasonably robust media server.

The github repo has instructions for disk replacement in the docs, for example:

<https://github.com/trapexit/backup-and-recovery-howtos/blob/master/docs/recovery_(mergerfs).md>

but I want to use the same mount point so will edit the `/etc/fstab` and reboot.

## Replacing an old drive

1. disable snapraid in the crontab
2. copy the data from the old drive to the replacement drive at a temporary mount point, eg:

        sudo rsync -avPHAX /mnt/disk1/ /media/usb/

3. remove the old drive from the machine.
4. update `/etc/fstab` so the uuid of the new drive replaces the old drive.
5. reboot - the new drive has replaced the old drive and is mounted at the same point
6. enable snapraid in crontab
7. run `snapraid touch`
8. run `snapraid sync`

