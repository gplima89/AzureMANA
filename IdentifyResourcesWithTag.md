Resources
| where type in (
    "microsoft.compute/virtualmachines",
    "microsoft.compute/virtualmachinescalesets"
)
| extend
    HasLegacyTag = iff(
        set_has_element(bag_keys(tags), "LegacyVMNVA"),
        "Yes",
        "No"
    ),
    publisher = case(
        type =~ "microsoft.compute/virtualmachines",
        tostring(properties.storageProfile.imageReference.publisher),
        type =~ "microsoft.compute/virtualmachinescalesets",
        tostring(properties.virtualMachineProfile.storageProfile.imageReference.publisher),
        ""
    ),
    offer = case(
        type =~ "microsoft.compute/virtualmachines",
        tostring(properties.storageProfile.imageReference.offer),
        type =~ "microsoft.compute/virtualmachinescalesets",
        tostring(properties.virtualMachineProfile.storageProfile.imageReference.offer),
        ""
    ),
    vmSize = tolower(case(
        type =~ "microsoft.compute/virtualmachines",
        tostring(properties.hardwareProfile.vmSize),
        type =~ "microsoft.compute/virtualmachinescalesets",
        tostring(sku.name),
        ""
    ))
| where isnotempty(publisher)
| where HasLegacyTag contains "No"
| where publisher contains "paloalto"
    or publisher contains "checkpoint"
    or publisher contains "fortinet"
    or publisher contains "f5"
    or publisher contains "cisco"
    or publisher contains "citrix"
    or publisher contains "a10"
    or publisher contains "arista"
    or publisher contains "aviatrix"
    or publisher contains "avi-networks"
    or publisher contains "barracuda"
    or publisher contains "cohesive"
    or publisher contains "imperva"
    or publisher contains "netapp"
    or publisher contains "asdivertissementinc1617837708654"
    or publisher contains "daceitdbasensetrafficpulse1579892024934"
    or publisher contains "ergoninformatikag1581586464404"
    or publisher contains "fatalsecurity1604924013537"
    or publisher contains "haproxy-technologies"
    or publisher contains "heimdall-data"
| where offer contains "check-point"
    or offer contains "infinity-gw"
    or offer contains "sg2"
    or offer contains "vsec"
    or offer contains "cp-vwan"
    or offer contains "checkpoint_wapp"
    or offer contains "cp_saas"
    or offer contains "cpcgcspm"
    or offer contains "checkpoint-sentinel"
    or offer contains "azure-sentinel-checkpoint-"
    or offer contains "checkpoint-"
    or offer contains "xdr_xpr"
    or offer contains "test_saas"
    or offer contains "forti"
    or offer contains "fgt"
    or offer contains "airs-flex"
    or offer contains "aks-cngfw"
    or offer contains "automation_test_vmseries"
    or offer contains "cloud-ngfw-private-offer"
    or offer contains "cloudngfw-sentinel-solution"
    or offer contains "cortex_xsoar"
    or offer contains "pan_swfw_cloud_ngfw"
    or offer contains "panorama"
    or offer contains "pan-prisma"
    or offer contains "pcce_twistlock"
    or offer contains "prisma"
    or offer contains "twistlock"
    or offer contains "vmseries"
    or offer contains "vwan-managed-nva"
    or offer contains "cloud-"
    or offer contains "dii_"
    or offer contains "instaclustr"
    or offer contains "netapp"
    or offer contains "riyoa"
    or offer contains "f5_networks"
    or offer contains "f5_threat"
    or offer contains "f5-big"
    or offer contains "f5-distributed"
    or offer contains "f5-nginx"
    or offer contains "f5-web"
    or offer contains "f5xc"
    or offer contains "flexible-buying-program"
    or offer contains "cisco_business"
    or offer contains "cisco_cloud"
    or offer contains "cisco_secure"
    or offer contains "cisco_smart"
    or offer contains "cisco_thousandeyes_mpo"
    or offer contains "cisco-aci"
    or offer contains "cisco-adaptive"
    or offer contains "cisco-asav"
    or offer contains "cisco-c8000v"
    or offer contains "cisco-catalyst"
    or offer contains "cisco-ccv"
    or offer contains "cisco-csr"
    or offer contains "cisco-dna"
    or offer contains "cisco-firepower"
    or offer contains "cisco-fmcv"
    or offer contains "cisco-ftdv"
    or offer contains "cisco-ise"
    or offer contains "cisco-meraki"
    or offer contains "cisco-multicloud"
    or offer contains "cisco-nexus"
    or offer contains "cisco-ngfwv"
    or offer contains "cisco-resource"
    or offer contains "cisco-sdwan"
    or offer contains "cisco-spaces"
    or offer contains "cisco-tdv"
    or offer contains "cisco-vcube"
    or offer contains "cisco-waas"
    or offer contains "cisco-wlc"
    or offer contains "cisco-wlc-template"
    or offer contains "secureaccess_mpo"
    or offer contains "vwaas-azure"
    or offer contains "adcvpxfips"
    or offer contains "adm-onprem"
    or offer contains "citrix_waf_mss"
    or offer contains "citrix-"
    or offer contains "citrix"
    or offer contains "connector-appliance"
    or offer contains "ctx"
    or offer contains "netscaler"
    or offer contains "sharefile_v5"
    or offer contains "xenapp"
    or offer contains "a10training"
    or offer contains "a10-vthunder"
    or offer contains "vthunder"
    or offer contains "awake-security-sentinel"
    or offer contains "cloudeos-router"
    or offer contains "dmf-controller-byol"
    or offer contains "velocloud"
    or offer contains "veos-router"
    or offer contains "playfab_xr"
    or offer contains "aviatrix"
    or offer contains "avx-sec"
    or offer contains "avi-vantage-adc"
    or offer contains "barracuda"
    or offer contains "prod-ccb"
    or offer contains "prod-zt"
    or offer contains "waf"
    or offer contains "cohesive"
    or offer contains "vns3"
    or offer contains "2021-04-16-11"
    or offer contains "2022-saas"
    or offer contains "3223_edge"
    or offer contains "dace2it_public"
    or offer contains "saas-workshop"
    or offer contains "safety-gear"
    or offer contains "sense-traffic"
    or offer contains "stp-vehicle"
    or offer contains "airlock-gateway"
    or offer contains "multi-port-forward-server"
    or offer contains "hapee"
    or offer contains "heimdall-data"
    or offer contains "imperva"
    or offer contains "securesphere"
| where (vmSize contains "standard_d" and (vmSize contains "_v5" or vmSize contains "_v4" or vmSize contains "_v3" or vmSize contains "_v2"))
    or (vmSize contains "standard_e" and (vmSize contains "_v5" or vmSize contains "_v4" or vmSize contains "_v3"))
    or (vmSize contains "standard_b" and vmSize contains "s_v2")
    or (vmSize contains "standard_a" and vmSize contains "_v2")
    or (vmSize contains "standard_f" and vmSize contains "s_v2")
    or (vmSize contains "standard_f" and not(vmSize contains "_v"))
    or vmSize contains "standard_g"
    or (vmSize contains "standard_l" and not(vmSize contains "_v"))
| project
    name,
    type,
    resourceGroup,
    subscriptionId,
    location,
    publisher,
    offer,
    vmSize,
    HasLegacyTag,
    tags
| order by subscriptionId, resourceGroup, name
| order by ['HasLegacyTag'] asc
