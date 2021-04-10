---
title: Ausschließliches Bereitstellen von zustandslosen Knotentypen in einem Service Fabric-Cluster
description: Erfahren Sie, wie Sie zustandslose Knotentypen in Azure Service Fabric-Clustern erstellen und bereitstellen.
author: peterpogorski
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: pepogors
ms.openlocfilehash: eb19005019a6e4e878f6b0bd6a145048d4a2804c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103563775"
---
# <a name="deploy-an-azure-service-fabric-cluster-with-stateless-only-node-types-preview"></a>Bereitstellen eines Azure Service Fabric-Clusters mit ausschließlich zustandslosen Knotentypen (Vorschau)
Für Service Fabric-Knotentypen gilt die Annahme, dass zu einem bestimmten Zeitpunkt ggf. zustandsbehaftete Dienste auf den Knoten platziert werden. Zustandslose Knotentypen lockern diese Annahme für einen Knotentyp und ermöglichen so die Nutzung anderer Funktionen wie schnellere Aufskalierungsvorgänge, Unterstützung für automatische Betriebssystemupgrades bei Bronze-Dauerhaftigkeit und horizontales Skalieren auf mehr als 100 Knoten in einer einzigen VM-Skalierungsgruppe.

* Primäre Knotentypen können nicht als zustandslos konfiguriert werden.
* Zustandslose Knotentypen werden nur mit den Bronze-Dauerhaftigkeitsebenen unterstützt.
* Zustandslose Knotentypen werden nur für Service Fabric-Laufzeitversion 7.1.409 oder höher unterstützt.


