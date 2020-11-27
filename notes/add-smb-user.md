# Adding a Samba user

Suppose we want to share some files with a certain user on our network.

First the user must exist on the host server:

    $ sudo useradd jayne
    $ sudo passwd jayne

Once the user has a local account their corresponding Samba samba user can be added
using `smbpasswd -a` command. The `smbpasswd` command when used with `-a `option adds
the new samba user and also allows you to set the password for the new samba user.

    $ sudo smbpasswd -a jayne
    New SMB password:
    Retype new SMB password:

Configure the Samba share in the `/etc/samba/smb.conf` file to allow the new
user to browse the share:

    [share1]
    comment = movies etc on thorpe
    path = /path/to/sharedir
    valid users = dave jayne
    guest ok = no
    read only = yes

Then, restart the samba service:

    sudo service smbd restart
