# Managing VMware vSphere Infrastructure using GOVC 

govc is a vSphere CLI built on top of govmomi.

The CLI is designed to be a user friendly CLI alternative to the GUI and well suited for automation tasks. It also acts as a test harness for the govmomi APIs and provides working examples of how to use the APIs.

![MyImage](https://github.com/collabnix/govc/blob/master/govc-gocmomi.png)<br>

# Content

[Getting Started with GOVC](https://github.com/collabnix/govc/blob/master/README.md#installation)<br>
[Workshop #1 - Creating a New Datacenter](https://github.com/collabnix/govc/blob/master/README.md#workshop-1)<br>
[Workshop #2 - Adding ESXi to the Cluster](https://github.com/collabnix/govc/blob/master/README.md#workshop-2)<br>
[Workshop #3 - Deploy Virtual Machine from ISO Image](https://github.com/collabnix/govc/blob/master/README.md#workshop-3)<br>
[Workshop #4 - Deploy Virtual Machine from template](https://github.com/collabnix/govc/blob/master/README.md#workshop-4)<br>


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

## Fetching Demo Datacenter Information

```
# govc datacenter.info demo
Name:                demo
  Path:              /demo
  Hosts:             0
  Clusters:          0
  Virtual Machines:  0
  Networks:          0
  Datastores:        0

```

## Creating a new cluster

```
govc cluster.create -dc=demo democluster

```

## How to add ESXi Host to this Cluster

- First we need to take care of cert and put it under thumbprint variable

```
thumbprint=$(govc about.cert -k -u <ESXi IP> -thumbprint | awk '{print $2}') 
```

- Now add the ESXi host flawlessly

```
govc cluster.add -cluster democluster -hostname <esxi IP> -username root -password <password> -thumbprint $thumbprint 
```



## How to remove ESXI Host from this cluster

- This is a 2 step process. First, put the ESXi host in maintenance mode by running below command:

```
govc host.maintenance.enter -dc=demo <ESXi IP> 
```

- Now you can remove ESXi

```
govc host.remove -dc=demo <ESXi IP>
```

# Workshop #2
<TBD>

# Workshop #3:
<TBD>

# Workshop #4: 
<TBD>
