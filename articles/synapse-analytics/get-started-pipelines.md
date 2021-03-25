---
title: 'Tutorial: Erste Schritte zum Integrieren mit Pipelines'
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
ms.openlocfilehash: 2ea7c3c440fcf95e4512464333efe8461788bceb
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98219401"
---
# <a name="integrate-with-pipelines"></a>Integrieren mit Pipelines

In diesem Tutorial erfahren Sie, wie Pipelines und Aktivitäten mithilfe von Synapse Studio integriert werden. 

## <a name="overview"></a>Übersicht

Sie können eine Vielzahl verschiedener Aufgaben in Azure Synapse integrieren.

1. Navigieren Sie in Synapse Studio zum Hub **Integrieren**.
1. Wählen Sie **+**  > **Pipeline** aus, um eine neue Pipeline zu erstellen. Klicken Sie auf das neue Pipelineobjekt, um den Pipeline-Designer zu öffnen.
1. Erweitern Sie unter **Aktivitäten** den Ordner **Synapse**, und ziehen Sie ein Objekt vom Typ **Notebook** in den Designer.
1. Wählen Sie die Registerkarte **Einstellungen** der Notebook-Aktivitätseigenschaften aus. Verwenden Sie die Dropdownliste, um ein beliebiges Notebook aus Ihrem aktuellen Synapse-Arbeitsbereich auszuwählen. 
1. Wählen Sie in der Pipeline **Trigger hinzufügen** > **Neu/Bearbeiten** aus.
1. Wählen Sie unter **Choose trigger** (Trigger auswählen) die Option **Neu** aus, und legen Sie **Wiederholung** auf „1 Stunde“ fest.
1. Klicken Sie auf **OK**. 
1. Wählen Sie **Alle veröffentlichen**. 


## <a name="monitor-pipeline"></a>Überwachen der Pipeline

1. Nach der Veröffentlichung der Pipeline wählen Sie zum sofortigen Ausführen der Pipeline (ohne bis zur nächsten Ausführung nach einer Stunde zu warten) die Option **Trigger hinzufügen** > **Jetzt auslösen** aus.
1. Navigieren Sie in Synapse Studio zum Hub **Überwachen**, und wählen Sie **Pipelineausführungen** aus, um den Status der Pipelineausführung zu überwachen.



## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Visualisieren von Daten mit Power BI](get-started-visualize-power-bi.md)
