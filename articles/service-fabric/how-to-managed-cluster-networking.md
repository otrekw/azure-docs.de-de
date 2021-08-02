---
title: Konfigurieren von Netzwerkeinstellungen für verwaltete Service Fabric-Cluster
description: Erfahren Sie, wie Sie Ihre verwalteten Service Fabric-Cluster für NSG-Regeln, RDP-Portzugriff, Lastenausgleichsregeln und viele mehr konfigurieren.
ms.topic: how-to
ms.date: 5/10/2021
ms.openlocfilehash: 5164a7e3aeb1e82700bd5c5bc4d44e55de64421b
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111895597"
---
# <a name="configure-network-settings-for-service-fabric-managed-clusters"></a>Konfigurieren von Netzwerkeinstellungen für verwaltete Service Fabric-Cluster

Verwaltete Service Fabric-Cluster werden mit einer standardmäßigen Netzwerkkonfiguration erstellt. Diese Konfiguration besteht aus obligatorischen Regeln für essenzielle Clusterfunktionen sowie einigen optionalen Regeln, die zur Vereinfachung der Kundenkonfiguration konzipiert sind, z. B. damit standardmäßig der gesamte ausgehende Datenverkehr zugelassen wird.

Über die standardmäßige Netzwerkkonfiguration hinaus können Sie die Netzwerkregeln an die Anforderungen Ihres Szenarios anpassen.

## <a name="nsg-rules-guidance"></a>Leitfaden zu NSG-Regeln

Berücksichtigen Sie die folgenden Überlegungen, wenn Sie neue NSG-Regeln für Ihren verwalteten Cluster erstellen.

* Verwaltete Service Fabric-Cluster reservieren den NSG-Regelprioritätsbereich 0 bis 999 für essenzielle Funktionen. Sie können keine benutzerdefinierten NSG-Regeln mit einem Prioritätswert unter 1000 erstellen.
* Verwaltete Service Fabric-Cluster reservieren den NSG-Regelprioritätsbereich 3001 bis 4000 für die Erstellung optionaler NSG-Regeln. Diese Regeln werden automatisch hinzugefügt, um die Konfiguration zu vereinfachen und zu beschleunigen. Sie können diese Regeln außer Kraft setzen, indem Sie benutzerdefinierte Regeln im Bereich 1000 bis 3000 hinzufügen.
* Benutzerdefinierte Regeln müssen eine Priorität im Bereich 1000 bis 3000 aufweisen.

## <a name="apply-nsg-rules"></a>Anwenden von NSG-Regeln

Bei klassischen (nicht verwalteten) Service Fabric-Clustern müssen Sie eine separate *Microsoft.Network/networkSecurityGroups*-Ressource deklarieren und verwalten, um [NSG-Regeln auf Ihren Cluster anzuwenden](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.servicefabric/service-fabric-secure-nsg-cluster-65-node-3-nodetype). Verwaltete Service Fabric-Cluster ermöglichen Ihnen die Zuweisung von NSG-Regeln direkt in der Clusterressource Ihrer Bereitstellungsvorlage.

Verwenden Sie die Eigenschaft [networkSecurityRules](/azure/templates/microsoft.servicefabric/managedclusters#managedclusterproperties-object) Ihrer *Microsoft.ServiceFabric/managedclusters*-Ressource (Version `2021-05-01` oder höher), um NSG-Regeln zuzuweisen. Beispiel:

```json
            "apiVersion": "2021-05-01",
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

Verwaltete Service Fabric-Cluster erstellen automatisch NAT-Regeln für eingehenden Datenverkehr für jede Instanz in einem Knotentyp. Wenn Sie die Portzuordnungen ermitteln möchten, mit denen Sie bestimmte Instanzen erreichen können (Clusterknoten), befolgen Sie die Schritte unten:

Suchen Sie mithilfe des Azure-Portals nach den NAT-Regeln für eingehenden Datenverkehr für den verwalteten Cluster für das Remotedesktopprotokoll (RDP).

1. Navigieren Sie zur verwalteten Clusterressourcengruppe innerhalb Ihres Abonnements, die mit dem folgenden Format benannt ist: SFC_{cluster-id}.

2. Wählen Sie den Lastenausgleich für den Cluster mit dem folgenden Format aus: LB-{cluster-name}.

3. Klicken Sie auf der Seite für Ihren Lastenausgleich auf „NAT-Regeln für eingehenden Datenverkehr“. Überprüfen Sie die NAT-Regeln für eingehenden Datenverkehr, um den eingehenden Front-End-Port zu bestätigen, um eine Portzuordnung für einen Knoten anzuzielen. 

   Der folgende Screenshot zeigt die NAT-Regeln für eingehenden Datenverkehr für drei verschiedene Knotentypen:

   ![Eingehende NAT-Regeln][Inbound-NAT-Rules]

   Standardmäßig befindet sich der Front-End-Port für Windows-Cluster im Bereich von 50000 und höher, und der Zielport ist 3389. Dies ist dem RDP-Dienst auf dem Zielknoten zugeordnet.

4. Stellen Sie eine Remoteverbindung mit dem spezifischen Knoten (Skalierungsgruppeninstanz) her. Sie können den Benutzernamen und das Kennwort, die Sie beim Erstellen des Clusters erstellt haben, oder beliebige andere Anmeldeinformationen, die Sie konfiguriert haben, verwenden.

Der folgende Screenshot zeigt die Verwendung einer Remotedesktopverbindung, um eine Verbindung zum Apps-Knoten (Instanz 0) in einem Windows-Cluster herzustellen:

![Remotedesktopverbindung][sfmc-rdp-connect]

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

Diese optionale Regel ermöglicht es Kunden, auf SFX zuzugreifen, mit PowerShell eine Verbindung zum Cluster herzustellen und Cluster-API-Endpunkte in Service Fabric aus dem Internet zu nutzen, indem LB-Ports für clientConnectionPort und httpGatewayPort geöffnet werden.

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

Verwaltete Service Fabric-Cluster erstellen eine NSG-Regel im Standardprioritätsbereich für alle LB-Ports (Lastenausgleich), die im Abschnitt „loadBalancingRules“ unter den Eigenschaften *ManagedCluster* konfiguriert sind. Diese Regel öffnet Lastenausgleichsports für eingehenden Datenverkehr aus dem Internet.  

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

Verwaltete Service Fabric-Cluster erstellen automatisch Lastenausgleichstests für Fabric Gatewayports sowie für alle Ports, die in den Eigenschaften für verwaltete Cluster im Abschnitt „loadBalancingRules“ konfiguriert wurden.

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

<!--Image references-->
[Inbound-NAT-Rules]: ./media/how-to-managed-cluster-networking/inbound-nat-rules.png
[sfmc-rdp-connect]: ./media/how-to-managed-cluster-networking/sfmc-rdp-connect.png

