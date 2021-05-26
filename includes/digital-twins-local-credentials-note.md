---
author: baanders
description: Includedatei für DefaultAzureCredential in Azure Digital Twins-Beispielen – Hinweis
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 1275e7e230fedc4fae77592bf1232e62ca757b8b
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2021
ms.locfileid: "110078926"
---
>[!NOTE]
> Dieses Beispiel verwendet [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet&preserve-view=true) (Teil der `Azure.Identity`-Bibliothek) zur Authentifizierung von Benutzern mit der Azure Digital Twins-Instanz, wenn Sie diese auf Ihrem lokalen Computer ausführen. Bei dieser Art der Authentifizierung sucht das Beispiel in Ihrer lokalen Umgebung nach Azure-Anmeldeinformationen, z. B. eine Anmeldung von einer lokalen [Azure CLI](/cli/azure/install-azure-cli) oder in Visual Studio/Visual Studio Code.
>
> Weitere Informationen zum Verwenden von `DefaultAzureCredential` und anderen Authentifizierungsoptionen finden Sie unter [Vorgehensweise: Schreiben von App-Authentifizierungscode](../articles/digital-twins/how-to-authenticate-client.md).