---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/27/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d09d69db33cadee1b5a214e9cb2f09a508f37242
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431844"
---
|Sprache                                 |1.x         |2.x| 3.x |
|-----------------------------------------|------------|---| --- |
|[C#](../articles/azure-functions/functions-reference-csharp.md)|Allgemeine Verfügbarkeit (.NET Framework 4.7)|Allgemeine Verfügbarkeit (.NET Core 2.2<sup>1</sup>)| Allgemeine Verfügbarkeit (.NET Core 3.1)<br/>[Vorschau (.NET 5.0)](../articles/azure-functions/dotnet-isolated-process-guide.md) |
|[JavaScript](../articles/azure-functions/functions-reference-node.md#node-version)|Allgemeine Verfügbarkeit (Node 6)|Allgemeine Verfügbarkeit (Node 10 und 8)| Allgemeine Verfügbarkeit (Node 12 und 10)<br />Vorschau (Node 14) |
|[F#](../articles/azure-functions/functions-reference-fsharp.md)|Allgemeine Verfügbarkeit (.NET Framework 4.7)|Allgemeine Verfügbarkeit (.NET Core 2.2<sup>1</sup>)| Allgemeine Verfügbarkeit (.NET Core 3.1) |
|[Java](../articles/azure-functions/functions-reference-java.md)|–|Allgemeine Verfügbarkeit (Java 8)| Allgemeine Verfügbarkeit (Java 11 und 8)|
|[PowerShell](../articles/azure-functions/functions-reference-powershell.md) |–|Allgemeine Verfügbarkeit (PowerShell Core 6)| Allgemeine Verfügbarkeit (PowerShell 7 und Core 6)|
|[Python](../articles/azure-functions/functions-reference-python.md#python-version)|–|Allgemeine Verfügbarkeit (Python 3.7 und 3.6)| Allgemeine Verfügbarkeit (Python 3.8, 3.7 und 3.6) <br/> Vorschau (Python 3.9)|
|[TypeScript](../articles/azure-functions/functions-reference-node.md#typescript) |–|Allgemeine Verfügbarkeit<sup>2</sup>| Allgemeine Verfügbarkeit<sup>2</sup> |


<sup>1</sup> Apps mit .NET-Klassenbibliotheken für Runtimeversion 2.x können nun im .NET Core 2.x-Kompatibilitätsmodus unter .NET Core 3.1 ausgeführt werden. Weitere Informationen finden Sie unter [Überlegungen zu Functions v2.x](../articles/azure-functions/functions-dotnet-class-library.md#functions-v2x-considerations).  
<sup>2</sup> Unterstützt durch Transpilierung in JavaScript

Weitere Informationen zu unterstützten Sprachversionen finden Sie im sprachspezifischen Artikel im Entwicklerhandbuch.   
Informationen zu geplanten Änderungen an der Sprachunterstützung finden Sie unter [Azure-Roadmap](https://azure.microsoft.com/roadmap/?tag=functions).
