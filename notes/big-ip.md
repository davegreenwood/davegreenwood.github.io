# Obfuscating login and password for BIG-IP Edge Client VPN connection

I need to connect to the workplace network via VPN using the [Big-IP client](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-client-configuration-12-0-0/5.html). Unfortunately, there is no certificate issued for connection, so each user must enter credentials at connection time. This is a problem for scripting access - I dont want to have the credentials stored in plain text.

We can encrypt user and password using **openssl** and store the encrypted file out of the way. It is noted that everything to view the credentials is on the one machine, but this does prevent casual viewing of plain text passwords.

First, if they don't already exist, create public/private keys, `ssh-keygen -t rsa`, following prompts. Then, to encrypt some text to a file, we can encode the user and password.

    echo "pass" | openssl rsautl -encrypt -inkey ~/.ssh/id_rsa -out ~/.vpn/vpn-pwd

And the same for the user:

    echo "user" | openssl rsautl -encrypt -inkey ~/.ssh/id_rsa -out ~/.vpn/vpn-usr

For maximum paranoia, delete bash history. Then, in a script to establish the connection, read the encoded files and pass them to te connect command.

    U=`openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in ~/.vpn/vpn-usr`
    P=`openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in ~/.vpn/vpn-pwd`

    f5fpc   --start  \
            --host https://vpn.work-server.org/ \
            --nocheck \
            --user $U  \
            --password $P
    sleep 2
    f5fpc --info
    echo 'disconnect with f5fpc -o'

A more sophisticated script might check for an existing connection - grep on the info command helps here.

    # Connect to the big-ip client.
    U=`openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in ~/.vpn/vpn-usr`
    P=`openssl rsautl -decrypt -inkey ~/.ssh/id_rsa -in ~/.vpn/vpn-pwd`
    connected=`f5fpc --info | grep vpn`

    if [[ -z $connected ]]
    then
        echo "not connected. connecting..."
        f5fpc  --start  \
                --host https://vpn.work-server.org/  \
                --nocheck \
                --user $U  \
                --password $P
        sleep 2
        f5fpc --info
    else
        echo "Already connected or connecting..."
        f5fpc --info
    fi
