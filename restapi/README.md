# Rest API Code-Snippets
In this folder you will find some codesnippets for the lenovo xclarity controller rest api

```bash
# Save Usercredentials as a variable in a TOKEN
export TOKEN=$(echo -n "USERID:PASSW0RD" | base64)
export IPADDRESS=172.16.249.10

# Query the
curl -sk https://$IPADDRESS/redfish/v1/Systems/1 -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN"

# Alle Rest API Strings von Systems anzeigen lassen
curl -sk https://$IPADDRESS/redfish/v1/Systems/1 -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN" | jq . | grep "@odata.id" | cut -d":" -f2 | tr -d '"'

# Alle Rest API Strings von Chassis anzeigen lassen
curl -sk https://$IPADDRESS/redfish/v1/Chassis/1 -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN" | jq . | grep "@odata.id" | cut -d":" -f2 | tr -d '"'

# Alle PCIDevices-Links anzeigen lassen
curl -sk https://$IPADDRESS/redfish/v1/Chassis/1/PCIeDevices -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN" | jq . | grep "@
odata.id" | cut -d":" -f2 | tr -d '"' | sort

# Links zu den Disks von Volume 1 anzeigen lassen
curl -sk https://$IPADDRESS/redfish/v1/Systems/1/Storage/RAID_Slot7/Volumes/1 -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN" | jq -r '.Links.Drives[]."@odata.id"'

# Größe des Raids in Bytes
curl -sk https://$IPADDRESS/redfish/v1/Systems/1/Storage/RAID_Slot7/Volumes/2 -X GET -k -H "Content-type: application/json" -H "Authorization: Basic $TOKEN" | jq -r '.CapacityBytes'
16792989466624

```