Es sind Beispielvorlagen verfügbar: [Vorlage für zustandslose Service Fabric-Knotentypen](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="enabling-stateless-node-types-in-service-fabric-cluster"></a>Aktivieren von zustandslosen Knotentypen in einem Service Fabric-Cluster
Legen Sie die **isStateless**-Eigenschaft auf TRUE fest, um mindestens einen Knotentyp in einer Clusterressource als zustandslos festzulegen. Wenn Sie einen Service Fabric-Cluster mit zustandslosen Knotentypen bereitstellen, denken Sie daran, mindestens einen primären Knotentyp in der Clusterressource zu verwenden.

* Die apiVersion der Service Fabric-Clusterressource sollte „2020-12-01-preview“ oder höher lauten.

```json
{
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "isStateless": false,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Bronze",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": false,
        "isStateless": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    }    
    ],
}
```

## <a name="configuring-virtual-machine-scale-set-for-stateless-node-types"></a>Konfigurieren einer VM-Skalierungsgruppe für zustandslose Knotentypen
Zum Aktivieren von zustandslosen Knotentypen sollten Sie Folgendes für die zugrunde liegende VM-Skalierungsgruppenressource konfigurieren:

* Den Wert der **singlePlacementGroup**-Eigenschaft, der auf **false** festgelegt werden muss, wenn eine Skalierung auf mehr als 100 VMs erforderlich ist.
* Der **Modus** **upgradePolicy** der Skalierungsgruppe sollte auf **Parallel** festgelegt werden.
* Der parallele Upgrademodus erfordert die konfigurierte Anwendungsintegritätserweiterung oder Integritätstests. Konfigurieren Sie den Integritätstest mit der Standardkonfiguration für zustandslose Knotentypen, wie unten vorgeschlagen. Nach der Bereitstellung von Anwendungen für den Knotentyp können Integritätstest-/Integritätserweiterungsports geändert werden, um die Anwendungsintegrität zu überwachen.

>[!NOTE]
> Die Standardanzahl der Fehlerdomänen für die Plattform wir auf 5 aktualisiert, wenn ein zustandsloser Knotentyp von einer VM-Skalierungsgruppe gesichert wird, die sich über mehrere Zonen erstreckt. Weitere Informationen finden Sie in dieser [Vorlage](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/15-VM-2-NodeTypes-Windows-Stateless-CrossAZ-Secure) .
> 
> **platformFaultDomainCount:5**
```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "properties": {
        "overprovision": "[variables('overProvision')]",
        "upgradePolicy": {
          "mode": "Rolling",
          "automaticOSUpgradePolicy": {
            "enableAutomaticOSUpgrade": true
          }
        }
    }
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Bronze",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
        },
        "typeHandlerVersion": "1.0"
    }
    },
    {
        "type": "extensions",
        "name": "HealthExtension",
        "properties": {
            "publisher": "Microsoft.ManagedServices",
            "type": "ApplicationHealthWindows",
            "autoUpgradeMinorVersion": true,
            "typeHandlerVersion": "1.0",
            "settings": {
            "protocol": "tcp",
            "port": "19000"
            }
            }
        },
    ]
}
```

## <a name="networking-requirements"></a>Netzwerkanforderungen
### <a name="public-ip-and-load-balancer-resource"></a>Öffentliche IP- und Load Balancer-Ressource
Um die Skalierung auf mehr als 100 VMs für eine VM-Skalierungsgruppenressource zu aktivieren, müssen die Load Balancer- und die IP-Ressource, auf die diese VM-Skalierungsgruppe verweist, beide eine *Standard*-SKU verwenden. Das Erstellen einer Load Balancer- oder IP-Ressource ohne die SKU-Eigenschaft erstellt eine Basic-SKU, die keine Skalierung auf mehr als 100 VMs unterstützt. Ein Standard-SKU-Lastenausgleich blockiert standardmäßig den gesamten Datenverkehr von außen. Sie müssen eine NSG im Subnetz bereitstellen, um den externen Datenverkehr zu ermöglichen.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> Es ist nicht möglich, eine direkte Änderung der SKU an den öffentlichen IP- und Load Balancer-Ressourcen vorzunehmen. Informationen zum Migrieren von vorhandenen Ressourcen mit Basic-SKU finden Sie im Abschnitt zur Migration in diesem Artikel.

### <a name="virtual-machine-scale-set-nat-rules"></a>NAT-Regeln für VM-Skalierungsgruppen
Die NAT-Regeln für eingehenden Datenverkehr des Lastenausgleichs müssen mit den NAT-Pools aus der VM-Skalierungsgruppe übereinstimmen. Jede VM-Skalierungsgruppe muss über einen eindeutigen NAT-Pool für eingehenden Datenverkehr verfügen.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Load Balancer-Ausgangsregeln der Standard-SKU
Load Balancer Standard und Standard Public IP führen im Vergleich zur Verwendung der Basic-SKUs neue Fähigkeiten und andere Verhaltensweisen für ausgehende Verbindungen ein. Wenn Sie bei der Arbeit mit Standard-SKUs ausgehende Verbindungen wünschen, müssen Sie diese explizit entweder mit Standard Public IP-Adressen oder Standard Public Load Balancer definieren. Weitere Informationen finden Sie unter [Ausgehende Verbindungen](../load-balancer/load-balancer-outbound-connections.md) und [Azure Load Balancer Standard](../load-balancer/load-balancer-overview.md).

>[!NOTE]
> Die Standardvorlage verweist auf eine Netzwerksicherheitsgruppe, die standardmäßig den gesamten ausgehenden Datenverkehr gestattet. Der eingehende Datenverkehr ist auf die Ports beschränkt, die für Vorgänge zur Service Fabric-Verwaltung erforderlich sind. Die Regeln der Netzwerksicherheitsgruppe können an Ihre Anforderungen angepasst werden.

>[!NOTE]
> Jeder Service Fabric-Cluster, der die Standard-SKU für SLB verwendet, muss sicherstellen, dass jeder Knotentyp über eine Regel verfügt, die ausgehenden Datenverkehr an Port 443 zulässt. Dies ist erforderlich, um die Clustereinrichtung abzuschließen, und jede Bereitstellung ohne diese Regel schlägt fehl.



### <a name="migrate-to-using-stateless-node-types-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migration zur Verwendung von zustandslosen Knotentypen aus einem Cluster mit einem Basic-SKU-Load Balancer und einer Basic-SKU-IP
Für alle Migrationsszenarien muss ein neuer, zustandsloser Knotentyp hinzugefügt werden. Der vorhandene Knotentyp kann nicht als ausschließlich zustandslos migriert werden.

Sie müssen zunächst eine völlig neue Load Balancer- und IP-Ressource mit der Standard-SKU erstellen, um einen Cluster zu migrieren, der einen Load Balancer und eine IP mit einer Basic-SKU verwendet hat. Es ist nicht möglich, diese Ressourcen direkt zu aktualisieren.

Auf den neuen Load Balancer und die neue IP sollte in den neuen zustandslosen Knotentypen verwiesen werden, die Sie verwenden möchten. Im Beispiel oben wird eine neue VM-Skalierungsgruppenressource hinzugefügt, die für zustandslose Knotentypen verwendet werden soll. Diese VM-Skalierungsgruppen verweisen auf den neu erstellten Load Balancer und die neue IP und sind in der Service Fabric-Clusterresource als zustandslose Knotentypen gekennzeichnet.

Damit Sie beginnen können, müssen Sie die neuen Ressourcen zu Ihrer bestehenden Resource Manager-Vorlage hinzufügen. Diese Ressourcen umfassen Folgendes:
* Eine öffentliche IP-Ressource mit Standard-SKU.
* Eine Load Balancer-Ressource mit Standard-SKU.
* Eine Netzwerksicherheitsgruppe (NSG), auf die das Subnetz verweist, in dem Sie Ihren virtuellen Computer bereitstellen.

Nachdem die Bereitstellung der Ressourcen abgeschlossen wurde, können Sie beginnen, die Knoten im Knotentyp zu deaktivieren, den Sie aus dem ursprünglichen Cluster entfernen möchten.

>[!NOTE]
> Bei Verwendung der automatischen Skalierung mit zustandslosen Knotentypen mit Bronze-Dauerhaftigkeit wird der Knotenzustand nach dem Herunterskalieren nicht automatisch bereinigt. Um den Knotenzustand ausgefallener Knoten während der automatischen Skalierung zu bereinigen, empfiehlt sich die Verwendung der [Service Fabric-Hilfsanwendung für die Autoskalierung](https://github.com/Azure/service-fabric-autoscale-helper).

## <a name="next-steps"></a>Nächste Schritte 
* [Reliable Services](service-fabric-reliable-services-introduction.md)
* [Knotentypen und VM-Skalierungsgruppen](service-fabric-cluster-nodetypes.md)

