---
title: Übersicht über Azure Firewall-Diensttags
description: Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 4/5/2021
ms.author: victorh
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d32d5daf67fe7ab30f3ac1b132bd74edd78e8712
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2021
ms.locfileid: "110700486"
---
# <a name="azure-firewall-service-tags"></a>Azure Firewall-Diensttags

Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern. Sie können kein eigenes Diensttag erstellen und auch nicht angeben, welche IP-Adressen in einem Tag enthalten sind. Microsoft verwaltet die Adresspräfixe, die mit dem Diensttag abgedeckt werden, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

Azure Firewall-Diensttags können im Zielfeld für Netzwerkregeln verwendet werden. Sie können sie anstelle bestimmter IP-Adressen verwenden.

## <a name="supported-service-tags"></a>Unterstützte Diensttags

Eine Liste mit Diensttags, die für Azure Firewall-Netzwerkregeln verfügbar sind, finden Sie unter [Diensttags für virtuelle Netzwerke](../virtual-network/service-tags-overview.md#available-service-tags).

## <a name="configuration"></a>Konfiguration

Azure Firewall unterstützt die Konfiguration von Diensttags über PowerShell, die Azure CLI oder das Azure-Portal.

### <a name="configure-via-azure-powershell"></a>Konfigurieren über Azure PowerShell

In diesem Beispiel müssen wir zunächst Kontext zu unserer zuvor erstellten Azure Firewall-Instanz abrufen.

```Get the context to an existing Azure Firewall
$FirewallName = "AzureFirewall"
$ResourceGroup = "AzureFirewall-RG"
$azfirewall = Get-AzFirewall -Name $FirewallName -ResourceGroupName $ResourceGroup
```

Als Nächstes müssen wir eine neue Regel erstellen.  Als Quelle oder Ziel können Sie den Textwert des Diensttags angeben, das Sie verwenden möchten, wie oben in diesem Artikel erwähnt.

````Create new Network Rules using Service Tags
$rule = New-AzFirewallNetworkRule -Name "AllowSQL" -Description "Allow access to Azure Database as a Service (SQL, MySQL, PostgreSQL, Datawarehouse)" -SourceAddress "10.0.0.0/16" -DestinationAddress Sql -DestinationPort 1433 -Protocol TCP
$ruleCollection = New-AzFirewallNetworkRuleCollection -Name "Data Collection" -Priority 1000 -Rule $rule -ActionType Allow
````

Anschließend müssen wir die Variable, die unsere Azure Firewall-Definition enthält, mit den neu erstellten Netzwerkregeln aktualisieren.

````Merge the new rules into our existing Azure Firewall variable
$azFirewall.NetworkRuleCollections.add($ruleCollection)
`````

Zuletzt müssen wir für die Änderungen an den Netzwerkregeln in der ausgeführten Azure Firewall-Instanz ein Commit durchführen.
````Commit the changes to Azure
Set-AzFirewall -AzureFirewall $azfirewall
````

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Firewall-Regeln finden Sie unter [Logik für die Azure Firewall-Regelverarbeitung](rule-processing.md).
