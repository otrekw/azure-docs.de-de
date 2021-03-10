---
author: baanders
description: Includedatei für DefaultAzureCredential in Azure Digital Twins-Beispielen – Hinweis
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: bec2681c33108417aab1a33932c4f5f4d2ed730f
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473688"
---
>[!NOTE]
> Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Bei dieser Art der Authentifizierung sucht das Beispiel in Ihrer lokalen Umgebung nach Azure-Anmeldeinformationen, z. B. eine Anmeldung von einer lokalen [Azure CLI](/cli/azure/install-azure-cli) oder in Visual Studio/Visual Studio Code.
>
> Weitere Informationen zum Verwenden von `DefaultAzureCredential` und anderen Authentifizierungsoptionen finden Sie unter [*Vorgehensweise: Schreiben von App-Authentifizierungscode*](../articles/digital-twins/how-to-authenticate-client.md).