# Managing Virtual Infrastructure using GOVC 

govc is a vSphere CLI built on top of govmomi.

The CLI is designed to be a user friendly CLI alternative to the GUI and well suited for automation tasks. It also acts as a test harness for the govmomi APIs and provides working examples of how to use the APIs.

## Installation

You can find prebuilt govc binaries on the releases page.

Download and install a binary locally like this:

% curl -L $URL_TO_BINARY | gunzip > /usr/local/bin/govc
% chmod +x /usr/local/bin/govc


# Example
## Install GOVC on Ubuntu 18.04

```
wget https://github.com/vmware/govmomi/releases/download/v0.19.0/govc_linux_amd64.gz
gunzip govc_linux_amd64.gz
mv govc_linux_amd64 govc
cp -rf govc /usr/local/bin/
```

## Export the credentials:

```
export GOVC_URL='root:password@<IPaddress>'
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
