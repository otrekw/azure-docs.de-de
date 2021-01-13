---
title: Datei einfügen
description: Datei einfügen
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 08/13/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3221c4f3e196cf1573bd7c0424fa3b4530c0b2ca
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90708270"
---
## <a name="create-azure-context"></a>Erstellen eines Azure-Kontexts

Um mithilfe von Docker-Befehlen Container in Azure Container Instances auszuführen, melden Sie sich zuerst bei Azure an:

```bash
docker login azure
```

Wenn Sie dazu aufgefordert werden, geben Sie Ihre Azure-Anmeldeinformationen ein, oder wählen Sie sie aus.


Führen Sie `docker context create aci` aus, um einen ACI-Kontext zu erstellen. Durch diesen Kontext wird Docker einem Azure-Abonnement und einer Ressourcengruppe zugeordnet, um die Erstellung und Verwaltung von Containerinstanzen zu ermöglichen. Um beispielsweise einen Kontext namens *myacicontext* zu erstellen:

```
docker context create aci myacicontext
```

Wählen Sie bei Aufforderung Ihre Azure-Abonnement-ID aus, wählen Sie dann eine vorhandene Ressourcengruppe aus, oder **erstellen Sie eine neue Ressourcengruppe**. Wenn Sie eine neue Ressourcengruppe ausgewählt haben, wird diese mit einem systemseitig generierten Namen erstellt. Azure Container Instances muss wie alle Azure-Ressourcen in einer Ressourcengruppe bereitgestellt werden. Mit Ressourcengruppen können verwandte Azure-Ressourcen organisiert und verwaltet werden.


Führen Sie `docker context ls` aus, um zu bestätigen, dass Sie Ihren Docker-Kontexten den ACI-Kontext hinzugefügt haben:

```
docker context ls
```