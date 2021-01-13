---
title: Azure Synapse Analytics-Ausgabe für Azure Stream Analytics
description: In diesem Artikel wird Azure Synapse Analytics als Ausgabe für Azure Stream Analytics beschrieben.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 08/25/2020
ms.openlocfilehash: 7e85df8ae67624a253a9fb617629d7355109c210
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98019600"
---
# <a name="azure-synapse-analytics-output-from-azure-stream-analytics"></a>Azure Synapse Analytics-Ausgabe für Azure Stream Analytics

[Azure Synapse Analytics](https://azure.microsoft.com/services/synapse-analytics) ist ein unbegrenzter Analysedienst, der Data Warehousing für Unternehmen mit Big Data-Analysen vereint. 

Bei Azure Stream Analytics-Aufträgen kann die Ausgabe in eine dedizierte SQL-Pooltabelle in Azure Synapse Analytics erfolgen, und es können Durchsatzraten von bis zu 200 MB/s verarbeitet werden. Dies unterstützt die anspruchsvollsten Anforderungen hinsichtlich Echtzeitanalyse und Datenverarbeitung im langsamsten Pfad für Workloads wie z. B. das Erstellen von Berichten und Dashboards.  

Die dedizierte SQL-Pooltabelle muss vorhanden sein, bevor Sie sie Ihrem Stream Analytics-Auftrag als Ausgabe hinzufügen können. Das Schema der Tabelle muss den Feldern und deren Typen in der Ausgabe Ihres Auftrags entsprechen. 

Wenn Sie Azure Synapse als Ausgabe verwenden möchten, müssen Sie sicherstellen, dass Sie das Speicherkonto konfiguriert haben. Navigieren Sie zu den Speicherkontoeinstellungen, um das Speicherkonto zu konfigurieren. Es sind nur die Speicherkontotypen zulässig, die Tabellen unterstützen: „Allgemein V2“ und „Allgemein V1“. Wählen Sie nur den Standard-Tarif aus. Der Premium-Tarif wird nicht unterstützt.

## <a name="output-configuration"></a>Ausgabekonfiguration

Die folgende Tabelle enthält die Eigenschaftennamen und die entsprechenden Beschreibungen zum Erstellen einer Azure Synapse Analytics-Ausgabe.

|Eigenschaftenname|BESCHREIBUNG|
|-|-|
|Ausgabealias |Ein Anzeigename, der in Abfragen verwendet wird, um die Abfrageausgabe an diese Datenbank weiterzuleiten. |
|Datenbank |Der Name des dedizierten SQL-Pools, an den Sie die Ausgabe senden. |
|Servername |Name des Azure Synapse-Servers.  |
|Username |Der Benutzername, der Schreibzugriff auf die Datenbank hat. Stream Analytics unterstützt nur die SQL-Authentifizierung. |
|Kennwort |Das Kennwort zum Herstellen einer Verbindung mit der Datenbank |
|Tabelle  | Der Name der Tabelle, in die die Ausgabe geschrieben wird. Beim Tabellennamen wird die Groß- und Kleinschreibung beachtet. Das Schema dieser Tabelle sollte genau der Anzahl der Felder und deren Typen entsprechen, die Ihre Auftragsausgabe generiert.|

## <a name="next-steps"></a>Nächste Schritte

* [Zugreifen auf Azure SQL-Datenbank oder Azure Synapse Analytics mit verwalteten Identitäten aus einem Azure Stream Analytics-Auftrag (Vorschau)](sql-database-output-managed-identity.md)
* [Schnellstart: Erstellen eines Stream Analytics-Auftrags mithilfe des Azure-Portals](stream-analytics-quick-create-portal.md)