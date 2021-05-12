---
author: baanders
description: Includedatei zum Einrichten der lokalen Authentifizierung für DefaultAzureCredential in Azure Digital Twins-Beispielen – mit Einführung
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: bd8676aaf4a8c084abb7cbc735d3564eeb7cf102
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108739359"
---
### <a name="set-up-local-azure-credentials"></a>Einrichten lokaler Azure-Anmeldeinformationen

Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Weitere Informationen zu den verschiedenen Möglichkeiten, wie sich eine Client-App bei Azure Digital Twins authentifizieren kann, finden Sie unter [Vorgehensweise: Schreiben von App-Authentifizierungscode](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]