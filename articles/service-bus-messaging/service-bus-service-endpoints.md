---
title: Konfigurieren von Dienstendpunkten virtueller Netzwerke für Azure Service Bus
description: In diesem Artikel werden Informationen zum Hinzufügen eines Microsoft.ServiceBus-Dienstendpunkts zu einem virtuellen Netzwerk beschrieben.
ms.topic: article
ms.date: 02/12/2021
ms.custom: fasttrack-edit
ms.openlocfilehash: 2e00c9429ab3e39f95bc5ce6df072a99e4f02b86
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100559577"
---
# <a name="allow-access-to-azure-service-bus-namespace-from-specific-virtual-networks"></a>Zulassen des Zugriffs auf den Azure Service Bus-Namespace aus bestimmten virtuellen Netzwerken
Die Integration von Service Bus mit [VNET-Dienstendpunkten][vnet-sep] ermöglicht den sicheren Zugriff auf Messagingfunktionen für Workloads, z. B. an virtuelle Netzwerke (VNETs) gebundene virtuelle Computer. Der Pfad für den Netzwerkdatenverkehr ist dabei an beiden Enden geschützt.

Nachdem die Konfiguration der Bindung an mindestens einen Dienstendpunkt des VNET-Subnetzes durchgeführt wurde, akzeptiert der entsprechende Service Bus-Namespace nur noch Datenverkehr von autorisierten virtuellen Netzwerken und optional bestimmten Internet-IP-Adressen. Aus Sicht des virtuellen Netzwerks wird durch die Bindung eines Service Bus-Namespace an einen Dienstendpunkt ein isolierter Netzwerktunnel vom Subnetz des virtuellen Netzwerks zum Messagingdienst konfiguriert.

Das Ergebnis ist eine private und isolierte Beziehung zwischen den Workloads, die an das Subnetz gebunden sind, und dem entsprechenden Service Bus-Namespace, obwohl sich die beobachtbare Netzwerkadresse des Messaging-Dienstendpunkts in einem öffentlichen IP-Bereich befindet.

