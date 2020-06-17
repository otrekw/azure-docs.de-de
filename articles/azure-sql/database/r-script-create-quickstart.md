---
title: Erstellen und Ausführen einfacher R-Skripts
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Ausführen einfacher R-Skripts in Machine Learning Services von Azure SQL-Datenbank (Vorschauversion).
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 9b78b696b42431c744c30c91a730fdc7ec8c1032
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84324605"
---
# <a name="quickstart-create-and-run-simple-r-scripts-in-azure-sql-database-machine-learning-services-preview"></a>Schnellstart: Erstellen und Ausführen einfacher R-Skripts in Machine Learning Services von Azure SQL-Datenbank (Vorschauversion)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

In dieser Schnellstartanleitung verwenden Sie Machine Learning Services (mit R) in Azure SQL-Datenbank, um einige R-Skripts zu erstellen und auszuführen.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- Eine [Datenbank in Azure SQL-Datenbank](single-database-create-quickstart.md) mit einer [Firewallregel auf Serverebene](firewall-create-server-level-portal-quickstart.md)
- [Machine Learning Services](machine-learning-services-overview.md) mit aktiviertem R.
- [SQL Server Management Studio](/sql/ssms/sql-server-management-studio-ssms) (SSMS)

In diesem Beispiel wird die gespeicherte Prozedur [sp_execute_external_script](/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) verwendet, um ein wohlgeformtes R-Skript zu umschließen.

## <a name="run-a-simple-script"></a>Ausführen eines einfachen Skripts

Sie können ein R-Skript ausführen, indem Sie es als Argument an die gespeicherte Systemprozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) übergeben.

In den folgenden Schritten führen Sie dieses R-Beispielskript in Ihrer Datenbank aus:

```r
a <- 1
b <- 2
c <- a/b
d <- a*b
print(c(c, d))
```

1. Öffnen Sie **SQL Server Management Studio**, und stellen Sie eine Verbindung mit Ihrer Datenbank her.

   Wenn Sie Unterstützung beim Herstellen der Verbindung benötigen, nutzen Sie die Informationen in [Schnellstart: Verwenden von SQL Server Management Studio zum Herstellen einer Verbindung mit einer Datenbank in Azure SQL-Datenbank und zum Abfragen der Datenbank](connect-query-ssms.md).

