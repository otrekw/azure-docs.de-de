---
title: Abfragen einer Datenbank mithilfe von R und Machine Learning Services (Vorschau)
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In diesem Artikel erfahren Sie, wie Sie ein R-Skript mit Machine Learning Services in Azure SQL-Datenbank verwenden, um eine Verbindung mit einer Azure SQL-Datenbank-Instanz herzustellen und sie mithilfe von Transact-SQL-Anweisungen abzufragen.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, carlrab
manager: cgronlun
ms.date: 05/29/2019
ms.openlocfilehash: 7103afc29e4021d950d9a3634b190f4439ecfe8d
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2020
ms.locfileid: "76768508"
---
# <a name="quickstart-use-r-with-machine-learning-services-to-query-an-azure-sql-database-preview"></a>Schnellstart: Verwenden von R mit Machine Learning Services zum Abfragen von Azure SQL-Datenbank (Vorschauversion)

In dieser Schnellstartanleitung verwenden Sie R mit Machine Learning Services, um eine Verbindung mit einer Azure SQL-Datenbank herzustellen und mithilfe von T-SQL-Anweisungen Daten abzufragen.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine [Azure SQL-Datenbank](sql-database-single-database-get-started.md).
- [Machine Learning Services](sql-database-machine-learning-services-overview.md) mit aktiviertem R. [Registrieren für die Vorschauversion](sql-database-machine-learning-services-overview.md#signup)
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Die Skripts in diesem Artikel wurden für die Datenbank **Adventure Works** geschrieben.

> [!NOTE]
> Während der Public Preview-Phase übernimmt Microsoft das Onboarding für Sie und aktiviert das maschinelle Lernen für Ihre vorhandene oder neue Datenbank. Die Bereitstellungsoption für verwaltete Instanzen wird momentan jedoch nicht unterstützt.

Machine Learning Services mit R ist ein Feature von Azure SQL-Datenbank zum Ausführen von R-Skripts in der Datenbank. Weitere Informationen zum R-Projekt finden Sie [hier](https://www.r-project.org/).

## <a name="get-sql-server-connection-information"></a>Abrufen von SQL Server-Verbindungsinformationen

Rufen Sie die Verbindungsinformationen ab, die Sie zum Herstellen einer Verbindung mit der Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Navigieren Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen**.

3. Sehen Sie sich auf der Seite **Übersicht** bei einer Einzeldatenbank den vollqualifizierten Servernamen neben **Servername** und bei einer verwalteten Instanz den vollqualifizierten Servernamen neben **Host** an. Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

## <a name="create-code-to-query-your-sql-database"></a>Erstellen von Code zum Abfragen Ihrer SQL-Datenbank

1. Öffnen Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit Ihrer SQL-Datenbank her.

   Wenn Sie Unterstützung beim Herstellen der Verbindung benötigen, nutzen Sie die Informationen in [Schnellstart: Verwenden von SQL Server Management Studio zum Herstellen der Verbindung mit einer Instanz von Azure SQL-Datenbank und deren Abfrage](sql-database-connect-query-ssms.md).

1. Übergeben Sie das vollständige R-Skript an die gespeicherte Prozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

   Das Skript wird durch das `@script`-Argument übergeben. Das `@script`-Argument darf nur aus zulässigem R-Code bestehen.
   
   >[!IMPORTANT]
   >Der Code in diesem Beispiel verwendet die AdventureWorksLT-Beispieldaten, die Sie beim Erstellen Ihrer Datenbank als Datenquelle auswählen können. Wenn Ihre Datenbank unterschiedliche Daten aufweist, verwenden Sie Tabellen aus Ihrer eigenen Datenbank in der SELECT-Abfrage. 

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'OutputDataSet <- InputDataSet;'
    , @input_data_1 = N'SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid'
    ```

   > [!NOTE]
   > Falls Sie Fehler erhalten, kann dies daran liegen, dass die öffentliche Vorschauversion von Machine Learning Services (mit R) für Ihre SQL-Datenbank nicht aktiviert ist. Weitere Informationen zu den [Voraussetzungen](#prerequisites) finden Sie oben.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die gespeicherte Prozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) aus.

1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen im Fenster **Meldungen** zurückgegeben werden.

## <a name="next-steps"></a>Nächste Schritte

- [Entwurf Ihrer ersten Azure SQL-Datenbank](sql-database-design-first-database.md)
- [Machine Learning Services (mit R) in Azure SQL-Datenbank](sql-database-machine-learning-services-overview.md)
- [Erstellen und Ausführen einfacher R-Skripts in Machine Learning Services von Azure SQL-Datenbank (Vorschauversion)](sql-database-quickstart-r-create-script.md)
- [Schreiben erweiterter R-Funktionen in Azure SQL-Datenbank mit Machine Learning Services (Vorschauversion)](sql-database-machine-learning-services-functions.md)
