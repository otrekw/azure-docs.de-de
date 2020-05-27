---
title: Synchronisieren von Daten aus Azure SQL Edge (Vorschau) mithilfe der SQL-Datensynchronisierung
description: Hier erfahren Sie, wie Sie Daten aus Azure SQL Edge (Vorschau) mithilfe der Azure SQL-Datensynchronisierung synchronisieren.
keywords: SQL Edge, Daten aus SQL Edge synchronisieren, SQL Edge Datensynchronisierung
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: tutorial
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 6bcdfc3eb09b6c5ed4f8dde4d48a34bee8746e1e
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83593444"
---
# <a name="tutorial-sync-data-from-sql-edge-to-azure-sql-database-by-using-sql-data-sync"></a>Tutorial: Synchronisieren von Daten zwischen SQL Edge und Azure SQL-Datenbank mithilfe der SQL-Datensynchronisierung

In diesem Tutorial erfahren Sie, wie Sie eine *Synchronisierungsgruppe* der Azure SQL-Datensynchronisierung verwenden, um eine inkrementelle Synchronisierung von Daten zwischen Azure SQL Edge und Azure SQL-Datenbank durchzuführen. SQL-Datensynchronisierung ist ein Dienst, der auf Azure SQL-Datenbank basiert und mit dem Sie die ausgewählten Daten bidirektional über mehrere SQL-Datenbanken und SQL Server-Instanzen hinweg synchronisieren können. Weitere Informationen zur SQL-Datensynchronisierung finden Sie unter [Azure SQL-Datensynchronisierung](../sql-database/sql-database-sync-data.md).

Da SQL Edge auf den neuesten Versionen der [SQL Server-Datenbank-Engine](/sql/sql-server/sql-server-technical-documentation/) basiert, kann jeder Mechanismus für die Datensynchronisierung, der auf eine lokale SQL Server-Instanz angewendet werden kann, auch zum Synchronisieren von Daten einer SQL Edge-Instanz genutzt werden, die auf einem Edgegerät ausgeführt wird.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial wird ein Windows-Computer benötigt, der mit dem [Data Sync Agent für die Azure SQL-Datensynchronisierung](../sql-database/sql-database-data-sync-agent.md) konfiguriert ist.

## <a name="before-you-begin"></a>Voraussetzungen

* Erstellen einer Azure SQL-Datenbank Informationen zum Erstellen einer Azure SQL-Datenbank-Instanz über das Azure-Portal finden Sie unter [Erstellen eines Singletons in Azure SQL-Datenbank](../sql-database/sql-database-single-database-get-started.md?tabs=azure-portal).

* Erstellen Sie die erforderlichen Tabellen und die weiteren benötigten Objekte in Ihrer Azure SQL-Datenbank-Bereitstellung.

* Erstellen Sie die erforderlichen Tabellen und Objekte in Ihrer Azure SQL Edge-Bereitstellung. Weitere Informationen finden Sie unter [SQL-Datenbank-DAC-Pakete in SQL Edge](stream-analytics.md).

* Registrieren Sie die Azure SQL Edge-Instanz mit Data Sync Agent für die Azure SQL-Datensynchronisierung. Weitere Informationen finden Sie unter [Hinzufügen einer lokalen SQL Server-Datenbank](../sql-database/sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="sync-data-between-an-azure-sql-database-and-sql-edge"></a>Synchronisieren von Daten zwischen einer Azure SQL-Datenbank und SQL Edge

Das Einrichten der Synchronisierung zwischen einer Azure SQL-Datenbank-Instanz und einer SQL Edge-Instanz mithilfe der SQL-Datensynchronisierung umfasst drei Hauptschritte:  

1. Verwenden Sie das Azure-Portal, um eine Synchronisierungsgruppe zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer Synchronisierungsgruppe](../sql-database/sql-database-get-started-sql-data-sync.md#create-sync-group). Sie können eine einzelne *Hub*-Datenbank verwenden, um mehrere Synchronisierungsgruppen zum Synchronisieren von Daten aus verschiedenen SQL Edge-Instanzen mit einer oder mehreren SQL-Datenbank-Instanzen in Azure zu erstellen.

2. Fügen Sie der Synchronisierungsgruppe Synchronisierungsmitglieder hinzu. Weitere Informationen finden Sie unter [Hinzufügen von Synchronisierungsmitgliedern](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

3. Richten Sie die Synchronisierungsgruppe ein, um die Tabellen auszuwählen, die in die Synchronisierung eingeschlossen werden. Weitere Informationen finden Sie unter [Konfigurieren einer Synchronisierungsgruppe](../sql-database/sql-database-get-started-sql-data-sync.md#add-sync-members).

Nachdem Sie die oben genannten Schritte ausgeführt haben, verfügen Sie über eine Synchronisierungsgruppe, die eine Azure SQL-Datenbank- und eine SQL Edge-Instanz umfasst.

Weitere Informationen zur SQL-Datensynchronisierung finden Sie in den folgenden Artikeln:

* [Datensynchronisierungs-Agent für die Azure SQL-Datensynchronisierung](../sql-database/sql-database-data-sync-agent.md)

* [Bewährte Methoden für die SQL-Datensynchronisierung](../sql-database/sql-database-best-practices-data-sync.md) und [Behandeln von Problemen mit der SQL-Datensynchronisierung](../sql-database/sql-database-troubleshoot-data-sync.md)

* [Überwachen der SQL-Datensynchronisierung mit Azure Monitor-Protokollen](../sql-database/sql-database-sync-monitor-oms.md)

* [Aktualisieren des Synchronisierungsschemas mit Transact-SQL](../sql-database/sql-database-update-sync-schema.md) oder mit [PowerShell](../sql-database/scripts/sql-database-sync-update-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von PowerShell zum Synchronisieren zwischen Azure SQL-Datenbank und Azure SQL Edge](../sql-database/scripts/sql-database-sync-data-between-azure-onprem.md) Ersetzen Sie in diesem Tutorial die Details zur Datenbank `OnPremiseServer` durch die Details für Azure SQL Edge.
