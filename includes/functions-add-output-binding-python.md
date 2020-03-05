---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8bdd8b9d900cc50fdeb34ff7d233ac4d7e17a45c
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191030"
---
Aktualisieren Sie *HttpExample\\\_\_init\_\_.py* gemäß dem folgenden Code, und fügen Sie den Parameter `msg` der Funktionsdefinition und `msg.set(name)` unter der `if name:`-Anweisung hinzu.

:::code language="python" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/__init__.py":::

Der Parameter `msg` ist eine Instanz der [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest). Die zugehörige `set`-Methode schreibt eine Zeichenfolgennachricht in die Warteschlange. In diesem Fall ist dies der Name, der in der URL-Abfragezeichenfolge an die Funktion übergeben wird.
