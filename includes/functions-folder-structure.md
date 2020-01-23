---
title: include file
description: include file
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: ea7396117935c13698a8c6cc6ef4029cc82b90bc
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279301"
---
Der Code für alle Funktionen in einer bestimmten Funktions-App befindet sich in einem Stammprojektordner, der eine Hostkonfigurationsdatei und mindestens einen Unterordner enthält. Jeder Unterordner enthält den Code für eine separate Funktion. Die folgende Darstellung zeigt die Ordnerstruktur:

```
FunctionApp
 | - host.json
 | - MyFirstFunction
 | | - function.json
 | | - ...  
 | - MySecondFunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

In der Version 2.x und höheren Versionen der Functions-Runtime müssen sich alle Funktionen in der Funktions-App denselben Sprachstapel teilen.  

Die Datei [host.json](../articles/azure-functions/functions-host-json.md) enthält die runtimespezifische Konfiguration und befindet sich im Stammordner der Funktions-App. Ein Ordner *bin* enthält Pakete und andere Bibliotheksdateien, die von der Funktions-App benötigt werden. Sprachspezifische Anforderungen für ein Funktions-App-Projekt:

* [C#-Klassenbibliothek (.csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [C#-Skript (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [F#-Skript](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)
