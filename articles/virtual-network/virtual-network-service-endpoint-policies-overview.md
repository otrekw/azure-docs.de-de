---
title: Richtlinien für Azure-VNET-Dienstendpunkte | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie virtuellen Netzwerkdatenverkehr zu Azure-Dienstressourcen mithilfe von Richtlinien für Dienstendpunkte filtern.
services: virtual-network
documentationcenter: na
author: RDhillon
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2020
ms.author: rdhillon
ms.openlocfilehash: 926da07ffaf0c61ca2a7fd02351ef3635ec4d73b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651292"
---
# <a name="virtual-network-service-endpoint-policies-for-azure-storage"></a>Richtlinien für VNET-Dienstendpunkte für Azure Storage

Richtlinien für VNET-Dienstendpunkte ermöglichen es Ihnen, ausgehenden virtuellen Netzwerkdatenverkehr zu Azure Storage-Konten über den Dienstendpunkt zu filtern. Außerdem lassen sie die Datenexfiltration nur zu bestimmten Azure Storage-Konten zu. Endpunktrichtlinien ermöglichen eine präzise Zugriffssteuerung für virtuellen Netzwerkdatenverkehr zu Azure Storage, wenn eine Verbindung über den Dienstendpunkt hergestellt wird.

![Sichern des ausgehenden virtuellen Netzwerkdatenverkehrs zu Azure Storage-Konten](./media/virtual-network-service-endpoint-policies-overview/vnet-service-endpoint-policies-overview.png)

Dieses Feature ist für __Azure Storage__ in __allen globalen Azure-Regionen__ allgemein verfügbar.

## <a name="key-benefits"></a>Hauptvorteile

Richtlinien für VNET-Dienstendpunkte bieten folgende Vorteile:

- __Verbesserte Sicherheit für Ihren virtuellen Netzwerkdatenverkehr zu Azure Storage__

  [Azure-Diensttags für Netzwerksicherheitsgruppen](https://aka.ms/servicetags) ermöglichen es Ihnen, den ausgehenden virtuellen Netzwerkdatenverkehr zu bestimmten Azure Storage-Regionen einzuschränken. Dies ermöglicht jedoch den Datenverkehr zu jedem beliebigen Konto innerhalb der ausgewählten Azure Storage-Region.
  
  Mit Endpunktrichtlinien können Sie die Azure Storage-Konten angeben, für die der ausgehende Zugriff auf das virtuelle Netzwerk zulässig ist, und den Zugriff auf alle anderen Speicherkonten einschränken. Damit erhalten Sie eine viel präzisere Sicherheitskontrolle, um die Datenexfiltration aus Ihrem virtuellen Netzwerk zu schützen.

- __Skalierbare, hoch verfügbare Richtlinien zum Filtern des Azure-Dienstdatenverkehrs__

   Endpunktrichtlinien bieten eine horizontal skalierbare, hoch verfügbare Lösung zum Filtern des Azure-Dienstdatenverkehrs aus virtuellen Netzwerken über Dienstendpunkte. Für die Verwaltung der zentralen Netzwerkappliances für diesen Datenverkehr in Ihren virtuellen Netzwerken fällt kein zusätzlicher Aufwand an.

## <a name="json-object-for-service-endpoint-policies"></a>JSON-Objekt für Dienstendpunktrichtlinien
Werfen Sie einen kurzen Blick auf das Objekt „Dienstendpunktrichtlinie“.

```json
"serviceEndpointPolicyDefinitions": [
    {
            "description": null,
            "name": "MySEP-Definition",
            "resourceGroup": "MySEPDeployment",
            "service": "Microsoft.Storage",
            "serviceResources": [ 
                    "/subscriptions/subscriptionID/resourceGroups/MySEPDeployment/providers/Microsoft.Storage/storageAccounts/mystgacc"
            ],
            "type": "Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions"
    }
]
```

## <a name="configuration"></a>Konfiguration

-   Sie können die Endpunktrichtlinien so konfigurieren, dass der virtuelle Netzwerkdatenverkehr zu bestimmten Azure Storage-Konten eingeschränkt wird.
-   Endpunktrichtlinien werden in einem Subnetz eines virtuellen Netzwerks konfiguriert. Dienstendpunkte für Azure Storage sollten im Subnetz aktiviert werden, damit die Richtlinie angewendet werden kann.
-   Mit der Endpunktrichtlinie können Sie der Zulassungsliste bestimmte Azure Storage Konten im Format „resourceID“ hinzufügen. Sie können den Zugriff einschränken auf
    - alle Speicherkonten in einem Abonnement,<br>
      `E.g. /subscriptions/subscriptionId`

    - alle Speicherkonten in einer Ressourcengruppe,<br>
      `E.g. subscriptions/subscriptionId/resourceGroups/resourceGroupName`
     
    - ein einzelnes Speicherkonto, indem Sie die entsprechende Azure Resource Manager-resourceID auflisten. Dies deckt den Datenverkehr zu Blobs, Tabellen, Warteschlangen, Dateien und Azure Data Lake Storage Gen2 ab. <br>
    `E.g. /subscriptions/subscriptionId/resourceGroups/resourceGroupName/providers/Microsoft.Storage/storageAccounts/storageAccountName`
-   Wenn an ein Subnetz mit Endpunkten keine Richtlinien angefügt sind, können Sie standardmäßig auf alle Speicherkonten im Dienst zugreifen. Sobald eine Richtlinie in diesem Subnetz konfiguriert wurde, können Compute-Instanzen in diesem Subnetz nur auf die in der Richtlinie angegebenen Ressourcen zugreifen. Der Zugriff auf alle anderen Speicherkonten wird verweigert.
-   Wenn Dienstendpunktrichtlinien auf ein Subnetz angewendet werden, wird der Azure Storage-*Bereich „Dienstendpunkt“* von „regional“ auf **global** aktualisiert. Dies bedeutet, dass der gesamte Datenverkehr zu Azure Storage danach über den Dienstendpunkt geschützt wird. Weil Dienstendpunktrichtlinien auch global gelten, wird Speicherkonten, die nicht explizit zugelassen wurden, der Zugriff verweigert. 
-   Sie können mehrere Richtlinien auf ein Subnetz anwenden. Wenn dem Subnetz mehrere Richtlinien zugeordnet sind, wird der virtuelle Netzwerkdatenverkehr zu Ressourcen zugelassen, die in einer beliebigen dieser Richtlinien angegeben sind. Der Zugriff auf alle anderen Dienstressourcen, die in keiner der Richtlinien angegeben sind, wird verweigert.

    > [!NOTE]  
    > Dienstendpunktrichtlinien sind **Zulassungsrichtlinien**. Deshalb werden – abgesehen von den angegebenen Ressourcen – alle anderen Ressourcen eingeschränkt. Stellen Sie bitte sicher, dass alle Abhängigkeiten von Dienstressourcen für Ihre Anwendungen in der Richtlinie identifiziert und aufgelistet werden.

- Nur Speicherkonten, die das Azure-Ressourcenmodell verwenden, können in der Endpunktrichtlinie angegeben werden. Azure-Dienstendpunktrichtlinien werden von Ihren klassischen Azure Storage-Konten nicht unterstützt.
- Der Zugriff auf sekundären georedundanten Speicher mit Lesezugriff wird automatisch zugelassen, wenn das primäre Konto angegeben ist.
- Speicherkonten können sich im selben oder einem anderen Abonnement oder Azure Active Directory-Mandanten befinden wie das virtuelle Netzwerk.

## <a name="scenarios"></a>Szenarien

- **Mittels Peering verknüpfte, verbundene oder mehrere virtuelle Netzwerke**: Um den Datenverkehr in virtuellen Peeringnetzwerken zu filtern, sollten Endpunktrichtlinien einzeln auf diese virtuellen Netzwerke angewendet werden.
- **Filtern von Internetdatenverkehr mit Netzwerkgeräten oder per Azure Firewall**: Hierbei wird der Datenverkehr der Azure-Dienste über Dienstendpunkte mit Richtlinien gefiltert, und der restliche Internet- oder Azure-Datenverkehr wird über Geräte oder per Azure Firewall gefiltert.
- **Filtern von Datenverkehr für Azure-Dienste, die in virtuellen Netzwerken bereitgestellt werden**: Zu diesem Zeitpunkt werden Azure-Dienstendpunktrichtlinien für verwaltete Azure-Dienste, die in Ihrem virtuellen Netzwerk bereitgestellt wurden, nicht unterstützt. 
- **Filtern von Datenverkehr aus der lokalen Umgebung für Azure-Dienste**: Dienstendpunktrichtlinien gelten nur für den Datenverkehr aus Subnetzen, die den Richtlinien zugeordnet sind. Um den Zugriff auf bestimmte Azure-Dienstressourcen aus lokalen Netzwerken zuzulassen, sollte der Datenverkehr mithilfe von virtuellen Netzwerkappliances oder Firewalls gefiltert werden.

## <a name="logging-and-troubleshooting"></a>Protokollierung und Problembehandlung
Für Richtlinien für Dienstendpunkte steht keine zentrale Protokollierung zur Verfügung. Informationen zu Dienstdiagnoseprotokollen finden Sie unter [Protokollierung für Dienstendpunkte](virtual-network-service-endpoints-overview.md#logging-and-troubleshooting).

### <a name="troubleshooting-scenarios"></a>Problembehandlungsszenarien
- Der Zugriff auf Speicherkonten, die in der Vorschau (nicht in der geografisch gekoppelten Region) funktionierten, wurde verweigert.
  - Mit dem Azure Storage-Upgrade auf die Nutzung von globalen Diensttags ist der Bereich „Dienstendpunkt“ jetzt „Global“. Dies gilt folglich auch für Dienstendpunktrichtlinien. Deshalb wird jeglicher Datenverkehr zu Azure Storage über Dienstendpunkte verschlüsselt, und nur den in der Richtlinie explizit aufgelisteten Speicherkonten wird der Zugriff gestattet.
  - Nehmen Sie alle erforderlichen Speicherkonten explizit in die Zulassungsliste auf, um den Zugriff wiederherzustellen.  
  - Wenden Sie sich an den Azure-Support.
- Der Zugriff wird für Konten verweigert, die in den Endpunktrichtlinien aufgeführt sind.
  - Netzwerksicherheitsgruppen oder Firewallfilter blockieren möglicherweise den Zugriff.
  - Wenn das Entfernen bzw. erneute Anwenden der Richtlinie zu Konnektivitätsverlust führt, gehen Sie folgendermaßen vor:
    - Überprüfen Sie, ob der Azure-Dienst so konfiguriert wurde, dass der Zugriff aus dem virtuellen Netzwerk über Endpunkte zugelassen ist. Überprüfen Sie außerdem, ob die Standardrichtlinie für die Ressource auf *Alle zulassen* festgelegt wurde.
    - Überprüfen Sie, ob die Dienstdiagnose den Datenverkehr über die Endpunkte anzeigt.
    - Überprüfen Sie, ob die Flowprotokolle der Netzwerksicherheitsgruppen den Zugriff anzeigen und ob die Speicherprotokolle den Zugriff wie erwartet über Dienstendpunkte anzeigen.
    - Wenden Sie sich an den Azure-Support.
- Der Zugriff wird für Konten verweigert, die in den Richtlinien für Dienstendpunkte nicht aufgeführt sind.
  - Überprüfen Sie, ob Azure Storage so konfiguriert wurde, dass der Zugriff aus dem virtuellen Netzwerk über Endpunkte zugelassen ist. Überprüfen Sie außerdem, ob die Standardrichtlinie für die Ressource auf *Alle zulassen* festgelegt wurde.
  - Stellen Sie sicher, dass die Konten keine **klassischen Speicherkonten** mit Dienstendpunktrichtlinien im Subnetz sind.
- Ein verwalteter Azure-Dienst funktioniert nach dem Anwenden einer Dienstendpunktrichtlinie über das Subnetz nicht mehr.
  - Zu diesem Zeitpunkt werden verwaltete Dienste mit Dienstendpunktrichtlinien nicht unterstützt. *Künftige Aktualisierungen finden Sie hier*.

## <a name="provisioning"></a>Bereitstellung

Richtlinien für Dienstendpunkte können in Subnetzen von einem Benutzer konfiguriert werden, der über Schreibzugriff auf ein virtuelles Netzwerk verfügt. Erfahren Sie mehr über [integrierte Rollen](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) in Azure und das Zuweisen bestimmter Berechtigungen zu [benutzerdefinierten Rollen](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Virtuelle Netzwerke und Azure Storage-Konten können sich in demselben Abonnement oder in verschiedenen Abonnements oder aber in Azure Active Directory-Mandanten befinden.

## <a name="limitations"></a>Einschränkungen

- Sie können Richtlinien für Dienstendpunkte nur in virtuellen Netzwerken bereitstellen, die mit dem Azure Resource Manager-Bereitstellungsmodell bereitgestellt wurden.
- Virtuelle Netzwerke müssen sich in derselben Region wie die Richtlinie des Dienstendpunkts befinden.
- Sie können eine Richtlinie für Dienstendpunkte in einem Subnetz nur anwenden, wenn die Dienstendpunkte für die in der Richtlinie angegebenen Azure-Dienste konfiguriert sind.
- Sie können Richtlinien für Dienstendpunkte nicht für Datenverkehr aus Ihrem lokalen Netzwerk zu Azure-Diensten verwenden.
- Verwaltete Azure-Dienste unterstützen derzeit keine Endpunktrichtlinien. Dies umfasst verwaltete Dienste, die in den freigegebenen Subnetzen (z. B. *Azure HDInsight, Azure Batch, Azure ADDS, Azure Application Gateway, Azure-VPN-Gateway, Azure Firewall*) oder in den dedizierten Subnetzen (z. B. *Azure App Service-Umgebung, Azure Cache for Redis, Azure API Management, Azure SQL MI, klassische verwaltete Dienste*) bereitgestellt wurden.

 > [!WARNING]
 > Azure-Dienste, die in Ihrem virtuellen Netzwerk bereitgestellt wurden (z.B. Azure HDInsight), greifen aufgrund von Infrastrukturanforderungen auf andere Azure-Dienste zu, wie z.B. Azure Storage. Das Einschränken der Endpunktrichtlinie auf bestimmte Ressourcen könnten den Zugriff auf diese Infrastrukturressourcen für die Azure-Dienste unterbrechen, die in Ihrem virtuellen Netzwerk bereitgestellt wurden.

- Klassische Speicherkonten werden in Endpunktrichtlinien nicht unterstützt. Richtlinien verweigern standardmäßig den Zugriff auf alle klassischen Speicherkonten. Wenn Ihre Anwendung sowohl auf Azure Resource Manager-Speicherkonten als auch auf klassische Speicherkonten zugreifen muss, sollten für diesen Datenverkehr keine Endpunktrichtlinien verwendet werden.

## <a name="pricing-and-limits"></a>Preise und Einschränkungen

Für die Nutzung von Richtlinien für Dienstendpunkte fallen keine zusätzlichen Gebühren an. Das aktuelle Preismodell für Azure-Dienste (wie z.B. Azure Storage) gilt unverändert für alle Dienstendpunkte.

Folgende Grenzwerte gelten für Richtlinien für Dienstendpunkte: 

 |Resource | Standardlimit |
 |---------|---------------|
 |ServiceEndpointPoliciesPerSubscription |500 |
 |ServiceEndpintPoliciesPerSubnet|100 |
 |ServiceResourcesPerServiceEndpointPolicyDefinition|200 |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Konfigurieren von Richtlinien für VNET-Dienstendpunkte](virtual-network-service-endpoint-policies-portal.md).
- Erfahren Sie mehr über [VNET-Dienstendpunkte](virtual-network-service-endpoints-overview.md).
