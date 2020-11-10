---
title: Schützen von Anmeldeinformationen für den Zugriff mit verknüpften Diensten in Apache Spark für Azure Synapse Analytics
description: In den in diesem Artikel enthaltenen Konzepten wird beschrieben, wie Apache Spark für Synapse Analytics mithilfe von verknüpften Diensten und der Tokenbibliothek sicher in andere Dienste integriert werden kann.
services: synapse-analytics
author: mlee3gsd
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: spark
ms.date: 08/26/2020
ms.author: martinle
ms.reviewer: euang
ms.openlocfilehash: 1cdb010e34674d52ebe2135ad1591a163a078708
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324961"
---
# <a name="securing-your-credentials-through-linked-services-with-the-tokenlibrary"></a>Schützen Ihrer Anmeldeinformationen mithilfe von verknüpften Diensten und der Tokenbibliothek
Es kommt häufig vor, dass auf Daten aus externen Quellen zugegriffen werden muss. Wenn die externe Datenquelle keinen anonymen Zugriff zulässt, müssen Sie die Verbindung wahrscheinlich mithilfe von Anmeldeinformationen, einem geheimen Schlüssel oder einer Verbindungszeichenfolge absichern.  

Azure Synapse Analytics bietet verknüpfte Dienste, um den Integrationsprozess zu vereinfachen. Dabei werden die Verbindungsdetails in einem verknüpften Dienst oder in Azure Key Vault gespeichert. Nachdem Sie einen verknüpften Dienst erstellt haben, kann Apache Spark auf den verknüpften Dienst zurückgreifen, um die Verbindungsinformationen im Code anzuwenden. 

Weitere Informationen finden Sie unter [Verknüpfte Dienste](../../data-factory/concepts-linked-services.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).
> [!NOTE]
> Wenn Sie innerhalb Ihres Arbeitsbereichs auf Dateien aus Azure Data Lake Storage zugreifen, wird AAD-Pass-Through für die Authentifizierung verwendet. Daher ist es nicht erforderlich, die Tokenbibliothek zu verwenden. 


## <a name="prerequisite"></a>Voraussetzung
* Verknüpfter Dienst: Sie müssen einen verknüpften Dienst für die externe Datenquelle erstellen und aus der Tokenbibliothek auf den verknüpften Dienst verweisen. Erfahren Sie mehr über [verknüpfte Dienste](../../data-factory/concepts-linked-services.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).


## <a name="connect-to-adls-gen2-outside-of-synapse-workspace"></a>Herstellen einer Verbindung mit ADLS Gen2 außerhalb des Synapse-Arbeitsbereichs

Synapse bietet eine integrierte Funktion für verknüpfte Dienste für Azure Data Lake Storage Gen2.

```scala
// Scala code
val sc = spark.sparkContext
sc.hadoopConfiguration.set("spark.storage.synapse.linkedServiceName", "<LINKED SERVICE NAME>")
sc.hadoopConfiguration.set("fs.azure.account.auth.type", "SAS")
sc.hadoopConfiguration.set("fs.azure.sas.token.provider.type", "com.microsoft.azure.synapse.tokenlibrary.LinkedServiceBasedSASProvider")

val df = spark.read.csv("abfss://<CONTAINER>@<ACCOUNT>.dfs.core.windows.net/<DIRECTORY PATH>")

df.show()
```

```python
# Python code
sc._jsc.hadoopConfiguration().set("spark.storage.synapse.linkedServiceName", "<lINKED SERVICE NAME>")
sc._jsc.hadoopConfiguration().set("fs.azure.account.auth.type", "SAS")
sc._jsc.hadoopConfiguration().set("fs.azure.sas.token.provider.type", "com.microsoft.azure.synapse.tokenlibrary.LinkedServiceBasedSASProvider")

df = spark.read.csv("abfss://<CONTAINER>@<ACCOUNT>.dfs.core.windows.net/<DIRECTORY PATH>")

df.show()
```
## <a name="use-the-token-library"></a>Verwenden der Tokenbibliothek

Zum Herstellen einer Verbindung mit anderen verknüpften Diensten können Sie die Tokenbibliothek (TokenLibrary) direkt aufrufen.

### <a name="getconnectionstring"></a>GetConnectionString
 Um die Verbindungszeichenfolge abzurufen, verwenden Sie die Funktion <b>getConnectionString</b> und übergeben den <b>Namen des verknüpften Diensts</b>.

```scala
// Scala
// retrieve connectionstring from TokenLibrary

import com.microsoft.azure.synapse.tokenlibrary.TokenLibrary

val connectionString: String = TokenLibrary.getConnectionString("<LINKED SERVICE NAME>")
println(connectionString)
```

```python
# Python
# retrieve connectionstring from TokenLibrary

from pyspark.sql import SparkSession

sc = SparkSession.builder.getOrCreate()
token_library = sc._jvm.com.microsoft.azure.synapse.tokenlibrary.TokenLibrary
connection_string = token_library.getConnectionString("<LINKED SERVICE NAME>")
print(connection_string)
```
```csharp
// C#
// retrieve connectionstring from TokenLibrary

using Microsoft.Spark.Extensions.Azure.Synapse.Analytics.Utils;

string connectionString = TokenLibrary.GetConnectionString(<LINKED SERVICE NAME>);
Console.WriteLine(connectionString);
```

### <a name="getconnectionstringasmap"></a>GetConnectionStringAsMap
Um bestimmte Werte aus einem <i>key=value</i>-Paar in einer Verbindungszeichenfolge wie 

<i>DefaultEndpointsProtocol=https;AccountName=\<AccountName>;AccountKey=\<AccountKey></i>

zu analysieren, verwenden Sie die Funktion <b>getConnectionStringAsMap</b> und übergeben den Schlüssel, um den Wert zurückzugeben.
```scala
// Linked services can be used for storing and retreiving credentials (e.g, account key)
// Example connection string (for storage): "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
import com.microsoft.azure.synapse.tokenlibrary.TokenLibrary

val accountKey: String = TokenLibrary.getConnectionStringAsMap("<LINKED SERVICE NAME">).get("<KEY NAME>")
println(accountKey)
```

```python
# Linked services can be used for storing and retreiving credentials (e.g, account key)
# Example connection string (for storage): "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
from pyspark.sql import SparkSession

sc = SparkSession.builder.getOrCreate()
token_library = sc._jvm.com.microsoft.azure.synapse.tokenlibrary.TokenLibrary
accountKey = token_library.getConnectionStringAsMap("<LINKED SERVICE NAME>").get("<KEY NAME>")
print(accountKey)
```

## <a name="next-steps"></a>Nächste Schritte

- [Schreiben in einen dedizierten SQL-Pool](./synapse-spark-sql-pool-import-export.md)

