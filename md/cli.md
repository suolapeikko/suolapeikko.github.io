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

`sudo plutil -p "/Library/Preferences/OpenDirectory/DynamicData/Active Directory/EU.plist"`


Network
-------

Show active network adapter:

`ifconfig | grep -B 6 'status: active' | head -n 1 | cut -d : -f 1§`

[Nmap cheat sheet](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)

Check count of spesific port (80) [socket statuses](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Protocol_operation) in 5 second intervals:

`while :; do sudo netstat -an|grep ESTABLISHED |grep 80| wc -l; sleep 5;  done`

Java
----

Check Java home:

`/usr/libexec/java_home`

[My Linux Java troubleshooting repository with commands that can also be applied to macOS with minor modifications](https://github.com/suolapeikko/troubleshooting_java)

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

Smart Cards
-----------

Smart Card testing utility:

`pcsctest`

[ATR value decoder](https://smartcard-atr.appspot.com)

Certificates
------------

[My certificate management repository](https://github.com/suolapeikko/about_certificates)

