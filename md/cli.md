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

`lsof -i -n`

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

iOS and Apple Configurator 2
----------------------------

List serials of all devices that are attached to your Mac:

`cfgutil -f get serialNumber | awk -v x=4 '{print $x}'`

Report information related to macOS Server caching servers running on the computer or on the local network:

`/usr/bin/AssetCacheLocatorUtil`

Clean/reformat configuration profile:

`security cms -D -i Signed.mobileconfig | xmllint --format - > Unsigned.mobileconfig`

Smart Cards
-----------

Smart Card testing utility:

`pcsctest`

[ATR value decoder](https://smartcard-atr.appspot.com)

Certificates and signing
------------------------

[My certificate management repository](https://suolapeikko.github.io/md/cli.html)

Sign configuration profile:

`security cms -S -N "3rd Party Mac Developer Installer: John Doe (A1B1C1D1E1F1)" -i unsigned.mobileconfig -o signed.mobileconfig`