Die Implementierung der VNET-Integration kann verhindern, dass andere Azure-Dienste mit Service Bus interagieren. Als Ausnahme können Sie bestimmten vertrauenswürdigen Diensten den Zugriff auf Service Bus-Ressourcen erlauben, auch wenn Netzwerkdienst-Endpunkte aktiviert sind. Eine Liste der vertrauenswürdigen Dienste finden Sie unter [Vertrauenswürdige Dienste](#trusted-microsoft-services).

Die folgenden Microsoft-Dienste müssen sich in einem virtuellen Netzwerk befinden:
- Azure App Service
- Azure-Funktionen

Virtuelle Netzwerke werden nur in Service Bus-Namespaces im [Tarif Premium](service-bus-premium-messaging.md) unterstützt. Bei der Verwendung von VNET-Dienstendpunkten mit Service Bus sollten Sie diese Endpunkte nicht in Anwendungen aktivieren, in denen Service Bus-Namespaces der Tarife Standard und Premium gemischt werden. Der Standard-Tarif unterstützt nämlich keine VNETs. Der Endpunkt ist nur auf Namespaces im Premium-Tarif beschränkt.

> [!IMPORTANT]
> Geben Sie mindestens eine IP-Regel oder eine VNET-Regel für den Namespace an, um nur Datenverkehr von den angegebenen IP-Adressen oder dem Subnetz eines virtuellen Netzwerks zuzulassen. Wenn keine IP- und VNET-Regeln vorliegen, kann (mithilfe des Zugriffsschlüssels) über das öffentliche Internet auf den Namespace zugegriffen werden.  

## <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>Erweiterte Sicherheitsszenarien basierend auf der VNET-Integration 

Lösungen, für die eine strikte und auf mehrere Bereiche aufgeteilte Sicherheit erforderlich ist und bei denen die VNET-Subnetze die Segmentierung zwischen den einzelnen aufgeteilten Diensten bereitstellen, benötigen im Allgemeinen weiterhin Kommunikationspfade zwischen den Diensten, die sich in diesen Bereichen befinden.

Für alle direkten IP-Routen zwischen den Bereichen (auch für HTTPS per TCP/IP) besteht die Gefahr, dass ab der Vermittlungsschicht Sicherheitsrisiken ausgenutzt werden. Bei Messagingdiensten werden vollständig isolierte Kommunikationspfade bereitgestellt, für die Nachrichten während des Übergangs zwischen Parteien sogar auf Datenträger geschrieben werden. Workloads in zwei einzelnen virtuellen Netzwerken, die beide an dieselbe Service Bus-Instanz gebunden sind, können über Nachrichten effizient und zuverlässig kommunizieren, während die Integrität der jeweiligen Netzwerkisolationsgrenze aufrechterhalten wird.
 
Dies bedeutet, dass Ihre sicherheitsrelevanten Cloudlösungen nicht nur Zugriff auf zuverlässige und skalierbare Azure-Funktionen für asynchrones Messaging eines Branchenführers haben, sondern dass das Messaging jetzt auch genutzt werden kann, um Kommunikationspfade zwischen sicheren Lösungsbereichen zu erstellen. Diese sind deutlich sicherer als alle Optionen des Peer-to-Peer-Kommunikationsmodus, einschließlich HTTPS und andere per TLS geschützte Socketprotokolle.

## <a name="binding-service-bus-to-virtual-networks"></a>Binden von Service Bus an virtuelle Netzwerke

*VNET-Regeln* sind das Feature für die Firewallsicherheit, mit dem gesteuert wird, ob Ihr Azure Service Bus-Server Verbindungen eines bestimmten VNET-Subnetzes akzeptiert.

Das Binden eines Service Bus-Namespace an ein virtuelles Netzwerk ist ein Prozess mit zwei Schritten. Zunächst müssen Sie einen **VNET-Dienstendpunkt** in einem Virtual Network-Subnetz erstellen und für **Microsoft.ServiceBus** aktivieren, wie unter [Übersicht über Dienstendpunkte][vnet-sep] beschrieben. Nachdem Sie den Dienstendpunkt hinzugefügt haben, binden Sie den Service Bus-Namespace mit einer **VNET-Regel** daran.

Die VNET-Regel ist eine Zuordnung des Service Bus-Namespace zu einem Subnetz eines virtuellen Netzwerks. Während die Regel vorhanden ist, wird allen Workloads, die an das Subnetz gebunden sind, Zugriff auf den Service Bus-Namespace gewährt. Service Bus stellt selbst niemals ausgehende Verbindungen her, muss keinen Zugriff erhalten und erhält daher niemals die Gewährung des Zugriffs auf Ihr Subnetz, indem diese Regel aktiviert wird.

> [!NOTE]
> Beachten Sie, dass ein Netzwerkdienstendpunkt in einem virtuellen Netzwerk ausgeführte Anwendungen für den Zugriff auf den Service Bus-Namespace bereitstellt. Das virtuelle Netzwerk steuert die Erreichbarkeit des Endpunkts, aber nicht die Vorgänge, die auf Service Bus-Entitäten (Warteschlangen, Themen oder Abonnements) durchgeführt werden können. Verwenden Sie Azure Active Directory (Azure AD), um Vorgänge zu autorisieren, die die Anwendung für den Namespace und seine Entitäten ausführen können. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren einer Anwendung mit Azure Active Directory für den Zugriff auf Azure Service Bus-Entitäten](authenticate-application.md).


## <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt erfahren Sie, wie Sie mit dem Azure-Portal einen VNET-Dienstendpunkt hinzufügen. Zum Beschränken des Zugriffs müssen Sie den VNET-Dienstendpunkt für diesen Event Hubs-Namespace integrieren.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem **Service Bus-Namespace**.
2. Wählen Sie im Menü links unter **Einstellungen** die Option **Netzwerk** aus.  

    > [!NOTE]
    > Die Registerkarte **Netzwerk** wird nur für Namespaces vom Typ **Premium** angezeigt.  
    
    :::image type="content" source="./media/service-bus-ip-filtering/default-networking-page.png" alt-text="Seite „Netzwerk“ – Standard" lightbox="./media/service-bus-ip-filtering/default-networking-page.png":::
    
    Wenn Sie die Option **Alle Netzwerke** auswählen, akzeptiert der Service Bus-Namespace Verbindungen von beliebigen IP-Adressen. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist. 

    ![Firewall – Option „Alle Netzwerke“ ausgewählt](./media/service-bus-ip-filtering/firewall-all-networks-selected.png)
