---
title: Übersicht über allgemeine Muster zur automatischen Skalierung
description: Lernen Sie allgemeine Muster zur automatischen Skalierung Ihrer Ressource in Azure kennen.
ms.topic: conceptual
ms.date: 05/07/2017
ms.subservice: autoscale
ms.openlocfilehash: adbfecd05bfd4330e7c7c972cfdb9fe7021eec31
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87073499"
---
# <a name="overview-of-common-autoscale-patterns"></a>Übersicht über allgemeine Muster zur automatischen Skalierung
In diesem Artikel werden allgemeine Muster zur automatischen Skalierung Ihrer Ressource in Azure beschrieben.

Die automatische Skalierung von Azure Monitor gilt nur für [VM.Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Clouddienste](https://azure.microsoft.com/services/cloud-services/), [App Service – Web-Apps](https://azure.microsoft.com/services/app-service/web/) und [API Management-Dienste](../../api-management/api-management-key-concepts.md).

## <a name="lets-get-started"></a>Erste Schritte

In diesem Artikel wird davon ausgegangen, dass Sie mit der automatischen Skalierung vertraut sind. Sie können [als Einstieg hier mit der Skalierung Ihrer Ressource beginnen][1]. Im Folgenden werden einige gängige Skalierungsmuster aufgeführt.

## <a name="scale-based-on-cpu"></a>Skalieren basierend auf der CPU

Sie haben eine Web-App (/VMSS/Clouddienstrolle) und möchten Folgendes durchführen:

- Sie möchten basierend auf der CPU auf- bzw. abskalieren.
- Darüber hinaus möchten Sie sicherstellen, dass eine Mindestanzahl von Instanzen vorhanden ist.
- Des Weiteren möchten Sie einen maximalen Grenzwert für die Anzahl der Instanzen, die skaliert werden können, festlegen.

![Skalieren basierend auf der CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Individuelles Skalieren an Wochentagen ggü. Wochenenden

Sie haben eine Web-App (/VMSS/Clouddienstrolle) und möchten Folgendes durchführen:

- Sie möchten, dass (an Wochenenden) standardmäßig 3 Instanzen vorhanden sind.
- Sie rechnen nicht mit Datenverkehr an Wochenenden, und möchten an Wochenenden daher auf 1 Instanz zentral herunterskalieren.

![Individuelles Skalieren an Wochentagen ggü. Wochenenden][3]

## <a name="scale-differently-during-holidays"></a>Individuelles Skalieren an Feiertagen

Sie haben eine Web-App (/VMSS/Clouddienstrolle) und möchten Folgendes durchführen:

- Sie möchten standardmäßig basierend auf der CPU-Auslastung hoch- bzw. herunterskalieren.
- Allerdings möchten Sie die Standardwerte während der Feiertage (oder an bestimmten für Ihr Unternehmen wichtigen Tagen) überschreiben und über größere Kapazitäten verfügen.

![Individuelles Skalieren an Feiertagen][4]

## <a name="scale-based-on-custom-metric"></a>Skalieren basierend auf benutzerdefinierten Metriken

Sie verfügen über ein Web-Front-End und eine API-Ebene, die mit dem Back-End kommuniziert.

- Sie möchten die API-Ebene basierend auf benutzerdefinierten Ereignissen im Front-End skalieren (Beispiel: Sie möchten Ihren Bezahlvorgang basierend auf der Anzahl der Artikel im Warenkorb skalieren.).

![Skalieren basierend auf benutzerdefinierten Metriken][5]

<!--Reference-->
[1]: ./autoscale-get-started.md
[2]: ./media/autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/autoscale-common-scale-patterns/custom-metric-scale.png
