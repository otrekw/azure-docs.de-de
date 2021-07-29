---
title: Automatische Skalierung für Azure API for FHIR
description: In diesem Artikel wird das Feature für die automatische Skalierung für Azure API for FHIR.
author: stevewohl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 07/26/2021
ms.author: zxue
ms.openlocfilehash: cc5fd2dd91c1edc39886938d91ebb54bcbc3ef0d
ms.sourcegitcommit: e6de87b42dc320a3a2939bf1249020e5508cba94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/27/2021
ms.locfileid: "114713378"
---
# <a name="autoscale-for-azure-api-for-fhir"></a>Automatische Skalierung für Azure API for FHIR 

Der Azure API for FHIR verwalteten Dienst ermöglicht Es Kunden, mit FHIR-konformen Gesundheitsdaten zu bleiben und diese sicher über die Dienst-API austauschen. Um unterschiedliche Transaktionsworkloads zu bewältigen, können Kunden manuelle Skalierung oder autoskalieren.

## <a name="what-is-autoscale"></a>Was ist die automatische Skalierung?

Standardmäßig ist die Azure API for FHIR auf manuelle Skalierung festgelegt. Diese Option funktioniert gut, wenn die Transaktionsworkloads bekannt und konsistent sind. Kunden können den Durchsatz über das Portal auf bis zu 10.000 anpassen und eine Anforderung zum `RU/s` Erhöhen des Grenzwerts übermitteln. 

Mit der automatischen Skalierung können Kunden verschiedene Workloads ausführen, und der Durchsatz wird ohne manuelle Anpassung `RU/s` automatisch hoch- und herunterskaliert.

## <a name="how-to-enable-autoscale"></a>Aktivieren der automatischen Skalierung

Sie können ein einmaliges Supportticket erstellen, um das Feature für die automatische Skalierung zu aktivieren. Das Microsoft-Supportteam aktiviert das Feature für die automatische Skalierung basierend auf der Supportpriorität.

> [!NOTE]
> Das Feature für die automatische Skalierung ist nicht im Azure-Portal.

## <a name="how-to-adjust-the-maximum-throughput-rus"></a>Wie passen Sie den maximalen Durchsatz von RU/s an?

Wenn die automatische Skalierung aktiviert ist, berechnet und legt das System den Anfangswert `Tmax` fest. Die Skalierbarkeit wird durch den maximalen Durchsatzwert oder bestimmt und wird zwischen `RU/s` `Tmax` `0.1 *Tmax` (oder 10 % `Tmax` ) und `Tmax RU/s` ausgeführt. 

Sie können die Maximale oder den `RU/s` Wert erhöhen und so hoch wie vom Dienst unterstützt `Tmax` werden. Wenn der Dienst ausgelastet ist, wird `RU/s` der Durchsatz auf den Wert `Tmax` hochskaliert. Wenn sich der Dienst im Leerlauf befindet, wird der Durchsatz auf einen Wert `RU/s` von 10 % `Tmax` herunterskaliert.
 
Sie können auch den Maximalwert oder `RU/s` den Wert `Tmax` verringern. Wenn Sie den maximalen Wert senken, ist der Mindestwert, den Sie festlegen können, : , gerundet auf den nächsten `RU/s` `MAX (4000, highest max RU/s ever provisioned / 10, current storage in GB * 400)` 1000 `RU/s` .

* **Beispiel 1:** Sie verfügen über 1 GB Daten, und die höchste bereitgestellte ist `RU/s` 10.000. Der Mindestwert ist Max (**4000**, 10.000/10, 1x400) = 4000. Die erste Zahl, **4000,** wird verwendet.
* **Beispiel 2:** Sie verfügen über 20 GB Daten, und die höchste bereitgestellte ist `RU/s` 100.000. Der Mindestwert ist Max (4000, **100.000/10,** 20x400) = 10.000. Die zweite Zahl, **100.000/10 =10.000,** wird verwendet.
* **Beispiel 3:** Sie verfügen über 80 GB Daten, und die höchste bereitgestellte RU/s beträgt 300.000. Der Mindestwert ist Max (4000, 300.000/10, **80x400**) = 32.000. Die dritte Zahl, **80x400=32.000,** wird verwendet.

Sie können den Höchstwert oder den Wert über das Portal anpassen, wenn es sich um eine gültige Zahl handelt und er nicht größer als `RU/s` `Tmax` 10.000 `RU/s` ist. Sie können ein Supportticket erstellen, um einen `Tmax` Wert von mehr als 10.000 an fordern.

## <a name="what-is-the-cost-impact-of-autoscale"></a>Welche Auswirkungen hat die automatische Skalierung auf die Kosten?

Das Feature für die automatische Skalierung verursacht Kosten, da die bereitgestellten Durchsatzeinheiten automatisch verwalten. Diese Kostensteigerung gilt nicht für Speicher- und Laufzeitkosten. Informationen zu den Preisen finden Sie unter [Azure API for FHIR Preise.](https://azure.microsoft.com/pricing/details/azure-api-for-fhir/)

>[!div class="nextstepaction"]
>[Informationen zur Azure-API für FHIR](overview.md)
