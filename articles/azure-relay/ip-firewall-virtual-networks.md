---
title: Konfigurieren einer IP-Firewall für den Azure Relay-Namespace
description: In diesem Artikel wird beschrieben, wie Sie mithilfe von Firewallregeln Verbindungen von bestimmten IP-Adressen mit Azure Relay-Namespaces zulassen.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 250158aff2ceb89e2823b711717f1d3a1cad438c
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90976026"
---
# <a name="configure-ip-firewall-for-an-azure-relay-namespace"></a>Konfigurieren einer IP-Firewall für einen Azure Relay-Namespace
Standardmäßig kann über das Internet auf Relaynamespaces zugegriffen werden, solange die Anforderung eine gültige Authentifizierung und Autorisierung aufweist. Mit der IP-Firewall können Sie den Zugriff auf eine Gruppe von IPv4-Adressen oder IPv4-Adressbereichen in der [CIDR-Notation (Classless Inter-Domain Routing)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) weiter einschränken.

Diese Funktion ist in Szenarien hilfreich, in denen Azure Relay nur von bestimmten bekannten Websites aus zugänglich sein soll. Mithilfe von Firewallregeln können Sie Regeln konfigurieren, um Datenverkehr von bestimmten IPv4-Adressen zuzulassen. Wenn Sie z. B. Relay mit [Azure Express Route](../expressroute/expressroute-faqs.md#supported-services) verwenden, können Sie eine **Firewallregel** erstellen, um Datenverkehr nur von den IP-Adressen Ihrer lokalen Infrastruktur zuzulassen. 


> [!IMPORTANT]
> Diese Funktion steht derzeit als Vorschau zur Verfügung. 


## <a name="enable-ip-firewall-rules"></a>Aktivieren von IP-Firewallregeln
Die IP-Firewallregeln werden auf der Namespaceebene angewendet. Daher gelten die Regeln für alle Clientverbindungen mit einem beliebigen unterstützten Protokoll. Jeder Verbindungsversuch von einer IP-Adresse, die nicht mit einer zulässigen IP-Regel im Namespace übereinstimmt, wird als nicht autorisiert abgelehnt. In der Antwort wird die IP-Regel nicht erwähnt. IP-Filterregeln werden der Reihe nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

### <a name="use-azure-portal"></a>Verwenden des Azure-Portals
In diesem Abschnitt erfahren Sie, wie Sie im Azure-Portal IP-Firewallregeln für einen Namespace erstellen. 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem **Relaynamespace**.
2. Wählen Sie im Menü auf der linken Seite die Option **Netzwerk** aus. Wenn Sie die Option **Alle Netzwerke** im Abschnitt **Zugriff zulassen von** auswählen, akzeptiert der Relaynamespace Verbindungen von jeder beliebigen IP-Adresse. Diese Einstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist. 

    ![Screenshot: Seite „Netzwerk“, auf der die Option „Alle Netzwerke“ ausgewählt ist](./media/ip-firewall/all-networks-selected.png)
1. Um den Zugriff auf bestimmte Netzwerke und IP-Adressen einzuschränken, wählen Sie die Option **Ausgewählte Netzwerke** aus. Gehen Sie im Abschnitt **Firewall** wie folgt vor:
    1. Wählen Sie die Option **Client-IP-Adresse hinzufügen** aus, um Ihrer aktuellen Client-IP Zugriff auf den Namespace zu gewähren. 
    2. Geben Sie für **Adressbereich** eine bestimmte IPv4-Adresse oder einen Bereich von IPv4-Adressen in der CIDR-Notation ein. 

        ![Firewall: Option „Alle Netzwerke“ ausgewählt](./media/ip-firewall/selected-networks-trusted-access-disabled.png)
3. Klicken Sie auf der Symbolleiste auf **Speichern**, um die Einstellungen zu speichern. Warten Sie einige Minuten, bis die Bestätigung in den Portalbenachrichtigungen angezeigt wird.


### <a name="use-resource-manager-template"></a>Verwenden von Resource Manager-Vorlagen
Mithilfe der folgenden Resource Manager-Vorlage können Sie einem vorhandenen Relaynamespace eine IP-Filterregel hinzufügen.

Die Vorlage nimmt einen Parameter an: **ipMask**. Dabei handelt es sich um eine einzelne IPv4-Adresse oder einen Block von IP-Adressen in CIDR-Notation. In CIDR-Notation steht beispielsweise 70.37.104.0/24 für die 256 IPv4-Adressen von 70.37.104.0 bis 70.37.104.255. „24“ gibt dabei die Anzahl signifikanter Präfixbits für den Bereich an.

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

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "relayNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Relay namespace"
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
      "namespaceNetworkRuleSetName": "[concat(parameters('relayNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('relayNamespaceName')]",
        "type": "Microsoft.Relay/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.Relay/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.Relay/namespaces/', parameters('relayNamespaceName'))]"
        ],
        "properties": {
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Gehen Sie zum Bereitstellen der Vorlage gemäß den Anweisungen für [Azure Resource Manager](../azure-resource-manager/templates/deploy-powershell.md) vor.



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Features in Bezug auf Netzwerksicherheit finden Sie unter [Netzwerksicherheit](network-security.md).


<!-- Links -->

[express-route]:  ../expressroute/expressroute-faqs.md#supported-services
