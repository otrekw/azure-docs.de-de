---
title: Konfigurieren von Dienstendpunkten virtueller Netzwerke für Azure Service Bus
description: In diesem Artikel werden Informationen zum Hinzufügen eines Microsoft.ServiceBus-Dienstendpunkts zu einem virtuellen Netzwerk beschrieben.
services: service-bus
documentationcenter: ''
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: aschhab
ms.openlocfilehash: d1766ffb579bb1a86da91ac73a396ce0d008f89e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82117623"
---
# <a name="configure-virtual-network-service-endpoints-for-azure-service-bus"></a>Konfigurieren von Dienstendpunkten virtueller Netzwerke für Azure Service Bus

Die Integration von Service Bus mit [VNET-Dienstendpunkten][vnet-sep] ermöglicht den sicheren Zugriff auf Messagingfunktionen für Workloads, z. B. an virtuelle Netzwerke (VNETs) gebundene virtuelle Computer. Der Pfad für den Netzwerkdatenverkehr ist dabei an beiden Enden geschützt.

Nachdem die Konfiguration der Bindung an mindestens einen Dienstendpunkt des VNET-Subnetzes durchgeführt wurde, akzeptiert der entsprechende Service Bus-Namespace nur noch Datenverkehr von autorisierten virtuellen Netzwerken. Aus Sicht des virtuellen Netzwerks wird durch die Bindung eines Service Bus-Namespace an einen Dienstendpunkt ein isolierter Netzwerktunnel vom Subnetz des virtuellen Netzwerks zum Messagingdienst konfiguriert.

Das Ergebnis ist eine private und isolierte Beziehung zwischen den Workloads, die an das Subnetz gebunden sind, und dem entsprechenden Service Bus-Namespace, obwohl sich die beobachtbare Netzwerkadresse des Messaging-Dienstendpunkts in einem öffentlichen IP-Bereich befindet.

>[!WARNING]
> Die Implementierung der VNET-Integration kann verhindern, dass andere Azure-Dienste mit Service Bus interagieren.
>
> Vertrauenswürdige Microsoft-Dienste werden bei Implementierung von Virtual Networks nicht unterstützt.
>
> Allgemeine Azure-Szenarien, die nicht mit Virtual Networks funktionieren (beachten Sie, dass die Liste **NICHT** vollständig ist):
> - Integration in Azure Event Grid
> - Azure IoT Hub-Routen
> - Azure IoT Device Explorer
>
> Die folgenden Microsoft-Dienste müssen in einem virtuellen Netzwerk ausgeführt werden:
> - Azure App Service
> - Azure-Funktionen

> [!IMPORTANT]
> Virtuelle Netzwerke werden nur in Service Bus-Namespaces im [Tarif Premium](service-bus-premium-messaging.md) unterstützt.
> 
> Bei der Verwendung von VNET-Dienstendpunkten mit Service Bus sollten Sie diese Endpunkte nicht in Anwendungen aktivieren, in denen Service Bus-Namespaces der Tarife Standard und Premium gemischt werden. Die Tarif Standard unterstützt keine VNETs. Der Endpunkt ist nur auf Namespaces im Premium-Tarif beschränkt.

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Erweiterte Sicherheitsszenarien basierend auf der VNET-Integration 

Lösungen, für die eine strikte und auf mehrere Bereiche aufgeteilte Sicherheit erforderlich ist und bei denen die VNET-Subnetze die Segmentierung zwischen den einzelnen aufgeteilten Diensten bereitstellen, benötigen im Allgemeinen weiterhin Kommunikationspfade zwischen den Diensten, die sich in diesen Bereichen befinden.

