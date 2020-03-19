---
title: Aktualisierung des Azure Traffic Analytics-Schemas – März 2020 | Microsoft-Dokumentation
description: Hier finden Sie Beispielabfragen mit neuen Feldern im Traffic Analytics-Schema.
services: network-watcher
documentationcenter: na
author: vinigam
manager: agummadi
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/06/2020
ms.author: vinigam
ms.openlocfilehash: 0e9d37e3a89473e59b94168f8f8c80e7a6621107
ms.sourcegitcommit: 5f39f60c4ae33b20156529a765b8f8c04f181143
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "78969057"
---
# <a name="sample-queries-with-new-fields-in-traffic-analytics-schema-august-2019-schema-update"></a>Beispielabfragen mit neuen Feldern im Traffic Analytics-Schema (Schemaaktualisierung vom August 2019)

Das [Traffic Analytics-Protokollschema](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-schema) wurde aktualisiert und enthält nun die folgenden neuen Felder: **SrcPublicIPs_s** , **DestPublicIPs_s**, **NSGRule_s**. In den nächsten Monaten werden die folgenden veralteten Felder eingestellt: **VMIP_s**, **Subscription_g**, **Region_s**, **NSGRules_s**, **Subnet_s**, **VM_s**, **NIC_s**, **PublicIPs_s**, **FlowCount_d**.
Die neuen Felder enthalten Informationen zu Quell- und Ziel-IPs und vereinfachen Abfragen.

In den folgenden drei Beispielen wird gezeigt, wie die alten Felder durch neue ersetzt werden.

## <a name="example-1---vmip_s-subscription_g-region_s-subnet_s-vm_s-nic_s-publicips_s"></a>Beispiel 1: VMIP_s, Subscription_g, Region_s, Subnet_s, VM_s, NIC_s, PublicIPs_s

Aus dem Feld „FlowDirection_s“ für AzurePublic- und ExternalPublic-Flows müssen keine Quell- und Zielfälle speziell für AzurePublic- und ExternalPublic-Flows abgeleitet werden. Im Fall eines virtuellen Netzwerkgeräts (Network Virtual Appliance, NVA) kann auch die Verwendung des Felds „FlowDirection_s“ unzulässig sein.

```Old Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "1"
| extend isAzureOrExternalPublicFlows = FlowType_s in ("AzurePublic", "ExternalPublic")
| extend SourceAzureVM = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', VM_s, "N/A"), VM1_s),
SourceAzureVMIP = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', VM_s, "N/A"), SrcIP_s),
SourceAzureVMSubscription = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Subscription_g, "N/A"), Subscription1_g),
SourceAzureRegion = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Region_s, "N/A"), Region1_s),
SourceAzureSubnet = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', Subnet_s, "N/A"), Subnet1_s),
SourceAzureNIC = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'O', NIC_s, "N/A"), NIC1_s),
DestAzureVM = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', VM_s, "N/A"), VM2_s),
DestAzureVMIP = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', VM_s, "N/A"), DestIP_s),
DestAzureVMSubscription = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Subscription_g, "N/A"), Subscription2_g),
DestAzureRegion = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Region_s, "N/A"), Region2_s),
DestAzureSubnet = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', Subnet_s, "N/A"), Subnet2_s),
DestAzureNIC = iif(isAzureOrExternalPublicFlows, iif(FlowDirection_s == 'I', NIC_s, "N/A"), NIC2_s),
SourcePublicIPsAggregated = iif(isAzureOrExternalPublicFlows and FlowDirection_s == 'I', PublicIPs_s, "N/A"),
DestPublicIPsAggregated = iif(isAzureOrExternalPublicFlows and FlowDirection_s == 'O', PublicIPs_s, "N/A")
```


