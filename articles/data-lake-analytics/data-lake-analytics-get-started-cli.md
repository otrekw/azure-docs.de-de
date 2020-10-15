---
title: Erstellen und Ausführen von Abfragen in Azure Data Lake Analytics – Azure CLI
description: Hier erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle ein Data Lake Analytics-Konto erstellen und einen U-SQL-Auftrag übermitteln.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 06/18/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: be7f9d66a10f3f0f4a1ae0f9e28e226f268a7f7c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87497442"
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli"></a>Erste Schritte mit Azure Data Lake Analytics mithilfe der Azure-Befehlszeilenschnittstelle

[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle Data Lake Analytics-Konten erstellen und U-SQL-Aufträge und Kataloge übermitteln. Der Auftrag liest eine Datei mit tabulatorgetrennten Werten (TSV) und konvertiert sie in eine Datei mit kommagetrennten Werten (CSV).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Für den Artikel müssen Sie mindestens Version 2.0 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

So melden Sie sich bei Ihrem Azure-Abonnement an

```azurecli
az login
```

Sie werden aufgefordert, zu einer URL zu navigieren und einen Authentifizierungscode einzugeben.  Befolgen Sie anschließend die Anleitung zum Eingeben Ihrer Anmeldeinformationen.

Nachdem Sie sich angemeldet haben, werden mit dem Anmeldebefehl Ihre Abonnements aufgeführt.

Gehen Sie wie folgt vor, um ein bestimmtes Abonnement zu verwenden:

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos

Zum Ausführen von Aufträgen ist ein Data Lake Analytics-Konto erforderlich. Zum Erstellen eines Data Lake Analytics-Kontos müssen Sie die folgenden Elemente angeben:

* **Azure-Ressourcengruppe**: Es muss ein Data Lake Analytics-Konto in einer Azure-Ressourcengruppe erstellt werden. [Azure Resource Manager](../azure-resource-manager/management/overview.md) ermöglicht es Ihnen, mit den Ressourcen in Ihrer Anwendung als Gruppe zu arbeiten. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren oder löschen.  

Gehen Sie wie folgt vor, um die vorhandenen Ressourcengruppen unter Ihrem Abonnement aufzulisten:

```azurecli
az group list
```

So erstellen Sie eine neue Ressourcengruppe:

```azurecli
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* **Name des Data Lake Analytics-Kontos**: Jedes Data Lake Analytics-Konto hat einen Namen.
* **Standort**. Verwenden Sie eines der Azure-Rechenzentren, die Data Lake Analytics unterstützen.
* **Data Lake Store-Standardkonto**: Jedes Data Lake Analytics-Konto verfügt über ein Data Lake Store-Standardkonto.

Gehen Sie wie folgt vor, um das vorhandene Data Lake Store-Konto anzuzeigen:

```azurecli
az dls account list
```

Gehen Sie wie folgt vor, um ein neues Data Lake Store-Konto zu erstellen:

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

Verwenden Sie die folgende Syntax, um ein Data Lake Analytics-Konto zu erstellen:

```azurecli
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Nach dem Erstellen eines Kontos können Sie die folgenden Befehle verwenden, um die Konten aufzulisten und die Kontodetails anzuzeigen:

```azurecli
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"
```

## <a name="upload-data-to-data-lake-store"></a>Hochladen von Daten in den Data Lake-Speicher

In diesem Tutorial verarbeiten Sie einige Suchprotokolle.  Das Suchprotokoll kann entweder in Data Lake Store oder Azure Blob Storage gespeichert werden.

Das Azure-Portal enthält eine Benutzeroberfläche zum Kopieren einiger Beispieldatendateien in das Data Lake Store-Standardkonto. Hierzu gehört auch eine Suchprotokolldatei. Weitere Informationen zum Hochladen von Daten in das Data Lake-Standardspeicherkonto finden Sie unter [Vorbereiten von Quelldaten](data-lake-analytics-get-started-portal.md).

Verwenden Sie die folgenden Befehle, um Dateien mit der Azure CLI hochzuladen:

```azurecli
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics hat auch Zugriff auf Azure Blob Storage.  Informationen zum Hochladen von Daten nach Azure Blob Storage finden Sie unter [Verwenden der Azure-CLI mit Azure Storage](../storage/common/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Übermitteln von Data Lake Analytics-Aufträgen

Die Data Lake Analytics-Aufträge werden in der Sprache U-SQL geschrieben. Weitere Informationen zu U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL](data-lake-analytics-u-sql-get-started.md) und in der [U-SQL language reference](https://docs.microsoft.com/u-sql/) (in englischer Sprache).

### <a name="to-create-a-data-lake-analytics-job-script"></a>So erstellen Sie ein Skript für Data Lake Analytics-Aufträge

Erstellen Sie mit dem folgenden U-SQL-Skript eine Textdatei, und speichern Sie die Textdatei auf der Arbeitsstation:

```usql
@a  =
    SELECT * FROM
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

Mit diesem U-SQL-Skript wird die Quelldatei mithilfe von **Extractors.Tsv()** gelesen, und anschließend wird eine CSV-Datei mithilfe von **Outputters.Csv()** erstellt.

Ändern Sie die beiden Pfade nur, wenn Sie die Quelldatei an einen anderen Speicherort kopieren.  Data Lake Analytics erstellt den Ausgabeordner, falls er nicht vorhanden ist.

Es ist einfacher, für Dateien, die unter Data Lake Store-Standardkonten gespeichert sind, relative Pfade zu verwenden. Sie können aber auch absolute Pfade verwenden.  Beispiel:

```usql
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

Sie müssen absolute Pfade verwenden, um auf Dateien in verknüpften Speicherkonten zuzugreifen.  Die Syntax für Dateien, die unter dem verknüpften Azure-Speicherkonto gespeichert werden, lautet wie folgt:

```usql
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> Azure-Blobcontainer mit öffentlichen Blobs werden nicht unterstützt.
> Azure-Blobcontainer mit öffentlichen Containern werden nicht unterstützt.
>

### <a name="to-submit-jobs"></a>Übermitteln von Aufträgen

Verwenden Sie die folgende Syntax, um einen Auftrag zu übermitteln.

```azurecli
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Beispiel:

```azurecli
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

### <a name="to-list-jobs-and-show-job-details"></a>So listen Sie Aufträge auf und zeigen Auftragsdetails an

```azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

### <a name="to-cancel-jobs"></a>So brechen Sie Aufträge ab

```azurecli
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a>Abrufen von Auftragsergebnissen

Nach Abschluss eines Auftrags können Sie die folgenden Befehle verwenden, um die Ausgabedateien aufzulisten und die Dateien herunterzuladen:

```azurecli
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destination>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs download --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "<Destination Path and File Name>"
```

Beispiel:

```azurecli
az dls fs download --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destination-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Nächste Schritte

* Ein Referenzdokument zur Azure CLI für Data Lake Analytics finden Sie unter [Data Lake Analytics](/cli/azure/dla).
* Ein Referenzdokument zur Azure CLI für Data Lake Storage finden Sie unter [Data Lake Storage](/cli/azure/dls).
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
