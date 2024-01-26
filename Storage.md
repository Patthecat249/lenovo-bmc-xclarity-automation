# Raid Konfiguration mit onecli abfragen

```bash
# Befehl ausführen
cd /root/xclarity-onecli && ./onecli misc raid show --bmc USERID:PASSW0RD@10.0.249.10

# Output
[root@TPPJAHN2 xclarity-onecli]# ./onecli misc raid show --bmc USERID:PASSW0RD@10.0.249.10
[1s]Certificate check finished [100%][===================================================================================================================================>]


raidconfig show:

                                                 Broadcom Controller Information:
    ==========================================================================================================================
    | Controller ID | Target ID |                     Product Name                     | Serial No.  | FRU No.  |  Part No.  |
    --------------------------------------------------------------------------------------------------------------------------
    | 1             | 7         | ThinkSystem RAID 930-16i 4GB Flash PCIe 12Gb Adapter | L2ST78SS047 |  01KN508 | SR17A18205 |
    ==========================================================================================================================

                                                                   Disk Information:
    ==============================================================================================================================================
    | Disk ID |     Product Name     | State  | Slot No. | Disk Type | Media Type | Health Status |  Capacity  | Serial No. | FRU No. | Part No. |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 0       | MTFDDAK960TCB-1AR1ZA | Online | 0        | SATA      | SSD        | Normal        | 894.253GB  | 1E3E1F41   | 01GT750 | D7A09327 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 1       | MTFDDAK960TCB-1AR1ZA | Online | 1        | SATA      | SSD        | Normal        | 894.253GB  | 1E3E1F40   | 01GT750 | D7A09327 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 2       | ST2400MM0129         | Online | 2        | SAS       | HDD        | Normal        | 2235.618GB | WBM0HE2Z   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 3       | ST2400MM0129         | Online | 3        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LMYL   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 4       | ST2400MM0129         | Online | 4        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LN5K   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 5       | ST2400MM0129         | Online | 5        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LMEN   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 6       | ST2400MM0129         | Online | 6        | SAS       | HDD        | Normal        | 2235.618GB | WBM0KRY0   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 7       | ST2400MM0129         | Online | 7        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LLYC   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 8       | ST2400MM0129         | Online | 8        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LML8   | 01GV182 | D7A01907 |
    ----------------------------------------------------------------------------------------------------------------------------------------------
    | 9       | ST2400MM0129         | Online | 9        | SAS       | HDD        | Normal        | 2235.618GB | WBM0LM4R   | 01GV182 | D7A01907 |
    ==============================================================================================================================================

                           Pool Information:
    ==============================================================
    | Pool ID | RAID State |        RAID Capacity        | Holes |
    --------------------------------------------------------------
    | 0       | RAID 0     | 1786.275GB (200.275GB free) | 1     |
    ==============================================================

                                                                            Volume Information:
    ==================================================================================================================================================================      
    | Volume ID |     Name      | Status  | Stripe Size |   Bootable   |  Capacity  |  Read Policy  | Write Policy  | I/O Policy | Access Policy | Disk Cache Policy |      
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------      
    | 0         | esx-root      | Optimal | 64KB        | Bootable     | 50.000GB   | No Read Ahead | Write Through | Direct I/O | Read Write    | Unchanged         |      
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------      
    | 1         | esx-datastore | Optimal | 64KB        | Not Bootable | 1536.000GB | No Read Ahead | Write Through | Direct I/O | Read Write    | Unchanged         |      
    ==================================================================================================================================================================      

                          Pool Information:
    =============================================================
    | Pool ID | RAID State |       RAID Capacity        | Holes |
    -------------------------------------------------------------
    | 1       | RAID 0     | 17873.938GB (0.000GB free) | 0     |
    =============================================================

                                                                            Volume Information:
    ===================================================================================================================================================================     
    | Volume ID |     Name      | Status  | Stripe Size |   Bootable   |  Capacity   |  Read Policy  | Write Policy  | I/O Policy | Access Policy | Disk Cache Policy |     
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------     
    | 2         | esx-datengrab | Optimal | 64KB        | Not Bootable | 17873.938GB | No Read Ahead | Write Through | Direct I/O | Read Write    | Unchanged         |     
    ===================================================================================================================================================================     

    Show Configuration Result:
    =====================
    | Device |  Result  |
    ---------------------
    | 1      | Success. |
    =====================
At the same time, the RAID configuration is saved into XML file:
/root/xclarity-onecli/logs/OneCli-20240124-114928-57/raid_show_result.xml
Succeed.
[root@TPPJAHN2 xclarity-onecli]#
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
```
## Check, if Update are available
```bash
# Download updates for Server Model 7X06
# List all ostype
./onecli update acquire --mt 7X06 --insecure --ostype --report

# Download single Package
onecli update acquire -I lnvgy_fw_drvln_pdl246c-2.10_anyos_noarch
onecli update acquire -I lnvgy_fw_drvwn_pdl346a-2.10_anyos_noarch

# For Platform
mkdir update-downloads && cd update-downloads && ../onecli update acquire --mt 7X06 --insecure --ostype platform

# For VMware
mkdir update-downloads && cd update-downloads && ../onecli update acquire --mt 7X06 --insecure --ostype vmware

[root@TPPJAHN2 update-downloads]# ../onecli update acquire --mt 7X06 --insecure --ostype vmware --report
Start to download from Lenovo......

# Output for VMware
Reporting acquisition status of updates for Machine-Type=7X06 OS="VMware"
Done
lnvgy_fw_drvwn_pdl304t-1.01_anyos_noarch - Remotely available
lnvgy_fw_xcc_cdi306x-1.08_anyos_noarch - Remotely available
lnvgy_fw_lxpm_pdl106w-1.01_anyos_noarch - Remotely available
lnvgy_fw_drvln_pdl204o-1.01_anyos_noarch - Remotely available
lnvgy_fw_uefi_ive112k-1.02_anyos_32-64 - Remotely available
lnvgy_utl_uxsp_c5sp13p-1.00_vmware6.0_32-64 - Remotely available
Succeed.

# Compare Updates
./onecli update compare --bmc USERID:PASSW0RD@10.0.249.10

# Execute Update
../onecli update flash --bmc USERID:PASSW0RD@10.0.249.10 --platform --comparexml /root/xclarity-onecli/update-downloads/logs/OneCli-20240124-175155-2929/Onecli-update-compare.xml

# Test mit SFTP-Server
../onecli update flash --fileserver sftp://sftp-user:Test1234@10.0.249.165:22/lenovo --bmc USERID:PASSW0RD@10.0.249.10 --platform --comparexml /root/xclarity-onecli/update-downloadty-onecli/update-downloads/logs/OneCli-20240124-175155-2929/Onecli-update-compare.xml

../onecli update flash --sftp sftp-user:Test1234@10.0.249.165:22/lenovo --bmc USERID:PASSW0RD@10.0.249.10 --platform --comparexml /root/xclarity-onecli/update-downloadty-onecli/update-downloads/logs/OneCli-20240124-175155-2929/Onecli-update-compare.xml

# Teste XCC Update
#../onecli update flash --includeid lnvgy_fw_xcc_tei3a8l-4.20_anyos_noarch --nocompare --bmc USERID:PASSW0RD@10.0.249.10
../onecli update flash --includeid lnvgy_fw_raid_mr3.5.930-51.22.0-5096-2_linux_x86-64 --platform --bmc USERID:PASSW0RD@10.0.249.10
../onecli update flash --includeid lnvgy_fw_xcc_tei3d8o-6.00_anyos_noarch --nocompare --bmc USERID:PASSW0RD@10.0.249.10

../onecli update flash --includeid lnvgy_fw_xcc_cdi3a8n-9.40_anyos_noarch --nocompare --bmc USERID:PASSW0RD@10.0.249.10
../onecli update flash --includeid lnvgy_fw_xcc_cdi3b2n-9.86_anyos_noarch --platform --bmc USERID:PASSW0RD@10.0.249.10


```

# Bootable Media Creator
```bash
# Create bootable media with command
<LXCE BoMC> --function=update --machine-type=7X06 -l C:\Users\pjahn.SVA\Downloads\lenovo-sr650-firmware\bootablemediacreator --description BootableMedia_20240125-010223 --noreboot --iso C:\Users\pjahn.SVA\Downloads\lenovo-sr650-firmware\bootablemediacreator\bootable.iso --copytoramdisk --update-selection C:\Lenovo_Support\update_selection_1706141291427_0.json
```