Für alle direkten IP-Routen zwischen den Bereichen (auch für HTTPS per TCP/IP) besteht die Gefahr, dass ab der Vermittlungsschicht Sicherheitsrisiken ausgenutzt werden. Bei Messagingdiensten werden vollständig isolierte Kommunikationspfade bereitgestellt, für die Nachrichten während des Übergangs zwischen Parteien sogar auf Datenträger geschrieben werden. Workloads in zwei einzelnen virtuellen Netzwerken, die beide an dieselbe Service Bus-Instanz gebunden sind, können über Nachrichten effizient und zuverlässig kommunizieren, während die Integrität der jeweiligen Netzwerkisolationsgrenze aufrechterhalten wird.
 
Dies bedeutet, dass Ihre sicherheitsrelevanten Cloudlösungen nicht nur Zugriff auf zuverlässige und skalierbare Azure-Funktionen für asynchrones Messaging eines Branchenführers haben, sondern dass das Messaging jetzt auch genutzt werden kann, um Kommunikationspfade zwischen sicheren Lösungsbereichen zu erstellen. Diese sind deutlich sicherer als alle Optionen des Peer-to-Peer-Kommunikationsmodus, einschließlich HTTPS und andere per TLS geschützte Socketprotokolle.

## <a name="binding-service-bus-to-virtual-networks"></a>Binden von Service Bus an virtuelle Netzwerke

*VNET-Regeln* sind das Feature für die Firewallsicherheit, mit dem gesteuert wird, ob Ihr Azure Service Bus-Server Verbindungen eines bestimmten VNET-Subnetzes akzeptiert.

Das Binden eines Service Bus-Namespace an ein virtuelles Netzwerk ist ein Prozess mit zwei Schritten. Zunächst müssen Sie einen **VNET-Dienstendpunkt** in einem Virtual Network-Subnetz erstellen und für **Microsoft.ServiceBus** aktivieren, wie unter [Übersicht über Dienstendpunkte][vnet-sep] beschrieben. Nachdem Sie den Dienstendpunkt hinzugefügt haben, binden Sie den Service Bus-Namespace mit einer **VNET-Regel** daran.

Die VNET-Regel ist eine Zuordnung des Service Bus-Namespace zu einem Subnetz eines virtuellen Netzwerks. Während die Regel vorhanden ist, wird allen Workloads, die an das Subnetz gebunden sind, Zugriff auf den Service Bus-Namespace gewährt. Service Bus stellt selbst niemals ausgehende Verbindungen her, muss keinen Zugriff erhalten und erhält daher niemals die Gewährung des Zugriffs auf Ihr Subnetz, indem diese Regel aktiviert wird.

## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt erfahren Sie, wie Sie mit dem Azure-Portal einen VNET-Dienstendpunkt hinzufügen. Zum Beschränken des Zugriffs müssen Sie den Dienstendpunkt des virtuellen Netzwerks (VNET-Dienstendpunkt) für diesen Event Hubs-Namespace integrieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem **Service Bus-Namespace**.
2. Wählen Sie im Menü auf der linken Seite die Option **Netzwerk** aus. Standardmäßig ist die Option **Alle Netzwerke** ausgewählt. Ihr Namespace akzeptiert Verbindungen von jeder IP-Adresse. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist. 

    ![Firewall: Option „Alle Netzwerke“ ausgewählt](./media/service-endpoints/firewall-all-networks-selected.png)
1. Wählen Sie oben auf der Seite die Option **Ausgewählte Netzwerke** aus.
2. Wählen Sie im Abschnitt **Virtuelles Netzwerk** der Seite die Option **+ Vorhandenes virtuelles Netzwerk hinzufügen** aus. 

    ![Hinzufügen eines vorhandenen virtuellen Netzwerks](./media/service-endpoints/add-vnet-menu.png)
3. Wählen Sie in der Liste mit den virtuellen Netzwerken das virtuelle Netzwerk und anschließend das **Subnetz** aus. Sie müssen den Dienstendpunkt aktivieren, bevor Sie das virtuelle Netzwerk der Liste hinzufügen. Wenn der Dienstendpunkt nicht aktiviert ist, erhalten Sie im Portal eine entsprechende Aufforderung.
   
   ![Auswählen des Subnetzes](./media/service-endpoints/select-subnet.png)

