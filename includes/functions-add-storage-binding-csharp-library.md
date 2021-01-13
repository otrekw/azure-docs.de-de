---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 5e1a2622df0038141dd5cb05237f93d5e33e0bfb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "78190913"
---
In einem C#-Klassenbibliotheksprojekt werden die Bindungen als Bindungsattribute der Funktionsmethode definiert. Die von Functions benötigte Datei *function.json* wird basierend auf diesen Attributen automatisch generiert.

Öffnen Sie die Projektdatei *HttpExample.cs*, und fügen Sie der Definition der Methode `Run` den folgenden Parameter hinzu:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="17":::

Der Parameter `msg` ist ein `ICollector<T>`-Typ und stellt eine Sammlung von Nachrichten dar, die in eine Ausgabebindung geschrieben werden, wenn die Funktion abgeschlossen wird. In diesem Fall ist die Ausgabe eine Speicherwarteschlange mit dem Namen `outqueue`. Die Verbindungszeichenfolge für das Storage-Konto wird durch `StorageAccountAttribute` festgelegt. Dieses Attribut gibt die Einstellung an, die die Verbindungszeichenfolge des Storage-Kontos enthält, und kann auf Klassen-, Methoden- oder Parameterebene angewandt werden. In diesem Fall können Sie `StorageAccountAttribute` weglassen, da Sie bereits das Standardspeicherkonto verwenden.

Die Run-Methodendefinition sollte nun wie folgt aussehen:  

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-18":::
