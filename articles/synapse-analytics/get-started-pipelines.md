---
title: 'Tutorial: Erste Schritte zum Orchestrieren mit Pipelines'
description: In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio orchestriert werden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: 5e32a6a9817f2a3176e96e39c5e261875e8f4ed1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093476"
---
# <a name="orchestrate-with-pipelines"></a>Orchestrieren mit Pipelines

In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio orchestriert werden. 

## <a name="overview"></a>Übersicht

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse orchestrieren.

1. Navigieren Sie in Synapse Studio zum Hub **Orchestrieren**.
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen.
1. Navigieren Sie zum Hub **Entwickeln**, und suchen Sie nach dem Notebook, das Sie zuvor erstellt haben.
1. Ziehen Sie dieses Notebook in die Pipeline.
1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Trigger auswählen** den Befehl **Neu** aus, und legen Sie dann unter **Serie** fest, dass der Trigger jede Stunde ausgeführt werden soll.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Alle veröffentlichen**. Die Pipeline wird stündlich ausgeführt.
1. Wählen Sie zum sofortigen Ausführen der Pipeline (ohne bis zur nächsten Ausführung nach einer Stunde zu warten) **Trigger hinzufügen** > **Neu/Bearbeiten** aus.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
                                 
