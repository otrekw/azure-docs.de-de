---
title: Auswirkungen des Azure Stack Edge-Updates vom Januar 2021 | Microsoft-Dokumentation
description: Beschreibt die Auswirkungen der IoT Edge-Rollenverwaltung auf Azure Stack Edge-Geräte nach der Installation des Updates vom Januar 2021.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 10/26/2020
ms.author: alkohli
ms.openlocfilehash: 4b54f75b7d90e4b3a0a11d2ecdc676bb48eeee99
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335681"
---
# <a name="iot-edge-role-management-changes-for-your-azure-stack-edge"></a>Änderungen der IoT Edge-Rollenverwaltung für Ihr Azure Stack Edge-Gerät

Verwenden Sie für die IoT Edge-Rollenverwaltung für Ihr Azure Stack Edge-Gerät die neueste Version von API, SDK und Azure PowerShell, die im Januar 2021 veröffentlicht wird. In diesem Artikel werden die Änderungen ausführlich beschrieben, die erforderlich sind, wenn Sie die neueste Version von API, SDK und PowerShell-Cmdlets für die IoT Edge-Rollenverwaltung auf Ihrem Azure Stack Edge-Gerät verwenden.

Das Update vom Januar 2021 ist nur für Azure Stack Edge Pro – GPU-, Azure Stack Edge Pro R- und Azure Stack Edge Mini R-Geräte verfügbar. Die Informationen in diesem Artikel gelten nur für diese Geräte. 

## <a name="iot-edge-role-management-changes"></a>Änderungen der IoT Edge-Rollenverwaltung

Nachdem Sie die optionale Gerätesoftwareversion vom Januar 2021 auf Ihrem Azure Stack Edge-Gerät installiert haben, müssen Sie die neueste Version von API, SDK und PowerShell-Cmdlets für die IoT Edge-Rollenverwaltung verwenden.

- Wenn Sie die Rollenverwaltungs-API mit Version 2019-08-01 verwenden, führen Sie ein Upgrade auf die API-Version durch, die im Januar 2021 veröffentlicht wird. 
- Wenn Sie die Rollenverwaltung mit SDK-Version 1.0.0 vornehmen, führen Sie ein Upgrade auf die Version durch, die im Januar 2021 veröffentlicht wird.
- Wenn Sie die Rollenverwaltung mit Azure PowerShell-Cmdlets (Vorschau) verwenden (z. B. `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` oder `Remove-AzStackEdgeRole`), müssen Sie auf die neuen Cmdlets warten, die im Februar 2021 veröffentlicht werden.

Die oben genannten Änderungen sind nur erforderlich, wenn Sie das Update vom Januar 2021 anwenden. Wenn Sie die vorhandene Version der Gerätesoftware beibehalten, wirkt sich dies nicht auf die IoT Edge-Rollenverwaltung aus. Es wird jedoch empfohlen, Ihr Gerät für neue Funktionen und Sicherheitsverbesserungen zu aktualisieren. 


## <a name="api-usage"></a>API-Verwendung

Wenn Sie die IoT Edge-Rollenverwaltung über die API vorgenommen haben, sollten Sie die neue API-Version (2020-12-01) verwenden, die zu einem späteren Zeitpunkt veröffentlicht wird. Wenn Sie die aktuelle Rollen-API verwenden und die kommende Version der Gerätesoftware installiert haben, müssen Sie zur Kubernetes-Rolle PUT, GET, DELETE gefolgt von der IoT Add-On-API für PUT wechseln.


### <a name="for-put-method"></a>PUT-Methode

#### <a name="current-http-request"></a>Aktuelle HTTP-Anforderung 

- Die API-Aufrufe werden über diesen URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01

- Der Anforderungstext sieht folgendermaßen aus:

    ```json
    {
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        }
    }
    ```

#### <a name="upcoming-http-request"></a>Kommende HTTP-Anforderung 

