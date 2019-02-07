# Managing VMware Virtual Infrastructure using GOVC 

govc is a vSphere CLI built on top of govmomi.

The CLI is designed to be a user friendly CLI alternative to the GUI and well suited for automation tasks. It also acts as a test harness for the govmomi APIs and provides working examples of how to use the APIs.

## Installation

You can find prebuilt govc binaries on the releases page.

Download and install a binary locally like this:


## Install GOVC on Ubuntu 18.04

```
wget https://github.com/vmware/govmomi/releases/download/v0.19.0/govc_linux_amd64.gz
gunzip govc_linux_amd64.gz
mv govc_linux_amd64 govc
cp -rf govc /usr/local/bin/
chmod +x /usr/local/bin/govc
```

## Export the credentials:

```
export GOVC_URL='root:password@<IPaddress>'
```

## Disable certificate verification.

```
export GOVC_INSECURE=1
```

## Listing the Datacenter

```
govc ls 
```

## Listing ESXi Host 

```
/ha-datacenter/vm
/ha-datacenter/vm/CSERACKPAS61
/ha-datacenter/vm/CSELABDNS.local (1)
```

##  How to do vMotion?

Say, I have VM in datastore1 and I want to migrate it to "Local" datastore. I can run the below command to perform it:

```
govc vm.migrate -ds Local PhotonOS
[31-01-19 16:59:43] migrating VirtualMachine:vm-673... OK
```

## How to disable firewall?

```
govc host.esxcli -host=<IP> network firewall set --enabled false
```

## How to enable sfcb service?

```
 govc host.esxcli -host=<Ip> system wbem set -e 1
```

## How to configure NTP

```
 govc host.date.change -host <IP>  -server <IP>
 govc host.service -host <IP> enable ntpd
 govc host.service -host <IP> start ntpd
```

## Deploy VM from Template

```
govc vm.clone -vm=<template-name> -on=true -waitip=false -host=<targethostIP> -ds=<targetdatastore> newVMname
govc vm.clone -vm=windows8Server64Guest-template -on=true -waitip=false -host=XXX.XXX.XXX.XXX -ds=ESX67 windows8Server64Guest
```
## Create new folder inside datastore

```
govc datastore.mkdir -ds=datastorename newfolder
```

## Copying VM files to different folder

```
govc datastore.cp -ds=dastorename VM1/VM1.vmdk newfolder/VM1.vmdk
```

## Unregistering VM from VC

```
govc vm.unregister VMname
```


# Workshop #1: 

## Creating a new Datacenter

```
govc datacenter.create demo
```


```
root@ubuntu14:/home/cse# govc datacenter.info demo
Name:                demo
  Path:              /demo
  Hosts:             0
  Clusters:          0
  Virtual Machines:  0
  Networks:          0
  Datastores:        0
root@ubuntu14:/home/cse#
root@ubuntu14:/home/cse#
```

