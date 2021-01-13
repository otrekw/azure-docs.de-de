---
title: Herstellen einer Verbindung mit Excel
description: Erfahren Sie, wie Sie Microsoft Excel mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance verbinden. Importieren Sie Daten zwecks Berichterstellung und Untersuchung in Excel.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 05/29/2020
ms.openlocfilehash: 304ad51bc40c8165c2dc11bb70287491e3d6dfb9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91444033"
---
# <a name="connect-excel-to-a-database-in-azure-sql-database-or-azure-sql-managed-instance-and-create-a-report"></a>Verbinden von Excel mit einer Datenbank in Azure SQL-Datenbank oder Azure SQL Managed Instance und Erstellen eines Berichts
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Sie können eine Verbindung von Excel mit einer Datenbank herstellen und dann Daten importieren und Tabellen und Diagramme basierend auf den Werten in der Datenbank erstellen. In diesem Tutorial richten Sie die Verbindung zwischen Excel und einer Datenbanktabelle ein, speichern die Datei, in der die Daten und Verbindungsinformationen für Excel vorgehalten werden, und erstellen aus den Datenbankwerten dann ein PivotChart.

Bevor Sie beginnen können, müssen Sie eine Datenbank erstellen. Wenn Sie keine haben, finden Sie unter [Erstellen einer Datenbank in Azure SQL-Datenbank](single-database-create-quickstart.md) und [Erstellen einer IP-Firewall auf Serverebene](firewall-create-server-level-portal-quickstart.md) eine Datenbank mit Beispieldaten, die in wenigen Minuten einsatzbereit ist.

In diesem Artikel importieren Sie Beispieldaten aus diesem Artikel in Excel, doch Sie können ähnliche Schritte auch mit Ihren eigenen Daten befolgen.

Sie benötigen auch eine Kopie von Excel. In diesem Artikel wird [Microsoft Excel 2016](https://products.office.com/)verwendet.

## <a name="connect-excel-and-load-data"></a>Verbinden von Excel und Laden von Daten

1. Um Excel mit einer Datenbank in SQL-Datenbank zu verbinden, öffnen Sie Excel, und erstellen Sie dann eine neue Arbeitsmappe, oder öffnen Sie eine vorhandene Excel-Arbeitsmappe.
2. Wählen Sie auf der Menüleiste im oberen Bereich der Seite die Registerkarte **Daten**, dann **Daten abrufen**, „From Azure“ (Aus Azure) und schließlich **From Azure SQL Database** (Aus Azure SQL-Datenbank) aus.

   ![Auswählen einer Datenquelle: Verbinden Sie Excel mit SQL-Datenbank.](./media/connect-excel/excel_data_source.png)

3. Geben Sie im Dialogfeld **SQL Server-Datenbank** den **Namen des Servers** ein, mit dem Sie die Verbindung herstellen möchten. Verwenden Sie dabei das Format <*Servername*> **.database.windows.net**. Beispiel: **msftestserver.database.windows.net**. Geben Sie optional den Namen der Datenbank ein. Wählen Sie **OK** aus, um das Fenster der Anmeldeinformationen zu öffnen.

   ![Dialogfeld „Zum Datenbankserver verbinden“](./media/connect-excel/server-name.png)

4. Wählen Sie im Dialogfeld **SQL Server-Datenbank** auf der linken Seite **Datenbank** aus, und geben Sie dann Ihren **Benutzernamen** und das **Kennwort** für den Server ein, mit dem Sie eine Verbindung herstellen möchten. Wählen Sie **Verbinden** aus, um den **Navigator** zu öffnen.

   ![Servername und Anmeldeinformationen eingeben](./media/connect-excel/connect-to-server.png)

   > [!TIP]
   > Je nach Netzwerkumgebung kann es sein, dass Sie keine Verbindung herstellen können, oder die Verbindung geht verloren, wenn der Server keinen Datenverkehr von Ihrer Client-IP-Adresse zulässt. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), klicken Sie nacheinander auf „Server mit SQL Server“, Ihren Server und unter „Einstellungen“ auf „Firewall“, und fügen Sie Ihre Client-IP-Adresse hinzu. Weitere Einzelheiten finden Sie unter [Konfigurieren von Firewalleinstellungen](firewall-configure.md) .

5. Wählen Sie im **Navigator** in der Liste die Datenbank aus, mit der Sie arbeiten möchten, wählen Sie die Tabellen oder Ansichten aus, mit denen Sie arbeiten möchten (hier wurde **vGetAllCategories** ausgewählt), und wählen Sie dann **Laden** aus, um die Daten aus Ihrer Datenbank in Ihr Excel-Arbeitsblatt zu verschieben.

    ![Wählen Sie eine Datenbank und Tabelle aus.](./media/connect-excel/select-database-and-table.png)

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importieren der Daten in Excel und Erstellen eines PivotChart

Nachdem Sie die Verbindung hergestellt haben, stehen mehrere unterschiedliche Optionen zum Laden der Daten zur Verfügung. Mit den folgenden Schritten erstellen Sie beispielsweise ein PivotChart basierend auf den Daten in Ihrer Datenbank in SQL-Datenbank.

