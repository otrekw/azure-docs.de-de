---
author: baanders
description: Includedatei zum Einrichten der lokalen Authentifizierung für DefaultAzureCredential in Azure Digital Twins-Beispielen – ohne Einführung
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 8bf0590b867d37a22706f679bfbeee2140935637
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494691"
---
Mit `DefaultAzureCredential` sucht das Beispiel nach Anmeldeinformationen in Ihrer lokalen Umgebung, wie eine Azure-Anmeldung in einer lokalen [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) oder in Visual Studio/Visual Studio Code. Das bedeutet, dass Sie sich **lokal** über einen dieser Mechanismen bei Azure anmelden, um Anmeldeinformationen für das Beispiel einzurichten.

Wenn Sie Visual Studio oder Visual Studio Code zum Ausführen des Codebeispiels verwenden, stellen Sie sicher, dass Sie bei diesem Editor mit denselben Anmeldeinformationen für Azure angemeldet sind, die Sie für den Zugriff auf Ihre Instanz von Azure Digital Twins verwenden möchten.

Andernfalls können Sie die [lokale **Azure CLI**](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) installieren, eine Eingabeaufforderung auf Ihrem Computer starten und den Befehl `az login` ausführen, um sich bei Ihrem Azure-Konto anzumelden. Wenn Sie danach Ihr Codebeispiel ausführen, sollte es Sie automatisch anmelden.