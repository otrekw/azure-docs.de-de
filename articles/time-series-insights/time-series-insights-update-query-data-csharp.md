---
title: Abfragen von Daten aus einer Vorschauumgebung mit C# – Azure Time Series Insights | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten mithilfe einer in C# geschriebenen App aus einer Time Series Insights-Umgebung abfragen.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/14/2020
ms.custom: seodec18
ms.openlocfilehash: fbc2cbc29cb23a21e7d3713091fc22f01bb1b15a
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2020
ms.locfileid: "81379818"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Abfragen von Daten aus der Azure Time Series Insights Preview-Umgebung mit C#

Dieses C#-Beispiel veranschaulicht, wie Daten von den [Preview-APIs für den Datenzugriff](https://docs.microsoft.com/rest/api/time-series-insights/preview) in der Azure Time Series Insights Preview-Umgebung abgefragt werden können.

> [!TIP]
> C#-Codebeispiele der Vorschauphase finden Sie unter [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample).

## <a name="summary"></a>Zusammenfassung

Im unten stehenden Beispielcode werden die folgenden Funktionen veranschaulicht:

* Unterstützung der automatischen SDK-Generierung von [Azure AutoRest](https://github.com/Azure/AutoRest).
* Abrufen eines Zugriffstokens durch Azure Active Directory mithilfe von [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).
* Übergeben dieses erhaltenen Zugriffstokens an den `Authorization`-Header nachfolgender Datenzugriffs-API-Anforderungen. 
* Im Beispiel wird eine Konsolenschnittstelle bereitgestellt, die veranschaulicht, wie HTTP-Anforderungen gestellt werden an:

    * [Vorschauumgebungen-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [„Umgebungsverfügbarkeit abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) und [„Ereignisschema abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [Vorschauabfrage-API](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [„Ereignisse abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents), [„Reihe abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries) und [„Aggregierte Reihe abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
    * [Time Series-Modell-APIs](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [„Hierarchien abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) und [Hierarchienbatch-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch)
        * [„Typen abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get) und [Typenbatch-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch)
        * [„Instanzen abrufen“-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) und [Instanzenbatch-API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch)
* Erweiterte [Such](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features)- und [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)-Funktionen.

## <a name="prerequisites-and-setup"></a>Voraussetzungen und Setup

Führen Sie vor dem Kompilieren und Ausführen des Beispielcodes die folgenden Schritte aus:

1. [Stellen Sie eine Azure Time Series Insights Preview](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment)-Umgebung bereit.
1. Konfigurieren Sie Ihre Azure Time Series Insights-Umgebung für Azure Active Directory wie unter [Authentifizierung und Autorisierung](time-series-insights-authentication-and-authorization.md) beschrieben. 
1. Führen Sie die [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) wie in [Readme.md](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) angegeben aus, um die Time Series Insights Preview-Clientabhängigkeiten zu generieren.
1. Öffnen Sie die Projektmappe `TSIPreviewDataPlaneclient.sln`, und legen Sie `DataPlaneClientSampleApp` als Standardprojekt in Visual Studio fest.
1. Installieren Sie erforderlichen Projektabhängigkeiten mithilfe der [unten](#project-dependencies) beschriebenen Schritte, und kompilieren Sie das Beispiel in eine ausführbare `.exe`-Datei.
1. Führen Sie die `.exe`-Datei aus, indem Sie darauf doppelklicken.

## <a name="project-dependencies"></a>Projektabhängigkeiten

Es wird empfohlen, die neueste Version von Visual Studio zu verwenden:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) ab Version 16.4.2

Der Beispielcode besitzt mehrere erforderliche Abhängigkeiten, die in der Datei [packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) zu sehen sind.

Laden Sie die Pakete in Visual Studio 2019 herunter, indem Sie die Optionen **Build** > **Projektmappe erstellen** auswählen. 

Alternativ fügen Sie jedes Paket mithilfe von [NuGet 2.12+](https://www.nuget.org/) hinzu. Beispiel:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C#-Beispielcode

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Das Codebeispiel kann ausgeführt werden, ohne die Standardumgebungsvariablen zu ändern.
> * Das Codebeispiel wird in eine ausführbare .NET-Konsolen-App kompiliert.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Abfragen finden Sie in der [Abfrage-API-Referenz](https://docs.microsoft.com/rest/api/time-series-insights/preview-query).

- Lesen Sie die Informationen, wie Sie [eine JavaScript-App mithilfe des Client-SDKs ](https://github.com/microsoft/tsiclient) mit Time Series Insights verbinden.
