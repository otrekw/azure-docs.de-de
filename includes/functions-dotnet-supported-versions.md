---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/01/2021
ms.author: glenga
ms.openlocfilehash: e65a628f846892294fdd9be693b89a535ac694a0
ms.sourcegitcommit: f9e368733d7fca2877d9013ae73a8a63911cb88f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111901443"
---
## <a name="supported-versions"></a>Unterstützte Versionen

Versionen der Functions-Laufzeit funktionieren mit bestimmten Versionen von .NET. Weitere Informationen zu den Functions-Versionen finden Sie unter [Übersicht über die Runtimeversionen von Azure Functions](../articles/azure-functions/functions-versions.md). Die Versionsunterstützung hängt davon ab, ob Ihre Funktionen prozessintern oder prozessextern (isoliert) ausgeführt werden. 

In der folgenden Tabelle sind die höchsten .NET Core- bzw. .NET Framework-Versionen aufgeführt, die mit einer bestimmten Version von Functions verwendet werden können. 

| Version der Functions-Laufzeit | In-Process<br/>([.NET-Klassenbibliothek](../articles/azure-functions/functions-dotnet-class-library.md)) | Out-of-Process<br/>([Isolierte .NET-Ausführung](../articles/azure-functions/dotnet-isolated-process-guide.md)) |
| ---- | ---- | --- |
| Functions 4.x<sup>1</sup> | .NET 6.0 (Vorschau)| .NET 6.0 (Vorschau) |
| Functions 3.x | .NET Core 3.1 | .NET 5.0 |
| Functions 2.x | .NET Core 2.1<sup>2</sup> | – |
| Functions 1.x | .NET Framework 4.8 | – |

<sup>1</sup> Azure Functions bietet experimentelle Unterstützung für die Ausführung Ihrer Funktionen im Vorschaurelease von .NET 6.0. Diese Vorabversion wird nicht offiziell unterstützt. Weitere Informationen finden Sie auf der Seite zur [frühen Vorschauversion von Azure Functions v4](https://aka.ms/functions-dotnet6earlypreview-wiki).  
<sup>2</sup> Weitere Informationen finden Sie unter [Überlegungen zu Functions-Versionen 2.x](../articles/azure-functions/functions-dotnet-class-library.md#functions-v2x-considerations).   

Aktuelle Informationen zu Azure Functions-Releases (einschließlich Informationen zur Entfernung bestimmter älterer Nebenversionen) finden Sie unter [Azure App Service-Ankündigungen](https://github.com/Azure/app-service-announcements/issues).
