---
title: Übersicht zu Azure Functions
description: Hier erfahren Sie, wie Sie mithilfe von Azure Functions in wenigen Minuten asynchrone Workloads optimieren.
author: mattchenderson
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 01/16/2020
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 0583b68df603b04d47ac6104f0cf127b3c4bedd0
ms.sourcegitcommit: 2989396c328c70832dcadc8f435270522c113229
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92173725"
---
# <a name="an-introduction-to-azure-functions"></a>Einführung in Azure Functions

Azure Functions ermöglicht das Ausführen kleiner Codeelemente (sogenannter Funktionen), ohne sich Gedanken über die Anwendungsinfrastruktur machen zu müssen. Mit Azure Functions stellt die Cloudinfrastruktur alle aktuellen Server bereit, die Sie zur skalierten Ausführung Ihrer Anwendung benötigen.

Eine Funktion wird durch eine bestimmte Art von Ereignis ausgelöst. Zu den [unterstützten Triggern](./functions-triggers-bindings.md) zählen unter anderem Reaktionen auf Datenänderungen, die Beantwortung von Nachrichten, ein Zeitplan oder das Ergebnis einer HTTP-Anforderung.

Sie können zwar auch direkt für eine Vielzahl von Diensten programmieren, die Integration mit anderen Diensten wird jedoch durch Bindungen optimiert. Mit Bindungen erhalten Sie [deklarativen Zugriff auf ein breites Spektrum an Azure- und Drittanbieterdiensten](./functions-triggers-bindings.md).

## <a name="features"></a>Features

Azure Functions bietet unter anderem folgende zentrale Features:

- **Serverlose Anwendungen:** Mit Functions können Sie [serverlose](https://azure.microsoft.com/solutions/serverless/) Anwendungen für Microsoft Azure entwickeln.

- **Verschiedene Programmiersprachen:** Schreiben Sie Funktionen wahlweise mit [C#, Java, JavaScript, Python oder PowerShell](supported-languages.md).

- **Preismodell mit nutzungsbasierter Bezahlung:** Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird. Informationen hierzu finden Sie unter der Hostingoption „Verbrauchstarif“ im [Preisabschnitt](#pricing).  

- **Eigene Abhängigkeiten:** Functions unterstützt NuGet und NPM, sodass Sie Ihre bevorzugten Bibliotheken verwenden können.

- **Integrierte Sicherheit:** Schützen Sie per HTTP ausgelöste Funktionen mit OAuth-Anbietern wie Azure Active Directory, Facebook, Google, Twitter und Microsoft-Konto.

- **Vereinfachte Integration:** Profitieren Sie von der einfachen Integration für Azure-Dienste und SaaS-Angebote (Software-as-a-Service).

- **Flexible Entwicklung:** Richten Sie Continuous Integration ein, und stellen Sie Ihren Code über [GitHub](../app-service/scripts/cli-continuous-deployment-github.md), [Azure DevOps Services](../app-service/scripts/cli-continuous-deployment-vsts.md) und andere [unterstützte Entwicklungstools](../app-service/deploy-local-git.md) bereit.

- **Zustandsbehaftete serverlose Architektur:** Orchestrieren Sie serverlose Anwendungen mit [Durable Functions](durable/durable-functions-overview.md).

- **Open Source:** Die Functions-Runtime ist Open-Source-Software und [auf GitHub verfügbar](https://github.com/azure/azure-webjobs-sdk-script).

## <a name="what-can-i-do-with-functions"></a>Welche Möglichkeiten bestehen mit Functions?

Functions ist eine hervorragende Lösung zur Verarbeitung von Massendaten, zur Integration von Systemen, zur Nutzung des Internets der Dinge (Internet of Things, IoT) sowie zur Erstellung einfacher APIs und Microservices.

Es steht eine Reihe von Vorlagen zur Verfügung, um Sie bei den ersten Schritten in Schlüsselszenarien zu unterstützen:

- **HTTP**: Führen Sie Code auf der Grundlage von [HTTP-Anforderungen](functions-create-first-azure-function.md) aus.

- **Timer**: Planen Sie die [Ausführung von Code zu vorgegebenen Zeiten](./functions-create-scheduled-function.md).

- **Azure Cosmos DB**: Verarbeiten Sie [neue und geänderte Azure Cosmos DB-Dokumente](./functions-create-cosmos-db-triggered-function.md).

- **Blob Storage**: Verarbeiten Sie [neue und geänderte Azure Storage-Blobs](./functions-create-storage-blob-triggered-function.md).

- **Queue Storage**: Reagieren Sie auf [Azure Storage-Warteschlangennachrichten](./functions-create-storage-queue-triggered-function.md).

- **Event Grid**: Reagieren Sie auf [Azure Event Grid-Ereignisse über Abonnements und Filter](../event-grid/resize-images-on-storage-blob-upload-event.md).

- **Event Hub**: Reagieren Sie auf [große Mengen von Azure Event Hub-Ereignissen](./functions-bindings-event-hubs.md).

- **Service Bus-Warteschlange**: Stellen Sie eine Verbindung mit anderen Azure-Diensten oder lokalen Diensten her, indem Sie [auf Service Bus-Warteschlangennachrichten reagieren](./functions-bindings-service-bus.md).

- **Service Bus-Thema**: Verbinden Sie andere Azure-Dienste oder lokale Diensten, indem Sie [auf Service Bus-Themennachrichten reagieren](./functions-bindings-service-bus.md).

## <a name="how-much-does-functions-cost"></a><a name="pricing"></a>Was kostet Functions?

Für Azure Functions stehen drei Arten von Tarifen zur Verfügung. Wählen Sie den, der Ihren Anforderungen am besten entspricht:

- **Verbrauchsplan**: Azure stellt alle erforderlichen Rechenressourcen bereit. Sie müssen sich nicht um die Ressourcenverwaltung kümmern und bezahlen nur für die Zeit, in der Ihr Code ausgeführt wird.

- **Premium-Plan**: Sie geben eine Anzahl vorab aufgewärmter Instanzen an, die immer online sind und sofort reagieren können. Wenn Ihre Funktion ausgeführt wird, bietet Azure alle zusätzlichen erforderlichen Rechenressourcen. Sie bezahlen für die fortlaufend ausgeführten vorab aufgewärmten Instanzen und alle zusätzlichen Instanzen, die Sie verwenden, wenn Azure Ihre App zentral hoch- und herunter skaliert.

- **App Service-Plan**: Funktionen werden auf die gleiche Weise ausgeführt wie Ihre Web-Apps. Wenn Sie App Service für Ihre anderen Anwendungen verwenden, können Ihre Funktionen ohne zusätzliche Kosten unter dem gleichen Plan ausgeführt werden.

Weitere Informationen zu Hostingplänen finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md). Ausführliche Preisinformationen finden Sie auf der Seite [Functions – Preise](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-function-vs-code.md)  
  Verwenden Sie [Visual Studio Code](functions-create-first-function-vs-code.md), die [Befehlszeile](functions-create-first-azure-function-azure-cli.md) oder das [Azure-Portal](functions-create-first-azure-function.md).

- [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
   Enthält weitere technische Informationen zur Azure Functions-Laufzeit sowie eine Referenz für das Programmieren von Funktionen sowie für das Festlegen von Triggern und Bindungen.

- [Skalieren von Azure Functions](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des Hostingplans „Verbrauchstarif“) und enthält Informationen zur Wahl des geeigneten Plans.

- [Was ist Azure App Service?](../app-service/overview.md)  
  Azure Functions nutzt Azure App Service für Kernfunktionen wie Bereitstellungen, Umgebungsvariablen und Diagnosen.
