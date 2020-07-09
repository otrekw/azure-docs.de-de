---
title: In Azure Analysis Services unterstützte Datenquellen | Microsoft-Dokumentation
description: Beschreibt Datenquellen und Connectors, die für tabellarische Datenmodelle mit dem Kompatibilitätsgrad 1200 und höher in Azure Analysis Services unterstützt werden.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5b0639179c21aefebc44222c9efaa4cd8d9c82f5
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85921902"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>In Azure Analysis Services unterstützte Datenquellen

Datenquellen und -connectors, die im Datenabruf- oder Tabellenimport-Assistenten in Visual Studio mit Analysis Services-Projekten angezeigt werden, werden sowohl für Azure Analysis Services als auch für SQL Server Analysis Services angezeigt. Allerdings werden nicht alle angezeigten Datenquellen und -connectors in Azure Analysis Services unterstützt. Mit welchen Arten von Datenquellen Sie eine Verbindung herstellen können, ist von vielen Faktoren abhängig. Hierzu zählen etwa der Modellkompatibilitätsgrad, verfügbare Datenconnectors, der Authentifizierungstyp und die Unterstützung lokaler Datengateways. In den folgenden Tabellen werden die unterstützten Datenquellen für Azure Analysis Services beschrieben:

## <a name="azure-data-sources"></a>Azure-Datenquellen

