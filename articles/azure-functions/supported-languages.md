---
title: In Azure Functions unterstützte Sprachen
description: Erfahren Sie, welche Sprachen für allgemeine Verfügbarkeit und welche experimentell und in Vorschauversionen unterstützt werden.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 029ea753439dca3093bf214a5adfb6d58a1fe567
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74942255"
---
# <a name="supported-languages-in-azure-functions"></a>In Azure Functions unterstützte Sprachen

In diesem Artikel werden die Unterstützungsebenen für Sprachen erläutert, die Sie mit Azure Functions verwenden können.

## <a name="levels-of-support"></a>Unterstützungsebenen

Es gibt drei Unterstützungsebenen:

* **Allgemein verfügbar (generally available, GA)** : Vollständige Unterstützung und Freigabe für die Verwendung in Produktionsumgebungen.
* **Vorschau**: Noch nicht unterstützt, die Erreichung der allgemeinen Verfügbarkeit wird aber für die Zukunft erwartet.
* **Experimentell**: Nicht unterstützt und möglicherweise in der Zukunft wieder aufgegeben; die eventuelle Erreichung des Vorschaustatus oder der allgemeinen Verfügbarkeit ist nicht garantiert.

## <a name="languages-by-runtime-version"></a>Sprachen nach Runtimeversion 

Es sind [drei Versionen der Azure Functions-Runtime](functions-versions.md) verfügbar. Die folgende Tabelle gibt an, welche Sprachen in den beiden Runtimeversionen unterstützt werden.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Experimentelle Sprachen

Die experimentellen Sprachen in Version 1.x sind nicht gut skalierbar und unterstützen nicht alle Bindungen.

Verwenden Sie keine experimentellen Funktionen für Bereiche, die für Sie sehr wichtig sind, da es dafür keinen offiziellen Support gibt. Für Probleme mit experimentellen Sprachen sollten keine Supportfälle erstellt werden. 

Für höhere Runtime-Versionen werden keine experimentellen Sprachen unterstützt. Die Unterstützung für neue Sprachen wird nur hinzugefügt, wenn die Sprache in der Produktion unterstützt werden kann. 

### <a name="language-extensibility"></a>Spracherweiterbarkeit

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
