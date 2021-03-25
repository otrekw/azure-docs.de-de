---
author: baanders
description: Includedatei zum Einrichten der lokalen Authentifizierung für DefaultAzureCredential in Azure Digital Twins-Beispielen – ohne Einführung
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: fb148551db798207a52bd7aef629da79dd3341e1
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102212812"
---
Mit `DefaultAzureCredential` sucht das Beispiel nach Anmeldeinformationen in Ihrer lokalen Umgebung, wie eine Azure-Anmeldung in einer lokalen [Azure CLI](/cli/azure/install-azure-cli) oder in Visual Studio oder Visual Studio Code. Aus diesem Grund sollten Sie sich *lokal* über einen dieser Mechanismen bei Azure anmelden, um Anmeldeinformationen für das Beispiel einzurichten.

Wenn Sie Visual Studio oder Visual Studio Code zum Ausführen des Codebeispiels verwenden, stellen Sie sicher, dass Sie bei diesem Editor mit denselben Anmeldeinformationen für Azure angemeldet sind, die Sie für den Zugriff auf Ihre Instanz von Azure Digital Twins verwenden möchten.

Andernfalls können Sie die [lokale Azure CLI installieren](/cli/azure/install-azure-cli), eine Eingabeaufforderung auf Ihrem Computer starten und den Befehl `az login` ausführen, um sich bei Ihrem Azure-Konto anzumelden. Wenn Sie nach der Anmeldung Ihr Codebeispiel ausführen, sollte es Sie automatisch anmelden.