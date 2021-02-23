---
title: Die REST-API für die Sitzungsverwaltung
description: Beschreibt die Verwaltung von Sitzungen
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 9eb1bb87792dbc61e7c85dbc20c136499f23f67c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530211"
---
# <a name="use-the-session-management-rest-api"></a>Verwenden der REST-API für die Sitzungsverwaltung

Um die Azure Remote Rendering-Funktion zu verwenden, müssen Sie eine *Sitzung* erstellen. Die einzelnen Sitzungen entsprechen jeweils einem Server, der in Azure zugeordnet wird, zu dem ein Clientgerät eine Verbindung herstellen kann. Wenn ein Gerät eine Verbindung herstellt, rendert der Server die angeforderten Daten und stellt das Ergebnis als Videostream bereit. Bei der Erstellung der Sitzung haben Sie ausgewählt, auf [welcher Art von Server](../reference/vm-sizes.md) die Ausführung stattfinden soll, und somit die Preise festgelegt. Wenn die Sitzung nicht mehr benötigt wird, sollte sie beendet werden. Falls sie nicht manuell angehalten wird, wird sie automatisch heruntergefahren, wenn die *Leasedauer der Sitzung* abläuft.

## <a name="rest-api-reference"></a>REST-API-Referenz

Informationen zur REST-API finden Sie [hier](https://docs.microsoft.com/rest/api/mixedreality/2021-01-01preview/remoterendering). Informationen zu den Swagger-Definitionen finden Sie [hier](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality).
Wir stellen ein PowerShell-Skript im [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) im Ordner *Scripts* bereit, das den Namen *RenderingSession.ps1* trägt. Dieses Skript veranschaulicht die Verwendung unseres Diensts. Das Skript und seine Konfiguration werden hier beschrieben: [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
Es stehen auch SDKs für [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java und Python bereit.

> [!IMPORTANT]
> Latenz ist ein entscheidender Faktor bei Verwendung des Remoterenderings. Die beste Leistung erzielen Sie, wenn Sie Sitzungen in der Region erstellen, die Ihnen am nächsten liegt. Der [Latenztest von Azure](https://www.azurespeed.com/Azure/Latency) kann verwendet werden, um zu bestimmen, welche Region Ihnen am nächsten liegt.

> [!IMPORTANT]
> Ein ARR-Runtime-SDK ist erforderlich, damit ein Clientgerät eine Verbindung zu einer Renderingsitzung herstellen kann. Diese SDKs sind für [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering?view=remoterendering) und [C++](https://docs.microsoft.com/cpp/api/remote-rendering/) verfügbar. Abgesehen vom Herstellen einer Verbindung zum Dienst können diese SDKs auch verwendet werden, um Sitzungen zu starten und zu stoppen.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der Azure-Front-End-APIs für die Authentifizierung](frontend-apis.md)
* [PowerShell-Beispielskripts](../samples/powershell-example-scripts.md)
