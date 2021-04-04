---
title: Azure Table Storage-Bindungen für Azure Functions
description: Hier erfahren Sie, wie Sie Azure Table Storage-Bindungen in Azure Functions verwenden.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 4f2b890dc60cd50b5fcaefabe8d418268b738c20
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92096724"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure Table Storage-Bindungen für Azure Functions

Azure Functions integriert sich in [Azure Storage](../storage/index.yml) mittels [Triggern und Bindungen](./functions-triggers-bindings.md). Die Integration in Table Storage ermöglicht Ihnen das Erstellen von Funktionen, die Tabellenspeicherdaten lesen und schreiben.

| Aktion | type |
|---------|---------|
| Lesen von Tabellenspeicherdaten in einer Funktion | [Eingabebindung](./functions-bindings-storage-table-input.md) |
| Einer Funktion das Schreiben von Tabellenspeicherdaten gestatten |[Ausgabebindung](./functions-bindings-storage-table-output.md) |

## <a name="packages---functions-2x-and-higher"></a>Pakete: Functions 2.x oder höher

Die Tabellenspeicherbindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage), Version 3.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Die Tabellenspeicherbindungen werden im NuGet-Paket [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs), Version 2.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table).

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Nächste Schritte

- [Lesen von Tabellenspeicherdaten, wenn eine Funktion ausgeführt wird](./functions-bindings-storage-table-input.md)
- [Schreiben von Tabellenspeicherdaten aus einer Funktion](./functions-bindings-storage-table-output.md)
