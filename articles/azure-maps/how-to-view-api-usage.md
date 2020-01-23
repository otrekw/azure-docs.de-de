---
title: Anzeigen der Azure Maps-API-Nutzungsmetriken | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie die Metriken für Ihre Microsoft Azure Maps-API-Aufrufe im Azure-Portal anzeigen.
author: walsehgal
ms.author: v-musehg
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 7ba50f63fb015a8696904df3decd13c811625459
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911334"
---
# <a name="view-azure-maps-api-usage-metrics"></a>Anzeigen der Azure Maps-API-Nutzungsmetriken

In diesem Artikel erfahren Sie, wie Sie die API-Nutzungsmetriken für Ihr Azure Maps-Konto im [Portal](https://portal.azure.com) anzeigen. Die Metriken werden in einem benutzerfreundlichen Diagrammformat für einen anpassbaren Zeitraum angezeigt.

## <a name="view-metric-snapshot"></a>Anzeigen der Metrikenmomentaufnahme

Sie sehen einige gängige Metriken auf der **Übersicht**-Seite Ihres Maps-Kontos. Derzeit werden *Anforderungen gesamt*, *Fehler gesamt*  und *Verfügbarkeit* für einen auswählbaren Zeitraum angezeigt.

![Übersicht zu Nutzungsmetriken von Azure Maps](media/how-to-view-api-usage/portal-overview.png)

Fahren Sie mit dem nächsten Abschnitt fort, wenn Sie diese Diagramme für Ihre spezielle Analyse anpassen müssen.

## <a name="view-detailed-metrics"></a>Anzeigen detaillierter Metriken

1. Melden Sie sich bei Ihrem Azure-Abonnement im [Portal](https://portal.azure.com) an.

2. Klicken Sie auf der linken Seite auf die Menüoption **Alle Ressourcen**, und navigieren Sie zu Ihrem *Azure Maps-Konto*.

3. Sobald Ihr Maps-Konto geöffnet ist, klicken Sie auf der linken Seite auf das Menü **Metriken**.

4. Wählen Sie im Bereich **Metriken** eine der folgenden Optionen aus:

   1. **Verfügbarkeit**, womit der *Durchschnitt* der API-Verfügbarkeit über einen bestimmten Zeitraum angezeigt wird.
   2. **Nutzung**, die Ihnen die *Anzahl* der Nutzungen für Ihr Konto anzeigt.

      ![Bereich für Nutzungsmetriken von Azure Maps](media/how-to-view-api-usage/portal-metrics.png)

5. Als Nächstes können Sie den *Zeitbereich* wählen, indem Sie auf **Letzte 24 Stunden (automatisch)** klicken. Standardmäßig ist der Zeitraum auf 24 Stunden festgelegt. Nach dem Klicken sehen Sie alle auswählbaren Zeitbereiche. Sie können auch die *Zeitgranularität* auswählen und in der gleichen Dropdownliste wählen, ob Sie die *lokale* oder *GMT*-Uhrzeit anzeigen möchten. Klicken Sie auf **Anwenden**.

    ![Zeitbereich für Metriken von Azure Maps](media/how-to-view-api-usage/time-range.png)

6. Sobald Sie Ihre Metrik hinzufügen, können Sie aus den für die Metrik relevanten Eigenschaften einen **Filter hinzufügen** und dann den Wert der Eigenschaft auswählen, für den Sie das Diagramm anzeigen möchten.

    ![Filter für Nutzungsmetriken von Azure Maps](media/how-to-view-api-usage/filter.png)

7. Sie können auch basierend auf Ihrer ausgewählten Metrikeigenschaft die **Aufteilung für Ihre Metrik anwenden**. Dadurch wird das Diagramm in mehrere Diagramme aufgeteilt, jeweils eins für jeden Wert dieser Eigenschaft. In der folgenden Abbildung entspricht die Farbe jedes Diagramms dem am unteren Rand des Diagramms angezeigten Eigenschaftswert.

    ![Aufteilung der Nutzungsmetriken von Azure Maps](media/how-to-view-api-usage/splitting.png)

8. Um mehrere Metriken im gleichen Diagramm zu beobachten, klicken Sie einfach oben auf die Schaltfläche **Metrik hinzufügen**.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Azure Maps-APIs, für die Sie die Nutzung verfolgen möchten:
> [!div class="nextstepaction"] 
> [Azure Maps Web SDK: Anleitung](how-to-use-map-control.md)

> [!div class="nextstepaction"] 
> [Azure Maps Android SDK: Anleitung](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Dokumentation zur REST API von Azure Maps](https://docs.microsoft.com/rest/api/maps)
