---
title: include file
description: include file
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 79a8bc73f416c8d10d83e7ad94a727094f072b00
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "94331706"
---
| Domänennamen                  | Ausgehende Ports | BESCHREIBUNG                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Erforderlich für die selbstgehostete Integration Runtime zur interaktiven Erstellung |
| `{datafactory}.{region}.datafactory.azure.net`<br> oder `*.frontend.clouddatahub.net` | 443            | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit dem Azure Data Factory-Dienst herzustellen. <br>Bei einer neu erstellten Data Factory ermitteln Sie den FQDN über den Schlüssel der selbstgehosteten Integration Runtime. Dieser Schlüssel hat das folgende Format: {datafactory}.{region}.datafactory.azure.net. Kann bei einer alten Data Factory der FQDN nicht über den Schlüssel der selbstgehosteten Integration Runtime ermittelt werden, verwenden Sie stattdessen „*.frontend.clouddatahub.net“. |
| `download.microsoft.com`    | 443            | Erforderlich für die selbstgehostete Integration Runtime zum Herunterladen der Aktualisierungen. Wenn Sie die automatische Aktualisierung deaktiviert haben, können Sie die Konfiguration dieser Domäne überspringen. |
| `*.core.windows.net`          | 443            | Wird von der selbstgehosteten Integration Runtime verwendet, um Verbindungen mit dem Azure Storage-Konto herzustellen, wenn Sie das Feature [gestaffeltes Kopieren](https://docs.microsoft.com/azure/data-factory/copy-activity-performance#staged-copy) verwenden. |
| `*.database.windows.net`      | 1433           | Nur erforderlich, wenn Sie von bzw. nach Azure SQL-Datenbank oder Azure Synapse Analytics kopieren, andernfalls optional. Verwenden Sie das Feature für gestaffeltes Kopieren, um Daten nach SQL-Datenbank oder Synapse Analytics zu kopieren, ohne Port 1433 zu öffnen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Nur erforderlich, wenn Sie von bzw. nach Azure Data Lake Store kopieren, andernfalls optional. |
