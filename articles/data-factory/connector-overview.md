---
title: Übersicht über Azure Data Factory-Connectors
description: Informationen zu den unterstützten Connectors in Data Factory.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/28/2020
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: 9b346c24b6a363e66e56f6c16b70058df3864c88
ms.sourcegitcommit: ba7fafe5b3f84b053ecbeeddfb0d3ff07e509e40
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2020
ms.locfileid: "91945826"
---
# <a name="azure-data-factory-connector-overview"></a>Übersicht über Azure Data Factory-Connectors

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory unterstützt die folgenden Datenspeicher und Formate über die Kopieraktivität, den Datenfluss, die Lookup-Aktivität, die Aktivität „Metadaten abrufen“ und die Löschaktivität. Klicken Sie auf die einzelnen Datenspeicher, um mehr über die unterstützten Funktionen und die entsprechende Konfiguration zu erfahren.

## <a name="supported-data-stores"></a>Unterstützte Datenspeicher

[!INCLUDE [Connector overview](../../includes/data-factory-v2-connector-overview.md)]

## <a name="integrate-with-more-data-stores"></a>Integrieren in weitere Datenspeicher

Azure Data Factory kann sich mit einer größeren Gruppe von Datenspeichern als mit denen in der obigen Liste verbinden. Wenn Sie Daten in/aus einem Datenspeicher verschieben müssen, der nicht in der Liste der integrierten Connectors für Azure Data Factory enthalten ist, finden Sie hier einige Erweiterungsmöglichkeiten:
- Für Datenbanken und Data Warehouses finden Sie in der Regel einen entsprechenden ODBC-Treiber, mit dem Sie den [generischen ODBC-Connector](connector-odbc.md) verwenden können.
- Für SaaS-Anwendungen:
    - Sie können den [generischen REST-Connector](connector-rest.md) verwenden, sofern dieser RESTful-APIs bereitstellt.
    - Sie können den [generischen OData-Connector](connector-odata.md) verwenden, wenn dieser einen OData-Feed bietet.
    - Sie können den [generischen HTTP-Connector](connector-http.md) verwenden, sofern dieser SOAP-APIs bereitstellt.
    - Sie können den [generischen ODBC-Connector](connector-odbc.md) verwenden, sofern dieser einen ODBC-Treiber bietet.
- Prüfen Sie für andere, ob Sie Daten in einen von Azure Data Factory unterstützten Datenspeicher laden oder verfügbar machen können, z. B. Azure Blob/File/FTP/SFTP usw. Lassen Sie Azure Data Factory anschließend eine Wahl treffen. Sie können einen benutzerdefinierten Datenlademechanismus über [Azure Function](control-flow-azure-function-activity.md), [Benutzerdefinierte Aktivität](transform-data-using-dotnet-custom-activity.md), [Datenbanken](transform-data-databricks-notebook.md)/[HDInsight](transform-data-using-hadoop-hive.md), [Webaktivität](control-flow-web-activity.md) usw. aufrufen.

## <a name="supported-file-formats"></a>Unterstützte Dateiformate

Azure Data Factory unterstützt die folgenden Dateiformate. Informationen zu formatbasierten Einstellungen finden Sie in den jeweiligen Artikeln.

- [Avro-Format](format-avro.md)
- [Binärformat](format-binary.md)
- [Common Data Model-Format](format-common-data-model.md)
- [Textformat mit Trennzeichen](format-delimited-text.md)
- [Delta-Format](format-delta.md)
- [Excel-Format](format-excel.md)
- [JSON-Format](format-json.md)
- [ORC-Format](format-orc.md)
- [Parquet-Format](format-parquet.md)
- [XML-Format](format-xml.md)

## <a name="next-steps"></a>Nächste Schritte

- [Kopieraktivität](copy-activity-overview.md)
- [Zuordnungsdatenfluss](concepts-data-flow-overview.md)
- [Lookup-Aktivität](control-flow-lookup-activity.md)
- [Aktivität „Metadaten abrufen“](control-flow-get-metadata-activity.md)
- [Aktivität löschen](delete-activity.md)
