---
title: Aktivieren einer verwalteten Identität für benutzerdefinierte Azure Event Grid-Themen und -Domänen
description: In diesem Artikel wird beschrieben, wie Sie die verwaltete Dienstidentität für ein benutzerdefiniertes Azure Event Grid-Thema oder eine benutzerdefinierte Azure Event Grid-Domäne aktivieren.
ms.topic: how-to
ms.date: 03/25/2021
ms.openlocfilehash: b93fd44282d9e19e7111dd52c73d8d4c01c67a10
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2021
ms.locfileid: "106278217"
---
# <a name="assign-a-system-managed-identity-to-an-event-grid-custom-topic-or-domain"></a>Zuweisen einer systemseitig verwalteten Identität zu einem benutzerdefinierten Event Grid-Thema oder einer benutzerdefinierten Event Grid-Domäne 
In diesem Artikel wird beschrieben, wie Sie eine systemseitig verwaltete Identität für ein benutzerdefiniertes Event Grid-Thema oder eine benutzerdefinierte Event Grid-Domäne aktivieren. Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](../active-directory/managed-identities-azure-resources/overview.md).

## <a name="enable-identity-at-the-time-of-creation"></a>Aktivieren der Identität zum Zeitpunkt der Erstellung

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Beim Erstellen im Azure-Portal können Sie die systemseitig zugewiesene Identität für ein benutzerdefiniertes Thema oder eine benutzerdefinierte Domäne aktivieren. Die folgende Abbildung zeigt, wie die vom System verwaltete Identität für ein benutzerdefiniertes Thema aktiviert wird. Im Wesentlichen wählen Sie im Assistenten zum Erstellen von Themen auf der Seite **Erweitert** die Option **Systemseitig zugewiesene Identität aktivieren** aus. Diese Option wird auch auf der Seite **Erweitert** des Assistenten zum Erstellen von Domänen angezeigt. 

![Aktivieren der Identität beim Erstellen eines benutzerdefinierten Themas](./media/managed-service-identity/create-topic-identity.png)

### <a name="using-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Sie können ein benutzerdefiniertes Thema oder eine benutzerdefinierte Domäne mit einer vom System zugewiesenen Identität auch mit der Azure-Befehlszeilenschnittstelle erstellen. Verwenden Sie den Befehl `az eventgrid topic create`, wobei der Parameter `--identity` auf `systemassigned` festgelegt ist. Wenn Sie für diesen Parameter keinen Wert angeben, wird der Standardwert `noidentity` verwendet. 

```azurecli-interactive
# create a custom topic with a system-assigned identity
az eventgrid topic create -g <RESOURCE GROUP NAME> --name <TOPIC NAME> -l <LOCATION>  --identity systemassigned
```

Entsprechend können Sie den Befehl `az eventgrid domain create` verwenden, um eine Domäne mit einer vom System verwalteten Identität zu erstellen.

## <a name="enable-identity-for-an-existing-custom-topic-or-domain"></a>Aktivieren der Identität für ein vorhandenes benutzerdefiniertes Thema oder eine vorhandene benutzerdefinierte Domäne
In diesem Abschnitt erfahren Sie, wie Sie eine vom System verwaltete Identität für ein vorhandenes benutzerdefiniertes Thema oder eine vorhandene benutzerdefinierte Domäne aktivieren. 

### <a name="using-azure-portal"></a>Verwenden des Azure-Portals
Das folgende Verfahren zeigt, wie die vom System verwaltete Identität für ein benutzerdefiniertes Thema aktiviert wird. Die Schritte zum Aktivieren einer Identität für eine Domäne sind ähnlich. 

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Suchen Sie oben in der Suchleiste nach **Event Grid-Themen**.
3. Wählen Sie das **benutzerdefinierte Thema** aus, für das Sie die verwaltete Identität aktivieren möchten. 
4. Wechseln Sie zur Registerkarte **Identität**. 
5. **Aktivieren** Sie den Schalter, um die Identität zu aktivieren. 
1. Wählen Sie auf der Symbolleiste **Speichern** aus, um die Einstellung zu speichern. 

    :::image type="content" source="./media/managed-service-identity/identity-existing-topic.png" alt-text="Seite „Identität“ für ein benutzerdefiniertes Thema"::: 

Sie können ähnliche Schritte zum Aktivieren der Identität für eine Event Grid-Domäne verwenden.

### <a name="use-the-azure-cli"></a>Verwenden der Azure-CLI
Verwenden Sie den Befehl `az eventgrid topic update`, wobei Sie `--identity` auf `systemassigned` festlegen, um die vom System zugewiesene Identität für ein vorhandenes benutzerdefiniertes Thema zu aktivieren. Wenn Sie die Identität deaktivieren möchten, geben Sie als Wert `noidentity` an. 

```azurecli-interactive
# Update the topic to assign a system-assigned identity. 
az eventgrid topic update -g $rg --name $topicname --identity systemassigned --sku basic 
```

Der Befehl zum Aktualisieren einer vorhandenen Domäne lautet entsprechend (`az eventgrid domain update`).


## <a name="next-steps"></a>Nächste Schritte
Fügen Sie die Identität einer geeigneten Rolle (z. B. Azure Service Bus-Datensender) für das Ziel (z. B. Service Bus-Warteschlange) zu. Ausführliche Schritte finden Sie unter [Gewähren des Zugriffs auf ein Event Grid-Ziel für die verwaltete Identität](add-identity-roles.md). 
