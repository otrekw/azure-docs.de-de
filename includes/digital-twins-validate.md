---
author: baanders
description: Includedatei mit einer Beschreibung, wie Azure Digital Twins-Modelle überprüft werden
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: bccf92c6c912747e28b3075f12ac1558cced30c9
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130379"
---
> [!TIP]
> Nachdem Sie ein Modell erstellt haben, sollten Sie Ihre Modelle offline validieren, bevor Sie sie in Ihre Azure Digital Twins-Instanz hochladen.

Für die Validierung von Modelldokumenten steht ein sprachunabhängiges Beispiel zur Verfügung, um sicherzustellen, dass die DTDL gültig ist, bevor Sie sie in Ihre Instanz hochladen. Sie finden es an der folgenden Stelle: [**DTDL-Validierungssteuerelement (Beispiel)**](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

* Das Beispiel für ein DTDL-Validierungssteuerelement basiert auf einer .NET-DTDL-Parserbibliothek, die auf NuGet als clientseitige Bibliothek verfügbar ist: [**Microsoft.Azure.DigitalTwins.Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Sie können die Bibliothek auch direkt verwenden, um eine eigene Validierungslösung zu entwerfen. Stellen Sie bei Verwendung der Parserbibliothek sicher, dass die verwendete Version mit der Version kompatibel ist, die von Azure Digital Twins ausgeführt wird. Aktuell ist dies Version  *3.12.4*.

Weitere Informationen zu dem Beispiel für ein Validierungssteuerelement und zur Parserbibliothek, einschließlich Verwendungsbeispielen, finden Sie unter [*Vorgehensweise: Clientseitige DTDL-Parserbibliothek*](../articles/digital-twins/how-to-parse-models.md).