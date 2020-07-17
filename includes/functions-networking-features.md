---
ms.openlocfilehash: 8d5c3fff2646536ffe5c593e13ccdab23397d44b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84906713"
---


| Funktion |[Verbrauchstarif](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium-Plan](../articles/azure-functions/functions-scale.md#premium-plan)|[Dedizierter Plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[IP-Einschränkungen für eingehenden Datenverkehr und Zugriff auf private Sites](../articles/azure-functions/functions-networking-options.md#inbound-ip-restrictions)|✅Ja|✅Ja|✅Ja|✅Ja|✅Ja|
|[Integration in ein virtuelles Netzwerk](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌Nein|✅Ja (regional)|✅Ja (regional und Gateway)|✅Ja| ✅Ja|
|[Trigger für virtuelle Netzwerke (nicht HTTP)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌Nein| ✅Ja |✅Ja|✅Ja|✅Ja|
|[Hybridverbindungen](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (nur Windows)|❌Nein|✅Ja|✅Ja|✅Ja|✅Ja|
|[IP-Einschränkungen für ausgehenden Datenverkehr](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌Nein| ✅Ja|✅Ja|✅Ja|✅Ja|