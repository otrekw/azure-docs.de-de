---
title: Mit VSTS in Kontakt treten
description: Fragen Sie Azure Synapse Analytics mit Visual Studio ab.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/15/2019
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 88dc534b8753311e49cafa9f84705258cdb0883d
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78198621"
---
# <a name="connect-to-azure-synapse-analytics-with-visual-studio-and-ssdt"></a>Herstellen einer Verbindung mit Azure Synapse Analytics mithilfe von Visual Studio und SSDT
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Mit Visual Studio können Sie einen SQL-Pool in Azure Synapse innerhalb weniger Minuten abfragen. Bei dieser Methode wird die SSDT-Erweiterung (SQL Server Data Tools) in Visual Studio 2019 verwendet. 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Einen vorhandenen SQL-Pool. Informationen zur Erstellung finden Sie unter [Erstellen eines SQL-Pools](sql-data-warehouse-get-started-provision.md).
* SSDT für Visual Studio. Wenn Sie über Visual Studio verfügen, verfügen Sie wahrscheinlich bereits über SSDT für Visual Studio. Installationshinweise und -optionen finden Sie unter [Installieren von Visual Studio und SSDT](sql-data-warehouse-install-visual-studio.md).
* Den vollqualifizierten SQL-Servernamen. Informationen zur Ermittlung dieser Informationen finden Sie unter [Herstellen einer Verbindung mit dem SQL-Pool](sql-data-warehouse-connect-overview.md).

## <a name="1-connect-to-your-sql-pool"></a>1. Stellen Sie eine Verbindung mit Ihrem SQL-Pool her.
1. Öffnen Sie Visual Studio 2019.
2. Öffnen Sie SQL Server-Objekt-Explorer, indem Sie **Ansicht** > **SQL Server-Objekt-Explorer** auswählen.
   
    ![SQL Server-Objekt-Explorer](media/sql-data-warehouse-query-visual-studio/open-ssdt.png)
3. Klicken Sie auf das Symbol **SQL Server hinzufügen** .
   
    ![SQL Server hinzufügen](media/sql-data-warehouse-query-visual-studio/add-server.png)
4. Füllen Sie die Felder im Fenster zum Herstellen einer Verbindung mit dem Server aus.
   
    ![Verbindung mit Server herstellen](media/sql-data-warehouse-query-visual-studio/connection-dialog.png)
   
   * **Servername**. Geben Sie den zuvor ermittelten **Servernamen** ein.
   * **Authentifizierung**. Wählen Sie SQL **Server-Authentifizierung** oder **Integrierte Active Directory-Authentifizierung**.
   * **Benutzername** und **Kennwort**. Geben Sie Benutzername und Kennwort ein, wenn Sie oben „SQL Server-Authentifizierung“ ausgewählt haben.
   * Klicken Sie auf **Verbinden**.
5. Erweitern Sie den Azure SQL-Server. Sie können die dem Server zugeordneten Datenbanken anzeigen. Erweitern Sie „AdventureWorksDW“, um die Tabellen in Ihrer Beispieldatenbank anzuzeigen.
   
    ![AdventureWorksDW erkunden](media/sql-data-warehouse-query-visual-studio/explore-sample.png)

## <a name="2-run-a-sample-query"></a>2. Ausführen einer Beispielabfrage
Nachdem jetzt eine Verbindung mit Ihrer Datenbank hergestellt wurde, schreiben wir eine Abfrage.

1. Klicken Sie mit der rechten Maustaste im SQL Server-Objekt-Explorer auf Ihre Datenbank.
2. Wählen Sie **Neue Abfrage** aus. Ein neues Abfragefenster wird geöffnet.
   
    ![Neue Abfrage](media/sql-data-warehouse-query-visual-studio/new-query2.png)
3. Kopieren Sie die folgende TSQL-Abfrage in das Abfragefenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Führen Sie die Abfrage aus, indem Sie auf den grünen Pfeil klicken oder die folgende Tastenkombination drücken: `CTRL`+`SHIFT`+`E`.
   
    ![Abfrage ausführen](media/sql-data-warehouse-query-visual-studio/run-query.png)
5. Sehen Sie sich die Abfrageergebnisse an. In diesem Beispiel weist die Tabelle „FactInternetSales“ 60398 Zeilen auf.
   
    ![Abfrageergebnisse](media/sql-data-warehouse-query-visual-studio/query-results.png)

## <a name="next-steps"></a>Nächste Schritte
Nun da Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [die Daten mit Power BI zu visualisieren](sql-data-warehouse-get-started-visualize-with-power-bi.md).

Informationen zum Konfigurieren der Umgebung für die Azure Active Directory-Authentifizierung finden Sie unter [Authentifizieren beim SQL-Pool](sql-data-warehouse-authentication.md).