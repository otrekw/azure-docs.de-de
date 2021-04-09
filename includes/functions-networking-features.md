---
ms.openlocfilehash: 2e92d150851c74a84f785d1f5f0ebe2e5870a54e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "97936758"
---


| Funktion |[Verbrauchstarif](../articles/azure-functions/consumption-plan.md)|[Premium-Plan](../articles/azure-functions/functions-premium-plan.md)|[Dedizierter Plan](../articles/azure-functions/dedicated-plan.md)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[IP-Einschränkungen für eingehenden Datenverkehr und Zugriff auf private Sites](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Ja|✅Ja|✅Ja|✅Ja|✅Ja|
|[Integration in ein virtuelles Netzwerk](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌Nein|✅Ja (regional)|✅Ja (regional und Gateway)|✅Ja| ✅Ja|
|[Trigger für virtuelle Netzwerke (nicht HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌Nein| ✅Ja |✅Ja|✅Ja|✅Ja|
|[Hybridverbindungen](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (nur Windows)|❌Nein|✅Ja|✅Ja|✅Ja|✅Ja|
|[IP-Einschränkungen für ausgehenden Datenverkehr](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌Nein| ✅Ja|✅Ja|✅Ja|✅Ja|