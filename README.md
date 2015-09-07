# Packer templates for Oracle Enterprise Linux

### Overview

This repository contains templates for Oracle Enterprise Linux that can create
Vagrant boxes using Packer.

## Current Boxes

64-bit boxes:

* [Oracle Enterprise Linux 7.1 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel71)
* [Oracle Enterprise Linux 7.1 Desktop (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel71-desktop)
* [Oracle Enterprise Linux 7.0 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel70)
* [Oracle Enterprise Linux 7.0 Desktop (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel70-desktop)
* [Oracle Enterprise Linux 6.7 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel67)
* [Oracle Enterprise Linux 6.7 Desktop (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel67-desktop)
* [Oracle Enterprise Linux 6.6 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel66)
* [Oracle Enterprise Linux 6.6 Desktop (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel66-desktop)
* [Oracle Enterprise Linux 6.5 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel65)
* [Oracle Enterprise Linux 6.5 Desktop (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel65-desktop)
* [Oracle Enterprise Linux 6.4 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel64)
* [Oracle Enterprise Linux 5.11 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel511)
* [Oracle Enterprise Linux 5.10 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel510)
* [Oracle Enterprise Linux 5.9 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel59)
* [Oracle Enterprise Linux 5.8 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel58)
* [Oracle Enterprise Linux 5.7 (64-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel57)

32-bit boxes:

* [Oracle Enterprise Linux 6.7 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel67-i386)
* [Oracle Enterprise Linux 6.6 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel66-i386)
* [Oracle Enterprise Linux 6.5 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel65-i386)
* [Oracle Enterprise Linux 6.4 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel64-i386)
* [Oracle Enterprise Linux 5.11 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel511-i386)
* [Oracle Enterprise Linux 5.10 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel510-i386)
* [Oracle Enterprise Linux 5.9 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel59-i386)
* [Oracle Enterprise Linux 5.8 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel58-i386)
* [Oracle Enterprise Linux 5.7 (32-bit)](https://atlas.hashicorp.com/boxcutter/boxes/oel57-i386)

## Building the Vagrant boxes

To build all the boxes, you will need Packer and both VirtualBox and  VMware
Fusion.

A GNU Make `Makefile` drives the process via the following targets:

    make        # Build all the box types (VirtualBox and VMware)
    make test   # Run tests against all the boxes
    make list   # Print out individual targets
    make clean  # Clean up build detritus

To build one particular box, e.g. `oel66`,
for just one provider, e.g. VirtualBox,
first run `make list` subcommand:

    make list

This command prints the list of available boxes.
Then you can build one particular box for choosen provider:

    make virtualbox/oel66
    
### Proxy Settings

The templates respect the following network proxy environment variables
and forward them on to the virtual machine environment during the box creation
process, should you be using a proxy:

* http_proxy
* https_proxy
* ftp_proxy
* rsync_proxy
* no_proxy

### Tests

The tests are written in [Serverspec](http://serverspec.org) and require the
`vagrant-serverspec` plugin to be installed with:

    vagrant plugin install vagrant-serverspec

The `Makefile` has individual targets for each box type with the prefix
`test-*` should you wish to run tests individually for each box.

Similarly there are targets with the prefix `ssh-*` for registering a
newly-built box with vagrant and for logging in using just one command to
do exploratory testing.  For example, to do exploratory testing
on the VirtualBox training environmnet, run the following command:

    make ssh-box/virtualbox/oel65-nocm.box

Upon logout `make ssh-*` will automatically de-register the box as well.

### Makefile.local override

You can create a `Makefile.local` file alongside the `Makefile` to override
some of the default settings.  The variables can that can be currently
used are:

* CM
* CM_VERSION
* HEADLESS
* \<iso_path\>
* UPDATE

`Makefile.local` is most commonly used to override the default configuration
management tool, for example with Chef:

    # Makefile.local
    CM := chef

Changing the value of the `CM` variable changes the target suffixes for
the output of `make list` accordingly.

Possible values for the CM variable are:

* `nocm` - No configuration management tool
* `chef` - Install Chef
* `puppet` - Install Puppet
* `salt`  - Install Salt

You can also specify a variable `CM_VERSION`, if supported by the
configuration management tool, to override the default of `latest`.
The value of `CM_VERSION` should have the form `x.y` or `x.y.z`,
such as `CM_VERSION := 11.12.4`

The variable `HEADLESS` can be set to run Packer in headless mode.
Set `HEADLESS := true`, the default is false.

The variable `UPDATE` can be used to perform OS patch management.  The
default is to not apply OS updates by default.  When `UPDATE := true`,
the latest OS updates will be applied.

The variable `PACKER` can be used to set the path to the packer binary.
The default is `packer`.

The variable `ISO_PATH` can be used to set the path to a directory with
OS install images.  This override is commonly used to speed up Packer
builds by pointing at pre-downloaded ISOs instead of using the default
download Internet URLs.

The variables `SSH_USERNAME` and `SSH_PASSWORD` can be used to change
the default name & password from the default `vagrant`/`vagrant`
respectively.

The variable `INSTALL_VAGRANT_KEY` can be set to turn off installation
of the default insecure vagrant key when the image is being used
outside of vagrant.  Set `INSTALL_VAGRANT_KEY := false`, the default
is true.

### Acknowledgments

[SmartyStreets](http://www.smartystreets.com) is providing basebox hosting for the boxcutter project.

![Powered By SmartyStreets](https://smartystreets.com/resources/images/smartystreets-flat.png)
