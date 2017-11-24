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
