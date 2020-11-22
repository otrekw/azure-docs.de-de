---
author: baanders
description: Includedatei für Azure Digital Twins – Möglichkeiten der Instanzverwaltung
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 47de00780fb259e7476c16dea9b7c63bc512a211
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94533891"
---
In diesem Artikel wird erläutert, wie Sie unterschiedliche Verwaltungsvorgänge mithilfe des [Azure Digital Twins .NET (C#) **SDK**](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true) durchführen. Sie können dieselben Verwaltungsaufrufe auch mit den anderen Sprach-SDKs erstellen, wie im folgenden Artikel beschrieben: [*Vorgehensweise: Verwenden der Azure Digital Twins-APIs und SDKs*](../articles/digital-twins/how-to-use-apis-sdks.md).

> [!TIP] 
> Es gibt für alle SDK-Methoden synchrone und asynchrone Versionen. Bei Pagingaufrufen geben die asynchronen Methoden `AsyncPageable<T>` zurück, während die synchronen Versionen `Pageable<T>` zurückgeben.

Eine weitere Verwaltungsoption besteht darin, die [**Rest-APIs**](/rest/api/azure-digitaltwins/) von Azure Digital Twins für diesen Themenbereich direkt aufzurufen.

Schließlich können Sie dieselben Verwaltungsvorgänge mithilfe der **CLI** von Azure Digital Twins ausführen. Weitere Informationen zur Verwendung der CLI finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](../articles/digital-twins/how-to-use-cli.md).