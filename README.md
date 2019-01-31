# Managing Virtual Infrastructure using GOVC 

## Install GOVC

```
wget https://github.com/vmware/govmomi/releases/download/v0.19.0/govc_linux_amd64.gz
gunzip govc_linux_amd64.gz
mv govc_linux_amd64 govc
cp -rf govc /usr/local/bin/
```

## Export the credentials:

```
export GOVC_URL='root:dell@123@100.98.26.108'
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
