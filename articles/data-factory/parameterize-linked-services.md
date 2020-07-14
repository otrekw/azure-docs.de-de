---
title: Parametrisieren von verknüpften Diensten in Azure Data Factory
description: Erfahren Sie, wie Sie in Azure Data Factory verknüpfte Dienste parametrisieren und dynamische Werte zur Laufzeit übergeben.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 85689661e7f0d170cd88edde8985f46285e679c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987778"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Parametrisieren von verknüpften Diensten in Azure Data Factory
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Sie können jetzt einen verknüpften Dienst parametrisieren und dynamische Werte zur Laufzeit übergeben. Wenn Sie beispielsweise Verbindungen mit verschiedenen Datenbanken auf demselben logischen SQL-Server herstellen möchten, können Sie nun den Datenbanknamen in der verknüpften Dienstdefinition parametrisieren. Dadurch wird vermieden, dass Sie für jede Datenbank auf dem logischen SQL-Server einen verknüpften Dienst erstellen müssen. Sie können auch andere Eigenschaften in der Definition des verknüpften Diensts parametrisieren, z.B. den *Benutzernamen*.

Sie können mithilfe der Data Factory-Benutzeroberfläche im Azure-Portal oder einer Programmierschnittstelle verknüpfte Dienste parametrisieren.

> [!TIP]
> Es wird empfohlen, Kennwörter oder Geheimnisse nicht zu parametrisieren. Speichern Sie stattdessen alle Verbindungszeichenfolgen in Azure Key Vault, und parametrisieren Sie den *geheimen Namen*.

Das folgende Video enthält eine siebenminütige Einführung und Demonstration dieses Features:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Unterstützte Datenspeicher

Zurzeit wird die Parametrisierung verknüpfter Dienste in der Data Factory-Benutzeroberfläche für die folgenden Datenspeicher unterstützt. Für alle anderen Datenspeicher können Sie den verknüpften Dienst parametrisieren, indem Sie auf der Registerkarte **Verbindungen** das Symbol **Code** auswählen und den JSON-Editor verwenden.

- Amazon Redshift
- Azure Cosmos DB (SQL-API)
- Azure Database for MySQL
- Azure SQL-Datenbank
- Azure Synapse Analytics (ehemals SQL DW)
- MySQL
- Oracle
- SQL Server

## <a name="data-factory-ui"></a>Data Factory-Benutzeroberfläche

![Hinzufügen dynamischen Inhalts zur Definition des verknüpften Diensts](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Erstellen eines neuen Parameters](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
