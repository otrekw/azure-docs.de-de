---
title: Konfigurieren von Netzwerkeinstellungen für verwaltete Service Fabric-Cluster (Vorschau)
description: Erfahren Sie, wie Sie Ihre verwalteten Service Fabric-Cluster für NSG-Regeln, RDP-Portzugriff, Lastenausgleichsregeln und viele mehr konfigurieren.
ms.topic: how-to
ms.date: 03/02/2021
ms.openlocfilehash: e17251523c0720665c4c6f5b7811304eebc9923e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743494"
---
# <a name="configure-network-settings-for-service-fabric-managed-clusters-preview"></a>Konfigurieren von Netzwerkeinstellungen für verwaltete Service Fabric-Cluster (Vorschau)

Verwaltete Service Fabric-Cluster werden mit einer standardmäßigen Netzwerkkonfiguration erstellt. Diese Konfiguration besteht aus obligatorischen Regeln für essenzielle Clusterfunktionen sowie einigen optionalen Regeln, die zur Vereinfachung der Kundenkonfiguration konzipiert sind.

Über die standardmäßige Netzwerkkonfiguration hinaus können Sie die Netzwerkregeln an die Anforderungen Ihres Szenarios anpassen.

## <a name="nsg-rules-guidance"></a>Leitfaden zu NSG-Regeln

Berücksichtigen Sie die folgenden Überlegungen, wenn Sie neue NSG-Regeln für Ihren verwalteten Cluster erstellen.

* Verwaltete Service Fabric-Cluster reservieren den NSG-Regelprioritätsbereich 0 bis 999 für essenzielle Funktionen. Sie können keine benutzerdefinierten NSG-Regeln mit einem Prioritätswert unter 1000 erstellen.
* Verwaltete Service Fabric-Cluster reservieren den NSG-Regelprioritätsbereich 3001 bis 4000 für die Erstellung optionaler NSG-Regeln. Diese Regeln werden automatisch hinzugefügt, um die Konfiguration zu vereinfachen und zu beschleunigen. Sie können diese Regeln außer Kraft setzen, indem Sie benutzerdefinierte Regeln im Bereich 1000 bis 3000 hinzufügen.
* Benutzerdefinierte Regeln müssen eine Priorität im Bereich 1000 bis 3000 aufweisen.

## <a name="apply-nsg-rules"></a>Anwenden von NSG-Regeln

Bei klassischen (nicht verwalteten) Service Fabric-Clustern müssen Sie eine separate *Microsoft.Network/networkSecurityGroups*-Ressource deklarieren und verwalten, um [NSG-Regeln auf Ihren Cluster anzuwenden](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-nsg-cluster-65-node-3-nodetype). Verwaltete Service Fabric-Cluster ermöglichen Ihnen die Zuweisung von NSG-Regeln direkt in der Clusterressource Ihrer Bereitstellungsvorlage.