|Datenquelle  |In-Memory  |DirectQuery  |Notizen |
|---------|---------|---------|---------|
|Azure SQL-Datenbank      |   Ja      |    Ja      |<sup>[2](#azprovider)</sup>, <sup>[3](#azsqlmanaged)</sup>|
|Azure Synapse Analytics (SQL Data Warehouse)      |   Ja      |   Ja       |<sup>[2](#azprovider)</sup>|
|Azure Blob Storage      |   Ja       |    Nein      | <sup>[1](#tab1400a)</sup> |
|Azure Table Storage     |   Ja       |    Nein      | <sup>[1](#tab1400a)</sup>|
|Azure Cosmos DB     |  Ja        |  Nein        |<sup>[1](#tab1400a)</sup> |
|Azure Data Lake Store Gen1      |   Ja       |    Nein      |<sup>[1](#tab1400a)</sup> |
|Azure Data Lake Store Gen2       |   Ja       |    Nein      |<sup>[1](#tab1400a)</sup>, <sup>[5](#gen2)</sup>|
|Azure HDInsight HDFS    |     Ja     |   Nein       |<sup>[1](#tab1400a)</sup> |
|Azure HDInsight Spark     |   Ja       |   Nein       |<sup>[1](#tab1400a)</sup>, <sup>[4](#databricks)</sup>|
||||

**Hinweise:**

<a name="tab1400a">1</a>: Nur für tabellarische Modelle 1400 und höhere.  
<a name="azprovider">2</a>: Bei Angabe als *Anbieterdatenquelle* in tabellarischen Modellen ab 1200 ist sowohl für In-Memory- als auch für DirectQuery-Modelle der Microsoft OLE DB-Treiber für SQL Server (MSOLEDBSQL; empfohlen), SQL Server Native Client 11.0 oder der .NET Framework-Datenanbieter für SQL Server erforderlich.  
<a name="azsqlmanaged">3</a>: Verwaltete Azure SQL-Instanz wird unterstützt. Da eine verwaltete Instanz innerhalb von Azure VNet mit einer privaten IP-Adresse ausgeführt wird, muss der öffentliche Endpunkt für die Instanz aktiviert sein. Andernfalls ist ein [lokales Datengateway](analysis-services-gateway.md) erforderlich.  
<a name="databricks">4</a>: Azure Databricks mit Spark-Connector wird derzeit nicht unterstützt.  
<a name="gen2">5</a>: Der ADLS Gen2-Connector wird derzeit zwar nicht unterstützt, aber der Azure Blob Storage-Connector kann mit einer ADLS Gen2-Datenquelle verwendet werden.

## <a name="other-data-sources"></a>Weitere Datenquellen

|Datenquelle | In-Memory | DirectQuery |Notizen   |
|  --- | --- | --- | --- |
|Access-Datenbank     |  Ja | Nein |  |
|Active Directory     |  Ja | Nein | <sup>[6](#tab1400b)</sup>  |
|Analysis Services     |  Ja | Nein |  |
|Analyseplattformsystem     |  Ja | Nein |  |
|CSV-Datei  |Ja | Nein |  |
|Dynamics 365     |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|Excel-Arbeitsmappe     |  Ja | Nein |  |
|Exchange      |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|Ordner      |Ja | Nein | <sup>[6](#tab1400b)</sup> |
|IBM Informix  |Ja | Nein |  |
|JSON-Dokument      |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|Zeilen aus Binärdatei      | Ja | Nein | <sup>[6](#tab1400b)</sup> |
|MySQL-Datenbank     | Ja | Nein |  |
|OData-Feed      |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|ODBC-Abfrage     | Ja | Nein |  |
|OLE DB     |   Ja | Nein |  |
|Oracle  | Ja  |Ja  | <sup>[9](#oracle)</sup> |
|PostgreSQL-Datenbank   | Ja | Nein | <sup>[6](#tab1400b)</sup> |
|Salesforce-Objekte|  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|Salesforce-Berichte |Ja | Nein | <sup>[6](#tab1400b)</sup> |
|SAP HANA     |  Ja | Nein |  |
|SAP Business Warehouse    |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
|SharePoint-Liste      |   Ja | Nein | <sup>[6](#tab1400b)</sup>, <sup>[11](#filesSP)</sup> |
|SQL Server |Ja   | Ja  | <sup>[7](#sqlim)</sup>, <sup>[8](#instgw)</sup> |
|SQL Server Data Warehouse |Ja   | Ja  | <sup>[7](#sqlim)</sup>, <sup>[8](#instgw)</sup> |
|Sybase-Datenbank     |  Ja | Nein |  |
|Teradata | Ja  | Ja  | <sup>[10](#teradata)</sup> |
|TXT-Datei  |Ja | Nein |  |
|XML-Tabelle    |  Ja | Nein | <sup>[6](#tab1400b)</sup> |
| | | |

**Hinweise:**  
<a name="tab1400b">6</a>: Nur für tabellarische Modelle ab 1400.  
<a name="sqlim">7</a>: Geben Sie bei Angabe als *Anbieterdatenquelle* in tabellarischen Modellen ab 1200 den Microsoft OLE DB-Treiber für SQL Server (MSOLEDBSQL; empfohlen), SQL Server Native Client 11.0 oder den .NET Framework-Datenanbieter für SQL Server an.  
<a name="instgw">8</a>: Bei Angabe von MSOLEDBSQL als Datenanbieter muss ggf. der [Microsoft OLE DB-Treiber für SQL Server](https://docs.microsoft.com/sql/connect/oledb/oledb-driver-for-sql-server) heruntergeladen und auf dem Computer installiert werden, auf dem sich auch das lokale Datengateway befindet.  
<a name="oracle">9</a>: Geben Sie für tabellarische Modelle vom Typ 1200 oder als *Anbieterdatenquelle* in tabellarischen Modellen ab 1400 den Oracle-Datenanbieter für .NET an.  
<a name="teradata">10</a>: Geben Sie für tabellarische Modelle vom Typ 1200 oder als *Anbieterdatenquelle* in tabellarischen Modellen ab 1400 den Teradata-Datenanbieter für .NET an.  
<a name="filesSP">11</a>: Dateien in der lokalen SharePoint-Instanz werden nicht unterstützt.

Zum Herstellen einer Verbindung zwischen lokalen Datenquellen und dem Azure Analysis Services-Server ist ein [lokales Gateway](analysis-services-gateway.md) erforderlich. Wenn Sie ein Gateway verwenden, sind 64-Bit-Anbieter erforderlich.

## <a name="understanding-providers"></a>Grundlegendes zu Anbietern

Bei der Erstellung eines tabellarischen Modellprojekts ab 1400 in Visual Studio geben Sie standardmäßig keinen Datenanbieter an, wenn mithilfe von **Daten abrufen** eine Verbindung mit einer Datenquelle hergestellt wird. Tabellarische Modelle ab 1400 verwenden [Power Query](/power-query/power-query-what-is-power-query)-Connectors, um Verbindungen, Datenabfragen und Mashups zwischen Datenquelle und Analysis Services zu verwalten. Diese werden gelegentlich als *strukturierte* Datenquellenverbindungen bezeichnet, da Verbindungseigenschaftseinstellungen für Sie festgelegt werden. Sie können jedoch Legacydatenquellen für ein Modellprojekt in Visual Studio aktivieren. In diesem Fall können Sie den **Tabellenimport-Assistenten** verwenden, um eine Verbindung mit bestimmten Datenquellen herzustellen, die in tabellarischen Modellen bis 1200 traditionell als *Legacy-* oder *Anbieterdatenquelle* unterstützt werden. Bei Angabe als Anbieterdatenquelle können Sie einen bestimmten Datenanbieter sowie andere erweiterte Verbindungseigenschaften angeben. So können Sie beispielsweise eine Verbindung mit einer SQL Server Data Warehouse-Instanz oder sogar mit einer Azure SQL-Datenbank als Legacydatenquelle herstellen. Anschließend können Sie den Datenanbieter „OLE DB-Treiber für SQL Server“ (MSOLEDBSQL) auswählen. In diesem Fall kann durch die Wahl eines OLE DB-Datenanbieters anstelle des Power Query-Connectors ggf. eine Verbesserung der Leistung erzielt werden. 

Wenn Sie den Tabellenimport-Assistenten in Visual Studio verwenden, ist für Verbindungen mit einer Datenquelle ein Datenanbieter erforderlich. Es wird automatisch ein Standarddatenanbieter ausgewählt. Der Datenanbieter kann bei Bedarf geändert werden. Entscheidende Faktoren für die Wahl des Anbietertyps können die Leistung, die Verwendung von In-Memory-Speicher oder DirectQuery durch das Modell und die Analysis Services-Plattform sein, auf der das Modell bereitgestellt wird.

### <a name="specify-provider-data-sources-in-tabular-1400-and-higher-model-projects"></a>Angeben von Anbieterdatenquellen in tabellarischen Modellprojekten ab 1400

Klicken Sie zum Aktivieren von Anbieterdatenquellen in Visual Studio auf **Tools** > **Optionen** > **Analysis Services Tabular** > **Datenimport**, und wählen Sie **Legacydatenquellen aktivieren** aus.

![Aktivieren von Legacydatenquellen](media/analysis-services-datasource/aas-enable-legacy-datasources.png)

Klicken Sie nach Aktivierung von Legacydatenquellen im **tabellarischen Modell-Explorer** auf **Datenquellen** > **Aus Datenquelle importieren (Legacy)** .

![Legacydatenquellen im tabellarischen Modell-Explorer](media/analysis-services-datasource/aas-import-legacy-datasources.png)

Verwenden Sie genau wie bei tabellarischen Modellprojekten vom Typ 1200 den **Tabellenimport-Assistenten**, um eine Verbindung mit einer Datenquelle herzustellen. Klicken Sie auf der Seite für die Verbindungsherstellung auf **Erweitert**. Geben Sie unter **Erweiterte Eigenschaften festlegen** den Datenanbieter und andere Verbindungseinstellungen an.

![Legacy-Datenquellen: erweiterte Eigenschaften](media/analysis-services-datasource/aas-import-legacy-advanced.png)

## <a name="impersonation"></a>Identitätswechsel
In einigen Fällen muss möglicherweise ein anderes Identitätswechselkonto angegeben werden. Das Identitätswechselkonto kann in Visual Studio oder SQL Server Management Studio (SSMS) angegeben werden.

Für lokale Datenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.
* Wenn Sie die Windows-Authentifizierung verwenden, legen Sie Windows-Benutzername/-Kennwort fest. Bei SQL Server wird die Windows-Authentifizierung mit einem bestimmten Identitätswechselkonto nur für In-Memory-Datenmodelle unterstützt.

Für Clouddatenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.

## <a name="oauth-credentials"></a>OAuth-Anmeldeinformationen

Für tabellarische Modelle mit einem Kompatibilitätsgrad von 1400 und höher, die den In-Memory-Modus verwenden, bieten Azure SQL-Datenbank, Azure Synapse (früher SQL Data Warehouse), Dynamics 365 und SharePoint-Liste Unterstützung für OAuth-Anmeldeinformationen. Azure Analysis Services verwaltet die Tokenaktualisierung für OAuth-Datenquellen, um Timeouts für Aktualisierungsvorgänge mit langer Laufzeit zu vermeiden. Legen Sie Anmeldeinformationen mithilfe von SSMS fest, um gültige Token zu generieren.

Der Direktabfragemodus wird mit OAuth-Anmeldeinformationen nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

* [Lokales Gateway](analysis-services-gateway.md)
* [Manage your server (Verwalten des Servers)](analysis-services-manage.md)
