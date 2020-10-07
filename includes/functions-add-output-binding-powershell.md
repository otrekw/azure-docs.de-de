---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: 753a3d16792e0dbaffe318cd188ea3f0d40bd5cb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "78191031"
---
Fügen Sie Code hinzu, der das Cmdlet `Push-OutputBinding` verwendet, um Text unter Verwendung der Ausgabebindung `msg` in die Warteschlange zu schreiben. Fügen Sie diesen Code hinzu, bevor Sie den OK-Status in der `if`-Anweisung festlegen.

:::code language="powershell" range="18-19" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::

Die Funktion sollte nun wie folgt aussehen:

:::code language="powershell" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::
