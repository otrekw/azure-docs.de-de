---
author: baanders
description: Includedatei mit einer Beschreibung, wie Azure Digital Twins-Modelle überprüft werden
ms.service: digital-twins
ms.topic: include
ms.date: 8/7/2020
ms.author: baanders
ms.openlocfilehash: 9b7a2f3c4b8909a33010797c74bf8ce9631af68b
ms.sourcegitcommit: 32ee8da1440a2d81c49ff25c5922f786e85109b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2021
ms.locfileid: "109789682"
---
> [!TIP]
> Nachdem Sie ein Modell erstellt haben, sollten Sie Ihre Modelle offline validieren, bevor Sie sie in Ihre Azure Digital Twins-Instanz hochladen.

Für die Validierung von Modelldokumenten steht ein sprachunabhängiges [DTDL-Validierungsbeispiel](/samples/azure-samples/dtdl-validator/dtdl-validator) zur Verfügung, um sicherzustellen, dass die DTDL gültig ist, bevor Sie sie in Ihre Instanz hochladen.

Das Beispiel für ein DTDL-Validierungssteuerelement basiert auf einer .NET-DTDL-Parserbibliothek, die auf NuGet als clientseitige Bibliothek verfügbar ist: [Microsoft.Azure.DigitalTwins.Parser](https://nuget.org/packages/Microsoft.Azure.DigitalTwins.Parser/). Sie können die Bibliothek auch direkt verwenden, um eine eigene Validierungslösung zu entwerfen. Stellen Sie bei Verwendung der Parserbibliothek sicher, dass die verwendete Version mit der Version kompatibel ist, die von Azure Digital Twins ausgeführt wird. Aktuell ist dies Version *3.12.4*.

Weitere Informationen zu dem Beispiel für ein Validierungssteuerelement und zur Parserbibliothek, einschließlich Verwendungsbeispielen, finden Sie unter [Vorgehensweise: Clientseitige DTDL-Parserbibliothek](../articles/digital-twins/how-to-parse-models.md).