Verwenden Sie die Eigenschaft [networkSecurityRules](/azure/templates/microsoft.servicefabric/managedclusters#managedclusterproperties-object) Ihrer *Microsoft.ServiceFabric/managedclusters*-Ressource (Version `2021-01-01-preview` oder höher), um NSG-Regeln zuzuweisen. Zum Beispiel:

```json
            "apiVersion": "2021-01-01-preview",
            "type": "Microsoft.ServiceFabric/managedclusters",
            ...
            "properties": {
                ...
                "networkSecurityRules" : [
                    {
                        "name": "AllowCustomers",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33000-33499",
                        "access": "Allow",
                        "priority": 2001,
                        "direction": "Inbound" 
                    },
                    {
                        "name": "AllowARM",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "AzureResourceManager",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33500-33699",
                        "access": "Allow",
                        "priority": 2002,
                        "direction": "Inbound" 
                    },
                    {
                        "name": "DenyCustomers",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "Internet",
                        "destinationAddressPrefix": "*",
                        "destinationPortRange": "33700-33799",
                        "access": "Deny",
                        "priority": 2003,
                        "direction": "Outbound"
                    },
                    {
                        "name": "DenyRDP",
                        "protocol": "*",
                        "sourcePortRange": "*",
                        "sourceAddressPrefix": "*",
                        "destinationAddressPrefix": "VirtualNetwork",
                        "destinationPortRange": "3389",
                        "access": "Deny",
                        "priority": 2004,
                        "direction": "Inbound",
                        "description": "Override for optional SFMC_AllowRdpPort rule. This is required in tests to avoid Sev2 incident for security policy violation."
                    }
                ],
                "fabricSettings": [
                ...
                ]
            }
```

## <a name="rdp-ports"></a>RDP-Ports

Verwaltete Service Fabric-Cluster erlauben standardmäßig keinen Zugriff auf die RDP-Ports. Sie können RDP-Ports zum Internet öffnen, indem Sie die folgende Eigenschaft in einer verwalteten Service Fabric-Clusterressource festlegen.

```json
"allowRDPAccess": true 
```

Wenn die Eigenschaft allowRDPAccess auf TRUE festgelegt ist, wird Ihrer Clusterbereitstellung die folgende NSG-Regel hinzugefügt.  

```json
{
    "name": "SFMC_AllowRdpPort", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules",
    "properties": {
        "description": "Optional rule to open RDP ports.",
        "protocol": "tcp",
        "sourcePortRange": "*",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Allow",
        "priority": 3002,
        "direction": "Inbound",
        "sourcePortRanges": [],
        "destinationPortRange": "3389"
    }
}
```

## <a name="clientconnection-and-httpgatewayconnection-ports"></a>clientConnectionPort und httpGatewayConnectionPort

### <a name="nsg-rule-sfmc_allowservicefabricgatewaytosfrp"></a>NSG-Regel: SFMC_AllowServiceFabricGatewayToSFRP

Es wird eine standardmäßige NSG-Regel hinzugefügt, um es dem Service Fabric-Ressourcenanbieter zu ermöglichen, auf den clientConnectionPort und den httpGatewayConnectionPort des Clusters zuzugreifen. Diese Regel erlaubt den Zugriff auf die Ports über das Diensttag „ServiceFabric“.

>[!NOTE]
>Diese Regel wird immer hinzugefügt und kann nicht außer Kraft gesetzt werden.

```json
{ 
    "name": "SFMC_AllowServiceFabricGatewayToSFRP", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules", 
    "properties": { 
        "description": "This is required rule to allow SFRP to connect to the cluster. This rule cannot be overridden.", 
        "protocol": "TCP", 
        "sourcePortRange": "*", 
        "sourceAddressPrefix": "ServiceFabric", 
        "destinationAddressPrefix": "VirtualNetwork", 
        "access": "Allow", 
        "priority": 500, 
        "direction": "Inbound", 
        "sourcePortRanges": [], 
        "destinationPortRanges": [ 
            "19000", 
            "19080" 
        ] 
    } 
}
```

### <a name="nsg-rule-sfmc_allowservicefabricgatewayports"></a>NSG-Regel: SFMC_AllowServiceFabricGatewayPorts

Dies ist eine optionale NSG-Regel, um den Zugriff auf den clientConnectionPort und den httpGatewayPort aus dem Internet zu erlauben. Diese Regel ermöglicht es Kunden, auf SFX zuzugreifen, über PowerShell eine Verbindung mit dem Cluster herzustellen und API-Endpunkte des Service Fabric-Clusters von außerhalb zu verwenden. 

>[!NOTE]
>Diese Regel wird nicht hinzugefügt, wenn bereits eine benutzerdefinierte Regel mit denselben Zugriffs-, Richtungs- und Protokollwerten für ein und denselben Port vorhanden ist. Sie können diese Regel mit benutzerdefinierten NSG-Regeln außer Kraft setzen. 

```json
{ 
    "name": "SFMC_AllowServiceFabricGatewayPorts", 
    "type": "Microsoft.Network/networkSecurityGroups/securityRules", 
    "properties": { 
        "description": "Optional rule to open SF cluster gateway ports. To override add a custom NSG rule for gateway ports in priority range 1000-3000.", 
        "protocol": "tcp", 
        "sourcePortRange": "*", 
        "sourceAddressPrefix": "*", 
        "destinationAddressPrefix": "VirtualNetwork", 
        "access": "Allow", 
        "priority": 3001, 
        "direction": "Inbound", 
        "sourcePortRanges": [], 
        "destinationPortRanges": [ 
            "19000", 
            "19080" 
        ] 
    } 
}
```

## <a name="load-balancer-ports"></a>Lastenausgleichsports

Verwaltete Service Fabric-Cluster erstellen eine NSG-Regel im Standardprioritätsbereich für alle Lastenausgleichsports, die in den Eigenschaften für *ManagedCluster* im Abschnitt „loadBalancingRules“ konfiguriert sind. Diese Regel öffnet Lastenausgleichsports für eingehenden Datenverkehr aus dem Internet.  

>[!NOTE]
>Diese Regel wird dem optionalen Prioritätsbereich hinzugefügt und kann durch Hinzufügen von benutzerdefinierten NSG-Regeln außer Kraft gesetzt werden.

```json
{
    "name": "SFMC_AllowLoadBalancedPorts",
    "type": "Microsoft.Network/networkSecurityGroups/securityRules",
    "properties": {
        "description": "Optional rule to open LB ports",
        "protocol": "*", 
        "sourcePortRange": "*",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "VirtualNetwork",
        "access": "Allow",
        "priority": 3003,
        "direction": "Inbound",
        "sourcePortRanges": [],
        "destinationPortRanges": [
        "80", "8080", "4343"
        ]
    }
}
```

## <a name="load-balancer-probes"></a>Lastenausgleichstests

Verwaltete Service Fabric-Cluster erstellen automatisch Lastenausgleichstests für Fabricgatewayports sowie für alle Ports, die in den Eigenschaften für verwaltete Cluster im Abschnitt „loadBalancingRules“ konfiguriert wurden.

```json
{ 
  "value": [ 
    { 
        "name": "FabricTcpGateway", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 19000, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
                { 
                    "id": "<>"
                } 
            ] 
        }, 
        "type": "Microsoft.Network/loadBalancers/probes" 
    }, 
    { 
        "name": "FabricHttpGateway", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 19080, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
                { 
                    "id": "<>" 
                } 
            ]
        },
        "type": "Microsoft.Network/loadBalancers/probes" 
    },
    {
        "name": "probe1_tcp_8080", 
        "properties": { 
            "provisioningState": "Succeeded", 
            "protocol": "Tcp", 
            "port": 8080, 
            "intervalInSeconds": 5, 
            "numberOfProbes": 2, 
            "loadBalancingRules": [ 
            { 
                "id": "<>" 
            } 
        ] 
      }, 
      "type": "Microsoft.Network/loadBalancers/probes" 
    } 
  ] 
} 
```

## <a name="next-steps"></a>Nächste Schritte

[Konfigurationsoptionen für verwaltete Service Fabric-Cluster](how-to-managed-cluster-configuration.md)

[Verwaltete Service Fabric-Cluster (Übersicht)](overview-managed-cluster.md)
