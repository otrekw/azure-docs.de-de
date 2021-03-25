---
title: Zuordnen von Datenfeldern zu Azure Sentinel-Entitäten | Microsoft-Dokumentation
description: Zuordnen von Datenfeldern in Tabellen zu Azure Sentinel-Entitäten in Analyseregeln, um bessere Informationen zu Vorfällen zu erhalten
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
ms.openlocfilehash: cb91d269f6b166510db54637d17d776e71137408
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102456160"
---
# <a name="map-data-fields-to-entities-in-azure-sentinel"></a>Zuordnen von Datenfeldern zu Entitäten in Azure Sentinel 

> [!IMPORTANT]
>
> - Die neue Version der Funktion zur Entitätszuordnung befindet sich in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten zusätzliche rechtliche Bedingungen, die für Azure-Features gelten, die sich in der Beta- oder Vorschauversion befinden bzw. anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.

> [!IMPORTANT]
>
> - Wichtige Informationen zur Abwärtskompatibilität und zu den Unterschieden zwischen der neuen und alten Version der Entitätszuordnung finden Sie unter [Hinweise zur neuen Version](#notes-on-the-new-version) am Ende dieses Dokuments.

## <a name="introduction"></a>Einführung

Die Entitätszuordnung ist ein wesentlicher Bestandteil der Konfiguration von [Analyseregeln mit geplanter Abfrage](tutorial-detect-threats-custom.md). Dadurch wird die Ausgabe der Regeln (Warnungen und Vorfälle) um wesentliche Informationen erweitert, die als Bausteine für alle nachfolgenden Untersuchungsprozesse und Korrekturmaßnahmen dienen.

Das unten beschriebene Verfahren ist Teil des Assistenten zum Erstellen von Analyseregeln. Es wird hier separat beschrieben, um das Szenario für das Hinzufügen oder Ändern von Entitätszuordnungen in einer vorhandenen Analyseregel zu erläutern.

## <a name="how-to-map-entities"></a>Zuordnen von Entitäten

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Analysen** aus.

1. Wählen Sie eine geplante Abfrageregel aus, und klicken Sie auf **Bearbeiten**. Oder erstellen Sie eine neue Regel, indem Sie oben auf dem Bildschirm auf **Erstellen &#10132; Geplante Abfrageregel** klicken.

1. Klicken Sie auf die Registerkarte **Regellogik festlegen**.

    :::image type="content" source="media/map-data-fields-to-entities/map-entities.png" alt-text="Zuordnen von Feldern zu Entitäten":::

1. Wählen Sie im Bereich **Warnungserweiterung** unter **Entitätszuordnung** in der Dropdownliste **Entitätstyp** einen Entitätstyp aus.

1. Wählen Sie einen **Bezeichner** für die Entität aus. Bezeichner sind Attribute einer Entität, die diese ausreichend identifizieren können. Wählen Sie einen Bezeichner in der Dropdownliste **Bezeichner** und dann in der Dropdownliste **Wert** ein Datenfeld aus, das dem Bezeichner entspricht. Mit wenigen Ausnahmen wird die Liste **Wert** durch die Datenfelder in der Tabelle gefüllt, die als Gegenstand der Regelabfrage definiert ist.

    Pro Entität können Sie **bis zu drei Bezeichner** definieren. Einige Bezeichner sind erforderlich, andere dagegen optional. Sie müssen mindestens einen erforderlichen Bezeichner auswählen. Andernfalls werden Sie in einer Warnmeldung auf die erforderlichen Bezeichner hingewiesen. Um optimale Ergebnisse (die bestmögliche eindeutige Identifizierung) zu erzielen, sollten Sie nach Möglichkeit **starke Bezeichner** verwenden. Die Verwendung mehrerer starker Bezeichner ermöglicht eine größere Korrelation zwischen Datenquellen. Die vollständige Liste der verfügbaren Entitäten und Bezeichner finden Sie [hier](entities-reference.md).

1. Klicken Sie auf **Neue Entität hinzufügen**, um weitere Entitäten zuzuordnen. Sie können **bis zu fünf Entitäten** in einer Analyseregel zuordnen. Außerdem können Sie mehrere Entitäten desselben Typs zuordnen. Beispielsweise können Sie zwei **IP**-Entitäten zuordnen: eine aus dem Feld *Quell-IP-Adresse* und eine aus dem Feld *Ziel-IP-Adresse*. So können beide nachverfolgt werden.

    Wenn Sie Ihre Meinung ändern oder einen Fehler verursacht haben, können Sie eine Entitätszuordnung entfernen, indem Sie auf das Papierkorbsymbol neben der Dropdownliste der Entität klicken.

1. Klicken Sie nach der Zuordnung der Entitäten auf die Registerkarte **Überprüfen und erstellen**. Klicken Sie nach der erfolgten Regelüberprüfung auf **Speichern**.

## <a name="notes-on-the-new-version"></a>Hinweise zur neuen Version

- Wenn Sie zuvor unter der alten Version Entitätszuordnungen für die Analyseregel definiert haben, werden die entsprechenden Zuordnungen im Abfragecode angezeigt. Entitätszuordnungen, die unter der neuen Version definiert wurden, **werden im Abfragecode nicht angezeigt**. In Analyseregeln kann jeweils nur eine Version von Entitätszuordnungen unterstützt werden. Dabei hat die neue Version Vorrang. Daher führt jede Zuordnung, die Sie hier definieren, dazu, dass **alle** im Abfragecode definierten Zuordnungen bei Ausführung der Abfrage **nicht berücksichtigt** werden. 

- Wenn Sie weiterhin die **alte Version** der Entitätszuordnung verwenden möchten (solange sich die neue Version noch in der Vorschauphase befindet), können Sie weiterhin mit einem Featureflag in der URL darauf zugreifen. Setzen Sie den Cursor zwischen `https://portal.azure.com/` und `#blade`, und fügen Sie den Text `?feature.EntityMapping=false` ein.

  - Es gelten weiterhin die Einschränkungen der alten Version. Sie können nur die Benutzer-, Host-, IP-Adressen-, URL- und Dateihashentitäten zuordnen.

  - Alle Entitätszuordnungen, die mit der neuen Version erstellt wurden, müssen **entfernt** werden, **bevor** wieder die alte Version verwendet wird. Alle mit der alten Version definierten Entitätszuordnungen **funktionieren andernfalls nicht**.

- Sobald die neue Version der Entitätszuordnung allgemein verfügbar ist, kann die alte Version nicht mehr verwendet werden. Es wird dringend empfohlen, die alten Entitätszuordnungen zu der neuen Version zu migrieren.


## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurde beschrieben, wie Sie Datenfelder Entitäten in Azure Sentinel-Analyseregeln zuordnen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Unter [Tutorial: Erstellen benutzerdefinierter Analyseregeln zum Erkennen von Bedrohungen](tutorial-detect-threats-custom.md) können Sie sich ein Gesamtbild machen.
- Weitere Informationen zu [Entitäten in Azure Sentinel](entities-in-azure-sentinel.md)
