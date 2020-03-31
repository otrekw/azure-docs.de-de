---
title: Verschieben von Ressourcen in ein neues Abonnement oder eine neue Ressourcengruppe
description: Verwenden Sie Azure Resource Manager, um Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: 40432c55a7f7e289d2e5cbc8afe94847074e4ca8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226354"
---
# <a name="move-resources-to-a-new-resource-group-or-subscription"></a>Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Artikel wird erklärt, wie Sie Azure-Ressourcen entweder in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe im gleichen Abonnement verschieben. Sie können das Azure-Portal, Azure PowerShell, die Azure-Befehlszeilenschnittstelle oder die REST-API verwenden, um Ressourcen zu verschieben.

Beim Verschieben wird sowohl die Quell- als auch die Zielgruppe gesperrt. Schreib- und Löschvorgänge in den Ressourcengruppen werden bis zum Abschluss der Verschiebung blockiert. Diese Sperre bedeutet, dass Sie in Ressourcengruppen keinen Ressourcen hinzufügen, aktualisieren oder löschen können. Es heißt aber nicht, dass die Ressourcen eingefroren sind. Wenn Sie beispielsweise eine SQL Server-Instanz und ihre Datenbank in eine neue Ressourcengruppe verschieben, weist eine Anwendung, die die Datenbank nutzt, keine Ausfallzeiten auf. Sie hat weiterhin Lese- und Schreibzugriff auf die Datenbank. Die Sperre kann maximal vier Stunden dauern, aber die meisten Verschiebungen werden in viel kürzerer Zeit beendet.

Wenn Sie ein Ressource verschieben, wird sie nur in eine neue Ressourcengruppe oder ein neues Abonnement verschoben. Der Speicherort der Ressource ändert sich nicht.

## <a name="checklist-before-moving-resources"></a>Checkliste vor dem Verschieben von Ressourcen

Vor dem Verschieben einer Ressource müssen einige wichtige Schritte ausgeführt werden. Indem Sie diese Bedingungen überprüfen, können Sie Fehler vermeiden.

1. Die zu verschiebenden Ressourcen müssen den Verschiebevorgang unterstützen. Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).

