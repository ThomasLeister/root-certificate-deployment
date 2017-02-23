# How-To: Root CA certificate integration for Linux and Windows

If you manage your own corporate or private Certificate Authority (CA), sooner or later you'll want to deploy the Root CA's certificate on your Linux and Windows Clients. This little How-To guides you through the process of deploying your root certificate.

**Assumption:** Your Root CA's certificate is existent as ```root.cert.pem```


## Linux

### System

Issue the following commands to import the new root certificate:

    sudo mkdir /usr/local/share/ca-certificates/extra
    sudo cp root.cert.pem /usr/local/share/ca-certificates/extra/root.cert.crt
    sudo update-ca-certificates

The system trust store ist used by most basic tools such as ```wget``` and ```curl```


### Browsers

Browsers on Linux, such as Firefox, Chromium, Chrome and Vivali won't use the system's trust store, but their own. Install ```libnss3-tools``` and execute the ```linux-browser-import.sh``` script to import your new root certificate into their trust stores (user-specific!)

Browsers will trust your CA after a restart.




## Windows

### System

#### Manually

Rename "root.cert.pem" to "root.cert.crt", so Windows will recognize the file as a certificate. Open the file and choose "Install certificate". Install the certificate to the "Trusted Certificate Authorities" directory - either system-wide or for a specific user only.


#### Via Active Directory

You can install new root certificates for every Windows Domain participant via ActiveDirectory.


### Browsers

#### Chrome, Vivaldi, Opera, Edge, Internet Explorer

These Browsers are using the Windows trust store and accept the certificate by default if it was installed into the Windows trust store before.


#### Firefox

Firefox uses it's own trust store and therefore doesn't accept the Root CA even if Windows does. You can manually import the Root Certificate in the Firefox Settings or enable experimental Windows trust store support:

Copy the file ```firefox-windows-truststore.js``` to Firefox's ```C:\Program Files (x86)\Mozilla Firefox\defaults\pref``` directory.

On the next start Firefox will trust your CA. This setting applies system-wide for all users.
