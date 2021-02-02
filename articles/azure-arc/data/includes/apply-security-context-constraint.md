---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 01/15/2021
ms.author: mikeray
ms.openlocfilehash: 17a8c9580a8e69213c7f34e8ec889f7e46c6d17d
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98696062"
---
In diesem Abschnitt wird erläutert, wie eine Einschränkung beim Sicherheitskontext (Security Context Constraint, SCC) angewendet wird. Für das Vorschaurelease lockern diese die Sicherheitseinschränkungen. 

1. Laden Sie die benutzerdefinierte Sicherheitskontexteinschränkung (Security Context Constraint, SCC) herunter. Verwenden Sie einen der folgenden Werte: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - ([Raw](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml))
   - `curl` Mit dem folgenden Befehl wird „arc-data-scc.yaml“ heruntergeladen:

      ```console
      curl https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml -o arc-data-scc.yaml
      ```

1. Create SCC (SCC erstellen).

   ```console
   oc create -f arc-data-scc.yaml
   ```

1. Wenden Sie die Sicherheitskontexteinschränkung auf das Dienstkonto an.

   > [!NOTE]
   > Verwenden Sie hier und im Befehl `azdata arc dc create` unten denselben Namespace. Beispiel: `arc`.

   ```console
   oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
   ```