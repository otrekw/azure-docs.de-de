---
title: include file
description: include file
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: lle
ms.openlocfilehash: 4ef7bc7ed1ab255bffdfec10c07bc040019d9f1e
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121613"
---
| Domänennamen                  | Ausgehende Ports | BESCHREIBUNG                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.core.windows.net`          | 443            | Wird von der selbstgehosteten Integration Runtime verwendet, um Verbindungen mit dem Azure Storage-Konto herzustellen, wenn Sie das Feature gestaffeltes Kopieren verwenden. |
| `*.database.windows.net`      | 1433           | Nur erforderlich, wenn Sie von bzw. nach Azure SQL-Datenbank oder Azure Synapse Analytics kopieren, andernfalls optional. Verwenden Sie das Feature für gestaffeltes Kopieren, um Daten nach SQL-Datenbank oder Azure Synapse Analytics zu kopieren, ohne Port 1433 zu öffnen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Nur erforderlich, wenn Sie von bzw. nach Azure Data Lake Store kopieren, andernfalls optional. |