2. Um den Zugriff auf bestimmte virtuelle Netzwerke zu beschränken, wählen Sie die Option **Ausgewählte Netzwerke** aus, sofern sie noch nicht ausgewählt wurde.
1. Wählen Sie im Abschnitt **Virtuelles Netzwerk** der Seite die Option **+ Vorhandenes virtuelles Netzwerk hinzufügen** aus. 

    ![Hinzufügen eines vorhandenen virtuellen Netzwerks](./media/service-endpoints/add-vnet-menu.png)

    >[!WARNING]
    > Wenn Sie die Option **Ausgewählte Netzwerke** auswählen und auf dieser Seite nicht mindestens eine IP-Firewallregel oder ein virtuelles Netzwerk hinzufügen, kann (mit dem Zugriffsschlüssel) über das öffentliche Internet auf den Namespace zugegriffen werden.
3. Wählen Sie in der Liste mit den virtuellen Netzwerken das virtuelle Netzwerk und anschließend das **Subnetz** aus. Sie müssen den Dienstendpunkt aktivieren, bevor Sie das virtuelle Netzwerk der Liste hinzufügen. Wenn der Dienstendpunkt nicht aktiviert ist, erhalten Sie im Portal eine entsprechende Aufforderung.
   
   ![Auswählen des Subnetzes](./media/service-endpoints/select-subnet.png)

4. Die folgende Erfolgsmeldung sollte angezeigt werden, nachdem der Dienstendpunkt für das Subnetz für **Microsoft.ServiceBus** aktiviert wurde. Wählen Sie unten auf der Seite die Option **Hinzufügen** aus, um das Netzwerk hinzuzufügen. 

    ![Auswählen des Subnetzes und Aktivieren des Endpunkts](./media/service-endpoints/subnet-service-endpoint-enabled.png)

    > [!NOTE]
    > Falls Sie den Dienstendpunkt nicht aktivieren können, können Sie den fehlenden VNET-Dienstendpunkt ignorieren und die Resource Manager-Vorlage verwenden. Diese Funktionalität ist nicht im Portal verfügbar.
6. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellungen zu speichern. Warten Sie einige Minuten, bis die Bestätigung in den Portalbenachrichtigungen angezeigt wird. Die Schaltfläche **Speichern** sollte deaktiviert sein. 

    ![Speichern des Netzwerks](./media/service-endpoints/save-vnet.png)

    > [!NOTE]
    > Anweisungen dazu, wie Sie den Zugriff von bestimmten IP-Adressen oder -Adressbereichen zulassen, finden Sie unter [Zulassen des Zugriffs von bestimmten IP-Adressen oder -Adressbereichen](service-bus-ip-filtering.md).

[!INCLUDE [service-bus-trusted-services](../../includes/service-bus-trusted-services.md)]

## <a name="use-resource-manager-template"></a>Verwenden von Resource Manager-Vorlagen
Die folgende Resource Manager-Beispielvorlage fügt einem vorhandenen Service Bus-Namespace eine VNET-Regel hinzu. Die Netzwerkregel gibt die ID eines Subnetzes in einem virtuellen Netzwerk an. 

Die ID ist ein voll qualifizierter Resource Manager-Pfad für das Subnetz des virtuellen Netzwerks. Beispielsweise `/subscriptions/{id}/resourceGroups/{rg}/providers/Microsoft.Network/virtualNetworks/{vnet}/subnets/default` für das Standardsubnetz eines virtuellen Netzwerks.

Legen Sie beim Hinzufügen von VNET- oder Firewallregeln für`defaultAction` den Wert `Deny` fest.

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

> [!IMPORTANT]
> Wenn keine IP- und VNET-Regeln vorliegen, fließt der gesamte Datenverkehr auch dann in den Namespace, wenn Sie für `defaultAction` den Wert `deny` festlegen.  Der Zugriff auf den Namespace erfolgt (mithilfe des Zugriffsschlüssels) über das öffentliche Internet. Geben Sie mindestens eine IP-Regel oder eine VNET-Regel für den Namespace an, um nur Datenverkehr von den angegebenen IP-Adressen oder dem Subnetz eines virtuellen Netzwerks zuzulassen.  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu virtuellen Netzwerken finden Sie unter den folgenden Links:

- [Azure-VNET-Dienstendpunkte][vnet-sep]
- [IP-Filterung mit Azure Service Bus][ip-filtering]

[vnet-sep]: ../virtual-network/virtual-network-service-endpoints-overview.md
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[ip-filtering]: service-bus-ip-filtering.md
