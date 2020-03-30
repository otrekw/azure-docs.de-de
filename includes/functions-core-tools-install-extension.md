---
title: include file
description: include file
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 94cac0932da5880e5e7b8a8fac3870b5bc464af9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "75564767"
---
## <a name="register-extensions"></a>Registrieren von Erweiterungen

Mit Ausnahme von HTTP- und Zeitgebertriggern werden Functions-Bindungen in der Runtimeversion 2.x und höher als Erweiterungspakete implementiert. In Version 2.x und höheren Versionen der Azure Functions-Runtime müssen Sie die in Ihren Funktionen für die Bindungstypen verwendeten Erweiterungen explizit registrieren. Die Ausnahmen hierbei sind HTTP-Bindungen und Zeitauslöser, die keine Erweiterungen erfordern.

Sie können auch Bindungserweiterungen einzeln installieren, oder Sie können einen Erweiterungsbündelverweis in der Datei „host.json“ hinzufügen. Erweiterungsbündel wirken eventuellen Kompatibilitätsproblemen bei Paketen entgegen, wenn mehrere Bindungstypen verwendet werden. Es handelt sich hierbei um den empfohlenen Ansatz zum Registrieren von Bindungserweiterungen. Erweiterungsbündel beseitigen außerdem die Notwendigkeit der Installation des .NET Core 2.x SDK. 

### <a name="extension-bundles"></a>Erweiterungsbündel

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Weitere Informationen finden Sie unter [Registrieren von Bindungserweiterungen von Azure Functions](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Sie sollten Erweiterungsbündel in de Datei „host.json“ hinzufügen, bevor Sie der Datei „function.json“ Bindungen hinzufügen.

### <a name="register-individual-extensions"></a>Registrieren einzelner Erweiterungen

Wenn Sie Erweiterungen installieren müssen, die nicht in einem Bündel enthalten sind, können Sie einzelne Erweiterungspakete für bestimmte Bindungen manuell registrieren. 

> [!NOTE]
> Um Erweiterungen mithilfe von `func extensions install` manuell zu registrieren, müssen Sie das .NET Core 2.x SDK installiert haben.

Nachdem Sie Ihre Datei *function.json* aktualisiert haben, sodass sie alle von der Funktion benötigten Bindungen einschließt, führen Sie den folgenden Befehl im Projektordner aus.

```bash
func extensions install
```

Der Befehl liest die Datei *function.json*, um zu ermitteln, welche Pakete Sie benötigen, installiert diese und erzeugt das Erweiterungenprojekt neu. Er fügt alle neuen Bindungen an die aktuelle Version hinzu, aktualisiert aber keine vorhandenen Bindungen. Verwenden Sie die Option `--force`, um vorhandene Bindungen beim Installieren neuer auf die neueste Version zu aktualisieren.
