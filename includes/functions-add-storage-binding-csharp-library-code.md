---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: be3c746146012195757ab06de0c424dbc8297e9a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "78190914"
---
Fügen Sie Code hinzu, der das Ausgabebindungsobjekt `msg` verwendet, um eine Warteschlangennachricht zu erstellen. Fügen Sie diesen Code vor der Rückgabe der Methode hinzu.

:::code language="csharp" range="28-32" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" :::

Die Funktion sollte nun wie folgt aussehen:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-36":::