4. Die folgende Erfolgsmeldung sollte angezeigt werden, nachdem der Dienstendpunkt für das Subnetz für **Microsoft.ServiceBus** aktiviert wurde. Wählen Sie **Hinzufügen** unten auf der Seite aus, um das Netzwerk hinzuzufügen. 

    ![Auswählen des Subnetzes und Aktivieren des Endpunkts](./media/service-endpoints/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Falls Sie den Dienstendpunkt nicht aktivieren können, können Sie den fehlenden VNET-Dienstendpunkt ignorieren und die Resource Manager-Vorlage verwenden. Diese Funktionalität ist nicht im Portal verfügbar.
6. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellungen zu speichern. Warten Sie einige Minuten, bis die Bestätigung in den Portalbenachrichtigungen angezeigt wird. Die Schaltfläche **Speichern** sollte deaktiviert sein. 

    ![Speichern des Netzwerks](./media/service-endpoints/save-vnet.png)

## <a name="use-resource-manager-template"></a>Verwenden von Resource Manager-Vorlagen
Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Service Bus-Namespace eine VNET-Regel hinzufügen.

Vorlagenparameter:

* **namespaceName**: Service Bus-Namespace
* **virtualNetworkingSubnetId**: Vollqualifizierter Resource Manager-Pfad für das Subnetz des virtuellen Netzwerks, z.B. `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` für das Standardsubnetz eines virtuellen Netzwerks

> [!NOTE]
> Obwohl keine Verweigerungsregeln möglich sind, ist in der Azure Resource Manager-Vorlage die Standardaktion auf **„Zulassen“** festgelegt. Dies schränkt die Verbindungen nicht ein.
> Bei der Erstellung von Regeln für virtuelle Netzwerke oder Firewalls muss die Standardaktion (***defaultAction***) geändert werden.
> 
> from
> ```json
> "defaultAction": "Allow"
> ```
> zu
> ```json
> "defaultAction": "Deny"
> ```
>

Vorlage:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "servicebusNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Service Bus namespace"
        }
      },
      "virtualNetworkName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Rule"
        }
      },
      "subnetName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Virtual Network Sub Net"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('servicebusNamespaceName'), concat('/', 'default'))]",
      "subNetId": "[resourceId('Microsoft.Network/virtualNetworks/subnets/', parameters('virtualNetworkName'), parameters('subnetName'))]"
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('servicebusNamespaceName')]",
        "type": "Microsoft.ServiceBus/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Premium",
          "tier": "Premium"
        },
        "properties": { }
      },
      {
        "apiVersion": "2017-09-01",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "type": "Microsoft.Network/virtualNetworks",
        "properties": {
          "addressSpace": {
            "addressPrefixes": [
              "10.0.0.0/23"
            ]
          },
          "subnets": [
            {
              "name": "[parameters('subnetName')]",
              "properties": {
                "addressPrefix": "10.0.0.0/23",
                "serviceEndpoints": [
                  {
                    "service": "Microsoft.ServiceBus"
                  }
                ]
              }
            }
          ]
        }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.ServiceBus/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.ServiceBus/namespaces/', parameters('servicebusNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": 
          [
            {
              "subnet": {
                "id": "[variables('subNetId')]"
              },
              "ignoreMissingVnetServiceEndpoint": false
            }
          ],
          "ipRules":[<YOUR EXISTING IP RULES>],
          "trustedServiceAccessEnabled": false,          
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Gehen Sie zum Bereitstellen der Vorlage gemäß den Anweisungen für [Azure Resource Manager][lnk-deploy] vor.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu virtuellen Netzwerken finden Sie unter den folgenden Links:

- [Azure-VNET-Dienstendpunkte][vnet-sep]
- [IP-Filterung mit Azure Service Bus][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: service-bus-ip-filtering.md
