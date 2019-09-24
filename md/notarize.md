# Package Notarization Quick Start Guide

## Add an application spesific password

1 Log in to appleid.apple.com using your developer ID
2 Create an app specific password for "altool" label and save the app password

## Sign the package

`productsign --sign "Developer ID Installer: Firstnme Lastname (ABCD12345)" my.pkg my_signed.pkg`

## (Optional) Verify package signature

`pkgutil --check-signature my_signed.pkg`

## Notarize the package

`xcrun altool --notarize-app -t osx -f my_signed.pkg --primary-bundle-id com.my.app -u first.last@acme.com --output-format xml`

(Note! It will prompt for password, and that password is the app password generated for altool)

## Grab the RequestUUID from the XML output of the previous command

## Check the notarization status

`xcrun altool --notarization-info RequestUUIDHere -u first.last@acme.com
`
## After notarization status shows success (instead of in progress), confirm success

`spctl -a -v --type install my_signed.pkg`