1. Übergeben Sie das vollständige R-Skript an die gespeicherte Prozedur [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).

   Das Skript wird durch das `@script`-Argument übergeben. Das `@script`-Argument darf nur aus zulässigem R-Code bestehen.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

   Falls Fehler auftreten, kann dies daran liegen, dass die öffentliche Vorschauversion von Machine Learning Services (mit R) für Ihre Datenbank nicht aktiviert ist. Weitere Informationen zu den [Voraussetzungen](#prerequisites) finden Sie oben.

   > [!NOTE]
   > Wenn Sie Administrator sind, können Sie externen Code automatisch ausführen. Sie können mit dem folgenden Befehl anderen Benutzern eine Berechtigung erteilen:
   <br>**GRANT EXECUTE ANY EXTERNAL SCRIPT TO** *\<username\>* .

2. Das korrekte Ergebnis wird berechnet, und die R-Funktion `print` gibt das Ergebnis im **Meldungsfenster** zurück.

   Die Ausgabe könnte beispielsweise wie folgt aussehen:

    **Ergebnisse**

    ```text
    STDOUT message(s) from external script:
    0.5 2
    ```

## <a name="run-a-hello-world-script"></a>Ausführen eines „Hello World“-Skripts

In einem typischen Beispielskript wird nur die Zeichenfolge „Hallo Welt“ (oder „Hello World“) ausgegeben. Führen Sie den folgenden Befehl aus.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'OutputDataSet<-InputDataSet'
    , @input_data_1 = N'SELECT 1 AS hello'
WITH RESULT SETS(([Hello World] INT));
GO
```

Zu den Eingaben, die der gespeicherten Prozedur übergeben werden, zählen:

| | |
|-|-|
| @language | definiert die aufzurufende Spracherweiterung (hier R) |
| @script | definiert die Befehle, die an die R-Runtime übergeben werden Ihr gesamtes R-Skript muss als Unicode-Text in dieses Argument eingeschlossen werden. Sie können den Text auch einer Variablen des Typs **nvarchar** hinzufügen und die Variable anschließend aufrufen. |
| @input_data_1 | Die von der Abfrage zurückgegebenen Daten werden an die R-Runtime übergeben, die die Daten als Datenrahmen zurückgibt. |
|WITH RESULT SETS | Mit dieser Klausel wird das Schema der zurückgegebenen Datentabelle definiert und „Hello World“ als Spaltenname und **int** als Datentyp festgelegt. |

Der Befehl gibt folgenden Text aus:

| Hello World |
|-------------|
| 1 |

## <a name="use-inputs-and-outputs"></a>Verwenden von Eingaben und Ausgaben

Standardmäßig akzeptiert [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) ein einzelnes Dataset als Eingabe, das Sie normalerweise in Form einer gültigen SQL-Abfrage bereitstellen. Anschließend wird ein einzelner R-Datenrahmen als Ausgabe zurückgegeben.

Hier verwenden wir die Standardvariablen für die Ein- und Ausgabe von [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql): **InputDataset** und **OutputDataset**.

1. Erstellen Sie eine kleine Tabelle mit Testdaten.

    ```sql
    CREATE TABLE RTestData (col1 INT NOT NULL)
    
    INSERT INTO RTestData
    VALUES (1);
    
    INSERT INTO RTestData
    VALUES (10);
    
    INSERT INTO RTestData
    VALUES (100);
    GO
    ```

1. Verwenden Sie die `SELECT`-Anweisung zum Abfragen der Tabelle.
  
    ```sql
    SELECT *
    FROM RTestData
    ```

    **Ergebnisse**

    ![Inhalt der RTestData-Tabelle](./media/r-script-create-quickstart/select-rtestdata.png)

1. Führen Sie folgendes R-Skript aus: Es ruft die Daten mithilfe der `SELECT`-Anweisung aus der Tabelle ab, übergibt sie über die R-Runtime und gibt sie anschließend als Datenrahmen zurück. Mit der `WITH RESULT SETS`-Klausel wird das Schema der für SQL-Datenbank zurückgegebenen Datentabelle definiert und der Spaltenname *NewColName* hinzugefügt.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'OutputDataSet <- InputDataSet;'
        , @input_data_1 = N'SELECT * FROM RTestData;'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    **Ergebnisse**

    ![Ausgabe des R-Skripts, das Daten aus einer Tabelle zurückgibt](./media/r-script-create-quickstart/r-output-rtestdata.png)

1. Ändern Sie nun die Namen der Eingabe- und Ausgabevariablen. Standardmäßig werden die Eingabe- und Ausgabevariablen **InputDataSet** und **OutputDataSet** verwendet. In diesem Skript werden die Namen in **SQL_in** und **SQL_out** geändert:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N' SQL_out <- SQL_in;'
        , @input_data_1 = N' SELECT 12 as Col;'
        , @input_data_1_name = N'SQL_in'
        , @output_data_1_name = N'SQL_out'
    WITH RESULT SETS(([NewColName] INT NOT NULL));
    ```

    Beachten Sie, dass R Groß- und Kleinschreibung beachtet. Die im R-Skript verwendeten Ein- und Ausgabevariablen (**SQL_out**, **SQL_in**) müssen unter Berücksichtigung von Groß-/Kleinschreibung den mit `@input_data_1_name` und `@output_data_1_name` definierten Werten entsprechen.

   > [!TIP]
   > Nur ein Eingabedataset kann als Parameter übergeben werden, und Sie können nur ein Dataset zurückgeben. Sie können aber andere Datasets in Ihrem R-Code aufrufen und zusätzlich zum Dataset Ausgaben anderer Typen zurückgeben. Außerdem können Sie auch allen Parametern das Schlüsselwort OUTPUT hinzufügen, damit es zusammen mit den Ergebnissen zurückgegeben wird.

1. Sie können über das R-Skript auch Werte ohne Eingabedaten generieren, indem kein Wert für `@input_data_1` festgelegt wird.

   Das folgende Skript gibt den Text „hello“ und „world“ aus.

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    mytextvariable <- c("hello", " ", "world");
    OutputDataSet <- as.data.frame(mytextvariable);
    '
        , @input_data_1 = N''
    WITH RESULT SETS(([Col1] CHAR(20) NOT NULL));
    ```

    **Ergebnisse**

    ![Abfrageergebnisse mit @script als Eingabe](./media/r-script-create-quickstart/r-data-generated-output.png)

## <a name="check-r-version"></a>Überprüfen der R-Version

Wenn Sie ermitteln möchten, welche Version von R in Ihrer Datenbank installiert ist, führen Sie das folgende Skript aus.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'print(version)';
GO
```

Die R-Funktion `print` gibt die Version im **Meldungsfenster** zurück. In der Beispielausgabe unten sehen Sie, dass für SQL-Datenbank in diesem Fall die R-Version 3.4.4 installiert ist.

**Ergebnisse**

```text
STDOUT message(s) from external script:
                   _
platform       x86_64-w64-mingw32
arch           x86_64
os             mingw32
system         x86_64, mingw32
status
major          3
minor          4.4
year           2018
month          03
day            15
svn rev        74408
language       R
version.string R version 3.4.4 (2018-03-15)
nickname       Someone to Lean On
```

## <a name="list-r-packages"></a>Auflisten von R-Paketen

Microsoft stellt verschiedene R-Pakete bereit, die mit Machine Learning Services in Ihrer Datenbank vorinstalliert werden.

Führen Sie folgendes Skript aus, um eine Liste der installierten R-Pakete (einschließlich Version, Abhängigkeiten, Lizenz und Bibliothekspfad) anzuzeigen:

```SQL
EXEC sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((
            Package NVARCHAR(255)
            , Version NVARCHAR(100)
            , Depends NVARCHAR(4000)
            , License NVARCHAR(1000)
            , LibPath NVARCHAR(2000)
            ));
```

Die Ausgabe stammt von `installed.packages()` in R und wird als Resultset zurückgegeben.

**Ergebnisse**

![Installierte Pakete in R](./media/r-script-create-quickstart/r-installed-packages.png)

## <a name="next-steps"></a>Nächste Schritte

Um ein Machine Learning-Modell mithilfe von R in einer SQL-Datenbank-Instanz zu erstellen, befolgen Sie diese Schnellstartanleitung:

> [!div class="nextstepaction"]
> [Erstellen und Trainieren eines Vorhersagemodells in R mit Machine Learning Services (Vorschauversion) für Azure SQL-Datenbank](r-train-score-model-create-quickstart.md)

Weitere Informationen zu Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion) finden Sie in den folgenden Artikeln.

- [Machine Learning Services (mit R) in Azure SQL-Datenbank (Vorschauversion)](machine-learning-services-overview.md)
- [Schreiben erweiterter R-Funktionen in Azure SQL-Datenbank mit Machine Learning Services (Vorschauversion)](machine-learning-services-functions.md)
- [Arbeiten mit R- und SQL-Daten in Azure SQL-Datenbank mit Machine Learning Services (Vorschauversion)](machine-learning-services-data-issues.md)
