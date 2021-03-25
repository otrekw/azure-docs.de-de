---
author: MikeRayMSFT
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.topic: include
ms.date: 03/02/2021
ms.author: mikeray
ms.openlocfilehash: 0fca43f76b24a08ca96be749f7f2a822b0be2418
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101687588"
---
In diesem Abschnitt wird erläutert, wie eine Einschränkung beim Sicherheitskontext (Security Context Constraint, SCC) angewendet wird. Für das Vorschaurelease lockern diese die Sicherheitseinschränkungen. 

1. Laden Sie die benutzerdefinierte Sicherheitskontexteinschränkung (Security Context Constraint, SCC) herunter. Verwenden Sie einen der folgenden Werte: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/main/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - [Raw](https://raw.githubusercontent.com/microsoft/azure_arc/main/arc_data_services/deploy/yaml/arc-data-scc.yaml)
   - `curl`
   
       Mit dem folgenden Befehl wird „arc-data-scc.yaml“ heruntergeladen:

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

   ```console
   oc create rolebinding arc-data-rbac --clusterrole=system:openshift:scc:arc-data-scc --serviceaccount=arc:default
   ```