```New Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "2"
| extend SourceAzureVM = iif(isnotempty(VM1_s), VM1_s, "N/A"),
SourceAzureVMIP = iif(isnotempty(SrcIP_s), SrcIP_s, "N/A"),
SourceAzureVMSubscription = iif(isnotempty(Subscription1_g), Subscription1_g, "N/A"),
SourceAzureRegion = iif(isnotempty(Region1_s), Region1_s, "N/A"),
SourceAzureSubnet = iif(isnotempty(Subnet1_s), Subnet1_s, "N/A"),
SourceAzureNIC = iif(isnotempty(NIC1_s), NIC1_s, "N/A"),
DestAzureVM = iif(isnotempty(VM2_s), VM2_s, "N/A"),
DestAzureVMIP = iif(isnotempty(DestIP_s), DestIP_s, "N/A"),
DestAzureVMSubscription = iif(isnotempty(Subscription2_g), Subscription2_g, "N/A"),
DestAzureRegion = iif(isnotempty(Region2_s), Region2_s, "N/A"),
DestAzureSubnet = iif(isnotempty(Subnet2_s), Subnet2_s, "N/A"),
DestAzureNIC = iif(isnotempty(NIC2_s), NIC2_s, "N/A"),
SourcePublicIPsAggregated = iif(isnotempty(SrcPublicIPs_s), SrcPublicIPs_s, "N/A"),
DestPublicIPsAggregated = iif(isnotempty(DestPublicIPs_s), DestPublicIPs_s, "N/A")
```


## <a name="example-2---nsgrules_s"></a>Beispiel 2: NSGRules_s

Das frühere Feld hatte das Format <Indexwert 0)>|<NSG_RULENAME>|<Flow Direction>|<Flow Status>|<FlowCount ProcessedByRule>

Früher wurden Daten übergreifend für NSG und NSGRules aggregiert. Jetzt erfolgt keine Aggregation. Daher enthält „NSGList_s“ nur eine NSG, und auch „NSGRules_s“ enthielt gewöhnlich nur eine Regel. Deshalb wurde hier die komplizierte Formatierung entfernt. Dies gilt auch für andere Felder wie nachfolgend gezeigt:

```Old Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "1"
| extend NSGRuleComponents = split(NSGRules_s, "|")
| extend NSGName = NSGList_s // remains same
| extend NSGRuleName = NSGRuleComponents[1],
         FlowDirection = NSGRuleComponents[2],
         FlowStatus = NSGRuleComponents[3],
         FlowCountProcessedByRule = NSGRuleComponents[4]
| project NSGName, NSGRuleName, FlowDirection, FlowStatus, FlowCountProcessedByRule
```

```New Kusto query
AzureNetworkAnalytics_CL
| where SubType_s == "FlowLog" and FASchemaVersion_s == "2"
| extend NSGRuleComponents = split(NSGRules_s, "|")
| project NSGName = NSGList_s,
NSGRuleName = NSGRule_s ,
FlowDirection = FlowDirection_s,
FlowStatus = FlowStatus_s,
FlowCountProcessedByRule = AllowedInFlows_d + DeniedInFlows_d + AllowedOutFlows_d + DeniedOutFlows_d
```

## <a name="example-3---flowcount_d"></a>Example 3: FlowCount_d

Da Daten nicht übergreifend für NSG zusammengefasst werden, ist „FlowCount_d“ einfach „AllowedInFlows_d“ + „DeniedInFlows_d“ + „AllowedOutFlows_d“ + „DeniedOutFlows_d“.
Nur einer der oben genannten vier Werte ist ungleich 0 (null), und die restlichen drei sind 0. Außerdem wird der Status und die Anzahl an der NIC angegeben, an der der Flow erfasst wurde.

Wenn der Flow zulässig war, wird eines der Felder mit dem Präfix „Allowed" aufgefüllt. Andernfalls wird ein Feld mit dem Präfix „Denied“ aufgefüllt.
Wenn es ein eingehender Flow war, wird eines der Felder mit dem Suffix „\_d“ (z. B. das Feld „InFlows_d“) aufgefüllt. Andernfalls wird „OutFlows_d“ aufgefüllt.

Abhängig von den beiden oben genannten Bedingungen ist bekannt, welches der vier Felder aufgefüllt wird.


## <a name="next-steps"></a>Nächste Schritte
Antworten auf häufig gestellte Fragen finden Sie in den [FAQ zu Traffic Analytics](traffic-analytics-faq.md). Weitere Informationen zur Funktionalität finden Sie in der [Dokumentation zu Traffic Analytics](traffic-analytics.md).
