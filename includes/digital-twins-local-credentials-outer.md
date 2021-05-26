---
author: baanders
description: Includedatei zum Einrichten der lokalen Authentifizierung für DefaultAzureCredential in Azure Digital Twins-Beispielen – mit Einführung
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: eacabf431d396fa042bf4dd60d3b32c688df46bf
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110065126"
---
### <a name="set-up-local-azure-credentials"></a>Einrichten lokaler Azure-Anmeldeinformationen

Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet&preserve-view=true) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Weitere Informationen zu den verschiedenen Möglichkeiten, wie sich eine Client-App bei Azure Digital Twins authentifizieren kann, finden Sie unter [Vorgehensweise: Schreiben von App-Authentifizierungscode](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]