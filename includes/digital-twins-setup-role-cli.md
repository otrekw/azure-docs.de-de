---
author: baanders
description: Includedatei für die Rollenanforderung beim Setup von Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 7/17/2020
ms.author: baanders
ms.openlocfilehash: 1714fac465531a2d3a5829e7080132510ccd1ec4
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407483"
---
[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites-permission-requirements"></a>Voraussetzungen: Berechtigungsanforderungen

Um alle Schritte in diesem Artikel durchführen zu können, müssen Sie in Ihrem Azure-Abonnement als Besitzer klassifiziert sein. 

Sie können Ihre Berechtigungsebene überprüfen, indem Sie in Cloud Shell folgenden Befehl ausführen:

```azurecli-interactive
az role assignment list --assignee <your-Azure-email>
```

Wenn Sie Besitzer sind, lautet der Wert von `roleDefinitionName` in der Ausgabe *Besitzer*:

:::image type="content" source="../articles/digital-twins/media/how-to-set-up-instance/cloud-shell/owner-role.png" alt-text="Cloud Shell-Fenster mit der Ausgabe des Befehls „az role assignment list“":::

Wenn Sie feststellen, dass der Wert *Mitwirkender* oder auf andere Weise nicht *Besitzer* lautet, können Sie auf eine der folgenden Vorgehensweisen zurückgreifen:
* Wenden Sie sich an den Besitzer Ihres Abonnements, und bitten Sie den Besitzer, die Schritte in diesem Artikel für Sie auszuführen.
* Wenden Sie sich an den Besitzer Ihres Abonnements oder an eine Person mit der Rolle „Benutzerzugriffsadministrator“ für das Abonnement, und fordern Sie an, zu einem Besitzer des Abonnements heraufgestuft zu werden, damit Sie selbst über die erforderlichen Berechtigungen zum Fortfahren verfügen. Ob diese Vorgehensweise angemessen ist, hängt von Ihrer Organisation und Ihrer Rolle darin ab.