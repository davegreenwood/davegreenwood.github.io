# Prevent OS X Firewall asking to accept or deny incoming connections

If an app does not have the proper code signing from Apple, it may repeatedly ask to allow or deny incoming connections - even if explicitly set in the firewall settings.
One especially disruptive case was the orca app for **plotly** in **jupyter** notebooks. Every run of a notebook would produce the deny/allow dialogue. To check if the app signing is the problem, in the terminal run:

    codesign -vvv orca.app/

In this case the output shows the app is not signed at all.

    orca.app/: code object is not signed at all
    In architecture: x86_64

It is possible to force a code sign on the machine with:

    sudo codesign --force --deep --sign - orca.app

The dialogue will appear one more time, then will remain satisfied until the app is upgraded or changed in some way.
