---
title: Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Optionen
description: Umwandlungsoptionen für den Übergang von Ansichten zu Arbeitsmappen in Azure Monitor.
author: austonli
ms.author: aul
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: b8b6b8b41c729c3cbb6cf4589d679e93149e5314
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102043404"
---
# <a name="azure-monitor-view-designer-to-workbooks-conversion-options"></a>Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Optionen
Der [Ansicht-Designer](view-designer.md) ist eine Funktion in Azure Monitor, mit der Sie verschiedene benutzerdefinierten Ansichten erstellen können, die Ihnen bei der Visualisierung von Daten in Ihrem Log Analytics-Arbeitsbereich mittels Diagrammen, Listen und Zeitachsen helfen. Sie laufen aus und werden durch Arbeitsmappen ersetzt, die zusätzliche Funktionalität bereitstellen. In diesem Artikel werden die grundlegenden Konzepte zwischen den beiden Optionen zum Umwandeln von Ansichten in Arbeitsmappen verglichen.

## <a name="basic-workbook-designs"></a>Grundlegende Arbeitsmappenentwürfe

Der Ansicht-Designer verfügt über einen festen statischen Darstellungsstil, während Arbeitsmappen es Ihnen ermöglichen, die Darstellung der Daten einzuschließen und diese zu ändern. Die folgenden Images stellen zwei Beispiele dafür dar, wie Sie Arbeitsmappen beim Umwandeln von Ansichten anordnen können.

[Vertikale Arbeitsmappe](view-designer-conversion-examples.md#vertical)
![Vertikal](media/view-designer-conversion-options/view-designer-vertical.png)

[Arbeitsmappe mit Registerkarten](view-designer-conversion-examples.md#tabbed)
![Registerkarte „Datentypverteilung“](media/view-designer-conversion-options/distribution-tab.png)
![Registerkarte „Datentypen im Zeitverlauf“](media/view-designer-conversion-options/over-time-tab.png)

## <a name="tile-conversion"></a>Kachelumwandlung
Der Ansicht-Designer verwendet die Übersichtskachelfunktion, um den Gesamtzustand darzustellen und zusammenzufassen. Dieser wird auf sieben Kacheln dargestellt, die von Zahlen bis hin zu Diagrammen reichen. In Arbeitsmappen können Benutzer ähnliche Visualisierungen erstellen und diese anheften, sodass sie dem ursprünglichen Stil von Übersichtskacheln entsprechen. 

![Galerie](media/view-designer-conversion-options/overview.png)


## <a name="view-dashboard-conversion"></a>Umwandlung von Ansichtsdashboards
Ansicht-Designer-Kacheln bestehen normalerweise aus zwei Abschnitten, einer Visualisierung und einer Liste, die mit den Daten aus der Visualisierung übereinstimmt, z. B. die Kachel **Ring und Liste**.

![Ringdiagramm](media/view-designer-conversion-options/donut-example.png)

Mit Arbeitsmappen ermöglichen wir es dem Benutzer, einen oder beide Abschnitte der Absicht abzufragen. Das Formulieren von Abfragen in Arbeitsmappen ist ein einfacher, zweistufiger Prozess. Zuerst werden die Daten aus der Abfrage generiert, und danach werden die Daten als Visualisierung gerendert.  Ein Beispiel dafür, wie diese Ansicht in Arbeitsmappen neu erstellt würde, sehen Sie im Folgenden:

![Convert](media/view-designer-conversion-options/convert-donut.png)


## <a name="next-steps"></a>Nächste Schritte
- [Zugreifen auf Arbeitsmappen und Berechtigungen](view-designer-conversion-access.md)