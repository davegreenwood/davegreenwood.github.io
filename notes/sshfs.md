# Using SSHFS to mount drives on a heterogeneous network.

It is not always possible to create shared folders or drives - especially
with the correct protocol. If connection via ssh is possible, then a drive,
or any directory, can be mounted using *SSHFS*.

## Installation
SSHFS needs to be installed on the client machine

### Linux

        sudo apt-get install sshfs

### Mac

    brew cask install osxfuse
    brew install sshfs

### Usage

First, make a directory to mount to:

    mkdir ~/remote

This is where the files will be accessible when mounted, it can be any name.
Next, issue the command to actually mount the drive/folder.

    sudo sshfs <SSH-ACCESSIBLE-IP>:<PATH-TO-DRIVE> ~/remote

Where `<SSH-ACCESSIBLE-IP>`  could be  `user@192.168.1.100`  or
`user@machine-name.local`  to give a couple of examples. Note the colon...

In fact, sudo is not necessary to mount in the user home directory, but sudo
is needed to dismount the drive. This is done in the standard way -

    sudo umount ~/remote

