---
title: Azure Virtual Desktop – Verbindungswartezeit für Benutzer – Azure
description: Verbindungswartezeit für Benutzer von Azure Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: femila
ms.openlocfilehash: ba4db9048154af2562d2c3b7431d2a75ba3c036f
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111747381"
---
# <a name="determine-user-connection-latency-in-azure-virtual-desktop"></a>Ermitteln der Verbindungswartezeit für Benutzer in Azure Virtual Desktop

Azure Virtual Desktop ist global verfügbar. Administratoren können virtuelle Computer (Virtual Machines, VMS) in jeder gewünschten Azure-Region erstellen. Die Verbindungswartezeit hängt vom Standort der Benutzer und der virtuellen Computer ab. Azure Virtual Desktop-Dienste werden fortlaufend in neuen geografischen Regionen eingeführt, um die Wartezeit zu verbessern.

Mit dem [Tool „Einschätzung der Servicequalität für Azure Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/) können Sie den besten Standort zum Optimieren der Wartezeit Ihrer virtuellen Computer ermitteln. Es wird empfohlen, das Tool alle zwei bis drei Monate zu verwenden, um sicherzustellen, dass sich der optimale Standort nicht geändert hat, während Azure Virtual Desktop in neuen Bereichen eingeführt wird.

## <a name="interpreting-results-from-the-azure-virtual-desktop-experience-estimator-tool"></a>Interpretieren der Ergebnisse vom Azure Virtual Desktop-Qualitätsschätzer

Bei Azure Virtual Desktop sollten Latenzen bis zu 150 ms keine Auswirkungen auf die Benutzererfahrung haben, wenn diese weder Rendering noch Videos umfasst. Latenzen zwischen 150 ms und 200 ms sollten für Textverarbeitung ausreichen. Eine Latenz von mehr als 200 ms kann die Benutzererfahrung beeinträchtigen. 

Darüber hinaus ist die Azure Virtual Desktop-Verbindung von der Internetverbindung des Computers abhängig, auf dem der Benutzer den Dienst verwendet. Die Verbindung eines Benutzers kann in einer der folgenden Situationen eventuell getrennt oder verlangsamt werden:

 - Der Benutzer verfügt nicht über eine stabile lokale Internetverbindung, und die Latenz beträgt mehr als 200 ms.
 - Das Netzwerk ist überlastet oder ratenbeschränkt.

Es wird empfohlen, VM-Standorte so nah wie möglich an Ihren Benutzern auszuwählen. Wenn sich die Benutzer beispielsweise in Indien befinden, die VM jedoch in den USA ausgeführt wird, kann die entstehende Latenz die gesamte Benutzererfahrung beeinträchtigen. 

## <a name="azure-front-door"></a>Azure Front Door

Azure Virtual Desktop verwendet [Azure Front Door](https://azure.microsoft.com/services/frontdoor/), um Benutzerverbindungen basierend auf der Quell-IP-Adresse an das nächste Azure Virtual Desktop-Gateway umzuleiten. Azure Virtual Desktop verwendet immer das vom Client ausgewählte Azure Virtual Desktop-Gateway.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum besten Standort für eine optimale Wartezeit finden Sie unter [Tool „Einschätzung der Servicequalität für Azure Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Tarife finden Sie unter [Azure Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Die ersten Schritte mit Ihrer Azure Virtual Desktop-Bereitstellung finden Sie in [unserem Tutorial](./create-host-pools-azure-marketplace.md).