1. Führen Sie die Schritte im vorherigen Abschnitt aus, wählen Sie jedoch dieses Mal in der Dropdownliste **Laden** anstelle von **Laden** die Option **Laden in** aus.
2. Wählen Sie als Nächstes das Format aus, in dem Sie diese Daten in der Arbeitsmappe anzeigen möchten. Wir haben hier **PivotChart**gewählt. Sie können auch ein **Neues Arbeitsblatt** erstellen oder die Option **Dem Datenmodell diese Daten hinzufügen** wählen. Weitere Informationen zu Datenmodellen finden Sie unter [Erstellen eines Datenmodells in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B).

    ![Format für Daten in Excel auswählen](./media/connect-excel/import-data.png)

    Das Arbeitsblatt enthält jetzt eine leere PivotTable und ein leeres PivotChart.
3. Aktivieren Sie unter **PivotTable-Felder**alle Kontrollkästchen für die Felder, die Sie anzeigen möchten.

    ![Konfigurieren Sie den Datenbankbericht.](./media/connect-excel/power-pivot-results.png)

> [!TIP]
> Wenn Sie andere Excel-Arbeitsmappen und -Arbeitsblätter mit der Datenbank verbinden möchten, wählen Sie die Registerkarte **Daten** und dann **Zuletzt verwendete Quellen** aus, um das Dialogfeld **Zuletzt verwendete Quellen** zu öffnen. Wählen Sie dort in der Liste die Verbindung aus, die Sie erstellt haben, und klicken Sie dann auf **Öffnen**.
> ![Dialogfeld „Zuletzt verwendete Quellen“](./media/connect-excel/recent-connections.png)

## <a name="create-a-permanent-connection-using-odc-file"></a>Erstellen einer permanenten Verbindung mithilfe einer ODC-Datei

Wenn Sie die Verbindungsdetails dauerhaft speichern möchten, können Sie eine ODC-Datei erstellen und diese Verbindung zu einer Option machen, die im Dialogfeld **Vorhandene Verbindungen** ausgewählt werden kann.

1. Wählen Sie auf der Menüleiste im oberen Bereich der Seite die Registerkarte **Daten** und dann **Vorhandene Verbindungen** aus, um das Dialogfeld **Vorhandene Verbindungen** zu öffnen.
   1. Wählen Sie **Browse for more** (Suche fortsetzen) aus, um das Dialogfeld **Datenquelle auswählen** zu öffnen.
   2. Wählen Sie die Datei **+NewSqlServerConnection.odc** und dann **Öffnen** aus, um den **Datenverbindungs-Assistenten** zu öffnen.

      ![Dialogfeld „Neue Verbindung“](./media/connect-excel/new-connection.png)

2. Geben Sie im **Datenverbindungs-Assistenten** den Servernamen und die Anmeldeinformationen für SQL-Datenbank ein. Wählen Sie **Weiter** aus.
   1. Wählen Sie in der Dropdownliste die Datenbank aus, die die Daten enthält.
   2. Wählen Sie die Tabelle oder Ansicht aus, an der Sie interessiert sind. Hier wurde vGetAllCategories ausgewählt.
   3. Wählen Sie **Weiter** aus.

      ![Datenverbindungs-Assistent](./media/connect-excel/data-connection-wizard.png)

3. Wählen Sie auf dem nächsten Bildschirm des Datenverbindungs-Assistenten den Speicherort der Datei, den **Dateinamen** und den **Anzeigenamen** aus. Sie können auch das Kennwort in der Datei speichern, dadurch können jedoch Ihre Daten möglicherweise unerwünschten Zugriffen ausgesetzt werden. Wählen Sie anschließend **Fertig stellen** aus.

    ![Speichern der Datenverbindung](./media/connect-excel/save-data-connection.png)

4. Wählen Sie aus, wie Ihre Daten importiert werden sollen. Hier wurde eine PivotTable ausgewählt. Sie können auch die Eigenschaften der Verbindung ändern, indem Sie **Eigenschaften** auswählen. Wählen Sie nach Abschluss des Vorgangs **OK** aus. Wenn Sie das Kennwort nicht mit der Datei gespeichert haben, werden Sie aufgefordert, Ihre Anmeldeinformationen einzugeben.

    ![Importieren von Daten](./media/connect-excel/import-data2.png)

5. Überprüfen Sie, ob die neue Verbindung gespeichert wurde, indem Sie die Registerkarte **Daten** erweitern und **Vorhandene Verbindungen** auswählen.

    ![Vorhandene Verbindung](./media/connect-excel/existing-connection.png)

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über das [Herstellen einer Verbindung mit und das Abfragen von SQL Server Management Studio](connect-query-ssms.md), um erweiterte Abfragen und Analysen durchzuführen.
* Erfahren Sie mehr über die Vorteile von [Pools für elastische Datenbanken](elastic-pool-overview.md).
* Erfahren Sie, wie Sie [eine Webanwendung erstellen, die eine Verbindung mit Azure SQL-Datenbank auf dem Back-End herstellt](../../app-service/app-service-web-tutorial-dotnet-sqldatabase.md).
