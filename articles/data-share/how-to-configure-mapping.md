---
title: Konfigurieren einer Datasetzuordnung in Azure Data Share
description: Hier erfahren Sie, wie Sie eine Datasetzuordnung für eine empfangene Freigabe mithilfe von Azure Data Share konfigurieren.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 08/14/2020
ms.openlocfilehash: eed3e8275400a3e677df53b9d62cf0e0bc70271c
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2020
ms.locfileid: "88257831"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Konfigurieren einer Datasetzuordnung für eine empfangene Freigabe in Azure Data Share

In diesem Artikel wird gezeigt, wie eine Datasetzuordnung für eine empfangene Freigabe in Azure Data Share konfiguriert wird. Diese müssen Sie konfigurieren, um einen Zieldatenspeicher für empfangene Daten festzulegen oder um diesen zu ändern.

## <a name="navigate-to-a-received-data-share"></a>Navigieren zu einer empfangenen Datenfreigabe

Navigieren Sie in Azure Data Share zu der empfangenen Freigabe, und klicken Sie auf die Registerkarte **Datasets**. 

![Datasetzuordnung](./media/dataset-mapping.png "Datasetzuordnung") 

Aktivieren Sie das Kontrollkästchen neben dem Dataset, dem Sie ein Ziel zuweisen möchten. Wählen Sie **+ Dem Ziel zuordnen** aus, um einen neuen Zielspeicher auszuwählen. Klicken Sie zuerst auf **Zuordnung aufheben**, wenn das Dataset bereits zugeordnet wurde und Sie den Zieldatenspeicher ändern möchten.

![Dem Ziel zuordnen](./media/dataset-map-target.png "Dem Ziel zuordnen") 

## <a name="select-a-target-store"></a>Auswählen eines Zielspeichers

Wählen Sie den Typ des Zieldatenspeichers aus, in dem die empfangenen Daten gespeichert werden sollen. Bei der auf Momentaufnahmen basierenden Freigabe werden Daten, die in zuvor zugeordneten Speicherkonten bereits vorhanden sind, nicht automatisch in den neuen Zielspeicher verschoben. Wählen Sie für die direkte Freigabe einen Datenspeicher am angegebenen Speicherort aus. Der Speicherort ist das Azure-Rechenzentrum, in dem sich der Quelldatenspeicher des Datenanbieters befindet.

![Zielspeicherkonto](./media/dataset-map-target-sql.png "Zielspeicher") 

## <a name="select-a-file-format-sql-sources-only"></a>Auswählen eines Dateiformats (nur SQL-Quellen)

Wenn die Quelldaten aus einer SQL-basierten Quelle stammen und Sie diese als Datei erhalten möchten, können Sie das Empfangsformat selbst festlegen. 

![Auswählen des Formats](./media/sql-file-formats.png "SQL-Dateiformate")

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Freigeben von Daten finden Sie im Tutorial zum [Freigeben Ihrer Daten](share-your-data.md).



