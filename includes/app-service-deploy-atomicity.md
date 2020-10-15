---
title: include file
description: include file
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 01/07/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: feed8b21833d4244d027d64d5e6547b94e4fa66f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75945144"
---
## <a name="what-happens-to-my-app-during-deployment"></a>Was geschieht während der Bereitstellung mit meiner App?

Alle offiziell unterstützten Bereitstellungsmethoden nehmen Änderungen an den Dateien im Ordner `/home/site/wwwroot` Ihrer App vor. Diese Dateien werden zum Ausführen Ihrer App verwendet. Daher kann die Bereitstellung aufgrund von gesperrten Dateien fehlschlagen. Die App kann sich während der Bereitstellung unvorhersehbar verhalten, da nicht alle Dateien gleichzeitig aktualisiert werden. Dies ist für eine kundenorientierte App nicht erwünscht. Es gibt mehrere Möglichkeiten, wie Sie diese Probleme umgehen können:

- [Führen Sie die App direkt aus dem ZIP-Paket](../articles/app-service/deploy-run-package.md) aus, ohne Sie zu entpacken.
- Beenden Sie die App, oder aktivieren Sie den Offlinemodus für die App während der Bereitstellung. Weitere Informationen finden Sie unter [Deal with locked files during deployment (Umgang mit gesperrten Dateien während der Bereitstellung)](https://github.com/projectkudu/kudu/wiki/Dealing-with-locked-files-during-deployment).
- Stellen Sie einen [Stagingslot](../articles/app-service/deploy-staging-slots.md) bereit, bei dem [Automatisch tauschen](../articles/app-service/deploy-staging-slots.md#configure-auto-swap) aktiviert ist. 