1. Für einige Dienste gelten bestimmte Einschränkungen oder Anforderungen beim Verschieben von Ressourcen. Wenn Sie einen der folgenden Dienste verschieben möchten, überprüfen Sie diese Anleitung vor dem Verschieben.

   * [Anleitung zum Verschieben von App Services](./move-limitations/app-service-move-limitations.md)
   * [Anleitung zum Verschieben von Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
   * [Anleitung zum Verschieben des klassischen Bereitstellungsmodells](./move-limitations/classic-model-move-limitations.md): Compute (klassisch), Storage (klassisch), Virtual Networks (klassisch), Cloud Services (klassisch)
   * [Move guidance for networking resources](./move-limitations/networking-move-limitations.md) (Anleitung zum Verschieben von Netzwerkressourcen)
   * [Anleitung zum Verschieben von Recovery Services](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
   * [Anleitung zum Verschieben von Virtual Machines](./move-limitations/virtual-machines-move-limitations.md)

1. Quellen- und Zielabonnements müssen aktiv sein. Wenn beim Aktivieren eines Kontos, das deaktiviert wurde, Schwierigkeiten auftreten, [erstellen Sie eine Azure-Supportanfrage](../../azure-portal/supportability/how-to-create-azure-support-request.md). Wählen Sie **Abonnementverwaltung** als Problemtyp aus.

1. Quell- und Zielabonnement müssen im selben [Azure Active Directory-Mandanten](../../active-directory/develop/quickstart-create-new-tenant.md) vorhanden sein. Um zu überprüfen, ob beide Abonnements die gleiche Mandanten-ID aufweisen, verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle.

   Verwenden Sie für Azure PowerShell Folgendes:

   ```azurepowershell-interactive
   (Get-AzSubscription -SubscriptionName <your-source-subscription>).TenantId
   (Get-AzSubscription -SubscriptionName <your-destination-subscription>).TenantId
   ```

   Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

   ```azurecli-interactive
   az account show --subscription <your-source-subscription> --query tenantId
   az account show --subscription <your-destination-subscription> --query tenantId
   ```

   Wenn die Mandanten-IDs für das Quell- und das Zielabonnement nicht gleich sind, verwenden Sie die folgenden Methoden, um die Mandanten-IDs aufeinander abzustimmen:

   * [Übertragen des Besitzes eines Azure-Abonnements auf ein anderes Konto](../../billing/billing-subscription-transfer.md)
   * [Zuweisen oder Hinzufügen eines Azure-Abonnements zu Azure Active Directory](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

1. Das Zielabonnement muss für den Ressourcenanbieter der verschobenen Ressource registriert sein. Andernfalls erhalten Sie eine Fehlermeldung, die besagt, dass das **Abonnement nicht für einen Ressourcentyp registriert ist**. Dieser Fehler kann auftreten, wenn eine Ressource in ein neues Abonnement verschoben wird, dieses aber noch nie mit diesem Ressourcentyp verwendet wurde.

   Verwenden Sie für PowerShell die folgenden Befehle zum Abrufen des Registrierungsstatus:

   ```azurepowershell-interactive
   Set-AzContext -Subscription <destination-subscription-name-or-id>
   Get-AzResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
   ```

   Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

   ```azurepowershell-interactive
   Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
   ```

   Verwenden Sie für die Azure CLI die folgenden Befehle zum Abrufen des Registrierungsstatus:

   ```azurecli-interactive
   az account set -s <destination-subscription-name-or-id>
   az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
   ```

   Verwenden Sie zum Registrieren eines Ressourcenanbieters Folgendes:

   ```azurecli-interactive
   az provider register --namespace Microsoft.Batch
   ```

1. Das Konto, das die Ressourcen verschiebt, muss mindestens über folgende Berechtigungen verfügen:

   * **Microsoft.Resources/subscriptions/resourceGroups/moveResources/action** für die Quellressourcengruppe.
   * **Microsoft.Resources/subscriptions/resourceGroups/write** für die Zielressourcengruppe.

1. Überprüfen Sie vor dem Verschieben der Ressource die Abonnementkontingente für das Abonnement, zu dem Sie die Ressourcen verschieben. Wenn das Verschieben der Ressourcen bedeutet, dass das Abonnement seine Einschränkungen überschreitet, müssen Sie prüfen, ob Sie eine Erhöhung des Kontingents anfordern können. Eine vollständige Liste zu diesen Einschränkungen und Informationen zur Anforderung einer Kontingenterhöhung finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-resource-manager/management/azure-subscription-service-limits.md).

1. **Für eine abonnementübergreifende Verschiebung müssen die Ressource und die davon abhängigen Ressourcen in derselben Ressourcengruppe angeordnet sein und zusammen verschoben werden.** Für eine VM mit verwalteten Datenträgern müssen die VM und die verwalteten Datenträger beispielsweise gemeinsam und auch zusammen mit den anderen abhängigen Ressourcen verschoben werden.

   Wenn Sie eine Ressource in ein neues Abonnement verschieben, sollten Sie überprüfen, ob die Ressource über abhängige Ressourcen verfügt und ob diese sich in derselben Ressourcengruppe befinden. Wenn sich die Ressourcen nicht in derselben Ressourcengruppe befinden, sollten Sie überprüfen, ob die Ressourcen in derselben Ressourcengruppe zusammengefasst werden können. Wenn ja, müssen Sie alle Ressourcen in derselben Ressourcengruppe vereinen, indem Sie übergreifend für die Ressourcengruppen einen Verschiebungsvorgang durchführen.

   Weitere Informationen finden Sie unter [Szenario für eine abonnementübergreifende Verschiebung](#scenario-for-move-across-subscriptions).

## <a name="scenario-for-move-across-subscriptions"></a>Szenario für eine abonnementübergreifende Verschiebung

Das Verschieben von Ressourcen aus einem Abonnement in ein anderes ist ein dreistufiger Prozess:

![Szenario für abonnementübergreifende Verschiebung](./media/move-resource-group-and-subscription/cross-subscription-move-scenario.png)

Der Einfachheit halber verwenden wir hier nur eine abhängige Ressource.

* Schritt 1: Wenn abhängige Ressourcen auf unterschiedliche Ressourcengruppen verteilt sind, müssen Sie diese zuerst in einer Ressourcengruppe zusammenfassen.
* Schritt 2: Verschieben Sie die Ressource und die abhängigen Ressourcen zusammen aus dem Quellabonnement in das Zielabonnement.
* Schritt 3: Optional können Sie die abhängigen Ressourcen auf unterschiedliche Ressourcengruppen im Zielabonnement verteilen. 

## <a name="validate-move"></a>Überprüfen der Verschiebung

Der [Vorgang zum Überprüfen der Verschiebung](/rest/api/resources/resources/validatemoveresources) ermöglicht Ihnen, Ihr Verschiebungsszenario zu testen, ohne tatsächlich Ressourcen zu verschieben. Verwenden Sie diesen Vorgang, um zu überprüfen, ob die Verschiebung erfolgreich sein wird. Die Überprüfung wird automatisch aufgerufen, wenn Sie eine Verschiebeanforderung senden. Verwenden Sie diesen Vorgang nur, wenn Sie die Ergebnisse vorab bestimmen müssen. Um diesen Vorgang ausführen zu können, benötigen Sie Folgendes:

* Name der Quellressourcengruppe
* Ressourcen-ID der Zielressourcengruppe
* Ressourcen-ID der einzelnen zu verschiebenden Ressourcen
* [Zugriffstoken](/rest/api/azure/#acquire-an-access-token) für Ihr Konto

Senden Sie die folgende Anforderung:

```HTTP
POST https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<source-group>/validateMoveResources?api-version=2019-05-10
Authorization: Bearer <access-token>
Content-type: application/json
```

Mit dem Anforderungstext:

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Wenn die Anforderung ordnungsgemäß formatiert ist, gibt der Vorgang Folgendes zurück:

```HTTP
Response Code: 202
cache-control: no-cache
pragma: no-cache
expires: -1
location: https://management.azure.com/subscriptions/<subscription-id>/operationresults/<operation-id>?api-version=2018-02-01
retry-after: 15
...
```

Die Statuscode 202 gibt an, dass die Überprüfungsanforderung akzeptiert wurde, jedoch noch nicht ermittelt wurde, ob der Verschiebungsvorgang erfolgreich verläuft. Die Wert `location` enthält eine URL, über die Sie den Status des lang andauernden Vorgangs überprüfen können.  

Um den Status zu überprüfen, senden Sie die folgende Anforderung:

```HTTP
GET <location-url>
Authorization: Bearer <access-token>
```

Während der Vorgang ausgeführt wird, wird weiterhin der Statuscode 202 angezeigt. Warten Sie die Anzahl von Sekunden, die im Wert `retry-after` angegeben ist, bevor Sie es erneut versuchen. Wenn der Verschiebungsvorgang erfolgreich überprüft wurde, erhalten Sie den Statuscode 204. Wenn bei der Verschiebungsüberprüfung ein Fehler auftritt, erhalten Sie eine Fehlermeldung, z.B.:

```json
{"error":{"code":"ResourceMoveProviderValidationFailed","message":"<message>"...}}
```

## <a name="use-the-portal"></a>Verwenden des Portals

Um Ressourcen zu verschieben, wählen Sie die Ressourcengruppe mit den jeweiligen Ressourcen aus, und klicken Sie dann auf die Schaltfläche **Verschieben**.

![Verschieben von Ressourcen](./media/move-resource-group-and-subscription/select-move.png)

Wählen Sie aus, ob Sie die Ressource in eine neue Ressourcengruppe oder ein neues Abonnement verschieben.

Wählen Sie die zu verschiebenden Ressourcen und die Zielressourcengruppe aus. Bestätigen, dass Sie Skripts für diese Ressourcen aktualisieren müssen, und wählen Sie **OK**aus. Wenn Sie im vorherigen Schritt das Symbol „Abonnement bearbeiten“ ausgewählt haben, müssen Sie auch das Zielabonnement auswählen.

![Ziel auswählen](./media/move-resource-group-and-subscription/select-destination.png)

Unter **Benachrichtigungen**wird angezeigt, dass der Verschiebevorgang ausgeführt wird.

![Verschiebestatus anzeigen](./media/move-resource-group-and-subscription/show-status.png)

Sobald der Vorgang abgeschlossen ist, werden Sie über das Ergebnis informiert.

![Ergebnis des Verschiebens anzeigen](./media/move-resource-group-and-subscription/show-result.png)

Wenn ein Fehler auftritt, lesen Sie [Problembehandlung beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](troubleshoot-move.md).

## <a name="use-azure-powershell"></a>Mithilfe von Azure PowerShell

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [Move-AzResource](/powershell/module/az.resources/move-azresource). Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden.

```azurepowershell-interactive
$webapp = Get-AzResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Um Ressourcen in ein neues Abonnement zu verschieben, schließen Sie einen Wert für den Parameter `DestinationSubscriptionId` ein.

Wenn ein Fehler auftritt, lesen Sie [Problembehandlung beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](troubleshoot-move.md).

## <a name="use-azure-cli"></a>Mithilfe der Azure-Befehlszeilenschnittstelle

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl [az resource move](/cli/azure/resource?view=azure-cli-latest#az-resource-move). Geben Sie die Ressourcen-IDs der zu verschiebenden Ressourcen an. Im folgenden Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden. Geben Sie im `--ids`-Parameter eine durch Leerzeichen getrennte Liste der zu verschiebenden Ressourcen-IDs an.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Um Ressourcen in ein neues Abonnement zu verschieben, geben Sie den Parameter`--destination-subscription-id` an.

Wenn ein Fehler auftritt, lesen Sie [Problembehandlung beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](troubleshoot-move.md).

## <a name="use-rest-api"></a>REST-API

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Vorgang zum [Verschieben von Ressourcen](/rest/api/resources/Resources/MoveResources).

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

Geben Sie im Anforderungstext die Zielgruppe und die zu verschiebenden Ressourcen an.

```json
{
 "resources": ["<resource-id-1>", "<resource-id-2>"],
 "targetResourceGroup": "/subscriptions/<subscription-id>/resourceGroups/<target-group>"
}
```

Wenn ein Fehler auftritt, lesen Sie [Problembehandlung beim Verschieben von Azure-Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](troubleshoot-move.md).

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Frage: Mein Ressourcenverschiebevorgang, der normalerweise ein paar Minuten dauert, läuft schon seit fast einer Stunde. Stimmt da etwas nicht?**

Das Verschieben einer Ressource ist ein komplexer Vorgang, der in verschiedenen Phasen erfolgt. Es kann mehr als nur den Ressourcenanbieter der Ressource umfassen, die Sie verschieben möchten. Aufgrund der Abhängigkeiten zwischen Ressourcenanbietern stellt Azure Resource Manager vier Stunden bereit, um den Vorgang abzuschließen. Dieser Zeitraum gibt Ressourcenanbietern die Möglichkeit, sich von vorübergehenden Problemen wiederherzustellen. Wenn Ihre Verschiebeanforderung noch innerhalb des 4-Stunden-Zeitraums liegt, wird weiterhin versucht, den Vorgang abzuschließen, was auch noch erfolgreich sein kann. Die Quell- und Zielressourcengruppen sind während dieses Zeitraums gesperrt, um Konsistenzprobleme zu vermeiden.

**Frage: Warum ist meine Ressourcengruppe während der Ressourcenverschiebung vier Stunden lang gesperrt?**

Das 4-Stunden-Fenster ist die maximal zulässige Zeit für das Verschieben von Ressourcen. Um Änderungen an den Ressourcen zu verhindern, die gerade verschoben werden, werden sowohl die Quell- als auch die Zielressourcengruppe für die Dauer der Ressourcenverschiebung gesperrt.

Es gibt zwei Phasen bei einer Verschiebeanforderung. In der ersten Phase wird die Ressource verschoben. In der zweiten Phase werden Benachrichtigungen an andere Ressourcenanbieter gesendet, die von der Ressource abhängen, die verschoben wird. Eine Ressourcengruppe kann für das gesamte 4-Stunden-Zeitfenster gesperrt werden, wenn ein Ressourcenanbieter in einer der beiden Phasen fehlschlägt. Während des gewährten Zeitraums versucht Resource Manager, den fehlgeschlagenen Schritt zu wiederholen.

Wenn eine Ressource nicht innerhalb des Zeitfensters von 4 Stunden verschoben werden kann, entsperrt Resource Manager beide Ressourcengruppen. Ressourcen, die erfolgreich verschoben wurden, befinden sich in der Zielressourcengruppe. Ressourcen, die nicht verschoben werden konnten, verbleiben in der Quellressourcengruppe.

**Frage: Welche Auswirkungen hat es, dass die Quell- und Zielressourcengruppen während der Ressourcenverschiebung gesperrt werden?**

Die Sperrung verhindert, dass Sie eine Ressourcengruppe löschen, eine neue Ressource in einer der Ressourcengruppen erstellen oder eine der an der Verschiebung beteiligten Ressourcen löschen.

In der folgenden Abbildung wird eine Fehlermeldung des Azure-Portals angezeigt, wenn ein Benutzer versucht, eine Ressourcengruppe zu löschen, die Bestandteil einer laufenden Verschiebung ist.

![Verschiebefehlermeldung bei Löschversuch](./media/move-resource-group-and-subscription/move-error-delete.png)

**Frage: Was bedeutet der Fehlercode „MissingMoveDependentResources“?**

Beim Verschieben einer Ressource müssen deren abhängige Ressourcen in der Zielressourcengruppe oder im Zielabonnement vorhanden sein oder in die Verschiebeanforderung eingeschlossen werden. Sie erhalten den Fehlercode „MissingMoveDependentResources“, wenn eine abhängige Ressource diese Anforderung nicht erfüllt. Die Fehlermeldung enthält Details über die abhängige Ressource, die in die Verschiebeanforderung eingeschlossen werden muss.

Beispielsweise könnte das Verschieben eines virtuellen Computers erfordern, dass sieben Ressourcentypen mit drei verschiedenen Ressourcenanbietern verschoben werden. Diese Ressourcenanbieter und -typen sind:

* Microsoft.Compute
   * virtualMachines
   * disks
* Microsoft.Network
  * networkInterfaces
  * publicIPAddresses
  * networkSecurityGroups
  * virtualNetworks
* Microsoft.Storage
  * storageAccounts

Ein weiteres gängiges Beispiel beinhaltet das Verschieben eines virtuellen Netzwerks. Sie müssen möglicherweise mehrere weitere Ressourcen verschieben, die diesem virtuellen Netzwerk zugeordnet sind. Die Verschiebeanforderung kann es erfordern, dass öffentliche IP-Adressen, Routingtabellen, virtuelle Netzwerkgateways, Netzwerksicherheitsgruppen und anderes verschoben werden.

**Frage: Warum kann ich einige Ressourcen in Azure nicht verschieben?**

Derzeit unterstützen nicht alle Ressourcen in Azure das Verschieben. Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).

## <a name="next-steps"></a>Nächste Schritte

Eine Liste der Ressourcen, die das Verschieben unterstützen, finden Sie unter [Unterstützung des Verschiebevorgangs für Ressourcen](move-support-resources.md).
