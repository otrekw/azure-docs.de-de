---
title: 'Satzpaarbildung und -zuordnung: Custom Translator'
titleSuffix: Azure Cognitive Services
description: Während der Ausführung des Trainings werden aus den Sätzen in parallelen Dokumenten Paare gebildet, bzw. die Sätze werden einander zugeordnet. Custom Translator lernt Übersetzungen Satz für Satz durch Lesen eines Satzes und der zugehörigen Übersetzung. Danach werden Wörter und Ausdrücke in diesen beiden Sätzen einander zugeordnet.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 04/19/2021
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: d775496ad9490cfac81eecc9c08ef7beddea73dc
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111968124"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Satzpaarbildung und -zuordnung in parallelen Dokumenten

Nach dem Hochladen der Dokumente werden aus den Sätzen in parallelen Dokumenten Paare gebildet, bzw. die Sätze werden einander zugeordnet. Custom Translator zeigt die Anzahl von Sätzen, für die ein Paar gebildet werden konnte, als „zugeordnete Sätze“ in jedem der Datasets an.

## <a name="pairing-and-alignment-process"></a>Paarbildungs- und Zuordnungsprozess

Custom Translator lernt Übersetzungen von Sätzen Satz für Satz. Er liest einen Satz aus dem Quelltext und anschließend die Übersetzung dieses Satzes aus dem Zieltext. Danach werden Wörter und Ausdrücke in diesen beiden Sätzen einander zugeordnet. Dieser Prozess ermöglicht die Erstellung einer Zuordnung zwischen den Wörtern und Ausdrücken in einem Satz und den entsprechenden Wörtern und Ausdrücken in der Übersetzung des Satzes. Durch die Zuordnung soll sichergestellt werden, dass das System mit Sätzen trainiert wird, bei denen es sich um gegenseitige Übersetzungen handelt.

## <a name="pre-aligned-documents"></a>Vorab zugeordnete Dokumente

Wenn Sie wissen, dass Sie über parallele Dokumente verfügen, können Sie die Satzzuordnung außer Kraft setzen, indem Sie vorab zugeordnete Textdateien bereitstellen. Sie können alle Sätze aus beiden Dokumenten in eine Textdatei mit einem Satz pro Zeile extrahieren und die Datei mit der Erweiterung `.align` hochladen. Die Erweiterung `.align` signalisiert Custom Translator, dass die Satzzuordnung übersprungen werden soll.

Um optimale Ergebnisse zu erzielen, sollten Sie darauf achten, dass Ihre Dateien einen Satz pro Zeile enthalten.  Verwenden Sie keine Zeilenumbruchzeichen in einem Satz, da dies zu Zuordnungsfehlern führen kann.

## <a name="suggested-minimum-number-of-sentences"></a>Empfohlene Mindestanzahl von Sätzen

Die folgende Tabelle enthält die für ein erfolgreiches Training erforderliche Mindestanzahl von Sätzen, die im jeweiligen Dokumenttyp enthalten sein müssen. Durch diese Einschränkung wird sichergestellt, dass Ihre parallelen Sätze genügend eindeutiges Vokabular zum erfolgreichen Trainieren eines Übersetzungsmodells enthalten. Faustregel: Eine höhere Anzahl bereichsspezifischer paralleler Sätze von menschlichen Übersetzern führt in der Regel zu einer höheren Modellqualität.

| Dokumenttyp   | Empfohlene Mindestanzahl von Sätzen | Maximale Anzahl von Sätzen |
|------------|--------------------------------------------|--------------------------------|
| Training   | 10.000                                     | Keine Obergrenze                 |
| Optimierung     | 500                                      | 2\.500       |
| Testen    | 500                                      | 2\.500  |
| Wörterbuch | 0                                          | 250.000                 |

> [!NOTE]
>
> - Das Training wird nicht gestartet und ist nicht erfolgreich, wenn die Mindestanzahl von 10.000 Sätzen für das Training unterschritten wird.
> - „Optimierung“ und „Testen“ sind optional. Ohne diese Optionen entfernt das System einen gewissen Prozentsatz aus dem Training, um ihn für die Überprüfung und zu Testzwecken zu verwenden.
> - Sie können ein Modell nur mit Wörterbuchdaten trainieren. Weitere Informationen finden Sie unter [Definition: Wörterbuch](./what-is-dictionary.md).
> -  Wenn Ihr Wörterbuch mehr als 250.000 Sätze enthält, ist unsere Dokumentübersetzung eine bessere Wahl. Weitere Informationen finden Sie unter [Dokumentübersetzung](../document-translation/overview.md).
> - Das kostenlose Abonnementtraining (F0) ist auf maximal 2.000.000 Zeichen begrenzt. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie ein [Wörterbuch](what-is-dictionary.md) in Custom Translator verwenden.