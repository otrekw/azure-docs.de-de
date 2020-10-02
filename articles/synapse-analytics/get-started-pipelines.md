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
ms.openlocfilehash: 72eea7c46dd005cd16ae5b8f0022c1174dd28f27
ms.sourcegitcommit: 0194a29a960e3615f96a2d9d8a7e681cf3e8f9ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89667488"
---
# <a name="orchestrate-with-pipelines"></a>Orchestrieren mit Pipelines

In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio orchestriert werden. 

## <a name="overview"></a>Übersicht

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse orchestrieren.

1. Navigieren Sie in Synapse Studio zum Hub **Orchestrieren**.
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen.
1. Navigieren Sie zum Hub **Entwickeln**, und wählen Sie eins der zuvor erstellten Notebooks aus.
1. Ziehen Sie dieses Notebook in die Pipeline.
1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Trigger auswählen** den Befehl **Neu** aus, und legen Sie dann unter **Serie** fest, dass der Trigger jede Stunde ausgeführt werden soll.
1. Klicken Sie auf **OK**.
1. Wählen Sie **Alle veröffentlichen**. Die Pipeline wird stündlich ausgeführt.
1. Wählen Sie zum sofortigen Ausführen der Pipeline (ohne bis zur nächsten Ausführung nach einer Stunde zu warten) **Trigger hinzufügen** > **Neu/Bearbeiten** aus.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
                                 
