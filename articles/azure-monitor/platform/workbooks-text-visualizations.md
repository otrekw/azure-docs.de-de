---
title: Textvisualisierungen für Azure Monitor-Arbeitsmappen
description: Erfahren Sie mehr über die zahlreichen Textvisualisierungen für Azure Monitor-Arbeitsmappen.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: lagayhar
ms.openlocfilehash: e8f2d9495484b781b26962c2946b5bada6c38b4c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89663289"
---
# <a name="text-visualizations"></a>Textvisualisierungen

Autoren können Textblöcke in Ihre Arbeitsmappen einschließen. Beim Text kann es sich um Analysen von Telemetriedaten durch einen Bearbeiter, Informationen, anhand derer Benutzer Ihre Daten interpretieren können, Abschnittsüberschriften usw. handeln.

[![Screenshot der Apdex-Tabelle mit Text](./media/workbooks-text-visualizations/apdex.png)](./media/workbooks-text-visualizations/apdex.png#lightbox)

Text wird über ein Markdown-Steuerelement hinzugefügt, mit dem die Formatierung umfassend gesteuert werden kann. Dazu gehören verschiedene Stile für Überschriften und Schriftarten, Hyperlinks, Tabellen usw.

Bearbeitungsmodus:

![Screenshot eines Textschritts im Bearbeitungsmodus](./media/workbooks-text-visualizations/text-edit-mode.png)

Vorschaumodus:

![Screenshot eines Textschritts im Bearbeitungsmodus auf der Registerkarte „Vorschau“](./media/workbooks-text-visualizations/text-edit-mode-preview.png)

## <a name="add-a-text-control"></a>Hinzufügen eines Textsteuerelements

1. Versetzen Sie die Arbeitsmappe in den Bearbeitungsmodus, indem Sie auf das Symbolleistenelement **Bearbeiten** klicken.
2. Klicken Sie auf den Link **Text hinzufügen**, um der Arbeitsmappe ein Textsteuerelement hinzuzufügen.
3. Fügen Sie im Editor-Feld Markdown hinzu.
4. Verwenden Sie die Option *Textstil*, um zwischen einfachem Markdown und mit dem Standardformat für Info/Warnung/Erfolg/Fehler des Azure-Portals umschlossenes Markdown zu wechseln.
5. Auf der Registerkarte**Vorschau** können Sie sehen, wie Ihre Inhalte angezeigt werden. Während der Bearbeitung wird in der Vorschau der Inhalt in einem Bereich angezeigt, der zum Begrenzen der Größe eine Scrollleiste aufweist. Zur Laufzeit wird der Markdowninhalt jedoch auf den benötigten Raum ohne Scrollleisten erweitert.
6. Wählen Sie die Schaltfläche **Bearbeitung abgeschlossen** aus, um die Bearbeitung des Schritts abzuschließen.

> [!TIP]
> Verwenden Sie dieses [Markdown-Cheat Sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet), um sich über verschiedene Formatierungsoptionen zu informieren.

## <a name="text-styles"></a>Textstile

Die folgenden Textstile sind für den Textschritt verfügbar:

| Style     | Erläuterung                                                                               |
|-----------|-------------------------------------------------------------------------------------------|
| `plain`   | Es wird keine zusätzliche Formatierung angewandt.                                                      |
| `info`    | Der Stil „info“ des Portals mit einem Symbol wie `ℹ` und im Allgemeinen mit blauem Hintergrund.      |
| `error`   | Der Stil „error“ (Fehler) des Portals mit einem Symbol wie `❌` und im Allgemeinen mit rotem Hintergrund.     |
| `success` | Der Stil „success“ (Erfolg) des Portals mit einem Symbol wie `✔` und im Allgemeinen mit grünem Hintergrund.  |
| `upsell`  | Der Stil „upsell“ des Portals mit einem Symbol wie `🚀` und im Allgemeinen mit violettem Hintergrund. |
| `warning` | Der Stil „warning“ (Warnung) des Portals mit einem Symbol wie `⚠` und im Allgemeinen mit blauem Hintergrund.   |

Anstatt einen bestimmten Stil auszuwählen, können Sie auch einen Textparameter als Quelle des Stils auswählen. Der Parameterwert muss einer der oben aufgeführten Textwerte sein. Ein fehlender oder unbekannter Wert wird als Stil `plain` behandelt.

Beispiel zum Stil „info“:

![Screenshot des Erscheinungsbilds des Stils „info“](./media/workbooks-text-visualizations/text-preview-info-style.png)

Beispiel für den Stil „warning“ (Warnung):

![Screenshot des Erscheinungsbilds des Stils „warning“](./media/workbooks-text-visualizations/text-warning-style.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie ein [Diagramm in Arbeitsmappen](workbooks-chart-visualizations.md) erstellen.
* Erfahren Sie, wie Sie ein [Raster in Arbeitsmappen](workbooks-grid-visualizations.md) erstellen.
