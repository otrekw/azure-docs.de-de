---
title: 'Schnellstart: Erste Schritte zum Erstellen eines Synapse-Arbeitsbereichs'
description: In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: workspace
ms.topic: tutorial
ms.date: 03/17/2021
ms.openlocfilehash: b22954edf4f3a5a935c470326aa43bd24ee2d708
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107366061"
---
# <a name="creating-a-synapse-workspace"></a>Erstellen eines Synapse-Arbeitsbereichs

In diesem Tutorial erfahren Sie, wie Sie einen Synapse-Arbeitsbereich, einen dedizierten SQL-Pool und einen serverlosen Apache Spark-Pool erstellen. 

## <a name="prerequisites"></a>Voraussetzungen

Um die Schritte dieses Tutorials ausführen zu können, benötigen Sie Zugriff auf eine Ressourcengruppe, für die Ihnen die Rolle **Besitzer** zugewiesen wurde. Erstellen Sie den Synapse-Arbeitsbereich in dieser Ressourcengruppe.

## <a name="create-a-synapse-workspace-in-the-azure-portal"></a>Erstellen eines Synapse-Arbeitsbereichs im Azure-Portal

### <a name="start-the-process"></a>Starten des Prozesses
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com). Geben Sie in der Suchleiste **Synapse** ein, und drücken Sie nicht die EINGABETASTE.
1. Wählen Sie in den Suchergebnissen unter **Dienste** den Eintrag **Azure Synapse Analytics** aus.
1. Wählen Sie **Hinzufügen** aus, um einen Arbeitsbereich zu erstellen.

## <a name="basics-tab--project-details"></a>Registerkarte „Grundlegende Einstellungen“ > „Projektdetails“
Füllen Sie die folgenden Felder aus:

1. **Abonnement**: Wählen Sie ein beliebiges Abonnement.
1. **Ressourcengruppe**: Wählen Sie eine beliebige Ressourcengruppe.
1. **Verwaltete Ressourcengruppe**: Lassen Sie dieses Feld leer.

## <a name="basics-tab--workspace-details"></a>Registerkarte „Grundlegende Einstellungen“ > „Details zum Arbeitsbereich“
Füllen Sie die folgenden Felder aus:

1. **Arbeitsbereichsname**: Wählen Sie einen beliebigen global eindeutigen Namen. In diesem Tutorial wird **myworkspace** verwendet.
1. **Region**: Wählen Sie eine beliebige Region.

Gehen Sie unter **Data Lake Storage Gen 2 auswählen** wie folgt vor:

1. Wählen Sie unter **Kontoname** die Option **Neu erstellen** aus, und geben Sie für das neue Speicherkonto den Namen **contosolake** oder einen ähnlichen Namen ein. Dieser Name muss eindeutig sein.
1. Wählen Sie unter **Dateisystemname** die Option **Neu erstellen** aus, und geben Sie den Namen **Benutzer** ein. Dadurch wird ein Speichercontainer namens **Benutzer** erstellt. Der Arbeitsbereich verwendet dieses Speicherkonto als primäres Speicherkonto für Spark-Tabellen und Spark-Anwendungsprotokolle.
1. Aktivieren Sie das Kästchen „Mir die Rolle "Mitwirkender an Storage-Blobdaten" für das Data Lake Storage Gen2-Konto zuweisen“. 

## <a name="completing-the-process"></a>Abschließen des Vorgangs
Wählen Sie **Bewerten + erstellen** > **Erstellen** aus. Ihr Arbeitsbereich steht nach wenigen Minuten zur Verfügung.

> [!NOTE]
> Wie Sie Arbeitsbereichsfunktionen für einen vorhandenen dedizierten SQL-Pool (ehemals SQL DW) aktivieren, erfahren Sie unter [Aktivieren von Synapse-Arbeitsbereichsfunktionen für einen dedizierten SQL-Pool (ehemals SQL DW)](./sql-data-warehouse/workspace-connected-create.md).


## <a name="open-synapse-studio"></a>Öffnen von Synapse Studio

Nachdem Ihr Azure Synapse-Arbeitsbereich erstellt wurde, haben Sie zwei Möglichkeiten zum Öffnen von Synapse Studio:

* Öffnen Sie Ihren Synapse-Arbeitsbereich im [Azure-Portal](https://portal.azure.com), und wählen Sie im Abschnitt **Übersicht** des Synapse-Arbeitsbereichs im Feld „Synapse Studio öffnen“ den Eintrag **Öffnen** aus.
* Navigieren Sie zu `https://web.azuresynapse.net`, und melden Sie sich bei Ihrem Arbeitsbereich an.

## <a name="place-sample-data-into-the-primary-storage-account"></a>Platzieren von Beispieldaten im primären Speicherkonto
In diesem Leitfaden zu den ersten Schritten verwenden Sie für viele Beispiele ein kleines Beispieldataset mit 100.000 Zeilen mit NYX Taxi-Daten. Sie beginnen damit, das Dataset im primären Speicherkonto zu platzieren, das Sie für den Arbeitsbereich erstellt haben.

* Laden Sie die folgende Datei auf Ihren Computer herunter: https://azuresynapsestorage.blob.core.windows.net/sampledata/NYCTaxiSmall/NYCTripSmall.parquet 
* Navigieren Sie in Synapse Studio zum Hub „Daten“. 
* Wählen Sie **Mit Link** aus.
* In der Kategorie **Azure Data Lake Storae Gen2** wird ein Element mit einem Namen wie **myworkspace (Primär – contosolake)** angezeigt.
* Wählen Sie den Container mit dem Namen **Benutzer (Primär)** aus.
* Wählen Sie **Hochladen** und dann die von Ihnen heruntergeladene Datei `NYCTripSmall.parquet` aus.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Analysieren mithilfe eines serverlosen SQL-Pools](get-started-analyze-sql-on-demand.md)
