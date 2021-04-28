---
author: baanders
description: Includedatei für Azure Digital Twins – Möglichkeiten der Instanzverwaltung
ms.service: digital-twins
ms.topic: include
ms.date: 11/11/2020
ms.author: baanders
ms.openlocfilehash: 1f66101fc1231be2e5ce36dc348b1ca850113867
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107303757"
---
In diesem Artikel wird erläutert, wie Sie unterschiedliche Verwaltungsvorgänge mithilfe des [Azure Digital Twins .NET (C#) **SDK**](/dotnet/api/overview/azure/digitaltwins/management) durchführen. Sie können dieselben Verwaltungsaufrufe auch mit den anderen Sprach-SDKs erstellen, wie im folgenden Artikel beschrieben: [*Vorgehensweise: Verwenden der Azure Digital Twins-APIs und SDKs*](../articles/digital-twins/how-to-use-apis-sdks.md).

> [!TIP] 
> Es gibt für alle SDK-Methoden synchrone und asynchrone Versionen. Bei Pagingaufrufen geben die asynchronen Methoden `AsyncPageable<T>` zurück, während die synchronen Versionen `Pageable<T>` zurückgeben.

Eine weitere Verwaltungsoption besteht darin, die [**Rest-APIs**](/rest/api/azure-digitaltwins/) von Azure Digital Twins für diesen Themenbereich über einen REST-Client wie Postman direkt aufzurufen. Anweisungen dazu finden Sie unter [ *Senden von Anforderungen mit Postman*](../articles/digital-twins/how-to-use-postman.md).

Schließlich können Sie dieselben Verwaltungsvorgänge mithilfe der **CLI** von Azure Digital Twins ausführen. Weitere Informationen zur Verwendung der CLI finden Sie unter [*Vorgehensweise: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](../articles/digital-twins/how-to-use-cli.md).