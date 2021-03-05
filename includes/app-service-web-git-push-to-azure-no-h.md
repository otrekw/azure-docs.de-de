---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 08f4e76869f124d946566f64da394c895d0af308
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102205978"
---
Kehren Sie zum lokalen Terminalfenster zurück, und fügen Sie Ihrem lokalen Git-Repository einen Azure-Remotespeicherort hinzu. Ersetzen Sie *\<deploymentLocalGitUrl-from-create-step>* durch die URL des Git-Remotespeicherorts, die Sie unter [Erstellen einer Web-App](#create-a-web-app) gespeichert haben.

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Führen Sie einen Pushvorgang zum Azure-Remotespeicherort durch, um Ihre App mit dem folgenden Befehl bereitzustellen. Wenn Sie von der Git-Anmeldeinformationsverwaltung zur Eingabe von Anmeldeinformationen aufgefordert werden, müssen Sie die Anmeldeinformationen eingeben, die Sie in **Konfigurieren eines Bereitstellungsbenutzers** erstellt haben (nicht die Anmeldeinformationen, die Sie zur Anmeldung beim Azure-Portal verwenden).

```bash
git push azure main
```

Die Ausführung dieses Befehls kann einige Minuten in Anspruch nehmen. Während der Ausführung werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:
