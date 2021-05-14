---
title: include file
description: include file
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: abnarain
ms.openlocfilehash: 44cd654bf9a4dddf32ef2bee44575622b3aa2563
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108760281"
---
| Domänennamen                  | Ausgehende Ports | BESCHREIBUNG                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Erforderlich für die selbstgehostete Integration Runtime zur interaktiven Erstellung |
| `{datafactory}.{region}.datafactory.azure.net`<br> oder `*.frontend.clouddatahub.net` | 443            | Erforderlich für die selbstgehostete Integration Runtime, um Verbindungen mit dem Azure Data Factory-Dienst herzustellen. <br>Bei einer neu erstellten Data Factory ermitteln Sie den FQDN über den Schlüssel der selbstgehosteten Integration Runtime. Dieser Schlüssel hat das folgende Format: {datafactory}.{region}.datafactory.azure.net. Kann bei einer alten Data Factory der FQDN nicht über den Schlüssel der selbstgehosteten Integration Runtime ermittelt werden, verwenden Sie stattdessen „*.frontend.clouddatahub.net“. |
| `download.microsoft.com`    | 443            | Erforderlich für die selbstgehostete Integration Runtime zum Herunterladen der Aktualisierungen. Wenn Sie die automatische Aktualisierung deaktiviert haben, können Sie die Konfiguration dieser Domäne überspringen. |
| `*.core.windows.net`          | 443            | Wird von der selbstgehosteten Integration Runtime verwendet, um Verbindungen mit dem Azure Storage-Konto herzustellen, wenn Sie das Feature [gestaffeltes Kopieren](../copy-activity-performance.md#staged-copy) verwenden. |
| `*.database.windows.net`      | 1433           | Nur erforderlich, wenn Sie von bzw. nach Azure SQL-Datenbank oder Azure Synapse Analytics kopieren, andernfalls optional. Verwenden Sie das Feature für gestaffeltes Kopieren, um Daten nach SQL-Datenbank oder Azure Synapse Analytics zu kopieren, ohne Port 1433 zu öffnen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Nur erforderlich, wenn Sie von bzw. nach Azure Data Lake Store kopieren, andernfalls optional. |
