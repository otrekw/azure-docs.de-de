---
author: baanders
description: Includedatei mit einer Beschreibung, wie Azure Digital Twins-Modelle überprüft werden
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: 0146660fa50f9a136cd82483200ca2e6c6cc0b89
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2020
ms.locfileid: "87985874"
---
> [!TIP]
> Nachdem Sie ein Modell erstellt haben, sollten Sie Ihre Modelle offline validieren, bevor Sie sie in Ihre Azure Digital Twins-Instanz hochladen.

Für die Validierung von Modelldokumenten steht ein sprachunabhängiges Beispiel zur Verfügung, um sicherzustellen, dass die DTDL gültig ist. Sie finden es an der folgenden Stelle: [**DTDL-Validierungssteuerelement (Beispiel)** ](https://docs.microsoft.com/samples/azure-samples/dtdl-validator/dtdl-validator).

Das Beispiel für ein DTDL-Validierungssteuerelement basiert auf einer .NET-DTDL-Parserbibliothek, die auf NuGet als clientseitige Bibliothek verfügbar ist: [**Microsoft.Azure.DigitalTwins.Parser**](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Sie können die Bibliothek auch direkt verwenden, um eine eigene Validierungslösung zu entwerfen. Stellen Sie bei Verwendung der Parserbibliothek sicher, dass die verwendete Version mit der Version kompatibel ist, die von Azure Digital Twins ausgeführt wird. Während der Vorschauphase ist dies die Version *3.7.0*.

Weitere Informationen zu dem Beispiel für ein Validierungssteuerelement und zur Parserbibliothek, einschließlich Verwendungsbeispielen, finden Sie unter [*Vorgehensweise: Clientseitige DTDL-Parserbibliothek*](../articles/digital-twins/how-to-parse-models.md).