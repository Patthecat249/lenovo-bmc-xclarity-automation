# lenovo-bmc-xclarity-automation
This Repository describes how to automate a Lenovo Bare-Metal-Configuration of Lenovo xClarity-Controller via "onecli" from Lenovo

## Download
- `https://datacentersupport.lenovo.com/us/en/documents/LNVO-TCLI`
- `https://download.lenovo.com/servers/mig/2023/11/16/58699/lnvgy_utl_lxce_onecli01l-4.3.0_linux_x86-64.tgz`

## Some commands
```bash
# Get Information about Server as HTML-Report
./onecli inventory getinfor --bmc username:password@bmc-ip-address --htmlreport --ffdc --output ./htmlreport/
```
## Show Description of IMM-Section
```bash 
./onecli config showdes imm --bmc USERID:PASSW0RD@10.0.249.10
```

## Show only RAID-Configuration
```bash
onecli raid show --bmc USERID:PASSW0RD@10.0.249.10
```

## Clear/Delete RAID-Configuration
```bash
# Delete whole RAID-Configuration without asking
./onecli misc raid clear --ctrl 1 --force --bmc USERID:PASSW0RD@10.0.249.10
```

## Creates RAID-Configuration
```bash
# Creates two volumes on DISK 0 and DISK 1 in RAID-0-Mode
./onecli misc raid add --file raid-config.ini --force --bmc USERID:PASSW0RD@10.0.249.10

# Content of raid-config.ini
[ctrl1-vol0]
     disks=0,1
     raid_level=0
     vol_name=volume-esx
     write_policy=0
     read_policy=0
     io_policy=0
     access_policy=0
     cache_policy=0
     volume_size=50GB
     strip_size=64K
[ctrl1-vol1]
     disks=0,1
     raid_level=0
     vol_name=volume-esx-datastore
     write_policy=0
     read_policy=0
     io_policy=0
     access_policy=0
     cache_policy=0
     volume_size=1536GB
     strip_size=64K
```

## Execute Batch-File with Parameters
```bash
# batch-file.txt must exist in local-folder
./onecli config batch --file batch-file.txt --bmc USERID:PASSW0RD@10.0.249.10
```

## Configurable Parameters
```bash
# Content of a batch-file should look like
set IMM.IMMInfo_Name "ESX02"
set IMM.IMMInfo_Contact "lenovo@patrickjahn.de"
set IMM.IMMInfo_Location "Serverhausen"
set IMM.IMMInfo_RoomId "Wohnzimmer"
set IMM.IMMInfo_RackId "Boden"
set IMM.IMMInfo_Lowest_U 1
set IMM.IMMInfo_FullPostalAddress "Serverstrasse 1 im RZ01"
set IMM.HostName1 "esx02"
set IMM.HostIPAddress1 10.0.249.10
set IMM.HostIPSubnet1 255.255.255.0
set IMM.GatewayIPAddress1 10.0.249.1
set IMM.DNS_Enable Enabled
set IMM.DNSPreference IPv4
set IMM.DNS_IP_Address1 10.0.249.53
set IMM.DNS_IP_Address2 8.8.8.8
set IMM.Search_Domain home.local
set IMM.TimeZone UTC+2:00
set IMM.DST Off
set BootOrder.BootOrder "Hard Disk=CD/DVD Rom=Network"
set BootModes.SystemBootMode "Legacy Mode"
set SecureBootConfiguration.SecureBootSetting Disabled
set IMM.IPv6Network1 Disabled
set IMM.DDNS_Enable Disabled
set IMM.LanOverUsb Disabled
```