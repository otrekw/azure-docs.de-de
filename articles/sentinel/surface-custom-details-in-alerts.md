---
title: Anzeigen benutzerdefinierter Details in Azure Sentinel-Warnungen | Microsoft-Dokumentation
description: Extrahieren und Anzeigen von benutzerdefinierten Ereignisdetails in Warnungen der Azure Sentinel-Analyseregeln für bessere und vollständigere Informationen zu Incidents
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2021
ms.author: yelevin
ms.openlocfilehash: 18e176f11da9c8dca86c5f3477cb282c478664cf
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122349304"
---
# <a name="surface-custom-event-details-in-alerts-in-azure-sentinel"></a>Anzeigen benutzerdefinierter Ereignisdetails in Azure Sentinel-Warnungen 

> [!IMPORTANT]
>
> - Das Feature zu benutzerdefinierten Details befindet sich in der **Vorschauphase**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

## <a name="introduction"></a>Einführung

[Analyseregeln für geplante Abfragen](detect-threats-custom.md) analysieren **Ereignisse** aus mit Azure Sentinel verbundenen Datenquellen und erzeugen **Warnungen**, wenn die Inhalte dieser Ereignisse aus Sicherheitssicht bedeutsam sind. Diese Warnungen werden weiter analysiert, gruppiert, nach den verschiedenen Azure Sentinel-Engines gefiltert und in **Incidents** zusammengefasst, die die Aufmerksamkeit eines SOC-Analysten garantieren. Wenn dem Analyst der Incident anzeigt wird, sind jedoch nur die Eigenschaften der Komponentenwarnungen sofort erkennbar. Den eigentlichen Inhalt (die in den Ereignissen enthaltenen Informationen) anzuzeigen, ist ein bisschen komplizierter.

Mit dem Feature zu **benutzerdefinierten Details** im **Analyseregel-Assistenten** können Sie Ereignisdaten in den Warnungen anzeigen, die aus diesen Ereignissen erstellt wurden, indem die Ereignisdaten Teil der Warnungseigenschaften werden. Dadurch wird der Ereignisinhalt in Ihren Incidents sofort sichtbar, sodass Sie deutlich schneller und effizienter selektieren, untersuchen, schlussfolgern und antworten können.

Das unten beschriebene Verfahren ist Teil des Assistenten zum Erstellen von Analyseregeln. Es wird hier separat beschrieben, um das Szenario für das Hinzufügen oder Ändern von benutzerdefinierten Details in einer vorhandenen Analyseregel zu erläutern.

## <a name="how-to-surface-custom-event-details"></a>Anzeigen von benutzerdefinierten Ereignisdetails

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Analysen** aus.

1. Wählen Sie eine geplante Abfrageregel aus, und klicken Sie auf **Bearbeiten**. Oder erstellen Sie eine neue Regel, indem Sie oben auf dem Bildschirm auf **Erstellen > Geplante Abfrageregel** klicken.

1. Klicken Sie auf die Registerkarte **Regellogik festlegen**.

1. Erweitern Sie im Abschnitt **Warnungsanreicherung (Vorschau)** den Bereich **Benutzerdefinierte Details**.

    :::image type="content" source="media/surface-custom-details-in-alerts/alert-enrichment.png" alt-text="Suchen und Auswählen der benutzerdefinierten Details":::

1. Fügen Sie im nun aufgeklappten Bereich **Custom details** (Benutzerdefinierte Details) Schlüssel-Wert-Paare entsprechend der Details hinzu, die Sie anzeigen möchten:

    1. Geben Sie in das Feld **Schlüssel** einen beliebigen Namen ein, der in Warnungen als Feldname erscheinen wird.

    1. Wählen Sie im Feld **Wert** aus der Dropdownliste den Ereignisparameter aus, den Sie in den Warnungen anzeigen möchten. Diese Liste wird mit Werten befüllt, die den Feldern in den Tabellen entsprechen, für die die Abfrageregel erstellt wird.
    
        :::image type="content" source="media/surface-custom-details-in-alerts/custom-details.png" alt-text="Hinzufügen benutzerdefinierter Details":::

1. Klicken Sie auf **Neu hinzufügen**, um weitere Details anzuzeigen, und wiederholen Sie die letzten Schritte zum Definieren von Schlüssel-Wert-Paaren. 

    Wenn Sie Ihre Meinung ändern oder einen Fehler gemacht haben, können Sie ein benutzerdefiniertes Detail entfernen, indem Sie auf das Papierkorbsymbol neben der Dropdownliste **Wert** für dieses Detail klicken.

1. Wenn Sie alle benutzerdefinierten Details angegeben haben, klicken Sie auf die Registerkarte **Überprüfen und erstellen**. Klicken Sie nach erfolgreicher Regelüberprüfung auf **Speichern**.

    > [!NOTE]
    > 
    > **Diensteinschränkungen**
    > - Sie können **bis zu 20 benutzerdefinierte Details** in einer Analyseregel definieren.
    >
    > - Die Größenbeschränkung für alle benutzerdefinierten Details beträgt insgesamt **2 KB**.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie mithilfe von Azure Sentinel-Analyseregeln benutzerdefinierte Details in Warnungen anzeigen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](detect-threats-custom.md) können Sie sich ein Gesamtbild machen.
- Erfahren Sie mehr über [Entitäten in Azure Sentinel](entities-in-azure-sentinel.md).
