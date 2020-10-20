---
title: 'Schnellstart: Beschaffen einer Telefonnummer über Azure Communication Services'
description: Es wird beschrieben, wie Sie über das Azure-Portal eine Communication Services-Telefonnummer erwerben.
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 10/05/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.custom: references_regions
ms.openlocfilehash: a9c33fad6d376c2f568f4eefbb0379486b5c40f3
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044767"
---
# <a name="quickstart-get-a-phone-number-using-the-azure-portal"></a>Schnellstart: Beschaffen einer Telefonnummer über das Azure-Portal

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]

Steigen Sie in Azure Communication Services ein, indem Sie über das Azure-Portal eine Telefonnummer erwerben.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Eine aktive Communication Services-Ressource.](../create-communication-resource.md)

## <a name="get-a-phone-number"></a>Beschaffen einer Telefonnummer

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Communication Services-Ressource, um mit der Bereitstellung von Telefonnummern zu beginnen.

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

### <a name="getting-new-phone-numbers"></a>Beschaffen von neuen Telefonnummern

Navigieren Sie im Ressourcenmenü zum Blatt „Telefonnummern“.

:::image type="content" source="../media/manage-phone-azure-portal-phone-page.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

Klicken Sie auf die Schaltfläche `Get` (Abrufen), um den Assistenten zu starten. Im Assistenten auf dem Blatt `Phone numbers` (Telefonnummern) werden einige Fragen gestellt, um die Telefonnummer zu ermitteln, die für Ihr Szenario am besten geeignet ist. 

Zuerst müssen Sie eine Auswahl für `Country/region` (Land/Region) treffen, um anzugeben, wo die Telefonnummer bereitgestellt werden soll. Nach der Auswahl des Lands bzw. der Region müssen Sie den `use case` (Telefonplan) auswählen, der Ihre Anforderungen am besten erfüllt. 

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

### <a name="select-a-phone-plan"></a>Auswählen eines Telefonplans

Die Auswahl eines Telefonplans ist in zwei Schritte unterteilt: 

