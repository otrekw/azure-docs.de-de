---
title: Sicherungsmaßnahmen bei der Bereitstellung von Service Fabric Azure Resource Manager
description: Dieser Artikel gibt einen Überblick über häufige Fehler bei der Bereitstellung eines Service Fabric-Clusters über Azure Resource Manager und erläutert, wie Sie diese vermeiden können.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: pepogors
ms.openlocfilehash: fe5ff2a5eeb4b2c73165d1577702eb6af7079b61
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75426736"
---
# <a name="service-fabric-guardrails"></a>Service Fabric-Sicherungsmaßnahmen 
Wenn Sie einen Service Fabric-Cluster bereitstellen, werden Sicherungsmaßnahmen angewendet, durch die eine Azure Resource Manager-Bereitstellung bei einer ungültigen Clusterkonfiguration fehlschlägt. Die folgenden Abschnitte bieten einen Überblick über häufige Probleme bei der Clusterkonfiguration und die erforderlichen Schritte, um diese Probleme zu beheben. 

## <a name="durability-mismatch"></a>Dauerhaftigkeitswert stimmt nicht überein
### <a name="overview"></a>Übersicht
Der Dauerhaftigkeitswert für einen Service Fabric-Knotentyp wird in zwei verschiedenen Abschnitten einer Azure Resource Manager-Vorlage definiert. Diese Definition erfolgt im Abschnitt zur Erweiterung der VM-Skalierungsgruppe der VM-Skalierungsgruppenressource und im Abschnitt zum Knotentyp der Service Fabric-Clusterressource. Es ist erforderlich, dass der Dauerhaftigkeitswert in diesen Abschnitten übereinstimmt, da andernfalls die Ressourcenbereitstellung fehlschlägt.

Der folgende Abschnitt enthält ein Beispiel für einen Dauerhaftigkeitskonflikt zwischen der Dauerhaftigkeitseinstellung der VM-Skalierungsgruppe und der Einstellung für die Dauerhaftigkeit des Service Fabric Knotentyps:  

**Dauerhaftigkeitseinstellung der VM-Skalierungsgruppe**
```json 
{
  "extensions": [
    {
      "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
      "properties": {
        "type": "ServiceFabricNode",
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
          "durabilityLevel": "Silver",
        }
      }
    }
  ]
}
```

**Dauerhaftigkeitseinstellung des Service Fabric-Knotentyps** 
```json
{
  "nodeTypes": [
    {
      "name": "[variables('vmNodeType0Name')]",
      "durabilityLevel": "Bronze",
      "isPrimary": true,
      "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    }
  ]
}
```

### <a name="error-messages"></a>Fehlermeldungen
* Virtual Machine Scale Set durability mismatch does not match the current Service Fabric Node Type durability level (Der Dauerhaftigkeitswert der VM-Skalierungsgruppe stimmt nicht mit der aktuellen Dauerhaftigkeitsstufe für den Service Fabric-Knotentyp überein)
* Virtual Machine Scale Set durability does not match the target Service Fabric Node Type durability level (Die Dauerhaftigkeit der VM-Skalierungsgruppe stimmt nicht mit der Dauerhaftigkeitsstufe für das Service Fabric-Knotentypziel überein)
* Virtual Machine Scale Set durability does match the current Service Fabric durability level or the target Service Fabric Node Type durability level (Die Dauerhaftigkeit der VM-Skalierungsgruppe stimmt nicht mit der aktuellen Service Fabric-Dauerhaftigkeitsstufe oder der Dauerhaftigkeitsstufe des Service Fabric-Zielknotentyps überein) 


### <a name="mitigation"></a>Minderung
So beheben Sie einen Dauerhaftigkeitskonflikt, der durch eine der oben genannten Fehlermeldungen angegeben wird:
1. Aktualisieren Sie die Dauerhaftigkeitsstufe entweder im Abschnitt zur Erweiterung der VM-Skalierungsgruppe oder im Abschnitt zum Service Fabric-Knotentyp der Azure Resource Manager-Vorlage, um sicherzustellen, dass die Werte übereinstimmen.
2. Stellen Sie die Azure Resource Manager-Vorlage mit den aktualisierten Werten erneut bereit.

## <a name="next-steps"></a>Nächste Schritte
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Problembehandlung für Service Fabric: [Leitfäden zur Problembehandlung](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
