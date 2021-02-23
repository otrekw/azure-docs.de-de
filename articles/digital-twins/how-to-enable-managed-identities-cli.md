---
title: 'Azure CLI: Aktivieren einer verwalteten Identität für die Weiterleitung von Ereignissen (Vorschau)'
titleSuffix: Azure Digital Twins
description: In diesem Artikel erfahren Sie, wie Sie eine systemseitig zugewiesene Identität für Azure Digital Twins aktivieren und zum Weiterleiten von Ereignissen mithilfe der Azure CLI verwenden.
author: baanders
ms.author: baanders
ms.date: 02/09/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 32cbe31f95c03f9b0b5eb1a31a28033dce18b112
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100473470"
---
# <a name="enable-a-managed-identity-for-routing-azure-digital-twins-events-preview-azure-cli"></a>Aktivieren einer verwalteten Identität für die Weiterleitung von Azure Digital Twins-Ereignissen (Vorschau): Azure CLI

[!INCLUDE [digital-twins-managed-service-identity-selector.md](../../includes/digital-twins-managed-service-identity-selector.md)]

In diesem Artikel wird beschrieben, wie Sie eine [systemseitig zugewiesene Identität für eine Azure Digital Twins-Instanz](concepts-security.md#managed-identity-for-accessing-other-resources-preview) (derzeit in der Vorschauphase) aktivieren und die Identität zum Weiterleiten von Ereignissen an unterstützte Ziele wie [Event Hub](../event-hubs/event-hubs-about.md), [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) und [Azure Storage Container](../storage/blobs/storage-blobs-introduction.md) verwenden.

In diesem Artikel wird die Vorgehensweise mit der [**Azure CLI**](/cli/azure/what-is-azure-cli) beschrieben.

Die folgenden Schritte werden in diesem Artikel behandelt: 

1. Erstellen Sie eine Azure Digital Twins-Instanz mit einer systemseitig zugewiesenen Identität, oder aktivieren Sie eine systemseitig zugewiesene Identität für eine vorhandene Azure Digital Twins-Instanz. 
1. Fügen Sie entsprechende Rollen zur Identität hinzu. Beispielsweise können Sie der Identität die Rolle **Azure Event Hub Data Sender** (Azure Event Hub-Datensender), wenn Sie einen Event Hub-Endpunkt verwenden, oder die Rolle **Azure Service Bus-Datensender** zuweisen, wenn Sie einen Service Bus-Endpunkt verwenden.
1. Erstellen Sie einen Endpunkt in Azure Digital Twins, der systemseitig zugewiesene Identitäten für die Authentifizierung verwenden kann.

## <a name="enable-system-managed-identities-for-an-instance"></a>Aktivieren von systemseitig verwalteten Identitäten für eine Instanz 

Wenn Sie eine systemseitig zugewiesene Identität für Ihre Azure Digital Twins-Instanz aktivieren, erstellt Azure automatisch eine Identität in [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) für diese. Diese Identität kann dann für die Authentifizierung bei Azure Digital Twins-Endpunkten für die Ereignisweiterleitung verwendet werden.

Sie können systemseitig verwaltete Identitäten für eine Azure Digital Twins-Instanz **als Teil der ersten Einrichtung der Instanz** oder **später für eine bereits vorhandene Instanz** aktivieren.

Beide dieser Erstellungsmethoden bieten Ihnen dieselben Konfigurationsoptionen und dasselbe Endergebnis für Ihre Instanz. In diesem Abschnitt werden beide Vorgehensweisen beschrieben.

### <a name="add-a-system-managed-identity-during-instance-creation"></a>Hinzufügen einer systemseitig verwalteten Identität während der Instanzerstellung

In diesem Abschnitt erfahren Sie, wie Sie eine systemseitig verwaltete Identität für eine Azure Digital Twins-Instanz aktivieren, die derzeit erstellt wird. 

Hierzu wird ein `--assign-identity`-Parameter zum `az dt create`-Befehl hinzugefügt, der zum Erstellen der Instanz verwendet wird. (Weitere Informationen zu diesem Befehl finden Sie in der zugehörigen [Referenzdokumentation](/cli/azure/ext/azure-iot/dt?view=azure-cli-latest&preserve-view=true#ext_azure_iot_az_dt_create) oder in den [allgemeinen Anweisungen zum Einrichten einer Azure Digital Twins-Instanz](how-to-set-up-instance-cli.md#create-the-azure-digital-twins-instance).)

Fügen Sie zum Erstellen einer Instanz mit einer systemseitig verwalteten Identität den `--assign-identity`-Parameter wie folgt hinzu:

```azurecli-interactive
az dt create -n {new_instance_name} -g {resource_group} --assign-identity
```

### <a name="add-a-system-managed-identity-to-an-existing-instance"></a>Hinzufügen einer systemseitig verwalteten Identität zu einer vorhandenen Instanz

In diesem Abschnitt fügen Sie eine systemseitig verwaltete Identität zu einer bereits vorhandenen Azure Digital Twins-Instanz hinzu.

Hierzu kann auch der `az dt create`-Befehl mit dem `--assign-identity`-Parameter verwendet werden. Anstatt einen neuen Namen einer zu erstellenden Instanz anzugeben, können Sie den Namen einer bereits vorhandenen Instanz angeben, um den `--assign-identity`-Wert dieser Instanz zu ändern.

Der Befehl zum **Aktivieren** der verwalteten Identität stimmt mit dem Befehl zum Erstellen einer Instanz mit einer systemseitig verwalteten Identität überein. Es wird lediglich der Wert des Instanznamenparameters geändert:

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --assign-identity
```

Verwenden Sie den folgenden ähnlichen Befehl zum Festlegen von `--assign-identity` auf `false`, um die verwaltete Identität für einer Instanz zu **deaktivieren**, für die sie derzeit aktiviert ist.

```azurecli-interactive
az dt create -n {name_of_existing_instance} -g {resource_group} --assign-identity false
```

## <a name="assign-azure-roles-to-the-identity"></a>Zuweisen von Azure-Rollen zur Identität 

Nachdem Sie eine systemseitig zugewiesene Identität für Ihre Azure Digital Twins-Instanz erstellt haben, müssen Sie dieser die entsprechenden Rollen für die Authentifizierung bei verschiedenen Arten von [Endpunkten](concepts-route-events.md) zum Weiterleiten von Ereignissen an unterstützte Ziele zuweisen. In diesem Abschnitt werden die Rollenoptionen und das Zuweisen dieser zur systemseitig zugewiesenen Identität beschrieben.

>[!NOTE]
> Hierbei handelt es sich um einen wichtigen Schritt. Wenn Sie diesen Schritt nicht durchführen, kann die Identität nicht auf Ihre Endpunkte zugreifen und Ereignisse werden nicht übermittelt.

### <a name="supported-destinations-and-azure-roles"></a>Unterstützte Ziele und Azure-Rollen 

Im Folgenden finden Sie die mindestens erforderlichen Rollen, die eine Identität je nach Art von Ziel für den Zugriff auf einen Endpunkt benötigt. Rollen mit höheren Berechtigungen (beispielsweise Datenbesitzerrollen) funktionieren ebenfalls.

| Destination | Azure-Rolle |
| --- | --- |
| Azure Event Hubs | Azure Event Hubs-Datensender |
| Azure-Servicebus | Azure Service Bus-Datensender |
| Azure-Speichercontainer | Mitwirkender an Storage-Blobdaten |

Weitere Informationen über Endpunkte, Routen und die verschiedenen unterstützten Ziele für das Routing in Azure Digital Twins finden Sie unter [*Konzepte: Ereignisrouten*](concepts-route-events.md).

### <a name="assign-the-role"></a>Zuweisen der Rolle

>[!NOTE]
> Dieser Abschnitt muss von einem Azure-Benutzer durchgeführt werden, der über die nötigen Berechtigungen zum Verwalten des Benutzerzugriffs auf Azure-Ressourcen (einschließlich des Gewährens und Delegieren von Berechtigungen) verfügt. Allgemeine Rollen, die diese Anforderung erfüllen, heißen *Besitzer* oder *Kontoadministrator*. Ebenso können die beiden Rollen *Benutzerzugriffsadministrator* und *Mitwirkender* kombiniert werden. Weitere Informationen über die erforderlichen Berechtigungen für Azure Digital Twins-Rollen finden Sie unter [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-portal.md#prerequisites-permission-requirements).

Sie können den Parameter `--scopes` zum Befehl `az dt create` hinzufügen, um die Identität einem oder mehreren Bereichen mit einer bestimmten Rolle zuzuweisen. Diese Vorgehensweise kann bereits bei der Erstellung der Instanz oder auch später verwendet werden, indem Sie den Namen einer bereits vorhandenen Instanz übergeben.

Hier sehen Sie ein Beispiel, bei dem eine Instanz mit einer systemseitig verwalteten Identität erstellt und dieser Identität eine benutzerdefinierte Rolle namens `MyCustomRole` in einem Event Hub zugewiesen wird.

```azurecli-interactive
az dt create -n {instance_name} -g {resource_group} --assign-identity --scopes "/subscriptions/<subscription ID>/resourceGroups/<resource_group>/providers/Microsoft.EventHub/namespaces/<Event_Hubs_namespace>/eventhubs/<event_hub_name>" --role MyCustomRole
```

Weitere Beispiele für Rollenzuweisungen mit diesem Befehl finden Sie in der [Referenzdokumentation zu **az dt create**](/cli/azure/ext/azure-iot/dt?view=azure-cli-latest&preserve-view=true#ext_azure_iot_az_dt_create).

Alternativ können Sie auch die Befehlsgruppe [**az role assignment**](/cli/azure/role/assignment?view=azure-cli-latest&preserve-view=true) verwenden, um Rollen zu erstellen und zu verwalten. Sie können diese Vorgehensweise verwenden, um weitere Szenarios zu unterstützen, in denen Sie die Rollenzuweisung nicht mit dem Erstellbefehl gruppieren möchten.

## <a name="create-an-endpoint-with-identity-based-authorization"></a>Erstellen eines Endpunkts mit identitätsbasierter Autorisierung

Nachdem Sie eine systemseitig verwaltete Identität für Ihre Azure Digital Twins-Instanz eingerichtet und dieser die entsprechenden Rollen zugewiesen haben, können Sie Azure Digital Twins-[Endpunkte](how-to-manage-routes-portal.md#create-an-endpoint-for-azure-digital-twins) erstellen, die die Identität für die Authentifizierung verwenden können. Diese Option ist nur für Event Hub- und Service Bus-Endpunkte verfügbar (sie wird nicht für Event Grid unterstützt).

>[!NOTE]
> Sie können einen Endpunkt nicht bearbeiten, der bereits mit einer schlüsselbasierten Identität erstellt wurde, um zur identitätsbasierten Authentifizierung zu wechseln. Sie müssen den Authentifizierungstyp auswählen, wenn der Endpunkt erstellt wird.

Hierzu wird ein `--auth-type`-Parameter zum `az dt endpoint create`-Befehl hinzugefügt, der zum Erstellen des Endpunkts verwendet wird. (Weitere Informationen zu diesem Befehl finden Sie in der zugehörigen [Referenzdokumentation](/cli/azure/ext/azure-iot/dt/endpoint/create?view=azure-cli-latest&preserve-view=true) oder in den [allgemeinen Anweisungen zum Einrichten eines Azure Digital Twins-Endpunkts](how-to-manage-routes-apis-cli.md#create-the-endpoint).)

Zum Erstellen eines Endpunkts, der die identitätsbasierte Authentifizierung verwendet, legen Sie den Authentifizierungstyp `IdentityBased` mit dem Parameter `--auth-type` fest. Im folgenden Beispiel wird diese Vorgehensweise für einen Event Hubs-Endpunkt veranschaulicht.

```azurecli-interactive
az dt endpoint create eventhub --endpoint-name {endpoint_name} --eventhub-resource-group {eventhub_resource_group} --eventhub-namespace {eventhub_namespace} --eventhub {eventhub_name} --auth-type IdentityBased -n {instance_name}
```

## <a name="considerations-for-disabling-system-managed-identities"></a>Überlegungen zum Deaktivieren von systemseitig verwalteten Identitäten

Da Identitäten separat von den Endpunkten verwaltet werden, die sie verwenden, ist es wichtig, dass die möglichen Auswirkungen von Änderungen an der Identität oder ihrer Rollen auf die Endpunkte Ihrer Azure Digital Twins-Instanz berücksichtigt werden. Wenn die Identität deaktiviert ist oder eine notwendige Rolle für einen Endpunkt entfernt wird, kann der Endpunkt unzugänglich und der Ablauf der Ereignisse unterbrochen werden.

Sie müssen den Endpunkt löschen und mit einem anderen Authentifizierungstyp [neu erstellen](how-to-manage-routes-apis-cli.md#create-an-endpoint-for-azure-digital-twins), um einen Endpunkt weiterhin zu verwenden, der mit einer verwalteten Identität eingerichtet wurde, die deaktiviert wurde. Es kann bis zu eine Stunde dauern, bis Ereignisse nach dieser Änderung wieder an den Endpunkt übermittelt werden.

## <a name="next-steps"></a>Nächste Schritte

Lernen Sie mehr über verwaltete Identitäten in Azure AD: 
* [*Verwaltete Identitäten für Azure-Ressourcen*](../active-directory/managed-identities-azure-resources/overview.md)