- Die API-Aufrufe für die Kubernetes-Rolle werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1?api-version=2020-12-01

    Der Anforderungstext sieht folgendermaßen aus:

    ```json
    {
        "kind": "Kubernetes",
        "properties": {
            "hostPlatform": "Linux",
            "kubernetesClusterInfo": {
                "version": "v1.17.3"
            },
            "kubernetesRoleResources": {
                "storage": {
                    "endpoints": []
                },
                "compute": {
                    "vmProfile": "DS1_v2"
                }
            }
        }
    }
    ```

- Die API-Aufrufe für das IoT Edge-Add-On werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01


    Der Anforderungstext sieht folgendermaßen aus:

    ```json
    {
        "kind": "IoT",
        "properties": {
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotDevice;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "348586569999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {
                        "connectionString": {
                            "value": "Encrypted<<HostName=iothub.azure-devices.net;DeviceId=iotEdge;SharedAccessKey=2C750FscEas3JmQ8Bnui5yQWZPyml0/UiRt1bQwd8=>>",
                            "encryptionCertThumbprint": "1245475856069999244",
                            "encryptionAlgorithm": "AES256"
                        }
                    }
                }
            }
        }
    }
    ```


### <a name="for-get-method"></a>GET-Methode

#### <a name="current-http-response"></a>Aktuelle HTTP-Antwort

- Die API-Aufrufe werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01


- Der Antworttext sieht folgendermaßen aus:

    ```json
        "kind": "IOT",
        "properties": {
            "hostPlatform": "Linux",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "shareMappings": [],
            "roleStatus": "Enabled"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1",
        "name": "IoTRole1",
        "type": "dataBoxEdgeDevices/roles"
    }    
    ```

#### <a name="upcoming-http-response"></a>Kommende HTTP-Antwort

- Die API-Aufrufe werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01
- Der Antworttext sieht folgendermaßen aus: 

    ```json
    {
        "kind": "IoT",
        "properties": {
            "provisioningState": "Creating",
            "ioTDeviceDetails": {
                "deviceId": "iotdevice",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "ioTEdgeDeviceDetails": {
                "deviceId": "iotEdge",
                "ioTHostHub": "iothub.azure-devices.net",
                "ioTHostHubId": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/Microsoft.Devices/IotHubs/testrxiothub",
                "authentication": {
                    "symmetricKey": {}
                }
            },
            "version": "0.1.0-beta10"
        },
        "id": "/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/res1/roles/kubernetesRole/addons/iotName",
        "name": " iotName",
        "type": "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addon",
    }
    ```

### <a name="for-delete-method"></a>DELETE-Methode

### <a name="current"></a>Aktuell

Die API-Aufrufe werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/IoTRole1?api-version=2019-08-01

### <a name="upcoming"></a>Anstehend

Die API-Aufrufe werden über den folgenden URI durchgeführt: https://management.azure.com/subscriptions/4385cf00-2d3a-425a-832f-f4285b1c9dce/resourceGroups/GroupForEdgeAutomation/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/testedgedevice/roles/KubernetesRole1/addons/iotaddon?api-version=2020-12-01


## <a name="sdk-usage"></a>SDK-Verwendung

Wenn Sie das SDK verwenden und das Geräteupdate vom Januar 2021 installiert haben, müssen Sie die Art der Einrichtung der IoT Edge-Rolle ändern, wie es in der folgenden Tabelle gezeigt wird. Anschließend sollten Sie das kommende NuGet-Paket herunterladen und installieren, um zum neuen SDK zu wechseln, wie es im folgenden Beispiel gezeigt wird.

### <a name="current-sdk-sample"></a>Aktuelles SDK-Beispiel

```csharp
var iotRoleStatus = "Enabled";
var iotHostPlatform = "Linux";
var id = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/iotrole"; 
var name = "iotrole";
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var iotRoleName = "iotrole";
var ioTDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeDeviceDetails = new IoTDeviceInfo(...);
var ioTEdgeAgentInfo = new IoTEdgeAgentInfo(...);
var shareMappings = new List<MountPointMap>(...);

var role = new IoTRole(roleStatus, 
    hostPlatform, 
    shareMappings, 
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    id, 
    name, 
    type);

DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, iotRoleName, role, resourceGroup);
```


### <a name="new-sdk-sample"></a>Neues SDK-Beispiel

```csharp
var k8sRoleStatus = "Enabled";
var k8sHostPlatform = "Linux";
var k8sId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole"; 
var k8sRoleName = "KubernetesRole";
var k8sClusterVersion = "v1.17.3"; //Final values will be updated here around January 2021
var k8sVmProfile = "DS1_v2"; //Final values will be updated here around January 2021
var type = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/role";
var k8sRole = new KubernetesRole(
    roleStatus,
    hostPlatform,
    shareMappings,
    k8sClusterVersion,
    k8sVmProfile,
    k8sId,
    k8sRoleName,
    type
);
DataBoxEdgeManagementClient.Roles.CreateOrUpdate(deviceName, k8sRoleName, k8sRole, resourceGroup); //Final usage will be updated here around January 2021

var ioTId = $@"/subscriptions/546ec571-2d7f-426f-9cd8-0d695fa7edba/resourceGroups/resourceGroup/providers/Microsoft.DataBoxEdge/dataBoxEdgeDevices/deviceName/roles/KubernetesRole/addons/iotaddon";
var ioTAddonName = "iotaddon";
var ioTAddonType = "Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/addons";
var addon = new IoTAddon(
    ioTDeviceDetails, 
    ioTEdgeDeviceDetails, 
    ioTEdgeAgentInfo, 
    ioTId, 
    ioTAddonName, 
    ioTAddonType);
DataBoxEdgeManagementClient.AddOns.CreateOrUpdate(deviceName, k8sRoleName, addonName, addon, resourceGroup); //Final usage will be updated here around January 2021
```

## <a name="cmdlet-usage"></a>Cmdlet-Verwendung

Wenn Sie die Cmdlets `Get-AzStackEdgeRole`, `New-AzStackEdgeRole`, `Set-AzStackEdgeRole` oder `Remove-AzStackEdgeRole` verwenden, müssen Sie auf die neue Version warten, die für den Februar 2021 geplant ist.

## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

#### <a name="i-am-using-azure-stack-edge-pro--fpga-does-the-january-2021-update-affect-the-fpga-model"></a>Ich verwende Azure Stack Edge Pro – FPGA. Wirkt sich das Update vom Januar 2021 auf das FPGA-Modell aus?

Nein. Das Update vom Januar 2021 betrifft nur Azure Stack Edge – GPU-, Azure Stack Edge Pro R- und Azure Stack Edge Mini R-Geräte. Azure Stack Edge Pro – FPGA ist von diesem Update nicht betroffen, und es sind keine Änderungen der IoT Edge-Rollenverwaltung erforderlich.

#### <a name="after-i-update-my-azure-stack-edge-pro---gpu-to-the-new-device-software-in-january-2021-are-any-of-the-existing-services-affected"></a>Sind nach der Aktualisierung meines Azure Stack Edge Pro – GPU-Geräts auf die neue Gerätesoftware im Januar 2021 bereits vorhandene Dienste betroffen?

Nein. Die konfigurierten Dienste sind nach der Installation des Geräteupdates im Januar 2021 nicht beeinträchtigt. <!--check w/ Anoob, what existing services you are talking about in this question-->

#### <a name="what-are-the-high-level-changes-to-iot-edge-management-api-sdk-or-cmdlet"></a>Welche allgemeinen Änderungen gelten für API, SDK oder Cmdlets für die IoT Edge-Geräteverwaltung?

IoT Edge ist ein Add-On unter der Kubernetes-Rolle. Deshalb müssen Sie sicherstellen, dass zuerst Kubernetes konfiguriert wird, und dann die IoT Edge-Konfiguration ausführen.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Anwenden von Updates](azure-stack-edge-gpu-install-update.md).

