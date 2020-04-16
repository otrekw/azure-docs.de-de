---
title: Visuelles Erstellen
description: Informationen zum Verwenden der visuellen Erstellung in Azure Data Factory
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
ms.reviewer: ''
manager: anandsub
ms.date: 12/19/2019
ms.openlocfilehash: e7de92878dac72470c0b65d1cf18c1a2d526a0bb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418489"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Visuelles Erstellen in Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mithilfe der Azure Data Factory-Benutzeroberfläche (UX) können Sie Ressourcen für Ihre Data Factory visuell erstellen und bereitstellen, ohne Code schreiben zu müssen. Sie können Aktivitäten auf eine Pipelinecanvas ziehen, Testläufe ausführen, iterativ debuggen sowie Ihre Pipelineausführungen bereitstellen und überwachen.

Die Azure Data Factory-Benutzererfahrung wird zurzeit nur unter Microsoft Edge und Google Chrome unterstützt.

## <a name="authoring-canvas"></a>Erstellungsbereich

Um den **Erstellungsbereich** zu öffnen, klicken Sie auf das Stiftsymbol. 

![Erstellungsbereich](media/author-visually/authoring-canvas.png)

Hier erstellen Sie die Pipelines, Aktivitäten, Datasets, verknüpften Dienste, Datenflüsse, Trigger und Integrationslaufzeiten, aus denen Ihre Factory besteht. Informationen zum Einstieg in das Erstellen einer Pipeline im Erstellungsbereich finden Sie unter [Kopieren von Daten mit der Kopieraktivität](tutorial-copy-data-portal.md). 

Die standardmäßige visuelle Erstellungsumgebung arbeitet direkt mit dem Data Factory-Dienst zusammen. Die Integration in Azure Repos Git oder GitHub wird ebenfalls unterstützt, um die Versionskontrolle und Zusammenarbeit an Ihren Data Factory-Pipelines zu ermöglichen. Weitere Informationen zu den Unterschieden dieser Erstellungsumgebungen finden Sie unter [Quellcodeverwaltung in Azure Data Factory](source-control.md).

## <a name="expressions-and-functions"></a>Ausdrücke und Funktionen

Anstelle statischer Werte können Sie Ausdrücke und Funktionen verwenden, um viele Eigenschaften in Azure Data Factory anzugeben.

Zum Angeben eines Ausdrucks für einen Eigenschaftswert wählen Sie **Dynamischen Inhalt hinzufügen** aus oder klicken auf **ALT+P**, wenn sich der Eingabefokus auf dem Feld befindet.

![Dynamischen Inhalt hinzufügen](media/author-visually/dynamic-content-1.png)

Dadurch wird der **Ausdrucks-Generator von Data Factory** geöffnet, in dem Sie Ausdrücke anhand von unterstützten Systemvariablen, Aktivitätsausgaben, Funktionen und benutzerdefinierten Variablen oder Parametern erstellen können. 

![Ausdrucks-Generator](media/author-visually/dynamic-content-2.png)

Informationen zur Ausdruckssprache finden Sie unter [Ausdrücke und Funktionen in Azure Data Factory](control-flow-expression-language-functions.md).

## <a name="provide-feedback"></a>Feedback geben

Wählen Sie **Feedback** aus, um Kommentare zu Funktionen abzugeben oder um Microsoft Probleme mit dem Tool zu melden:

![Feedback](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Nächste Schritte

Unter [Programmgesteuertes Überwachen und Verwalten von Pipelines](monitor-programmatically.md) erfahren Sie mehr über das Überwachen und Verwalten von Pipelines.
