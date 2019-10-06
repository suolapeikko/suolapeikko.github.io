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

The command above will prompt for an application password that you generated in the first step of this guide. You can also use password parameter by adding the application password to your Keychain and then adding this to the command above:  `-p "@keychain:altool"`

## Copy the RequestUUID from the XML output of the previous command

Just copy the RequestUUID part from the XML output of the previous command

## Check the notarization status

`xcrun altool --notarization-info RequestUUIDHere -u first.last@acme.com`

## Staple the ticket to your distribution

`xcrun stapler staple my_signed.pkg`

## (Optional) After notarization status shows success (instead of in progress), you can confirm the success

`spctl -a -v --type install my_signed.pkg`
