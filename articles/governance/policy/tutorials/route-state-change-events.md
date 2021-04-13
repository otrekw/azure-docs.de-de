---
title: 'Tutorial: Weiterleiten von Ereignissen vom Typ „Richtlinienzustandsänderung“ an Event Grid mithilfe der Azure CLI'
description: In diesem Tutorial wird Event Grid konfiguriert, um auf Ereignisse vom Typ „Richtlinienzustandsänderung“ zu lauschen und einen Webhook aufzurufen.
ms.date: 03/29/2021
ms.topic: tutorial
ms.custom: devx-track-azurecli
ms.openlocfilehash: 1fe87e4fd3349df7d8f5d57b2b2d95f95ed3fba8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105734875"
---
# <a name="tutorial-route-policy-state-change-events-to-event-grid-with-azure-cli"></a>Tutorial: Weiterleiten von Ereignissen vom Typ „Richtlinienzustandsänderung“ an Event Grid mithilfe der Azure CLI

In diesem Artikel erfahren Sie, wie Sie Azure Policy-Ereignisabonnements einrichten, um Ereignisse vom Typ „Richtlinienzustandsänderung“ an einen Webendpunkt zu senden. Azure Policy-Benutzer können Ereignisse abonnieren, die ausgegeben werden, wenn sich der Richtlinienzustand für Ressourcen ändert. Diese Ereignisse können Webhooks, [Azure Functions](../../../azure-functions/index.yml), [Azure Storage-Warteschlangen](../../../storage/queues/index.yml) oder andere Ereignishandler auslösen, die von [Azure Event Grid](../../../event-grid/index.yml) unterstützt werden. Üblicherweise senden Sie Ereignisse an einen Endpunkt, der die Ereignisdaten verarbeitet und entsprechende Aktionen ausführt. Der Einfachheit halber werden die Ereignisse in diesem Tutorial allerdings an eine Web-App gesendet, die die Nachrichten sammelt und anzeigt.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

- Für diesen Schnellstart muss mindestens Version 2.0.76 der Azure CLI ausgeführt werden. Führen Sie `az --version` aus, um die Version zu finden. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

- Registrieren Sie die entsprechenden Ressourcenanbieter erneut, auch wenn Sie Azure Policy oder Event Grid bereits verwendet haben:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Provider register: Register the Azure Event Grid provider
  az provider register --namespace Microsoft.EventGrid
  ```

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Event Grid-Themen sind Azure-Ressourcen und müssen in einer Azure-Ressourcengruppe platziert werden. Die Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mithilfe des Befehls [az group create](/cli/azure/group) eine Ressourcengruppe. 

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen `<resource_group_name>` am Standort _westus_ erstellt. Ersetzen Sie `<resource_group_name>` durch einen eindeutigen Namen für Ihre Ressourcengruppe.

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az group create --name <resource_group_name> --location westus
```

## <a name="create-an-event-grid-system-topic"></a>Erstellen eines Event Grid-Systemthemas

Nachdem wir nun über eine Ressourcengruppe verfügen, erstellen wir als Nächstes ein [Systemthema](../../../event-grid/system-topics.md). Ein Systemthema in Event Grid stellt mindestens ein Ereignis dar, das von Azure-Diensten wie Azure Policy und Azure Event Hubs veröffentlicht wurde. Von diesem Systemthema wird der Thementyp `Microsoft.PolicyInsights.PolicyStates` für Azure Policy-Zustandsänderungen verwendet. Ersetzen Sie `<SubscriptionID>` im Parameter **scope** durch die ID Ihres Abonnements und `<resource_group_name>` im Parameter **resource-group** durch die zuvor erstellte Ressourcengruppe.

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az eventgrid system-topic create --name PolicyStateChanges --location global --topic-type Microsoft.PolicyInsights.PolicyStates --source "/subscriptions/<SubscriptionID>" --resource-group "<resource_group_name>"
```

## <a name="create-a-message-endpoint"></a>Erstellen eines Nachrichtenendpunkts

Vor dem Abonnieren des Themas erstellen wir zunächst den Endpunkt für die Ereignisnachricht. Der Endpunkt führt in der Regel Aktionen auf der Grundlage der Ereignisdaten aus. Der Einfachheit halber stellen Sie in dieser Schnellstartanleitung eine [vorgefertigte Web-App](https://github.com/Azure-Samples/azure-event-grid-viewer) bereit, die die Ereignisnachrichten anzeigt. Die bereitgestellte Lösung umfasst einen App Service-Plan, eine App Service-Web-App und Quellcode von GitHub.

Ersetzen Sie `<your-site-name>` durch einen eindeutigen Namen für Ihre Web-App. Der Name der Web-App muss eindeutig sein, da er Teil des DNS-Eintrags ist.

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az deployment group create \
  --resource-group <resource_group_name> \
  --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
  --parameters siteName=<your-site-name> hostingPlanName=viewerhost
```

