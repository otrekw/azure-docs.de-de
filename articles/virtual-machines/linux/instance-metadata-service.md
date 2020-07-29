---
title: Azure-Instanzmetadatendienst
description: RESTful-Schnittstelle zum Abrufen von Informationen zu Compute-, Netzwerk- und anstehenden Wartungsereignissen von virtuellen Computern
services: virtual-machines
author: KumariSupriya
manager: paulmey
ms.service: virtual-machines
ms.subservice: monitoring
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 04/29/2020
ms.author: sukumari
ms.reviewer: azmetadatadev
ms.openlocfilehash: 0d31d982e7788970cbf7aad7dd64db9e6d4b9b10
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86502196"
---
# <a name="azure-instance-metadata-service-imds"></a>Azure Instance Metadata Service (IMDS)

Der Azure Instance Metadata Service (IMDS) stellt Informationen zu derzeit ausgeführten Instanzen virtueller Computer bereit, die zum Verwalten und Konfigurieren Ihrer virtuellen Computer verwendet werden können.
Hierzu gehören die SKU, der Speicher, Netzwerkkonfigurationen und bevorstehende Wartungsereignisse. Eine vollständige Liste der verfügbaren Daten finden Sie unter [Metadaten-APIs](#metadata-apis).
Der Instance Metadata Service ist sowohl für VMs als auch für Instanzen von VM-Skalierungsgruppen verfügbar. Er steht nur für laufende VMs zur Verfügung, die mit dem [Azure Resource Manager](/rest/api/resources/) erstellt/verwaltet werden.

Azure IMDS ist ein REST-Endpunkt, der unter einer bekannten, nicht routingfähigen IP-Adresse (`169.254.169.254`) zur Verfügung steht, auf die nur innerhalb der VM zugegriffen werden kann. Die Kommunikation zwischen dem virtuellen Computer und IMDS verlässt niemals den Host.
Es wird empfohlen, dass die HTTP-Clients bei Abfragen von IMDS Webproxys innerhalb des virtuellen Computers umgehen und `169.254.169.254` gleich behandeln wie [`168.63.129.16`](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="security"></a>Sicherheit

Auf den Instanzmetadatendienst-Endpunkt kann nur innerhalb der ausgeführten Instanz des virtuellen Computers an einer nicht routingfähigen IP-Adresse zugegriffen werden. Darüber hinaus wird jede Anforderung mit einem `X-Forwarded-For`-Header vom Dienst abgelehnt.
Zudem müssen Anforderungen einen `Metadata: true`-Header enthalten, um sicherzustellen, dass die eigentliche Anforderung direkt vorgesehen war und nicht Teil einer unbeabsichtigten Umleitung ist.

> [!IMPORTANT]
> Der Instance Metadata Service ist kein Kanal für vertrauliche Daten. Der Endpunkt ist für alle Prozesse auf der VM offen. Informationen, die über diesen Dienst verfügbar gemacht werden, sollten als freigegebene Informationen für alle Anwendungen betrachtet werden, die in der VM ausgeführt werden.

## <a name="usage"></a>Verwendung

### <a name="accessing-azure-instance-metadata-service"></a>Zugriff auf den Azure Instance Metadata Service

Erstellen Sie für den Zugriff auf den Instance Metadata Service eine VM über den [Azure Resource Manager](/rest/api/resources/) oder das [Azure-Portal](https://portal.azure.com), und arbeiten Sie die Beispiele unten durch.
Weitere Beispiele zum Abfragen von IMDS finden Sie unter [Beispiele für Azure-Instanzmetadaten](https://github.com/microsoft/azureimds).

Unten finden Sie den Beispielcode zum Abrufen aller Metadaten für eine Instanz; Informationen zum Zugriff auf eine bestimmte Datenquelle finden Sie im Abschnitt [Metadaten-API](#metadata-apis). 

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2019-06-01"
```

**Antwort**

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```json
{
  "compute": {
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
        "dataDisks": [
          {
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
              "uri": ""
            },
            "lun": "0",
            "managedDisk": {
              "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
              "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
              "uri": ""
            },
            "writeAcceleratorEnabled": "false"
          }
        ],
        "imageReference": {
          "id": "",
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04.0-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": "30",
          "diffDiskSettings": {
            "option": "Local"
          },
          "encryptionSettings": {
            "enabled": "false"
          },
          "image": {
            "uri": ""
          },
          "managedDisk": {
            "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
            "storageAccountType": "Standard_LRS"
          },
          "name": "exampleosdiskname",
          "osType": "Linux",
          "vhd": {
            "uri": ""
          },
          "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Prod;Role:WorkerRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

### <a name="data-output"></a>Datenausgabe

Standardmäßig gibt der Instanzmetadatendienst Daten im JSON-Format (`Content-Type: application/json`) zurück. Falls angefordert, können einige APIs jedoch Daten in verschiedenen Formaten zurückgeben.
In der folgenden Tabelle werden andere Datenformate aufgeführt, die die APIs eventuell unterstützen.

API | Standarddatenformat | Andere Formate
--------|---------------------|--------------
/attested | json | none
/identity | json | none
/instance | json | text
/scheduledevents | json | none

Um auf ein nicht standardmäßiges Antwortformat zuzugreifen, geben Sie das angeforderte Format als QueryString-Parameter in der Anforderung an. Beispiel:

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2017-08-01&format=text"
```

> [!NOTE]
> Für Blattknoten in /metadata/instance funktioniert `format=json` nicht. Für diese Abfragen muss `format=text` explizit angegeben werden, da das Standardformat JSON ist.

### <a name="versioning"></a>Versionsverwaltung

Der Instance Metadata Service verwendet Versionsangaben, und die Angabe der API-Version in der HTTP-Anforderung ist zwingend erforderlich.

Folgende Dienstversionen werden unterstützt: 2017-04-02, 2017-08-01, 2017-12-01, 2018-02-01, 2018-04-02, 2018-10-01, 2019-02-01, 2019-03-11, 2019-04-30, 2019-06-01, 2019-06-04, 2019-08-01, 2019-08-15.

Beachten Sie, dass das Rollout einer neuen Version nach der Veröffentlichung eine Weile dauert. Aktuell wird noch die Version 2019-11-01 bereitgestellt, die möglicherweise noch nicht in allen Regionen verfügbar ist.

Wenn neuere Versionen hinzugefügt werden, kann auf ältere Versionen aus Kompatibilitätsgründen weiterhin zugegriffen werden, falls Ihre Skripts von bestimmten Datenformaten abhängig sind.

Wenn keine Version angegeben ist, wird ein Fehler mit einer Liste der neuesten unterstützten Versionen zurückgegeben.

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Das folgende Beispiel zeigt die Fehlerbedingung an, wenn keine Version angegeben wird, wird die Antwort zur besseren Lesbarkeit im Schöndruck ausgegeben.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance"
```

**Antwort**

```json
{
    "error": "Bad request. api-version was not specified in the request. For more information refer to aka.ms/azureimds",
    "newest-versions": [
        "2018-10-01",
        "2018-04-02",
        "2018-02-01"
    ]
}
```

## <a name="metadata-apis"></a>Metadaten-APIs

Metadata Service enthält mehrere APIs, die verschiedene Datenquellen darstellen.

API | BESCHREIBUNG | Eingeführt in Version
----|-------------|-----------------------
/attested | Siehe [Bestätigte Daten](#attested-data) | 2018-10-01
/identity | Siehe [Abrufen eines Zugriffstokens](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md) | 2018-02-01
/instance | Siehe [Instanz-API](#instance-api) | 2017-04-02
/scheduledevents | Siehe [Azure-Metadatendienst: Geplante Ereignisse (Vorschau) für Windows-VMs](scheduled-events.md) | 2017-08-01

## <a name="instance-api"></a>Instanz-API

Die Instanz-API macht die wichtigen Metadaten für die VM-Instanzen verfügbar, einschließlich VM, Netzwerk und Speicher. Auf die folgenden Kategorien kann durch Instanz/Compute zugegriffen werden:

Daten | BESCHREIBUNG | Eingeführt in Version
-----|-------------|-----------------------
azEnvironment | Azure-Umgebung, in der die VM ausgeführt wird | 2018-10-01
customData | Diese Funktion ist derzeit deaktiviert. Diese Dokumentation wird aktualisiert, wenn die Funktion verfügbar wird. | 2019-02-01
location | Azure-Region, in der die VM ausgeführt wird | 2017-04-02
name | Name des virtuellen Computers | 2017-04-02
offer | Angebotsinformationen für das VM-Image, diese sind nur für Images vorhanden, die über den Azure-Imagekatalog bereitgestellt werden | 2017-04-02
osType | Linux oder Windows | 2017-04-02
placementGroupId | [Platzierungsgruppe](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) der VM-Skalierungsgruppe | 2017-08-01
Tarif | Der [Plan](/rest/api/compute/virtualmachines/createorupdate#plan) mit Name, Produkt und Herausgeber für einen virtuellen Computer, wenn es sich um ein Azure Marketplace-Image handelt | 2018-04-02
platformUpdateDomain |  [Updatedomäne](manage-availability.md), in der die VM ausgeführt wird | 2017-04-02
platformFaultDomain | [Fehlerdomäne](manage-availability.md), in der die VM ausgeführt wird | 2017-04-02
Provider | Anbieter des virtuellen Computers | 2018-10-01
publicKeys | [Sammlung von öffentlichen Schlüsseln](/rest/api/compute/virtualmachines/createorupdate#sshpublickey), dem virtuellen Computer und den entsprechenden Pfaden zugewiesen | 2018-04-02
publisher | Herausgeber des VM-Images | 2017-04-02
resourceGroupName | [Ressourcengruppe](../../azure-resource-manager/management/overview.md) für den virtuellen Computer | 2017-08-01
resourceId | Die [vollqualifizierte](/rest/api/resources/resources/getbyid) ID der Ressource | 11.03.2019
sku | Spezifische SKU für das VM-Image | 2017-04-02
storageProfile | Siehe [Speicherprofil](#storage-metadata). | 2019-06-01
subscriptionId | Azure-Abonnement für den virtuellen Computer | 2017-08-01
tags | [Tags](../../azure-resource-manager/management/tag-resources.md) für den virtuellen Computer  | 2017-08-01
tagsList | Tags, die zur einfacheren programmgesteuerten Analyse als JSON-Arrays formatiert sind  | 2019-06-04
version | Version des VM-Image | 2017-04-02
vmId | [Eindeutiger Bezeichner](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) für die VM | 2017-04-02
vmScaleSetName | [Name Ihrer VM-Skalierungsgruppe](../../virtual-machine-scale-sets/overview.md) | 2017-12-01
vmSize | [Größe des virtuellen Computers](sizes.md) | 2017-04-02
Zone | [Verfügbarkeitszone](../../availability-zones/az-overview.md) Ihres virtuellen Computers | 2017-12-01

### <a name="sample-1-tracking-vm-running-on-azure"></a>Beispiel 1: Nachverfolgen einer in Azure ausgeführten VM

Als Dienstanbieter müssen Sie möglicherweise die Anzahl von VMs nachverfolgen, auf denen Ihre Software ausgeführt wird, oder benötigen Agents, die die Eindeutigkeit der VM überwachen müssen. Um eine eindeutige ID für eine VM abrufen zu können, verwenden Sie das Feld `vmId` des Instanzmetadatendiensts.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-08-01&format=text"
```

**Antwort**

```text
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="sample-2-placement-of-containers-data-partitions-based-faultupdate-domain"></a>Beispiel 2: Platzierung von Containern und Datenpartitionen basierend auf der Fehler-/Updatedomäne

In bestimmten Szenarien ist die Platzierung unterschiedlicher Datenreplikate von primärer Bedeutung. Beispielsweise müssen Sie bei der [HDFS-Replikatplatzierung](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) oder bei der Containerplatzierung über einen [Orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) die `platformFaultDomain` und die `platformUpdateDomain` kennen, auf denen die VM ausgeführt wird.
Sie können auch [Verfügbarkeitszonen](../../availability-zones/az-overview.md) für die Instanzen nutzen, um diese Entscheidungen zu treffen.
Sie können diese Daten direkt über den Instanzmetadatendienst abfragen.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-08-01&format=text"
```

**Antwort**

```text
0
```

### <a name="sample-3-getting-more-information-about-the-vm-during-support-case"></a>Beispiel 3: Abrufen von weiteren Informationen zur VM während einer Supportanfrage

Als Dienstanbieter erhalten Sie möglicherweise eine Supportanfrage, für die Sie weitere Informationen zur VM erfahren möchten. Wenn Sie den Kunden auffordern, die Computemetadaten weiterzuleiten, können Sie dem Supportmitarbeiter grundlegende Informationen über die Art von VM in Azure bereitstellen.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute?api-version=2019-06-01"
```

**Antwort**

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```json
{
    "azEnvironment": "AzurePublicCloud",
    "customData": "",
    "location": "centralus",
    "name": "negasonic",
    "offer": "lampstack",
    "osType": "Linux",
    "placementGroupId": "",
    "plan": {
        "name": "5-6",
        "product": "lampstack",
        "publisher": "bitnami"
    },
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "provider": "Microsoft.Compute",
    "publicKeys": [],
    "publisher": "bitnami",
    "resourceGroupName": "myrg",
    "resourceId": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/myrg/providers/Microsoft.Compute/virtualMachines/negasonic",
    "sku": "5-6",
    "storageProfile": {
        "dataDisks": [
          {
            "caching": "None",
            "createOption": "Empty",
            "diskSizeGB": "1024",
            "image": {
              "uri": ""
            },
            "lun": "0",
            "managedDisk": {
              "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
              "storageAccountType": "Standard_LRS"
            },
            "name": "exampledatadiskname",
            "vhd": {
              "uri": ""
            },
            "writeAcceleratorEnabled": "false"
          }
        ],
        "imageReference": {
          "id": "",
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04.0-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "diskSizeGB": "30",
          "diffDiskSettings": {
            "option": "Local"
          },
          "encryptionSettings": {
            "enabled": "false"
          },
          "image": {
            "uri": ""
          },
          "managedDisk": {
            "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
            "storageAccountType": "Standard_LRS"
          },
          "name": "exampleosdiskname",
          "osType": "Linux",
          "vhd": {
            "uri": ""
          },
          "writeAcceleratorEnabled": "false"
        }
    },
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "Department:IT;Environment:Test;Role:WebRole",
    "version": "7.1.1902271506",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmScaleSetName": "",
    "vmSize": "Standard_A1_v2",
    "zone": "1"
}
```

### <a name="sample-4-getting-azure-environment-where-the-vm-is-running"></a>Beispiel 4: Abrufen der Azure-Umgebung, in der die VM ausgeführt wird

Azure verfügt über verschiedene Sovereign Clouds wie [Azure Government](https://azure.microsoft.com/overview/clouds/government/). In manchen Fällen muss die Azure-Umgebung gewisse Laufzeitentscheidungen treffen. Im folgenden Beispiel wird gezeigt, wie Sie dieses Verhalten erzielen können.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/azEnvironment?api-version=2018-10-01&format=text"
```

**Antwort**

```text
AzurePublicCloud
```

Die Cloud und die Werte der Azure-Umgebung sind unten aufgelistet.

 Cloud   | Azure-Umgebung
---------|-----------------
[Allgemein verfügbar in globalen Azure-Regionen](https://azure.microsoft.com/regions/)     | AzurePublicCloud
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | AzureUSGovernmentCloud
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)         | AzureChinaCloud
[Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/)                    | AzureGermanCloud

## <a name="network-metadata"></a>Netzwerkmetadaten 

Netzwerkmetadaten sind Teil der Instanz-API. Die folgenden Netzwerkkategorien stehen über den Instanz-/Netzwerkendpunkt zur Verfügung.

Daten | BESCHREIBUNG | Eingeführt in Version
-----|-------------|-----------------------
ipv4/privateIpAddress | Lokale IPv4-Adresse der VM | 2017-04-02
ipv4/publicIpAddress | Öffentliche IPv4-Adresse der VM | 2017-04-02
subnet/address | Subnetzadresse der VM | 2017-04-02
subnet/prefix | Subnetzpräfix, Beispiel 24 | 2017-04-02
ipv6/ipAddress | Lokale IPv6-Adresse der VM | 2017-04-02
macAddress | VM-Mac-Adresse | 2017-04-02

> [!NOTE]
> Alle API-Antworten stellen JSON-Zeichenfolgen dar. Alle folgenden Beispielantworten werden zur besseren Lesbarkeit im Schöndruck gedruckt.

#### <a name="sample-1-retrieving-network-information"></a>Beispiel 1: Abrufen von Netzwerkinformationen

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Antwort**

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```json
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="sample-2-retrieving-public-ip-address"></a>Beispiel 2: Abrufen der öffentlichen IP-Adresse

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-08-01&format=text"
```

## <a name="storage-metadata"></a>Speichermetadaten

Speichermetadaten sind Teil der Instanz-API unter dem Endpunkt „instance/compute/storageProfile“.
Er kann Details zu den Speicherdatenträgern bereitstellen, die der VM zugeordnet sind. 

Das Speicherprofil eines virtuellen Computers ist in drei Kategorien unterteilt: Imagereferenz, Betriebssystemdatenträger und Datenträger für Daten.

Das Imagereferenzobjekt enthält die folgenden Informationen zum Betriebssystemimage:

Daten    | BESCHREIBUNG
--------|-----------------
id      | Ressourcen-ID
offer   | Angebot des Plattform- oder Marketplace-Images
publisher | Imageherausgeber
sku     | Image-SKU
version | Version des Plattform- oder Marketplace-Images

Das Betriebssystemdatenträgerobjekt enthält die folgenden Informationen zum Betriebssystemdatenträger, der vom virtuellen Computer verwendet wird:

Daten    | BESCHREIBUNG
--------|-----------------
caching | Cachinganforderungen
createOption | Informationen zur Erstellung des virtuellen Computers
diffDiskSettings | Einstellungen für kurzlebige Datenträger
diskSizeGB | Größe des Datenträgers in GB
image   | Virtuelle Festplatte des Quellenbenutzerimages
lun     | Logische Gerätenummer des Datenträgers
managedDisk | Parameter für verwaltete Datenträger
name    | Name des Datenträgers
vhd     | Virtuelle Festplatte
writeAcceleratorEnabled | Gibt an, ob writeAccelerator für den Datenträger aktiviert ist.

Das Datenträgerarray für Daten enthält eine Liste der Datenträger für Daten, die an den virtuellen Computer angefügt sind. Jedes Datenträgerobjekt enthält die folgenden Informationen:

Daten    | BESCHREIBUNG
--------|-----------------
caching | Cachinganforderungen
createOption | Informationen zur Erstellung des virtuellen Computers
diffDiskSettings | Einstellungen für kurzlebige Datenträger
diskSizeGB | Größe des Datenträgers in GB
encryptionSettings | Verschlüsselungseinstellungen für den Datenträger
image   | Virtuelle Festplatte des Quellenbenutzerimages
managedDisk | Parameter für verwaltete Datenträger
name    | Name des Datenträgers
osType  | Typ des Betriebssystems, das auf dem Datenträger enthalten ist
vhd     | Virtuelle Festplatte
writeAcceleratorEnabled | Gibt an, ob writeAccelerator für den Datenträger aktiviert ist.

Im folgenden Beispiel wird die Abfrage der Speicherinformationen des virtuellen Computers gezeigt.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/storageProfile?api-version=2019-06-01"
```

**Antwort**

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```json
{
    "dataDisks": [
      {
        "caching": "None",
        "createOption": "Empty",
        "diskSizeGB": "1024",
        "image": {
          "uri": ""
        },
        "lun": "0",
        "managedDisk": {
          "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampledatadiskname",
          "storageAccountType": "Standard_LRS"
        },
        "name": "exampledatadiskname",
        "vhd": {
          "uri": ""
        },
        "writeAcceleratorEnabled": "false"
      }
    ],
    "imageReference": {
      "id": "",
      "offer": "UbuntuServer",
      "publisher": "Canonical",
      "sku": "16.04.0-LTS",
      "version": "latest"
    },
    "osDisk": {
      "caching": "ReadWrite",
      "createOption": "FromImage",
      "diskSizeGB": "30",
      "diffDiskSettings": {
        "option": "Local"
      },
      "encryptionSettings": {
        "enabled": "false"
      },
      "image": {
        "uri": ""
      },
      "managedDisk": {
        "id": "/subscriptions/xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/macikgo-test-may-23/providers/Microsoft.Compute/disks/exampleosdiskname",
        "storageAccountType": "Standard_LRS"
      },
      "name": "exampleosdiskname",
      "osType": "Linux",
      "vhd": {
        "uri": ""
      },
      "writeAcceleratorEnabled": "false"
    }
}
```

## <a name="vm-tags"></a>VM-Tags

VM-Tags sind in der Instanz-API unter dem Instanz/Compute/Tags-Endpunkt enthalten.
Möglicherweise wurden Tags auf Ihre Azure-VM angewendet, um sie logisch in einer Taxonomie zu strukturieren. Die einer VM zugewiesenen Tags können mithilfe der Abfrage unten abgerufen werden.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/tags?api-version=2018-10-01&format=text"
```

**Antwort**

```text
Department:IT;Environment:Test;Role:WebRole
```

Das `tags` Feld ist eine Zeichenfolge, in der die Tags durch Semikolons getrennt sind. Diese Ausgabe kann ein Problem darstellen, wenn in den Tags Semikolons verwendet werden. Wenn ein Parser geschrieben wurde, um die Tags programmgesteuert zu extrahieren, sollten Sie auf das Feld `tagsList` zurückgreifen. Das Feld `tagsList` ist ein JSON-Array ohne Trennzeichen und folglich einfacher zu analysieren.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance/compute/tagsList?api-version=2019-06-04"
```

**Antwort**

```json
[
  {
    "name": "Department",
    "value": "IT"
  },
  {
    "name": "Environment",
    "value": "Test"
  },
  {
    "name": "Role",
    "value": "WebRole"
  }
]
```

## <a name="attested-data"></a>Bestätigte Daten

Das von Instance Metadata Service bereitgestellte Szenario beinhaltet die Garantie, dass die bereitgestellten Daten von Azure stammen. Wir signieren einen Teil dieser Informationen, sodass sich Marketplace-Images darauf verlassen können, dass ihr Image in Azure ausgeführt wird.

### <a name="sample-1-getting-attested-data"></a>Beispiel 1: Abrufen bestätigter Daten

> [!NOTE]
> Alle API-Antworten stellen JSON-Zeichenfolgen dar. Die folgenden Beispielantworten werden zur besseren Lesbarkeit automatische strukturiert und eingerückt.

**Anforderung**

```bash
curl -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/attested/document?api-version=2018-10-01&nonce=1234567890"
```

Die API-Version ist ein obligatorisches Feld. Informationen zu unterstützten API-Versionen finden Sie im [Abschnitt zur Verwendung](#usage).
Nonce ist eine optionale 10-stellige Zeichenfolge. Wenn nicht angegeben, gibt IMDS stattdessen den aktuellen UTC-Zeitstempel zurück.

> [!NOTE]
> Wegen des Cachemechanismus von IMDS kann ein zuvor zwischengespeicherter Nonce-Wert zurückgegeben werden.

**Antwort**

> [!NOTE]
> Die Antwort stellt eine JSON-Zeichenfolge dar. Die folgende Beispielantwort wird zur besseren Lesbarkeit im Schöndruck gedruckt.

```json
{
 "encoding":"pkcs7","signature":"MIIEEgYJKoZIhvcNAQcCoIIEAzCCA/8CAQExDzANBgkqhkiG9w0BAQsFADCBugYJKoZIhvcNAQcBoIGsBIGpeyJub25jZSI6IjEyMzQ1NjY3NjYiLCJwbGFuIjp7Im5hbWUiOiIiLCJwcm9kdWN0IjoiIiwicHVibGlzaGVyIjoiIn0sInRpbWVTdGFtcCI6eyJjcmVhdGVkT24iOiIxMS8yMC8xOCAyMjowNzozOSAtMDAwMCIsImV4cGlyZXNPbiI6IjExLzIwLzE4IDIyOjA4OjI0IC0wMDAwIn0sInZtSWQiOiIifaCCAj8wggI7MIIBpKADAgECAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBBAUAMCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tMB4XDTE4MTEyMDIxNTc1N1oXDTE4MTIyMDIxNTc1NlowKzEpMCcGA1UEAxMgdGVzdHN1YmRvbWFpbi5tZXRhZGF0YS5henVyZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAML/tBo86ENWPzmXZ0kPkX5dY5QZ150mA8lommszE71x2sCLonzv4/UWk4H+jMMWRRwIea2CuQ5RhdWAHvKq6if4okKNt66fxm+YTVz9z0CTfCLmLT+nsdfOAsG1xZppEapC0Cd9vD6NCKyE8aYI1pliaeOnFjG0WvMY04uWz2MdAgMBAAGjYDBeMFwGA1UdAQRVMFOAENnYkHLa04Ut4Mpt7TkJFfyhLTArMSkwJwYDVQQDEyB0ZXN0c3ViZG9tYWluLm1ldGFkYXRhLmF6dXJlLmNvbYIQZ8VuSofHbJRAQNBNpiASdDANBgkqhkiG9w0BAQQFAAOBgQCLSM6aX5Bs1KHCJp4VQtxZPzXF71rVKCocHy3N9PTJQ9Fpnd+bYw2vSpQHg/AiG82WuDFpPReJvr7Pa938mZqW9HUOGjQKK2FYDTg6fXD8pkPdyghlX5boGWAMMrf7bFkup+lsT+n2tRw2wbNknO1tQ0wICtqy2VqzWwLi45RBwTGB6DCB5QIBATA/MCsxKTAnBgNVBAMTIHRlc3RzdWJkb21haW4ubWV0YWRhdGEuYXp1cmUuY29tAhBnxW5Kh8dslEBA0E2mIBJ0MA0GCSqGSIb3DQEBCwUAMA0GCSqGSIb3DQEBAQUABIGAld1BM/yYIqqv8SDE4kjQo3Ul/IKAVR8ETKcve5BAdGSNkTUooUGVniTXeuvDj5NkmazOaKZp9fEtByqqPOyw/nlXaZgOO44HDGiPUJ90xVYmfeK6p9RpJBu6kiKhnnYTelUk5u75phe5ZbMZfBhuPhXmYAdjc7Nmw97nx8NnprQ="
}
```

Das Signaturblob ist eine signierte [pkcs7](https://aka.ms/pkcs7)-Version des Dokuments. Es enthält das zum Signieren verwendete Zertifikat zusammen mit den VM-Details wie „vmId“, „sku“, „nonce“, „subscriptionId“, „timeStamp“ für die Erstellung und den Ablauf des Dokuments sowie die Planinformationen zum Image. Die Planinformationen werden nur für Azure Marketplace-Images ausgefüllt. Das Zertifikat kann aus der Antwort extrahiert und verwendet werden, um sicherzustellen, dass die Antwort gültig ist und von Azure stammt.
Das Dokument enthält die folgenden Felder:

Daten | BESCHREIBUNG
-----|------------
nonce | Eine Zeichenfolge, die optional mit der Anforderung angegeben werden kann. Wenn keine Nonce angegeben wurde, wird der aktuelle UTC-Zeitstempel verwendet.
Tarif | Der [Plan für das Azure Marketplace-Image](/rest/api/compute/virtualmachines/createorupdate#plan). Enthält die Plan-ID („name“), das Produktimage oder -angebot („product“) und die Herausgeber-ID („publisher“).
timestamp/createdOn | Der UTC-Zeitstempel für den Zeitpunkt, an dem das signierte Dokument erstellt wurde
timestamp/expiresOn | Der UTC-Zeitstempel für den Zeitpunkt, an dem das signierte Dokument abläuft
vmId |  [Eindeutiger Bezeichner](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) für die VM
subscriptionId | Azure-Abonnement für den virtuellen Computer, eingeführt am `2019-04-30`
sku | Spezifische SKU für das VM-Image, eingeführt in `2019-11-01`

### <a name="sample-2-validating-that-the-vm-is-running-in-azure"></a>Beispiel 2: Überprüfen der Ausführung des virtuellen Computers in Azure

Marketplace-Anbieter möchten sicherstellen, dass ihre Software nur für die Ausführung in Azure lizenziert ist. Wenn die virtuelle Festplatte in eine lokale Umgebung kopiert wird, müssen sie über die Möglichkeit verfügen, dies zu erkennen. Durch Aufrufen von Instance Metadata Service können Marketplace-Anbieter signierte Daten abrufen, die garantieren, dass Antworten ausschließlich von Azure kommen.

> [!NOTE]
> Die Installation von jq ist erforderlich.

**Anforderung**

```bash
# Get the signature
curl --silent -H Metadata:True --noproxy "*" "http://169.254.169.254/metadata/attested/document?api-version=2019-04-30" | jq -r '.["signature"]' > signature
# Decode the signature
base64 -d signature > decodedsignature
# Get PKCS7 format
openssl pkcs7 -in decodedsignature -inform DER -out sign.pk7
# Get Public key out of pkc7
openssl pkcs7 -in decodedsignature -inform DER  -print_certs -out signer.pem
# Get the intermediate certificate
curl -s -o intermediate.cer "$(openssl x509 -in signer.pem -text -noout | grep " CA Issuers -" | awk -FURI: '{print $2}')"
openssl x509 -inform der -in intermediate.cer -out intermediate.pem
# Verify the contents
openssl smime -verify -in sign.pk7 -inform pem -noverify
```

**Antwort**

```json
Verification successful
{
  "nonce": "20181128-001617",
  "plan":
    {
      "name": "",
      "product": "",
      "publisher": ""
    },
  "timeStamp":
    {
      "createdOn": "11/28/18 00:16:17 -0000",
      "expiresOn": "11/28/18 06:16:17 -0000"
    },
  "vmId": "d3e0e374-fda6-4649-bbc9-7f20dc379f34",
  "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
  "sku": "RS3-Pro"
}
```

Vergewissern Sie sich, dass die Signatur von Microsoft Azure stammt, und überprüfen Sie die Zertifikatkette auf Fehler.

```bash
# Verify the subject name for the main certificate
openssl x509 -noout -subject -in signer.pem
# Verify the issuer for the main certificate
openssl x509 -noout -issuer -in signer.pem
#Validate the subject name for intermediate certificate
openssl x509 -noout -subject -in intermediate.pem
# Verify the issuer for the intermediate certificate
openssl x509 -noout -issuer -in intermediate.pem
# Verify the certificate chain, for Azure China 21Vianet the intermediate certificate will be from DigiCert Global Root CA
openssl verify -verbose -CAfile /etc/ssl/certs/Baltimore_CyberTrust_Root.pem -untrusted intermediate.pem signer.pem
```

> [!NOTE]
> Wegen des Cachemechanismus von IMDS kann ein zuvor zwischengespeicherter Nonce-Wert zurückgegeben werden.

Die Nonce im signierten Dokument kann verglichen werden, wenn Sie in der ursprünglichen Anforderung einen nonce-Parameter angegeben haben.

> [!NOTE]
> Die Zertifikate für öffentliche Clouds und Sovereign Clouds unterscheiden sich voneinander.

Cloud | Zertifikat
------|------------
[Allgemein verfügbar in globalen Azure-Regionen](https://azure.microsoft.com/regions/) | *.metadata.azure.com
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)          | *.metadata.azure.us
[Azure China 21Vianet](https://azure.microsoft.com/global-infrastructure/china/)     | *.metadata.azure.cn
[Azure Deutschland](https://azure.microsoft.com/overview/clouds/germany/)                | *.metadata.microsoftazure.de

> [!NOTE]
> Es gibt ein bekanntes Problem mit dem zum Signieren verwendeten Zertifikat. Die Zertifikate stimmen mit `metadata.azure.com` für die öffentliche Cloud möglicherweise nicht exakt überein. Daher sollte die Überprüfung der Zertifizierung einen allgemeinen Namen aus einer beliebigen Unterdomäne von `.metadata.azure.com` zulassen.

Falls das Zwischenzertifikat bei der Überprüfung aufgrund von Netzwerkeinschränkungen nicht heruntergeladen werden kann, können Sie das Zwischenzertifikat anheften. Azure übergibt die Zertifikate jedoch beim Rollover gemäß der Standard-PKI-Methode. Die angehefteten Zertifikate müssen bei einem Rollover aktualisiert werden. Wenn eine Änderung geplant ist, bei der das Zwischenzertifikat aktualisiert werden muss, wird der Azure-Blog auf den neuesten Stand gebracht, und Azure-Kunden werden benachrichtigt. Die Zwischenzertifikate sind [hier](https://www.microsoft.com/pki/mscorp/cps/default.htm) zu finden. Die Zwischenzertifikate können sich für die einzelnen Regionen unterscheiden.

> [!NOTE]
> Das Zwischenzertifikat für Azure China 21ViaNet wird von DigiCert Global Root CA anstelle von Baltimore verwendet.
Wenn Sie im Rahmen der Änderung der Stammzertifizierungsstelle Zwischenzertifikate für Azure China angeheftet haben, müssen zudem die Zwischenzertifikate aktualisiert werden.

## <a name="managed-identity-via-metadata-service"></a>Verwaltete Identität über Metadata Service

Auf dem virtuellen Computer kann eine systemseitig zugewiesene verwaltete Identität aktiviert werden, oder dem virtuellen Computer können benutzerseitig zugewiesene verwaltete Identitäten zugewiesen werden.
Token für verwaltete Identitäten können dann von Instance Metadata Service angefordert werden. Diese Token können für die Authentifizierung bei anderen Azure-Diensten wie z. B. Azure Key Vault verwendet werden.

Ausführliche Schritte zum Aktivieren dieses Features finden Sie unter [Abrufen eines Zugriffstokens](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md).

## <a name="scheduled-events-via-metadata-service"></a>Geplante Ereignisse über Metadata Service
Sie können den Status der geplanten Ereignisse über den Metadata Service abrufen, dann können Benutzer eine Reihe von Aktionen angeben, die beim Eintreten dieser Ereignisse ausgeführt werden.  Details finden Sie unter [Geplante Ereignisse](scheduled-events.md). 

## <a name="regional-availability"></a>Regionale Verfügbarkeit

Der Dienst ist in allen Azure-Clouds **allgemein verfügbar**.

## <a name="sample-code-in-different-languages"></a>Beispielcode in verschiedenen Sprachen

Beispiele zum Aufrufen des Metadatendiensts über verschiedene Sprachen auf der VM:

Sprache      | Beispiel
--------------|----------------
Bash          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.sh
C#            | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.cs
Go            | https://github.com/Microsoft/azureimds/blob/master/imdssample.go
Java          | https://github.com/Microsoft/azureimds/blob/master/imdssample.java
NodeJS        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.js
Perl          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.pl
PowerShell    | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.ps1
Puppet        | https://github.com/keirans/azuremetadata
Python        | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.py
Ruby          | https://github.com/Microsoft/azureimds/blob/master/IMDSSample.rb

## <a name="error-and-debugging"></a>Fehler und Debuggen

Wenn ein Datenelement nicht gefunden wird oder eine Anforderung ungültig ist, gibt der Instanzmetadatendienst standardmäßige HTTP-Fehler zurück. Beispiel:

HTTP-Statuscode | `Reason`
----------------|-------
200 – OK |
400 – Ungültige Anforderung | Fehlender `Metadata: true`-Header oder fehlender `format=json`-Parameter beim Abfragen eines Blattknotens
404 – Nicht gefunden | Das angeforderte Element ist nicht vorhanden.
405 – Methode unzulässig | Es werden ausschließlich `GET`-Anforderungen unterstützt.
410 Nicht mehr vorhanden | Wiederholen Sie den Vorgang später (nach max. 70 Sekunden).
429 – Zu viele Anforderungen | Die API unterstützt derzeit maximal 5 Abfragen pro Sekunde.
500 – Dienstfehler     | Wiederholen Sie den Vorgang später.

### <a name="known-issues-and-faq"></a>Bekannte Probleme und FAQ

1. Ich erhalte die Fehlermeldung `400 Bad Request, Required metadata header not specified`. Was bedeutet dies?
   * Für den Instanzmetadatendienst muss der Header `Metadata: true` in der Anforderung übergeben werden. Durch Übergeben dieses Headers im REST-Aufruf wird der Zugriff auf den Instanzmetadatendienst ermöglicht.
1. Warum erhalte ich keine Compute-Informationen für meine VM?
   * Der Instanzmetadatendienst unterstützt derzeit nur Instanzen, die mit dem Azure Resource Manager erstellt wurden. In Zukunft kann zusätzliche Unterstützung für Clouddienst-VMs hinzugefügt werden.
1. Ich habe meinen virtuellen Computer vor einiger Zeit über den Azure Resource Manager erstellt. Warum sehe ich keine Computemetadateninformationen?
   * Fügen Sie für VMs, die nach September 2016 erstellt wurden, ein [Tag](../../azure-resource-manager/management/tag-resources.md) hinzu, damit Computemetadaten angezeigt werden. Fügen Sie bei älteren VMs (die vor September 2016 erstellt wurden) Erweiterungen oder Datenträger zu den VM-Instanzen hinzu, oder entfernen Sie sie, um Metadaten zu aktualisieren.
1. Nicht alle Daten wurden für die neue Version eingetragen.
   * Fügen Sie für VMs, die nach September 2016 erstellt wurden, ein [Tag](../../azure-resource-manager/management/tag-resources.md) hinzu, damit Computemetadaten angezeigt werden. Fügen Sie bei älteren VMs (die vor September 2016 erstellt wurden) Erweiterungen oder Datenträger zu den VM-Instanzen hinzu, oder entfernen Sie sie, um Metadaten zu aktualisieren.
1. Warum erhalte ich die Fehler `500 Internal Server Error` oder `410 Resource Gone`?
   * Wiederholen Sie die Anforderung basierend auf dem System des exponentiellen Backoffs oder anderen Methoden, die unter [Behandeln vorübergehender Fehler](/azure/architecture/best-practices/transient-faults) beschrieben werden. Wenn das Problem weiterhin besteht, erstellen Sie im Azure-Portal ein Supportproblem für den virtuellen Computer.
1. Gilt dies auch für VM-Skalierungsgruppeninstanzen?
   * Ja, Metadata Service ist für Skalierungsgruppeninstanzen verfügbar.
1. Ich habe die Tags in VM-Skalierungsgruppen aktualisiert, aber sie werden im Gegensatz zu Einzelinstanz-VMs nicht in den Instanzen angezeigt.
   * Derzeit werden Tags für Skalierungsgruppen in VMs nur nach einem Neustart, einer Neuerstellung des Images oder der Änderung eines Datenträgers für die Instanz angezeigt.
1. Ich erhalte ein Anforderungstimeout für den Aufruf des Diensts.
   * Metadatenaufrufe müssen über die primäre IP-Adresse erfolgen, die der primären Netzwerkkarte des virtuellen Computers zugewiesen ist. Wenn Sie die Routen geändert haben, muss außerdem eine Route für die Adresse 169.254.169.254/32 in der lokalen Routingtabelle des virtuellen Computers vorhanden sein.
   * <details>
        <summary>Überprüfen der Routingtabelle</summary>

        1. Erstellen Sie eine Sicherung der lokalen Routingtabelle mit einem Befehl wie beispielsweise `netstat -r`, und suchen Sie den IMDS-Eintrag, z. B.:
            ```console
            ~$ netstat -r
            Kernel IP routing table
            Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
            default         _gateway        0.0.0.0         UG        0 0          0 eth0
            168.63.129.16   _gateway        255.255.255.255 UGH       0 0          0 eth0
            169.254.169.254 _gateway        255.255.255.255 UGH       0 0          0 eth0
            172.16.69.0     0.0.0.0         255.255.255.0   U         0 0          0 eth0
            ```
        1. Überprüfen Sie, ob eine Route für `169.254.169.254` vorhanden ist, und notieren Sie sich die entsprechende Netzwerkschnittstelle (z. B. `eth0`).
        1. Speichern Sie die Schnittstellenkonfiguration für die entsprechende Schnittstelle in der Routingtabelle. (Beachten Sie, dass der genaue Name der Konfigurationsdatei variieren kann.)
            ```console
            ~$ cat /etc/netplan/50-cloud-init.yaml
            network:
            ethernets:
                eth0:
                    dhcp4: true
                    dhcp4-overrides:
                        route-metric: 100
                    dhcp6: false
                    match:
                        macaddress: 00:0d:3a:e4:c7:2e
                    set-name: eth0
            version: 2
            ```
        1. Notieren Sie sich bei Verwendung einer dynamischen IP-Adresse die MAC-Adresse (Media Access Control). Wenn Sie eine statische IP-Adresse verwenden, notieren Sie sich die aufgelisteten IPs und/oder die MAC-Adresse.
        1. Vergewissern Sie sich, dass die Schnittstelle der primären NIC und der primären IP-Adresse des virtuellen Computers entspricht. Sie finden die primäre NIC und primäre IP-Adresse in der Netzwerkkonfiguration im Azure-Portal oder durch Suchen [mithilfe der Azure-Befehlszeilenschnittstelle](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-show). Notieren Sie sich die öffentlichen und privaten IP-Adressen (und bei Verwendung der Azure-Befehlszeilenschnittstelle die MAC-Adresse). PowerShell-CLI-Beispiel:
            ```powershell
            $ResourceGroup = '<Resource_Group>'
            $VmName = '<VM_Name>'
            $NicNames = az vm nic list --resource-group $ResourceGroup --vm-name $VmName | ConvertFrom-Json | Foreach-Object { $_.id.Split('/')[-1] }
            foreach($NicName in $NicNames)
            {
                $Nic = az vm nic show --resource-group $ResourceGroup --vm-name $VmName --nic $NicName | ConvertFrom-Json
                Write-Host $NicName, $Nic.primary, $Nic.macAddress
            }
            # Output: ipexample606 True 00-0D-3A-E4-C7-2E
            ```
        1. Wenn sie nicht übereinstimmen, aktualisieren Sie die Routingtabelle so, dass die primäre NIC und die primäre IP-Adresse als Ziel festgelegt sind.
    </details>

## <a name="support-and-feedback"></a>Support und Feedback

Senden Sie Ihr Feedback und Ihre Kommentare unter https://feedback.azure.com.

Um Support für den Dienst zu beziehen, erstellen Sie im Azure-Portal ein Supportproblem für den virtuellen Computer, auf dem Sie nach wiederholten Versuchen keine Metadatenantwort erhalten.
Verwenden Sie den Problemtyp `Management`, und wählen Sie `Instance Metadata Service` als Kategorie aus.

![Instance Metadata-Support](./media/instance-metadata-service/InstanceMetadata-support.png "Screenshot: Öffnen einer Supportanfrage bei Problemen mit Instance Metadata Service")

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen:
1. [Abrufen eines -Zugriffstokens für die VM](../../active-directory/managed-identities-azure-resources/how-to-use-vm-token.md)
1. [Geplante Ereignisse](scheduled-events.md)
