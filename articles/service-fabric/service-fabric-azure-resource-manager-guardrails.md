---
title: Sicherungsmaßnahmen bei der Bereitstellung von Service Fabric Azure Resource Manager
description: Dieser Artikel gibt einen Überblick über häufige Fehler bei der Bereitstellung eines Service Fabric-Clusters über Azure Resource Manager und erläutert, wie Sie diese vermeiden können.
services: service-fabric
documentationcenter: .net
author: peterpogorski
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: pepogors
ms.openlocfilehash: aa1499c57ead28bfcee90a2f224ef9c3bb1d7f58
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "86247823"
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


## <a name="seed-node-deletion"></a>Löschung von Startknoten 
### <a name="overview"></a>Übersicht
Ein Service Fabric-Cluster weist eine [Zuverlässigkeitsebene](./service-fabric-cluster-capacity.md#reliability-characteristics-of-the-cluster)-Eigenschaft auf, die verwendet wird, um die Anzahl der Systemdienste zu bestimmen, die auf dem primären Knotentyp des Clusters ausgeführt werden. Die Anzahl der erforderlichen Replikate bestimmt die Mindestanzahl der Knoten, die vom primären Knotentyp des Clusters betrieben werden müssen. Wenn die Anzahl der Knoten vom primären Knotentyp unter das erforderliche Minimum für die Zuverlässigkeitsebene fällt, wird der Cluster instabil.  

### <a name="error-messages"></a>Fehlermeldungen 
Ein Vorgang zum Entfernen des Startknotens wurde erkannt und wird abgelehnt. 
* Dieser Vorgang würde dazu führen, dass nur {0} potenzielle Startknoten im Cluster verbleiben, während mindestens {1} benötigt werden.
* Das Entfernen von {0} Startknoten von {1} würde aufgrund des Verlusts des Startknotenquorums zum Ausfall des Clusters führen. Die maximale Anzahl Startknoten, die in einem Durchgang entfernt werden können, ist {2}.
 
### <a name="mitigation"></a>Minderung 
Stellen Sie sicher, dass Ihr primärer Knotentyp über genügend virtuelle Computer für die für Ihren Cluster angegebene Zuverlässigkeit verfügt. Sie können keinen virtuellen Computer entfernen, wenn dies die VM-Skalierungsgruppe unter die Mindestanzahl Knoten für die bestimmte Zuverlässigkeitsebene bringen würde.
* Wenn die Zuverlässigkeitsebene ordnungsgemäß angegeben ist, stellen Sie sicher, dass genügend Knoten vom primären Knotentyp vorhanden sind, wie es für die Zuverlässigkeitsebene erforderlich ist. 
* Wenn die Zuverlässigkeitsebene falsch ist, leiten Sie eine Änderung der Service Fabric-Ressource ein, um zuerst die Zuverlässigkeitsebene herabzusetzen, bevor Sie Vorgänge für die VM-Skalierungsgruppe einleiten, und warten Sie, bis diese abgeschlossen ist.
* Wenn die Zuverlässigkeitsebene Bronze ist, führen Sie bitte diese [Schritte](./service-fabric-cluster-scale-in-out.md#manually-remove-vms-from-a-node-typevirtual-machine-scale-set) aus, um Ihren Cluster ordnungsgemäß abzuskalieren.

## <a name="next-steps"></a>Nächste Schritte
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Windows Server: [Erstellen eines Service Fabric-Clusters für Windows Server](service-fabric-cluster-creation-for-windows-server.md)
* Erstellen eines Clusters auf virtuellen Computern oder Computern mit Linux: [Erstellen eines Linux-Clusters](service-fabric-cluster-creation-via-portal.md)
* Problembehandlung für Service Fabric: [Leitfäden zur Problembehandlung](https://github.com/Azure/Service-Fabric-Troubleshooting-Guides)
