# openshift-origin-liberty-cartridge

This cartridge provides an OpenShift Community Cartridge for IBM WebSphere Liberty
Profile. This cartridge does not actually ship the Liberty Profile binaries. These
have to be installed manually before this cartridge actually works. The Binaries can
be installed in 2 different ways. The following sections describe the 2 different methods.

## Installing the cartridge

Log into your node server(s) and install the cartridge. You can do this using the
following procedure, but you can install it however you want. You can basically just
copy + paste the commands in your shell and you should be good to go.

### Download the required WebSphere Liberty Installer

    node# cd /opt
    node# wget https://public.dhe.ibm.com/ibmdl/export/pub/software/websphere/wasdev/downloads/wlp/8.5.5.4/wlp-developers-runtime-8.5.5.4.jar

### Clone the Repository

    node# git clone https://github.com/juhoffma/openshift-origin-liberty-cartridge.git

    Initialized empty Git repository in /opt/openshift-origin-liberty-cartridge/.git/
    remote: Counting objects: 139, done.
    remote: Compressing objects: 100% (103/103), done.
    Receiving objects: 100% (139/139), 898.18 KiB | 1.33 MiB/s, done.
    remote: Total 139 (delta 46), reused 106 (delta 15), pack-reused 0
    Resolving deltas: 100% (46/46), done.

### Install the WebSphere Liberty Profile underneath the versions director

    node# java -jar /opt/wlp-developers-runtime-8.5.5.4.jar --acceptLicense /opt/openshift-origin-liberty-cartridge/versions/8.5.5.4

    Before you can use, extract, or install IBM WebSphere Application
    Server for Developers V8.5.5, you must accept the terms of
    International License Agreement for Non-Warranted Programs and
    additional license information. Please read the following license
    agreements carefully.


    The --acceptLicense argument was found. This indicates that you have
    accepted the terms of the license agreement.


    Extracting files to /opt/openshift-origin-liberty-cartridge/versions/8.5.5.4/wlp
    Successfully extracted all product files.

### Install and activate the cartridge

    node# oo-admin-cartridge --action install -s /opt/openshift-origin-liberty-cartridge --mco


    1 / 1
    vm.openshift.example.com
       output: install succeeded for /opt/openshift-origin-liberty-cartridge
    Finished processing 1 / 1 hosts in 3174.10 ms

    broker# oo-admin-ctl-cartridge -c import-node --activate --obsolete --force
    Importing cartridges from node 'vm.openshift.example.com'.
    Updating 26 cartridges ...
    54f62e57e659c5cd31000001 # U mysql-5.5 (active)
    54f62e57e659c5cd31000002 # U mysql-5.1 (active)
    54f62e57e659c5cd31000003 # U jenkins-1 (active)
    54f62e57e659c5cd31000004 # U nodejs-0.10 (active)
    54f62e57e659c5cd31000005 # U haproxy-1.4 (active)
    54f62e57e659c5cd31000006 # U jbosseap-6 (active)
    54f62e57e659c5cd31000007 # U jbossews-2.0 (active)
    54f62e57e659c5cd31000008 # U jbossews-1.0 (active)
    54f62e57e659c5cd31000009 # U php-5.4 (active)
    54f62e57e659c5cd3100000a # U php-5.3 (active)
    54f62e57e659c5cd3100000b # U mongodb-2.4 (active)
    54f62e57e659c5cd3100000c # U postgresql-9.2 (active)
    54f62e57e659c5cd3100000d # U postgresql-8.4 (active)
    54f62e57e659c5cd3100000e # U python-3.3 (active)
    54f62e57e659c5cd3100000f # U python-2.7 (active)
    54f62e57e659c5cd31000010 # U python-2.6 (active)
    54f62e57e659c5cd31000011 # U perl-5.10 (active)
    54f62e57e659c5cd31000012 # U diy-0.1 (active)
    54f62e57e659c5cd31000013 # U jenkins-client-1 (active)
    54f62e57e659c5cd31000014 # U ruby-1.8 (active)
    54f62e57e659c5cd31000015 # U ruby-1.9 (active)
    54f62e57e659c5cd31000016 # U ruby-2.0 (active)
    54f62e57e659c5cd31000017 # U amq-6.1.1 (active)
    54f62e57e659c5cd31000018 # U cron-1.4 (active)
    54f62e57e659c5cd31000019 # U fuse-6.1.1 (active)
    54f62e57e659c5cd3100001a # A hoffmann-liberty-8.5.5.4 (active)

Make sure you see the line reporting the cartridge **54f62e57e659c5cd3100001a # A hoffmann-liberty-8.5.5.4 (active)**

    broker# oo-admin-broker-cache --clear; oo-admin-console-cache --clear

## Installation outside of the cartridge context

You can also install the WebSphere Liberty Profile outside of the cartridge and
define the location using a node level variable like you can do with the full
[WebSphere](https://github.com/juhoffma/openshift-origin-websphere-cartridge) cartridge.

To make this work all you have to do is to create the file
`/etc/openshift/env/OPENSHIFT_LIBERTY_INSTALL_DIR` and put the installation location into
it. See the [Official Documentation](https://access.redhat.com/documentation/en-US/OpenShift_Enterprise/2/html-single/Administration_Guide/index.html#Creating_Environment_Variables_on_Node_Hosts) for an example on how to configure node level variables.

# Special things
Command port needs to be disabled. See http://www-01.ibm.com/support/docview.wss?uid=swg1PM89272

    command.port=-1

