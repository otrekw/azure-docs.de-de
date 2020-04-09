---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: c27f3fea6d2dd8b891220ba06b375b33e6cd950b
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673472"
---
In einem Java-Projekt werden die Bindungen als Bindungsanmerkungen für die Funktionsmethode definiert. Die Datei *function.json* wird dann automatisch auf der Grundlage dieser Anmerkungen generiert.

Navigieren Sie zum Speicherort Ihres Funktionscodes (unter _src/main/java_), öffnen Sie die Projektdatei *Function.java*, und fügen Sie der Definition der Methode `run` die folgenden Parameter hinzu:

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="20-21":::

Der Parameter `msg` ist ein [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding)-Typ. Dieser stellt eine Sammlung von Zeichenfolgen dar, die als Nachrichten in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. In diesem Fall ist die Ausgabe eine Speicherwarteschlange mit dem Namen `outqueue`. Die Verbindungszeichenfolge für das Storage-Konto wird durch die Methode `connection` festgelegt. Übergeben Sie anstelle der eigentlichen Verbindungszeichenfolge die Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält.

Die Definition der Methode `run` sollte nun wie im folgenden Beispiel aussehen:  

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="16-22":::