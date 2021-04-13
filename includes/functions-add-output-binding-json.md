---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 9b3859d854b3900cc30eda6a95b8425da6763e59
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "94340983"
---
Bindungsattribute werden direkt in der Datei „function.json“ definiert. Abhängig vom Bindungstyp sind möglicherweise zusätzliche Eigenschaften erforderlich. Die [Warteschlangenausgabekonfiguration](../articles/azure-functions/functions-bindings-storage-queue-output.md#configuration) beschreibt die Felder, die für eine Azure Storage-Warteschlangenbindung erforderlich sind. Mit der Erweiterung können Bindungen einfach in der Datei „function.json“ hinzugefügt werden. 

Klicken Sie zum Erstellen einer Bindung im Ordner „HttpTrigger“ mit der rechten Maustaste (STRG+Klicken unter macOS) auf die Datei `function.json`, und wählen Sie **Bindung hinzufügen...** aus. Befolgen Sie die Anweisungen, um die folgenden Bindungseigenschaften für die neue Bindung zu definieren:

| Prompt | Wert | BESCHREIBUNG |
| -------- | ----- | ----------- |
| **Select binding direction** (Wählen Sie die Bindungsrichtung aus) | `out` | Die Bindung ist eine Ausgabebindung. |
| **Select binding with direction** (Wählen Sie die Bindung mit Richtung aus) | `Azure Queue Storage` | Die Bindung ist eine Azure Storage-Warteschlangenbindung. |
| **Der Name, der zum Identifizieren dieser Bindung in Ihrem Code verwendet wird** | `msg` | Name, der den Bindungsparameter identifiziert, auf den in Ihrem Code verwiesen wird. |
| **Die Warteschlange, an die die Nachricht gesendet wird** | `outqueue` | Der Name der Warteschlange, in den die Bindung schreibt. Wenn der *queueName* nicht vorhanden ist, erstellt die Bindung ihn bei der ersten Verwendung. |
| **Select setting from "local.setting.json"** (Wählen Sie eine Einstellung aus „local.setting.json“ aus) | `AzureWebJobsStorage` | Der Name einer Anwendungseinstellung, die die Verbindungszeichenfolge für das Storage-Konto enthält. Die Einstellung `AzureWebJobsStorage` enthält die Verbindungszeichenfolge für das Speicherkonto, das Sie mit der Funktions-App erstellt haben. |

Eine Bindung wird dem Array `bindings` in der Datei „function.json“ hinzugefügt, die wie folgt aussehen sollte:

:::code language="json" source="~/functions-docs-javascript/functions-add-output-binding-storage-queue-cli/HttpExample/function.json" range="18-24":::
