---
title: Verwenden von R mit Machine Learning Services (Vorschauversion) in Azure SQL-Datenbank zum Abfragen einer Datenbank
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: In diesem Artikel erfahren Sie, wie Sie ein R-Skript mit Machine Learning Services in Azure SQL-Datenbank verwenden, um eine Verbindung mit einer Datenbank in Azure SQL-Datenbank herzustellen und diese mithilfe von Transact-SQL-Anweisungen abzufragen.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: sqldbrb=2
ms.devlang: python
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph, sstein
manager: cgronlun
ms.date: 05/29/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 62f1015bc945b5c01f719d7e96e6446c49ac8b8a
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92675156"
---
# <a name="quickstart-use-r-with-azure-sql-database-machine-learning-services-preview-to-query-a-database"></a>Schnellstart: Verwenden von R mit Machine Learning Services (Vorschauversion) in Azure SQL-Datenbank zum Abfragen einer Datenbank 

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

In dieser Schnellstartanleitung verwenden Sie R mit Machine Learning Services in Azure SQL-Datenbank, um eine Verbindung mit einer Datenbank in Azure SQL-Datenbank herzustellen und mithilfe von T-SQL-Anweisungen Daten abzufragen.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine [Azure SQL-Datenbank](single-database-create-quickstart.md)
- [Machine Learning Services](../managed-instance/machine-learning-services-overview.md) mit aktiviertem R.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

> [!IMPORTANT]
> Die Skripts in diesem Artikel wurden für die Datenbank **Adventure Works** geschrieben.

Machine Learning Services mit R ist ein Feature von Azure SQL-Datenbank zum Ausführen von R-Skripts in der Datenbank. Weitere Informationen zum R-Projekt finden Sie [hier](https://www.r-project.org/).

## <a name="get-the-sql-server-connection-information"></a>Ermitteln von SQL Server-Verbindungsinformationen

Ermitteln Sie die Verbindungsinformationen, die Sie zum Herstellen einer Verbindung mit der Datenbank in Azure SQL-Datenbank benötigen. In den weiteren Verfahren benötigen Sie den vollqualifizierten Server- oder Hostnamen, den Datenbanknamen und die Anmeldeinformationen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Navigieren Sie zur Seite **SQL-Datenbanken** oder **Verwaltete SQL-Instanzen** .

3. Auf der Seite **Übersicht** finden Sie den vollqualifizierten Servernamen für eine Datenbank in Azure SQL-Datenbank neben **Servername** oder den vollqualifizierten Servernamen für eine verwaltete Instanz in Azure SQL Managed Instance neben **Host** . Um den Namen des Servers oder Hosts zu kopieren, zeigen Sie darauf, und wählen Sie das Symbol **Kopieren** aus.

## <a name="create-code-to-query-your-database"></a>Erstellen von Code zum Abfragen der Datenbank

1. Öffnen Sie **SQL Server Management Studio** , und stellen Sie eine Verbindung mit Ihrer Datenbank her.

   Wenn Sie Unterstützung beim Herstellen der Verbindung benötigen, nutzen Sie die Informationen in [Schnellstart: Verwenden von SQL Server Management Studio zum Herstellen einer Verbindung mit einer Datenbank in Azure SQL-Datenbank und zum Abfragen der Datenbank](connect-query-ssms.md).

1. Übergeben Sie das vollständige R-Skript an die gespeicherte Prozedur [sp_execute_external_script](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

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
   > Falls Fehler auftreten, kann dies daran liegen, dass die öffentliche Vorschauversion von Machine Learning Services (mit R) für Ihre Datenbank nicht aktiviert ist. Weitere Informationen zu den [Voraussetzungen](#prerequisites) finden Sie oben.

## <a name="run-the-code"></a>Ausführen des Codes

1. Führen Sie die gespeicherte Prozedur [sp_execute_external_script](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) aus.

1. Stellen Sie sicher, dass die obersten 20 Kategorie-/Produktzeilen im Fenster **Meldungen** zurückgegeben werden.

## <a name="next-steps"></a>Nächste Schritte

- [Entwerfen Ihrer ersten Datenbank in Azure SQL-Datenbank](design-first-database-tutorial.md)
- [Machine Learning Services (mit R) in Azure SQL-Datenbank](../managed-instance/machine-learning-services-overview.md)
- [Erstellen und Ausführen einfacher R-Skripts in Machine Learning Services von Azure SQL-Datenbank (Vorschauversion)](/sql/machine-learning/tutorials/quickstart-r-create-script?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context)