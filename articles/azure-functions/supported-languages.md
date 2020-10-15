---
title: In Azure Functions unterstützte Sprachen
description: Erfahren Sie, welche Sprachen unterstützt werden (GA) und welche sich in der Vorschau befinden sowie wie die Entwicklung mit Functions auf andere Sprachen ausgeweitet werden kann.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 91a6ea886c3828678771b24d69bb7987af1fb105
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "83994900"
---
# <a name="supported-languages-in-azure-functions"></a>In Azure Functions unterstützte Sprachen

In diesem Artikel werden die Unterstützungsebenen für Sprachen erläutert, die Sie mit Azure Functions verwenden können. Außerdem werden Strategien zum Erstellen von Funktionen in Sprachen beschrieben, die nicht nativ unterstützt werden.

## <a name="levels-of-support"></a>Unterstützungsebenen

Es gibt zwei Unterstützungsebenen:

* **Allgemein verfügbar (generally available, GA)** : Vollständige Unterstützung und Freigabe für die Verwendung in Produktionsumgebungen.
* **Vorschau**: Noch nicht unterstützt, die Erreichung der allgemeinen Verfügbarkeit wird aber für die Zukunft erwartet.

## <a name="languages-by-runtime-version"></a>Sprachen nach Runtimeversion 

Es sind [drei Versionen der Azure Functions-Runtime](functions-versions.md) verfügbar. Die folgende Tabelle gibt an, welche Sprachen in den beiden Runtimeversionen unterstützt werden.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

## <a name="custom-handlers-preview"></a>Benutzerdefinierte Handler (Vorschau) 

Benutzerdefinierte Handler sind einfache Webserver, die Ereignisse vom Azure Functions-Host empfangen. Jede Sprache, die HTTP-Primitive unterstützt, kann einen benutzerdefinierten Handler implementieren. Dies bedeutet, dass benutzerdefinierte Handler zum Erstellen von Funktionen in Sprachen verwendet werden können, die nicht offiziell unterstützt werden. Weitere Informationen finden Sie unter [Benutzerdefinierte Azure Functions-Handler (Vorschau)](functions-custom-handlers.md).

## <a name="language-extensibility"></a>Spracherweiterbarkeit

Ab Version 2.x ist die Runtime auf [Spracherweiterbarkeit](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility) ausgelegt. Die Sprachen JavaScript und Java in der Runtime 2.x verfügen über diese Erweiterbarkeit.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Entwickeln von Funktionen in unterstützten Sprachen finden Sie in den folgenden Ressourcen:

+ [Entwicklerreferenz zur C#-Klassenbibliothek](functions-dotnet-class-library.md)
+ [Entwicklerreferenz für C#-Skript](functions-reference-csharp.md)
+ [Java-Entwicklerreferenz](functions-reference-java.md)
+ [JavaScript-Entwicklerreferenz](functions-reference-node.md)
+ [PowerShell-Entwicklerreferenz](functions-reference-powershell.md)
+ [Python-Entwicklerreferenz](functions-reference-python.md)
+ [TypeScript-Entwicklerreferenz](functions-reference-node.md#typescript)