1. Auswahl des [Nummerntyps](../../concepts/telephony-sms/plan-solution.md#phone-number-types-in-azure-communication-services)
2. Auswahl des [Plans](../../concepts/telephony-sms/plan-solution.md#phone-number-plans-in-azure-communication-services)

Derzeit gibt es zwei Nummerntypen: `Geographic` (Geografisch) und `Toll-free` (Gebührenfrei). Nachdem Sie einen Nummerntyp ausgewählt haben, werden Ihnen mehrere Pläne zur Auswahl angeboten.

In unserem Beispiel haben wir den Nummerntyp `Toll-free` und den Plänen `Outbound calling` und `Inbound and Outbound SMS` ausgewählt.

:::image type="content" source="../media/manage-phone-azure-portal-select-plans.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

Klicken Sie als Nächstes unten auf der Seite auf die Schaltfläche `Next: Numbers` (Weiter: Nummern), um die Telefonnummern anzupassen, die Sie bereitstellen möchten.

### <a name="customizing-phone-numbers"></a>Anpassen von Telefonnummern

Auf der Seite `Numbers` (Nummern) passen Sie die Telefonnummern an, die Sie bereitstellen möchten.

:::image type="content" source="../media/manage-phone-azure-portal-select-numbers-start.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

> [!NOTE]
> In dieser Schnellstartanleitung wird der Anpassungsvorgang für den Nummerntyp `Toll-free` (Gebührenfrei) veranschaulicht. Der Vorgang weicht etwas ab, wenn Sie den Nummerntyp `Geographic` (Geografisch) ausgewählt haben, aber das Endergebnis ist identisch.

Treffen Sie in der Liste mit den verfügbaren Ortsvorwahlen eine Auswahl für `Area code` (Ortsvorwahl), und geben Sie die Menge an, die Sie bereitstellen möchten. Klicken Sie anschließend auf `Search` (Suchen), um nach Nummern zu suchen, die Ihre ausgewählten Anforderungen erfüllen. Die Telefonnummern, die Ihre Anforderungen erfüllen, werden zusammen mit den monatlichen Kosten angezeigt.

:::image type="content" source="../media/manage-phone-azure-portal-found-numbers.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

> [!NOTE]
> Die Verfügbarkeit hängt davon ab, welchen Nummerntyp, Standort und Plan Sie ausgewählt haben.
> Die Nummern werden im Zeitraum bis zum Ablauf der Transaktion für kurze Zeit reserviert. Wenn die Transaktion abläuft, müssen Sie die Nummern erneut auswählen.

Klicken Sie am unteren Rand der Seite auf die Schaltfläche `Next: Summary` (Weiter: Zusammenfassung), um eine Zusammenfassung des Kaufs anzuzeigen und Ihre Bestellung aufzugeben.

### <a name="place-order"></a>Aufgeben der Bestellung

Auf der Seite mit der Zusammenfassung werden noch einmal der Nummerntyp, die Features, die Telefonnummern und die monatlichen Gesamtkosten für die Bereitstellung der Telefonnummern angezeigt.

> [!NOTE]
> Bei den angezeigten Preisen handelt es sich um die **monatlichen Gebühren**. Dies sind die Kosten für das Leasen der ausgewählten Telefonnummer an Sie. Nicht in dieser Anzeige enthalten sind die **Kosten für die nutzungsbasierte Bezahlung**, die beim Tätigen bzw. Empfangen von Anrufen anfallen. Die Preislisten finden Sie [hier](../../concepts/pricing.md). Diese Kosten richten sich nach dem Nummerntyp und den angerufenen Zielen. Die Preise pro Minute für einen Anruf von einer Nummer in der Region Seattle bei einer Nummer in der Region New York und für einen Anruf von derselben Nummer bei einer Mobiltelefonnummer im Vereinigten Königreich können sich beispielsweise unterscheiden.

Klicken Sie abschließend unten auf der Seite auf `Place order` (Bestellen), um die Bestellung zu bestätigen.

:::image type="content" source="../media/manage-phone-azure-portal-get-numbers-summary.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

## <a name="find-your-phone-numbers-on-the-azure-portal"></a>Suchen nach Ihren Telefonnummern im Azure-Portal

Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Azure Communication Services-Ressource:

:::image type="content" source="../media/manage-phone-azure-portal-start.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

Wählen Sie im Menü das Blatt „Telefonnummern“ aus, um Ihre Telefonnummern zu verwalten.

:::image type="content" source="../media/manage-phone-azure-portal-phones.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

> [!NOTE]
> Es kann einige Minuten dauern, bis die bereitgestellten Nummern auf dieser Seite angezeigt werden.

### <a name="customizing-phone-number-features"></a>Anpassen von Features für Telefonnummern
Auf der Seite `Numbers` können Sie die Telefonnummer auswählen, indem Sie auf die Nummer klicken, für die Sie den Plan anpassen möchten.

:::image type="content" source="../media/manage-phone-azure-portal-capability-update.png" alt-text="Screenshot: Hauptseite einer Communication Services-Ressource":::

Wählen Sie die Funktionen aus der Liste der verfügbaren Anruf- und SMS-Funktionen aus, und klicken Sie dann auf `Confirm`, um die Auswahl anzuwenden.

## <a name="troubleshooting"></a>Problembehandlung

Häufig gestellte Fragen und Probleme:

- Derzeit wird der Erwerb von Telefonnummern nur in den USA unterstützt. Als Grundlage hierfür gilt die Rechnungsadresse des Abonnements, dem die Ressource zugeordnet ist. Derzeit ist es nicht möglich, eine Ressource in ein anderes Abonnement zu verschieben.

- Wenn eine Telefonnummer freigegeben wurde, kann sie bis zum Ende des Abrechnungszeitraums nicht freigegeben oder erneut erworben werden.

- Wenn eine Communication Services-Ressource gelöscht wird, werden die Telefonnummern, die dieser Ressource zugeordnet sind, gleichzeitig automatisch freigegeben.

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erwerben einer Telefonnummer
> * Verwalten Ihrer Telefonnummer
> * Freigeben einer Telefonnummer

> [!div class="nextstepaction"]
> [Senden einer SMS](../telephony-sms/send.md)
> [Erste Schritte für das Tätigen von Anrufen](../voice-video-calling/getting-started-with-calling.md)