Die Bereitstellung kann einige Minuten dauern. Nach erfolgreichem Abschluss der Bereitstellung können Sie Ihre Web-App anzeigen und sich vergewissern, dass sie ausgeführt wird. Navigieren Sie hierzu in einem Webbrowser zu `https://<your-site-name>.azurewebsites.net`.

Die Website sollte angezeigt werden, und es sollten momentan keine Nachrichten vorliegen.

## <a name="subscribe-to-the-system-topic"></a>Abonnieren des Systemthemas

Sie abonnieren ein Thema, um Event Grid mitzuteilen, welche Ereignisse Sie nachverfolgen möchten und wohin sie gesendet werden sollen. Im folgenden Beispiel wird das von Ihnen erstellte Systemthema abonniert. Außerdem wird die URL Ihrer Web-App als Endpunkt für den Empfang von Ereignisbenachrichtigungen übergeben. Ersetzen Sie `<event_subscription_name>` durch einen Namen für Ihr Ereignisabonnement. Verwenden Sie für `<resource_group_name>` und `<your-site-name>` jeweils den zuvor erstellten Wert.

Der Endpunkt für Ihre Web-App muss das Suffix `/api/updates/` enthalten.

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Create the subscription
az eventgrid system-topic event-subscription create \
  --name <event_subscription_name> \
  --resource-group <resource_group_name> \
  --system-topic-name PolicyStateChanges \
  --endpoint https://<your-site-name>.azurewebsites.net/api/updates
```

Zeigen Sie wieder Ihre Web-App an. Wie Sie sehen, wurde ein Abonnementüberprüfungsereignis an sie gesendet. Klicken Sie auf das Augensymbol, um die Ereignisdaten zu erweitern. Event Grid sendet das Überprüfungsereignis, damit der Endpunkt bestätigen kann, dass er Ereignisdaten empfangen möchte. Die Web-App enthält Code zur Überprüfung des Abonnements.

:::image type="content" source="../media/route-state-change-events/view-subscription-event.png" alt-text="Screenshot: Event Grid-Abonnementüberprüfungsereignis in der vorgefertigten Web-App":::

## <a name="create-a-policy-assignment"></a>Erstellen einer Richtlinienzuweisung

In dieser Schnellstartanleitung wird eine Richtlinienzuweisung erstellt und die Definition **Tag für Ressourcengruppen erforderlich** zugewiesen. Diese Richtliniendefinition dient dazu, Ressourcengruppen zu identifizieren, bei denen das im Rahmen der Richtlinienzuweisung konfigurierte Tag fehlt.

Führen Sie den folgenden Befehl aus, um eine Richtlinienzuweisung zu erstellen, die auf die Ressourcengruppe ausgerichtet ist, die Sie für das Event Grid-Thema erstellt haben:

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az policy assignment create --name 'requiredtags-events' --display-name 'Require tag on RG' --scope '<ResourceGroupScope>' --policy '<policy definition ID>' --params '{ "tagName": { "value": "EventTest" } }'
```

In dem Befehl werden folgende Informationen verwendet:

