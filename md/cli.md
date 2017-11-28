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
---------------------

Show active network adapter:

`ifconfig | grep -B 6 'status: active' | head -n 1 | cut -d : -f 1§`

[Nmap cheat sheet](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/)

