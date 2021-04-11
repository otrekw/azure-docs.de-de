---
title: Windows Virtual Desktop – Verbindungswartezeit für Benutzer – Azure
description: Verbindungswartezeit für Benutzer von Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: helohr
manager: femila
ms.openlocfilehash: 2fa03fa948c7768f5e74cc68b528e847c5351273
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106447853"
---
# <a name="determine-user-connection-latency-in-windows-virtual-desktop"></a>Ermitteln der Verbindungswartezeit für Benutzer in Windows Virtual Desktop

Windows Virtual Desktop ist global verfügbar. Administratoren können virtuelle Computer (Virtual Machines, VMS) in jeder gewünschten Azure-Region erstellen. Die Verbindungswartezeit hängt vom Standort der Benutzer und der virtuellen Computer ab. Windows Virtual Desktop-Dienste werden fortlaufend in neuen geografischen Regionen eingeführt, um die Wartezeit zu verbessern.

Mit dem [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/) können Sie den besten Standort zum Optimieren der Wartezeit Ihrer virtuellen Computer ermitteln. Es wird empfohlen, das Tool alle zwei bis drei Monate zu verwenden, um sicherzustellen, dass sich der optimale Standort nicht geändert hat, während Windows Virtual Desktop in neuen Bereichen eingeführt wird.

## <a name="interpreting-results-from-the-windows-virtual-desktop-experience-estimator-tool"></a>Interpretieren der Ergebnisse vom Windows Virtual Desktop-Qualitätsschätzer

Bei Windows Virtual Desktop sollten Latenzen bis zu 150 ms keine Auswirkungen auf die Benutzererfahrung haben, wenn diese weder Rendering noch Videos umfasst. Latenzen zwischen 150 ms und 200 ms sollten für Textverarbeitung ausreichen. Eine Latenz von mehr als 200 ms kann die Benutzererfahrung beeinträchtigen. 

Darüber hinaus ist die Windows Virtual Desktop-Verbindung von der Internetverbindung des Computers abhängig, auf dem der Benutzer den Dienst verwendet. Die Verbindung eines Benutzers kann in einer der folgenden Situationen eventuell getrennt oder verlangsamt werden:

 - Der Benutzer verfügt nicht über eine stabile lokale Internetverbindung, und die Latenz beträgt mehr als 200 ms.
 - Das Netzwerk ist überlastet oder ratenbeschränkt.

Es wird empfohlen, VM-Standorte so nah wie möglich an Ihren Benutzern auszuwählen. Wenn sich die Benutzer beispielsweise in Indien befinden, die VM jedoch in den USA ausgeführt wird, kann die entstehende Latenz die gesamte Benutzererfahrung beeinträchtigen. 

## <a name="azure-front-door"></a>Azure Front Door

Windows Virtual Desktop verwendet [Azure Front Door](https://azure.microsoft.com/services/frontdoor/), um Benutzerverbindungen basierend auf der Quell-IP-Adresse an das nächste Windows Virtual Desktop-Gateway umzuleiten. Windows Virtual Desktop verwendet immer das vom Client ausgewählte Windows Virtual Desktop-Gateway.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum besten Standort für eine optimale Wartezeit finden Sie unter [Tool „Einschätzung der Servicequalität für Windows Virtual Desktop“](https://azure.microsoft.com/services/virtual-desktop/assessment/).
- Tarife finden Sie unter [Windows Virtual Desktop – Preise](https://azure.microsoft.com/pricing/details/virtual-desktop/).
- Die ersten Schritte mit Ihrer Windows Virtual Desktop-Bereitstellung finden Sie in [unserem Tutorial](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-azure-marketplace).
