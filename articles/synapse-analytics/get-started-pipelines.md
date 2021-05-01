---
title: 'Tutorial: Erste Schritte beim Integrieren mit Pipelines'
description: In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio integriert werden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 19bff62883341947eb5290118494b8244c5476ac
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107518251"
---
# <a name="integrate-with-pipelines"></a>Integrieren mit Pipelines

In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio integriert werden. 

## <a name="create-a-pipeline-and-add-a-notebook-activity"></a>Erstellen einer Pipeline und Hinzufügen einer Notebook-Aktivität

1. Navigieren Sie in Synapse Studio zum Hub **Integrieren**.
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen. Klicken Sie auf das neue Pipelineobjekt, um den Pipeline-Designer zu öffnen.
1. Erweitern Sie unter **Aktivitäten** den Ordner **Synapse**, und ziehen Sie ein Objekt vom Typ **Notebook** in den Designer.
1. Wählen Sie die Registerkarte **Einstellungen** der Notebook-Aktivitätseigenschaften aus. Verwenden Sie die Dropdownliste, um ein Notebook aus Ihrem aktuellen Synapse-Arbeitsbereich auszuwählen.

## <a name="schedule-the-pipeline-to-run-every-hour"></a>Planen der stündlichen Ausführung einer Pipeline

1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Choose trigger** (Trigger auswählen) die Option **Neu** aus, und legen Sie **Wiederholung** auf „1 Stunde“ fest.
1. Klicken Sie auf **OK**. 
1. Wählen Sie **Alle veröffentlichen**. 

## <a name="forcing-a-pipeline-to-run-immediately"></a>Erzwingen der sofortigen Ausführung einer Pipeline

Nach dem Veröffentlichen der Pipeline möchten Sie sie vielleicht sofort ausführen und nicht eine Stunde lang warten.

1. Öffnen Sie die Pipeline.
1. Klicken Sie auf **Trigger hinzufügen** > **Trigger Now** (Jetzt auslösen).

## <a name="monitor-pipeline-execution"></a>Überwachen der Pipelineausführung

1. Wechseln Sie zum Hub **Überwachen**.
1. Wählen Sie **Pipelineausführungen** aus, um den Status der Pipelineausführung zu überwachen.
1. In dieser Ansicht können Sie zwischen der tabellarischen **Listenansicht** und einem grafischen **Gantt-Diagramm** wechseln. 
1. Klicken Sie auf einen Pipelinenamen, um den Status der Aktivitäten in dieser Pipeline anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
