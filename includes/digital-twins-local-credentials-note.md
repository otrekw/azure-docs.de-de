---
author: baanders
description: Includedatei für DefaultAzureCredential in Azure Digital Twins-Beispielen – Hinweis
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 462098536ae4598c7875a1b8291e477f6b4a8d7d
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108778368"
---
>[!NOTE]
> Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Bei dieser Art der Authentifizierung sucht das Beispiel in Ihrer lokalen Umgebung nach Azure-Anmeldeinformationen, z. B. eine Anmeldung von einer lokalen [Azure CLI](/cli/azure/install-azure-cli) oder in Visual Studio/Visual Studio Code.
>
> Weitere Informationen zum Verwenden von `DefaultAzureCredential` und anderen Authentifizierungsoptionen finden Sie unter [Vorgehensweise: Schreiben von App-Authentifizierungscode](../articles/digital-twins/how-to-authenticate-client.md).