Useful Command Line Commands for macOS
======================================

Kerberos, LDAP and AD
---------------------

List of KDC servers:

`host -t SRV _kerberos._tcp.company.com`

List of servers supporting kpasswd service:

`host -t SRV _kpasswd._tcp.company.com`

User account attributes from LDAP:

`ldapsearch -H "ldap://dc.company.com" -b "dc=us,dc=company,dc=com" -LLL sAMAccountName="accountname"`

Show Active Directory settings:

`dsconfigad -show`

Show user IDs (domain user ID is > 1000):

`dscl . -list /Users UniqueID`

Get Active Directory Domain details:

`sudo plutil -p "/Library/Preferences/OpenDirectory/DynamicData/Active Directory/<fill_domain_here>.plist"`

Bind Mac to Active Directory:

`dsconfigad -a name_for_example_serial -u service_account -ou "OU=Macintosh,OU=Computers,DC=company,DC=com" -domain company.com -mobile enable -mobileconfirm enable -localhome enable -useuncpath enable`

Network
-------

Show active network adapter:

`ifconfig | grep -B 6 'status: active' | head -n 1 | cut -d : -f 1§`

[Nmap cheat sheet](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)

Check count of spesific port (80) [socket statuses](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Protocol_operation) in 5 second intervals:

`while :; do sudo netstat -an|grep ESTABLISHED |grep 80| wc -l; sleep 5;  done`

COMMAND | PID | USER | FD | TYPE | DEVICE | SIZE/OFF | NODE | NAME

Or better yet, list all states for spesific port (80):

`while :; do sudo netstat -nat | grep 80 | awk '{print $6}' | sort | uniq -c | sort -n; sleep 5; echo; done`

Show all connections:

`lsof -i -n`

Show all TCP states for spesific port (80):

`ss -tan |grep 80`

Check if you can connect to a specific server and port (Linux):

`echo -n > /dev/tcp/www.server.com/1234`

or

`nc -zv www.server.com 1234`

Check if you can connect to a specific server and port (macOS):

`nc -z www.server.com 1234`

or (macOS Mojave)

`/System/Library/CoreServices/Applications/Network\ Utility.app/Contents/Resources/stroke www.server.com 1234 1234`

Java
----

Check Java home:

`/usr/libexec/java_home`

