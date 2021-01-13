---
title: Umwandlung von Azure Monitor-Ansichts-Designer in Arbeitsmappen – Optionen
description: ''
author: austonli
ms.author: aul
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 7bfa831332451718c0c9c05023b90104d2b8b02b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "77658709"
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

![Ring](media/view-designer-conversion-options/donut-example.png)

Mit Arbeitsmappen ermöglichen wir es dem Benutzer, einen oder beide Abschnitte der Absicht abzufragen. Das Formulieren von Abfragen in Arbeitsmappen ist ein einfacher, zweistufiger Prozess. Zuerst werden die Daten aus der Abfrage generiert, und danach werden die Daten als Visualisierung gerendert.  Ein Beispiel dafür, wie diese Ansicht in Arbeitsmappen neu erstellt würde, sehen Sie im Folgenden:

![Convert](media/view-designer-conversion-options/convert-donut.png)


## <a name="next-steps"></a>Nächste Schritte
- [Zugreifen auf Arbeitsmappen und Berechtigungen](view-designer-conversion-access.md)