- **Name:** Der tatsächliche Name der Zuweisung. Für dieses Beispiel wurde _requiredtags-events_ verwendet.
- **DisplayName:** Der Anzeigename für die Richtlinienzuweisung. In diesem Fall wird _Require tag on RG_ verwendet.
- **Bereich:** Ein Bereich bestimmt, für welche Ressourcen oder Ressourcengruppe die Richtlinienzuweisung erzwungen wird. Er kann von einem Abonnement bis zu Ressourcengruppen reichen. Ersetzen Sie &lt;scope&gt; durch den Namen Ihrer Ressourcengruppe. Ein Ressourcengruppenbereich hat folgendes Format: `/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup>`.
- **Richtlinie:** Die Richtliniendefinitions-ID, auf deren Grundlage Sie die Zuweisung erstellen. In diesem Fall handelt es sich hierbei um die ID der Richtliniendefinition _Tag für Ressourcengruppen erforderlich_. Führen Sie den folgenden Befehl aus, um die Richtliniendefinitions-ID abzurufen: `az policy definition list --query "[?displayName=='Require a tag on resource groups']"`

Warten Sie nach dem Erstellen der Richtlinienzuweisung, bis in der Web-App eine Ereignisbenachrichtigung vom Typ **Microsoft.PolicyInsights.PolicyStateCreated** angezeigt wird. Der Wert `data.complianceState` der erstellten Ressourcengruppe lautet zunächst _NonCompliant_ (Nicht konform).

:::image type="content" source="../media/route-state-change-events/view-policystatecreated-event.png" alt-text="Screenshot: Event Grid-Abonnement des Ereignisses „PolicyStateCreated“ für die Ressourcengruppe in der vorgefertigten Web-App":::

> [!NOTE]
> Wenn die Ressourcengruppe andere Richtlinienzuweisungen aus der Abonnement- oder Verwaltungsgruppenhierarchie erbt, werden entsprechende Ereignisse ebenfalls angezeigt. Vergewissern Sie sich, dass das Ereignis die Zuweisung in diesem Tutorial betrifft, indem Sie die Eigenschaft `data.policyDefinitionId` auswerten.

## <a name="trigger-a-change-on-the-resource-group"></a>Auslösen einer Änderung für die Ressourcengruppe

Die Ressourcengruppe ist konform, wenn ein Tag mit dem Namen **EventTest** vorhanden ist. Fügen Sie das Tag mithilfe des folgenden Befehls der Ressourcengruppe hinzu, und ersetzen Sie dabei `<SubscriptionID>` durch Ihre Abonnement-ID und `<ResourceGroup>` durch den Namen der Ressourcengruppe:

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az tag create --resource-id '/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup>' --tags EventTest=true
```

Warten Sie, bis in der Web-App eine Ereignisbenachrichtigung vom Typ **Microsoft.PolicyInsights.PolicyStateChanged** angezeigt wird, Nachdem Sie der Ressourcengruppe das erforderliche Tag hinzugefügt haben. Erweitern Sie das Ereignis. Daraufhin sehen Sie, dass der Wert `data.complianceState` nun _Compliant_ (Konform) lautet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie diese Web-App und das Azure Policy-Ereignisabonnement weiterverwenden möchten, überspringen Sie die Bereinigung der in diesem Artikel erstellten Ressourcen. Führen Sie andernfalls den folgenden Befehl aus, um die in diesem Artikel erstellten Ressourcen zu löschen.

Ersetzen Sie `<resource_group_name>` durch die weiter oben erstellte Ressourcengruppe.

```azurecli-interactive
az group delete --name <resource_group_name>
```

## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, wie Sie Themen und Ereignisabonnements für Azure Policy erstellen. Nun können Sie sich eingehender über Ereignisse vom Typ „Richtlinienzustandsänderung“ sowie über die Möglichkeiten von Event Grid informieren:

- [Reagieren auf Azure Policy-Ereignisse vom Typ „Richtlinienzustandsänderung“](../concepts/event-overview.md)
- [Azure Policy als Event Grid-Quelle](../../../event-grid/event-schema-policy.md)
- [Einführung in Azure Event Grid](../../../event-grid/overview.md)