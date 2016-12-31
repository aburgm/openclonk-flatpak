# openclonk-flatpak

This repository contains files needed to create a
[flatpak](http://flatpak.org/) for OpenClonk.

## Contents

 * kdeflatpak.gpg: GPG public key of the KDE runtime and SDK, imported from `http://distribute.kde.org/kdeflatpak.gpg`
 * org.openclonk.openclonk.json: flatpak-builder manifest used to build the OpenClonk flatpak

## Instructions

To build the flatpak, first install the `flatpak` and `flatpak-builder` tools
on your system, e.g. using `apt-get` on ubuntu.

First, install the KDE runtime and SDK:

```
flatpak remote-add kde http://distribute.kde.org/flatpak-testing/ --gpg-import=kdeflatpak.gpg
flatpak install kde org.kde.Platform
flatpak install kde org.kde.Sdk
```

Then build the OpenClonk flatpak by running

```
flatpak-builder openclonk org.openclonk.openclonk.json --gpg-sign=<gpg-key-id> --repo=oc-repo
```

The `<gpg-key-id>` should be the GPG Key ID of a key that is used to sign the
build (OSTree commit), i.e. a secret key needs to be available.

This creates (or updates) a flatpak repo in the directory `oc-repo`. If this
directory is accessible through HTTP or local file access, the flatpak can be
installed:

```
gpg --export=<gpg-key-id> > my-flatpak-key.gpg
flatpak add-remote openclonk http://<path-to-oc-repo> --gpg-import=my-flatpak-key.gpg
flatpak install org.openclonk.openclonk
```

To run it, search the list of installed applications for OpenClonk where a new
entry called "OpenClonk (snapshot)" should show up, or type:

```
flatpak run org.openclonk.openclonk
```
