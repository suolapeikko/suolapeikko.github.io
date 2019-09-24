# Package Notarization Quick Start Guide

## Add an application specific password

1) Log in to appleid.apple.com using your Apple Developer ID

2) Create an application specific password for "altool" label and save the app password

## Sign the package

`productsign --sign "Developer ID Installer: Firstnme Lastname (ABCD12345)" my.pkg my_signed.pkg`

## (Optional) Verify package signature

`pkgutil --check-signature my_signed.pkg`

## Notarize the package

`xcrun altool --notarize-app -t osx -f my_signed.pkg --primary-bundle-id com.my.app -u first.last@acme.com --output-format xml`

It will prompt for a password, and that password is the application password generated for altool. You can also automate the password fill-in by adding the password to Keychain and add this to the command:  `-p “@keychain:ALTOOL`

## Grab the RequestUUID from the XML output of the previous command

## Check the notarization status

`xcrun altool --notarization-info RequestUUIDHere -u first.last@acme.com
`
## After notarization status shows success (instead of in progress), confirm success

`spctl -a -v --type install my_signed.pkg`
