---
title: 'Tutorial: Erste Schritte mit dem Synapse Knowledge Center'
description: In diesem Tutorial erfahren Sie, wie Sie das Synapse Knowledge Center verwenden.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 11/16/2020
ms.openlocfilehash: 59364a0a2c1d1e411f41dc35dee812c8e920b72d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98209201"
---
# <a name="explore-the-synapse-knowledge-center"></a>Überblick über Synapse Knowledge Center

In diesem Tutorial erfahren Sie, wie Sie Synapse Studio Knowledge Center verwenden.

## <a name="getting-to-the-knowledge-center"></a>Aufrufen des Knowledge Center

Es gibt zwei Möglichkeiten, das Knowledge Center in Synapse Studio aufzurufen:

  1. Klicken Sie im Start-Hub oben rechts auf **Learn**.
  2. Klicken Sie oben im Menü auf **?** und dann auf **Knowledge Center**.

Wählen Sie eine der beiden Methoden aus, und öffnen Sie das **Knowledge Center**.

## <a name="overview"></a>Übersicht

In **Knowledge Center** haben Sie drei Möglichkeiten:
* **Use samples immediately** (Beispiele sofort verwenden). Wenn Sie ein kurzes Beispiel für die Funktionsweise von Synapse sehen möchten, wählen Sie diese Option aus.
* **Katalog durchsuchen** Mit dieser Option können Sie Beispieldatasets verknüpfen und Beispielcode in Form von SQL-Skripts, Notebooks und Pipelines hinzufügen.
* **Einführung in Synapse Studio**: Mit dieser Option können Sie sich einen kurzen Überblick über die Hauptbereiche von Synapse Studio verschaffen. Dies ist hilfreich, wenn Sie Synapse Studio noch nie verwendet haben.

## <a name="exploring-blob-storage-with-serverless-sql-pool"></a>Untersuchen von Blobspeicher mit einem serverlosen SQL-Pool

1. Navigieren Sie zu **Knowledge Center**, und klicken Sie auf **Use samples immediately** (Beispiele sofort verwenden).
1. Wählen Sie **Daten mit SQL abfragen** aus.
1. Klicken Sie auf **Beispiel verwenden**.
1. Ein neues SQL-Beispielskript wird geöffnet.
1. Scrollen Sie zur ersten Abfrage (Zeilen 28 bis 32), und wählen Sie den Abfragetext aus.
1. Klicken Sie auf "Ausführen". Nur der von Ihnen ausgewählte Code wird ausgeführt.

## <a name="loading-more-nyc-taxi-data"></a>Laden von weiteren NYC Taxi-Daten
1. Navigieren Sie zu **Knowledge Center**, und klicken Sie auf **Katalog durchsuchen**.
1. Wählen Sie oben die Registerkarte **SQL scripts** (SQL-Skripts) aus.
1. Wählen Sie das Datenerfassungsbeispiel **Load the New York Taxicab dataset** (New York Taxicab-Dataset laden) aus, und klicken Sie auf **Weiter**.
1. Wählen Sie unter **SQL-Pool** die Option **Vorhandenen Pool auswählen** und dann **SQLPOOL1** aus. Wählen Sie anschließend die Datenbank **SQLPOOL1** aus, die Sie zuvor erstellt haben.
1. Klicken Sie auf **Skript öffnen**.
1. Ein neues SQL-Beispielskript wird geöffnet.
1. Klicken Sie auf **Run**.
1. Dadurch werden mehrere Tabellen für alle NYC Taxi-Daten erstellt und mit dem T-SQL-Befehl COPY geladen. Wenn Sie diese Tabellen in den vorherigen Schnellstartschritten erstellt haben, wählen Sie nur Code zum Erstellen (CREATE) und Kopieren (COPY) für Tabellen aus, die nicht vorhanden sind, und führen Sie ihn aus.

    > [!NOTE] 
    > Wenn Sie den Beispielkatalog für SQL-Skripts mit einem dedizierten SQL-Pool (ehemals SQL DW) verwenden, können Sie nur einen vorhandenen dedizierten SQL-Pool (ehemals SQL DW) verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Synapse Analytics](get-started.md)
* [Erstellen eines Arbeitsbereichs](quickstart-create-workspace.md)
* [Verwenden eines serverlosen SQL-Pools](quickstart-sql-on-demand.md)
