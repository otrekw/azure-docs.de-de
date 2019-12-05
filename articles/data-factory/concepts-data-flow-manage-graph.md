---
title: Azure Data Factory-Datenflussdiagramme
description: Arbeiten mit Data Factory-Datenflussdiagrammen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 38f85dea554000daf89689a311c091bc9796a658
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74018629"
---
# <a name="mapping-data-flow-graphs"></a>Zuordnungsdatenflussdiagramme

Die Entwurfsoberfläche für Zuordnungsdatenflüsse-ist eine „Konstruktionsoberfläche“, mit der Sie Datenflüsse von oben nach unten und von links nach rechts erstellen können. Jeder Transformation mit einem Pluszeichen (+) ist eine Toolbox angefügt. Konzentrieren Sie sich auf Ihre Geschäftslogik, statt auf das Verbinden von Knoten über Edges in einer Freiform-DAG-Umgebung.

Nachstehend finden Sie integrierte Mechanismen zum Verwalten des Datenflussdiagramms.

## <a name="move-nodes"></a>Verschieben von Knoten

![Optionen für die Transformation für das Aggregieren](media/data-flow/agghead.png "Aggregator-Header")

Ohne ein Drag & Drop-Paradigma besteht die Möglichkeit zum „Verschieben“ eines Transformationsknotens darin, den eingehenden Datenstrom zu ändern. Stattdessen verschieben Sie Transformationen, indem Sie den „eingehenden Datenstrom“ ändern.

## <a name="streams-of-data-inside-of-data-flow"></a>Datenströme innerhalb des Datenflusses

In Azure Data Factory-Datenfluss stellen Datenströme den Fluss der Daten dar. Im Bereich der Transformationseinstellungen wird ein Feld „Eingehender Datenstrom“ angezeigt. Daraus geht hervor, welcher eingehende Datenstrom die Transformation speist. Sie können den physischen Speicherort Ihres Transformationsknotens im Diagramm ändern, indem Sie auf den Namen des eingehenden Datenstroms klicken und einen anderen Datenstrom auswählen. Dann werden die aktuelle Transformation sowie alle nachfolgenden Transformationen in diesem Datenstrom an den neuen Speicherort verschoben.

Wenn Sie eine Transformation mit einer oder mehreren nachfolgenden Transformationen verschieben, wird der neue Speicherort im Datenfluss über eine neue Verzweigung verknüpft.

Wenn es nach dem von Ihnen ausgewählten Knoten keine nachfolgenden Transformationen gibt, wird nur diese Transformation an den neuen Speicherort verschoben.

## <a name="hide-graph-and-show-graph"></a>Ausblenden und Anzeigen des Diagramms

Ganz rechts im unteren Konfigurationsbereich gibt es eine Schaltfläche, über die Sie den unteren Bereich beim Arbeiten mit Transformationskonfigurationen auf Vollbildmodus erweitern können. Dann können Sie mithilfe der Schaltflächen „Zurück“ und „Weiter“ durch die Konfigurationen des Diagramms navigieren. Wenn Sie zur Diagrammansicht zurückwechseln möchten, klicken Sie auf die Schaltfläche „Nach unten“, und kehren Sie zum geteilten Bildschirm zurück.

## <a name="search-graph"></a>Diagramm suchen

Sie können das Diagramm über die Schaltfläche „Suchen“ auf der Entwurfsoberfläche suchen.

![Suchen,](media/data-flow/search001.png "Diagramm suchen")

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihren Datenflussentwurf abgeschlossen haben, aktivieren Sie die Debug-Schaltfläche und testen Sie sie im Debugmodus entweder direkt im [Datenfluss-Designer](concepts-data-flow-debug-mode.md) oder über die Option zum [Debuggen der Pipeline](control-flow-execute-data-flow-activity.md).
