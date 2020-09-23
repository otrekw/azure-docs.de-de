---
title: Bereitstellen von Feedback zu Anomalien an den Metrics Advisor-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie Feedback zu Anomalien, die von Ihrer Metrics Advisor-Instanz gefunden wurden, senden und die Ergebnisse optimieren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: metrics-advisor
ms.topic: conceptual
ms.date: 09/10/2020
ms.author: aahi
ms.openlocfilehash: 50d422edf1a4b45132d0b86eac9d4947cef5e5bf
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90930487"
---
# <a name="adjust-anomaly-detection-using-feedback"></a>Anpassen der Anomalieerkennung anhand von Feedback

Wenn Sie mit einigen der von Metrics Monitor bereitgestellten Ergebnisse der Anomalieerkennung nicht zufrieden sind, können Sie manuell Feedback hinzufügen, das sich auf das auf Ihre Daten angewandte Modell auswirkt. 

Verwenden Sie die Schaltflächen in der rechten oberen Ecke der Seite, um den Anmerkungsmodus für Feedback zu aktivieren.

:::image type="content" source="../media/feedback/annotation-mode.png" alt-text="Anmerkungsmodus für Feedback":::

Nachdem der Anmerkungsmodus für Feedback aktiviert wurde, können Sie Feedback für einen Punkt oder mehrere fortlaufende Punkte angeben.

## <a name="give-feedback-for-one-point"></a>Bereitstellen von Feedback zu einem Punkt 

Klicken Sie bei aktiviertem Anmerkungsmodus für Feedback auf einen Punkt, um das Panel **Feedback hinzufügen** zu öffnen. Sie können den Typ des Feedbacks festlegen, das Sie angeben möchten. Dieses Feedback wird bei der Erkennung für zukünftige Punkte berücksichtigt.  

* Wählen Sie **Anomalie** aus, wenn Sie der Ansicht sind, dass der Punkt fälschlicherweise von Metrics Monitor markiert wurde. Sie können angeben, ob ein Punkt als Anomalie eingestuft werden soll. 
* Wählen Sie **Änderungspunkt** aus, wenn Sie der Ansicht sind, dass der Punkt den Anfang einer Trendänderung anzeigt.
* Wählen Sie **Zeitraum** aus, um Saisonalität anzuzeigen. Metrics Monitor kann saisonale Zeiträume automatisch erkennen, Sie können dies aber auch manuell angeben. 

Sie können einen Kommentar dazu im Textfeld **Kommentar** angeben. Klicken Sie auf **Speichern**, um Ihr Feedback zu speichern.

:::image type="content" source="../media/feedback/feedback-menu.png" alt-text="Menü „Feedback“":::

## <a name="give-feedback-for-multiple-continuous-points"></a>Bereitstellen von Feedback zu mehreren fortlaufenden Punkten

Sie können Feedback zu mehreren fortlaufenden Punkten gleichzeitig angeben, indem Sie die linke Maustaste gedrückt halten und den Mauszeiger über die Punkte ziehen, die Sie mit Anmerkungen versehen möchten. Es wird das gleiche Menü „Feedback“ wie oben angezeigt. Wenn Sie auf **Speichern** klicken, wird dasselbe Feedback auf alle ausgewählten Punkte angewandt.

:::image type="content" source="../media/feedback/continuous-points.png" alt-text="Auswählen mehrerer Punkte":::

## <a name="how-to-view-my-feedback"></a>Anzeigen des eigenen Feedbacks

Um festzustellen, ob die Anomalieerkennung eines Punkts geändert wurde, zeigen Sie auf den Punkt. Die QuickInfo zeigt **Affected by feedback: true** (Von Feedback betroffen: true) an, wenn die Erkennung geändert wurde. Wenn **false** angezeigt wird, wurde zwar die Feedbackberechnung an dem Punkt ausgeführt, aber das Ergebnis der Anomalieerkennung wurde nicht geändert.

:::image type="content" source="../media/feedback/affected-point.png" alt-text="Von Feedback betroffener Punkt":::

## <a name="when-should-i-annotate-an-anomaly-as-normal"></a>Wann sollte ich eine Anomalie als „normal“ kennzeichnen?

Es gibt viele Gründe, warum eine Anomalie als falscher Alarm eingestuft werden kann. Verwenden Sie die folgenden Features von Metrics Advisor, wenn eines der folgenden Szenarien zutrifft:


|Szenario  |Empfehlung |
|---------|---------|
|Die Anomalie wird durch eine bekannte Datenquellenänderung (z. B. eine Systemänderung) verursacht.     | Fügen Sie für diese Anomalieerkennung keine Anmerkung hinzu, wenn Sie davon ausgehen, dass dieses Szenario nur selten auftritt.        |
|Die Anomalie wird durch einen Feiertag verursacht.     | Verwenden Sie [Preset events](configure-metrics.md#preset-events) (Vordefinierte Ereignisse) zum Markieren der Anomalieerkennung zu bestimmten Zeiten.       |
|Es gibt ein reguläres Muster für das Erkennen von Anomalien (z. B. an Wochenenden), die nicht als solche eingestuft werden sollten.      |Verwenden Sie die Feedbackfunktion oder vordefinierte Ereignisse.        |

## <a name="next-steps"></a>Nächste Schritte
- [Diagnostizieren eines Incidents](diagnose-incident.md)
- [Konfigurieren von Metriken und Optimieren der Konfiguration](configure-metrics.md)
- [Konfigurieren von Warnungen und Erhalten von Benachrichtigungen per Hook](../how-tos/alerts.md)