[My Linux Java troubleshooting repository with commands that can also be applied to macOS with minor modifications](https://suolapeikko.github.io/md/troubleshooting_java.html)

System Integrity Protection (SIP)
---------------------------------

Disable SIP by booting into Recovery Mode (command+R) and starting Terminal:

`csrutil disable`

Re-enable SIP with DTrace support:

`csrutil enable --without dtrace`

DTrace
------

List available DTrace scripts:

`apropos dtrace`

[My FinMacSysAdmin examples](https://github.com/suolapeikko/dtrace)

Kernel Extensions
------

Find the bundle identifiers of apps that require kernel extension approval:

`kextstat | grep -v com.apple`

Find the Team ID of apps that require kernel extension approval:

`sqlite3 /var/db/SystemPolicyConfiguration/KextPolicy`

`SELECT * FROM kext_policy;`

Load / unload kernel extensions:

`kextload <kernel_extension>`

`kextunload <kernel_extension>`

iOS and Apple Configurator 2
----------------------------

List serials of all devices that are attached to your Mac:

`cfgutil -f get serialNumber | awk -v x=4 '{print $x}'`

Report information related to macOS Server caching servers running on the computer or on the local network:

`/usr/bin/AssetCacheLocatorUtil`

Clean/reformat configuration profile:

`security cms -D -i Signed.mobileconfig | xmllint --format - > Unsigned.mobileconfig`

Profiles
--------

List all Configuration Profiles with extended information:

`sudo /usr/bin/profiles show -P`

List all profiles options:

`profiles -h`

Renew a certificate payload. Remember to use sudo if you want to renew computer level profile, otherwise leave it out:

`sudo profiles -W -p <profile identifier>`

Smart Cards
-----------

Smart Card testing utility:

`pcsctest`

[ATR value decoder](https://smartcard-atr.appspot.com)

Certificates and signing
------------------------

[My certificate management notes](https://suolapeikko.github.io/md/about_certificates.html)

List valid identities:

`security find-identity -p basic -v`

Sign configuration profile:

`security cms -S -N "Developer ID Installer: John Doe (A1B1C1D1E1F1)" -i unsigned.mobileconfig -o signed.mobileconfig`

Show signing info:

`codesign -dv --verbose=4 /Applications/MyApp.app/`

Sign dmg:

`codesign --force -s "Developer ID Application: John Doe (A1B1C1D1E1F1)" MyApp.dmg`

Sign package:

`productsign --sign "Developer ID Installer: John Doe (A1B1C1D1E1F1)" TestPic-1.0b.pkg Signed-TestPic-1.0b.pkg`

Check code signing:

`codesign -dv --verbose=4 MyApp.dmg`

Check package signature:

`pkgutil --check-signature Signed-TestPic-1.0b.pkg`

Check whether the signature passes GateKeeper test:

`spctl -a -v --type install Signed-TestPic-1.0b.pkg`

Grab certificates from URL:

`openssl s_client -connect my.server:443 2>&1 < /dev/null | sed -n '/-----BEGIN/,/-----END/p' > certificate.pem`


How to create SSH keys and add them to authorized hosts list on the server
--------------------------------------------------------------------------------

Create SSH keys with custom name and passphrase:

`ssh-keygen -t rsa`

SSH into your host and create .ssh directory with correct rights:

`mkdir .ssh`

`chmod 700 .ssh`

Create authorized_keys file to .ssh and add rights:

`cd .ssh`

`touch authorized_keys`

`chmod 644 authorized_keys`

Copy contents of host_id_rsa.pub to authorized_keys

Packaging
---------

Build a package (folder structure TestPic/, TestPic/payload):

`pkgbuild --root payload --install-location "/Users/me/Desktop/" --identifier com.github.TestPic --version 1.0 TestPic.pkg`

Inspect the package:

`lsbom $(pkgutil --bom TestPic.pkg)`

Expand the package:

`pkgutil --expand TestPic.pkg TestPicExpanded`

Install the package:

`sudo installer -pkg TestPic.pkg -tgt /`

Find all the packages in the path:

`pkgutil --pkgs=com.github.*`

Inspect the package:

`pkgutil --info com.github.TestPic`

Inspect what was installed from the package:

`pkgutil --files com.github.TestPic`

Forget a package:

`pkgutil --forget com.github.TestPic`

Sign package:

`productsign --sign "Developer ID Installer: John Doe (A1B1C1D1E1F1)" TestPic-1.0b.pkg Signed-TestPic-1.0b.pkg`

Check package signature:

`pkgutil --check-signature Signed-TestPic-1.0b.pkg`

Check whether the signature passes GateKeeper test:

`spctl -a -v --type install Signed-TestPic-1.0b.pkg`

Create a macOS VM for VMWare Fusion
-----------------------------------

Download and install [AutoDMG](https://github.com/MagerValp/AutoDMG)

Download and install [vfuse](https://github.com/chilcote/vfuse)

Download and install [Qemu](https://github.com/chilcote/vfuse/wiki/Received-signal-11)

Download macOS from Mac App Store, wrap it into a disk image (create an empty dmg using Disk Utility and copy macOS installer there), drag the installer from newly created disk image to AutoDMG and let it convert it.

Create a template:
```
{
    "fusion_path": "",
    "source_dmg": "/Users/me/Development/macOS/Mojave/osx-10.14.4-18E226.apfs.dmg",
    "output_dir": "/Users/me/Desktop",
    "output_name": "mojave-vm-10.14.4",
    "cache": false,
    "mem_size": 4096,
    "disk_type": 0,
    "bridged": false,
    "mac_address": "",
    "enable3d": false,
    "vnc_port": 5901,
    "vnc_passwd": "",
    "hw_model": "iMacPro1,1",
    "serial_number": "VMWARE000001"
}
```

Create the VM with initial snapshot using vfuse and Qemu:

`vfuse --use-qemu /opt/local/bin/qemu-img -t generic_template.json --snapshot`
