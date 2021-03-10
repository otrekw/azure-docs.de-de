---
author: baanders
description: Includedatei zum Einrichten der lokalen Authentifizierung für DefaultAzureCredential in Azure Digital Twins-Beispielen – mit Einführung
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: dcef8030c47e771e5cd771dac09b5f96e3d38abd
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473641"
---
### <a name="set-up-local-azure-credentials"></a>Einrichten lokaler Azure-Anmeldeinformationen

Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Weitere Informationen zu den verschiedenen Möglichkeiten, wie sich eine Client-App bei Azure Digital Twins authentifizieren kann, finden Sie unter [*Vorgehensweise: Schreiben von App-Authentifizierungscode*](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]