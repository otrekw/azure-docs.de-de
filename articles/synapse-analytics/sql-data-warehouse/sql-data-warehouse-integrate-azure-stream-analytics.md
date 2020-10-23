---
title: Verwenden von Azure Stream Analytics
description: Tipps für die Verwendung von Azure Stream Analytics mit Ihrem Data Warehouse in Azure Synapse für die Entwicklung von Echtzeitlösungen.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 9/25/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 60fb258fe2c6063b9b9a3ced0f4ba5f71ffd9d7c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449518"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Verwenden von Azure Stream Analytics mit Azure Synapse Analytics

Azure Stream Analytics ist ein vollständig verwalteter Dienst, der eine geringe Latenz, hohe Verfügbarkeit und eine skalierbare komplexe Ereignisverarbeitung durch das Streaming von Daten in der Cloud bietet. Die Grundlagen finden Sie unter [Einführung in Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). In [Erste Schritte mit Azure Stream Analytics](../../stream-analytics/stream-analytics-real-time-fraud-detection.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) können Sie sich dann mit dem Erstellen einer End-to-End-Lösung mit Stream Analytics vertraut machen.

In diesem Artikel erfahren Sie, wie Sie Ihr Data Warehouse als Ausgabesenke für Datenerfassung mit hohem Durchsatz mit Azure Stream Analytics-Aufträgen verwenden.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure Stream Analytics-Auftrag. Zum Erstellen eines Azure Stream Analytics-Auftrags führen Sie die folgenden Schritte im Tutorial [Erste Schritte mit Azure Stream Analytics](../../stream-analytics/stream-analytics-real-time-fraud-detection.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) aus:  

    1. Erstellen einer Event Hub-Eingabe
    2. Konfigurieren und starten der Ereignisgenerator-Anwendung
    3. Bereitstellen eines Stream Analytics-Auftrags
    4. Festlegen der Auftragseingabe und -abfrage
* Ein Azure Synapse SQL-Pool für Ihr Data Warehouse. Befolgen Sie zum Erstellen eines neuen Data Warehouse die Schritte im [Schnellstart zum Erstellen eines neuen Data Warehouse](create-data-warehouse-portal.md).

## <a name="specify-streaming-output-to-point-to-your-data-warehouse"></a>Festlegen der Streamingausgabe an Ihr Data Warehouse

### <a name="step-1"></a>Schritt 1

Navigieren Sie im Azure-Portal zu Ihrem Stream Analytics Auftrag, und klicken Sie im Menü **Auftragstopologie** auf **Ausgaben**.

### <a name="step-2"></a>Schritt 2

Klicken Sie auf die Schaltfläche **Hinzufügen**, und wählen Sie im Dropdownmenü die Option **Azure Synapse Analytics** aus.

![Auswählen von Azure Synapse Analytics](./media/sql-data-warehouse-integrate-azure-stream-analytics/sql-pool-azure-stream-analytics-output.png)

### <a name="step-3"></a>Schritt 3

Geben Sie die folgenden Werte ein:

* *Ausgabealias:* Geben Sie einen Anzeigenamen für diese Auftragsausgabe ein.
* *Abonnement*:
  * Befindet sich Ihr Data Warehouse im selben Abonnement wie der Stream Analytics-Auftrag, klicken Sie auf ***Select Azure Synapse Analytics from your subscriptions*** (Azure Synapse Analytics aus Abonnements auswählen).
  * Wenn sich das Data Warehouse in einem anderen Abonnement befindet, klicken Sie auf „Provide Azure Synapse Analytics settings manually“ (Azure Synapse Analytics-Einstellungen manuell bereitstellen).
* *Datenbank*: Wählen Sie in der Dropdownliste die Zieldatenbank aus.
* *Benutzername*: Geben Sie den Benutzernamen eines Kontos mit Schreibberechtigungen für die Datenbank an.
* *Kennwort*: Geben Sie das Kennwort für das angegebene Benutzerkonto an.
* *Tabelle*: Geben Sie den Namen der Zieltabelle in der Datenbank an.
* Klicken Sie auf die Schaltfläche **Speichern**.

![Ausgefülltes Azure Synapse Analytics-Formular](./media/sql-data-warehouse-integrate-azure-stream-analytics/sql-pool-azure-stream-analytics-output-db-settings.png)

### <a name="step-4"></a>Schritt 4

Bevor Sie einen Test ausführen können, müssen Sie die Tabelle in Ihrem Data Warehouse erstellen.  Führen Sie das folgende Skript für die Tabellenerstellung mithilfe von SQL Server Management Studio (SSMS) oder mit Ihrem bevorzugten Abfragetool aus.

```sql
CREATE TABLE SensorLog
(
    RecordType VARCHAR(2)
    , SystemIdentity VARCHAR(2)
    , FileNum INT
    , SwitchNum VARCHAR(50)
    , CallingNum VARCHAR(25)
    , CallingIMSI VARCHAR(25)
    , CalledNum VARCHAR(25)
    , CalledIMSI VARCHAR(25)
    , DateS VARCHAR(25)
    , TimeS VARCHAR(25)
    , TimeType INT
    , CallPeriod INT
    , CallingCellID VARCHAR(25)
    , CalledCellID VARCHAR(25)
    , ServiceType VARCHAR(25)
    , [Transfer] INT
    , IncomingTrunk VARCHAR(25)
    , OutgoingTrunk VARCHAR(25)
    , MSRN VARCHAR(25)
    , CalledNum2 VARCHAR(25)
    , FCIFlag VARCHAR(25)
    , callrecTime VARCHAR(50)
    , EventProcessedUtcTime VARCHAR(50)
    , PartitionId int
    , EventEnqueuedUtcTime VARCHAR(50)
    )
WITH (DISTRIBUTION = ROUND_ROBIN)
```

### <a name="step-5"></a>Schritt 5

Klicken Sie im Azure-Portal für Ihren Stream Analytics-Auftrag auf den Auftragsnamen.  Klicken Sie im Bereich ***Ausgabedetails*** auf die Schaltfläche ***Testen***.

![Schaltfläche „Testen“ für Ausgabedetails](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asatest.png): Wenn die Verbindung mit der Datenbank erfolgreich hergestellt wurde, wird im Portal eine entsprechende Meldung angezeigt.

### <a name="step-6"></a>Schritt 6

Klicken Sie unter ***Auftragstopologie*** auf das Menü ***Abfrage***, und ändern Sie die Abfrage so, dass Daten in die von Ihnen erstellte Streamausgabe eingefügt werden.  Klicken Sie auf die Schaltfläche ***Ausgewählte Abfrage testen***, um die Abfrage zu testen.  Klicken Sie auf die Schaltfläche ***Abfrage speichern***, wenn der Abfragetest erfolgreich war.

![Speichern der Abfrage](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asaquery.png)

### <a name="step-7"></a>Schritt 7

Starten Sie den Azure Stream Analytics-Auftrag.  Klicken Sie im Menü ***Übersicht*** auf die Schaltfläche ***Starten***.

![Starten des Stream Analytics-Auftrags](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastart.png)

Klicken Sie im Bereich „Auftrag starten“ auf die Schaltfläche ***Starten***.

![Klicken auf „Starten“](./media/sql-data-warehouse-integrate-azure-stream-analytics/sqlpool-asastartconfirm.png)

## <a name="next-steps"></a>Nächste Schritte

Einen Überblick über die Integration finden Sie unter [Integrieren anderer Dienste](sql-data-warehouse-overview-integrate.md).
Weitere Hinweise zur Entwicklung finden Sie unter [Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse](sql-data-warehouse-overview-develop.md).
