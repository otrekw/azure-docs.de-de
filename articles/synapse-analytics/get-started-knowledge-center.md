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
ms.date: 04/04/2021
ms.openlocfilehash: f4cc631bd3ff05dc63566677ec96ef0360d362c9
ms.sourcegitcommit: db925ea0af071d2c81b7f0ae89464214f8167505
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2021
ms.locfileid: "107517350"
---
# <a name="explore-the-synapse-knowledge-center"></a>Überblick über Synapse Knowledge Center

In diesem Tutorial erfahren Sie, wie Sie Synapse Studio Knowledge Center verwenden.

## <a name="finding-to-the-knowledge-center"></a>Suchen des Knowledge Center

Es gibt zwei Möglichkeiten, das Knowledge Center in Synapse Studio aufzurufen:

  1. Klicken Sie im Start-Hub oben rechts auf **Learn**.
  2. Klicken Sie oben im Menü auf **?** und dann auf **Knowledge Center**.

Wählen Sie eine der beiden Methoden aus, und öffnen Sie das **Knowledge Center**.

## <a name="exploring-the-knowledge-center"></a>Weitere Informationen zum Knowledge Center

Sie sehen nach dem Öffnen, dass das **Knowledge Center** drei Aktionen ermöglicht:
* **Use samples immediately** (Beispiele sofort verwenden). Wenn Sie ein kurzes Beispiel für die Funktionsweise von Synapse sehen möchten, wählen Sie diese Option aus.
* **Katalog durchsuchen** Mit dieser Option können Sie Beispieldatasets verknüpfen und Beispielcode in Form von SQL-Skripts, Notebooks und Pipelines hinzufügen.
* **Einführung in Synapse Studio**: Mit dieser Option können Sie sich einen kurzen Überblick über die Hauptbereiche von Synapse Studio verschaffen. Dies ist hilfreich, wenn Sie Synapse Studio noch nie verwendet haben.

## <a name="use-samples-immediately-three-samples-to-help-you-get-started-fast"></a>Sofortiges Verwenden von Beispielen: Drei Beispiele für den schnellen Einstieg

Dieser Abschnitt enthält drei Elemente:
* Beispieldaten mit Spark erkunden
* Daten mit SQL abfragen
* Externe Tabelle mit SQL erstellen

1. Klicken Sie im **Knowledge Center** auf **Use samples immediately** (Beispiele sofort verwenden).
1. Wählen Sie **Daten mit SQL abfragen** aus.
1. Klicken Sie auf **Beispiel verwenden**.
1. Ein neues SQL-Beispielskript wird geöffnet.
1. Scrollen Sie zur ersten Abfrage (Zeilen 28 bis 32), und wählen Sie den Abfragetext aus.
1. Klicken Sie auf "Ausführen". Nur der von Ihnen ausgewählte Code wird ausgeführt.

## <a name="gallery-a-collectiopn-of-sample-data-sets-and-sample-code"></a>Katalog: Eine Sammlung von Beispieldatasets und Beispielcode

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

> [!div class="nextstepaction"]
> [Hinzufügen eines Administrators](get-started-add-admin.md)

