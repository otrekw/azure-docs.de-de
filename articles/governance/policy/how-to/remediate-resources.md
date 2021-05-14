---
title: Korrigieren nicht konformer Ressourcen
description: Dieser Leitfaden führt Sie schrittweise durch den Korrekturprozess von Ressourcen, die mit Richtlinien in Azure Policy nicht konform sind.
ms.date: 05/01/2021
ms.topic: how-to
ms.openlocfilehash: 677e203b724f2046dfd7ba8d7a509965a36a8172
ms.sourcegitcommit: f6b76df4c22f1c605682418f3f2385131512508d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108326163"
---
# <a name="remediate-non-compliant-resources-with-azure-policy"></a>Korrigieren nicht konformer Ressourcen mit Azure Policy

Ressourcen, die mit der Richtlinie **deployIfNotExists** oder **modify** nicht konform sind, können über die **Wiederherstellung** in einen konformen Zustand versetzt werden. Die Wiederherstellung erfolgt durch die Anweisung an Azure Policy, den Effekt **deployIfNotExists** oder die **Änderungsvorgänge** der zugewiesenen Richtlinie auf Ihre vorhandenen Ressourcen und Abonnements anzuwenden. Dies gilt unabhängig davon, ob die Zuweisung an eine Verwaltungsgruppe, ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource erfolgt. Dieser Artikel zeigt die Schritte, die erforderlich sind, um die Korrektur mithilfe von Azure Policy zu verstehen und durchzuführen.

## <a name="how-remediation-security-works"></a>Sicherheit durch Wiederherstellung

Wenn Azure Policy die Vorlage in der Richtliniendefinition **deployIfNotExists** ausführt, wird hierfür eine [Verwaltete Identität](../../../active-directory/managed-identities-azure-resources/overview.md) verwendet.
Azure Policy erstellt für jede Ihrer Zuweisungen eine verwaltete Identität, muss jedoch wissen, welchen Rollen die verwaltete Identität gewährt werden soll. Wenn der verwalteten Identität Rollen fehlen, wird während der Zuweisung der Richtlinie oder in einer Initiative ein Fehler angezeigt. Bei Verwendung des Portals gewährt Azure Policy der verwalteten Identität automatisch die aufgelisteten Rollen, sobald die Zuweisung ausgelöst wurde. Wenn Sie das SDK verwenden, müssen die Rollen der verwalteten Identität manuell erteilt werden. Der _Speicherort_ der verwalteten Identität hat keinen Einfluss auf die Funktionsweise mit Azure Policy.

:::image type="content" source="../media/remediate-resources/missing-role.png" alt-text="Screenshot einer deployIfNotExists-Richtlinie, der eine definierte Berechtigung für die verwaltete Identität fehlt." border="false":::

> [!IMPORTANT]
> In den folgenden Szenarios muss der verwalteten Identität der Zuweisung [manuell der Zugriff erteilt werden](#manually-configure-the-managed-identity), da andernfalls ein Fehler bei der Wartungsbereitstellung auftritt.
>
> - Die Zuweisung wird über das SDK erstellt.
> - Eine von **deployIfNotExists** oder **modify** geänderte Ressource liegt außerhalb des Bereichs der Richtlinienzuweisung.
> - Die Vorlage greift auf Eigenschaften von Ressourcen außerhalb des Bereichs der Richtlinienzuweisung zu.

## <a name="configure-policy-definition"></a>Konfigurieren einer Richtliniendefinition

Im ersten Schritt werden die Rollen definiert, die **deployIfNotExists** und **modify** in der Richtliniendefinition benötigt, um die Inhalte Ihrer eingebundenen Vorlage bereitstellen zu können. Fügen Sie unter der Eigenschaft **Details** eine **roleDefinitionIds**-Eigenschaft hinzu. Diese Eigenschaft ist ein Array von Zeichenfolgen, die Rollen in Ihrer Umgebung entsprechen. Ein vollständiges Beispiel finden Sie bei den [Beispielen für „deployIfNotExists“](../concepts/effects.md#deployifnotexists-example) oder bei den [Beispielen für „modify“](../concepts/effects.md#modify-examples).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

Die Eigenschaft **roleDefinitionIds** verwendet die vollständige Ressourcen-ID und nicht den kurzen **roleName** der Rolle. Verwenden Sie den folgenden Code, um die ID für die Rolle „Mitwirkender“ in Ihrer Umgebung abzurufen:

```azurecli-interactive
az role definition list --name 'Contributor'
```

## <a name="manually-configure-the-managed-identity"></a>Manuelles Konfigurieren der verwalteten Identität

Bei der Erstellung einer Zuweisung über das Portal generiert Azure Policy die verwaltete Identität und weist ihr die in **roleDefinitionIds** definierten Rollen zu. In den folgenden Bedingungen müssen die Schritte zum Erstellen der verwalteten Identität und zum Zuweisen von Berechtigungen manuell erfolgen:

- Bei der Verwendung des SDK (z.B. Azure PowerShell)
- Wenn eine Ressource außerhalb des Zuweisungsbereichs von der Vorlage geändert wird
- Wenn eine Ressource außerhalb des Zuweisungsbereichs von der Vorlage gelesen wird

### <a name="create-managed-identity-with-powershell"></a>Erstellen einer verwalteten Identität mit PowerShell

Zum Erstellen einer verwalteten Identität während der Zuweisung der Richtlinie muss **Speicherort** definiert sein und **AssignIdentity** verwendet werden. Im folgenden Beispiel wird die Definition der integrierten Richtlinie **Transparente SQL DB-Datenbankverschlüsselung bereitstellen** abgerufen, die Zielressourcengruppe festgelegt und anschließend die Zuweisung erstellt.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Get the built-in "Deploy SQL DB transparent data encryption" policy definition
$policyDef = Get-AzPolicyDefinition -Id '/providers/Microsoft.Authorization/policyDefinitions/86a912f6-9a06-4e26-b447-11b16ba8659f'

# Get the reference to the resource group
$resourceGroup = Get-AzResourceGroup -Name 'MyResourceGroup'

# Create the assignment using the -Location and -AssignIdentity properties
$assignment = New-AzPolicyAssignment -Name 'sqlDbTDE' -DisplayName 'Deploy SQL DB transparent data encryption' -Scope $resourceGroup.ResourceId -PolicyDefinition $policyDef -Location 'westus' -AssignIdentity
```

Die Variable `$assignment` enthält nun die Prinzipal-ID der verwalteten Identität und die Standardwerte, die bei der Erstellung einer Richtlinienzuweisung zurückgegeben werden. Ein Zugriff darauf ist über `$assignment.Identity.PrincipalId` möglich.

### <a name="grant-defined-roles-with-powershell"></a>Zuweisen definierter Rollen mit PowerShell

Die neue verwaltete Identität muss die Replikation über Azure Active Directory ausführen, bevor ihr die erforderlichen Rollen zugewiesen werden können. Nach Abschluss der Replikation wird im folgenden Beispiel die Richtliniendefinition in `$policyDef` für die **roleDefinitionIds** wiederholt und [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) zum Zuweisen der Rollen zur neuen verwalteten Identität verwendet.

```azurepowershell-interactive
# Use the $policyDef to get to the roleDefinitionIds array
$roleDefinitionIds = $policyDef.Properties.policyRule.then.details.roleDefinitionIds

if ($roleDefinitionIds.Count -gt 0)
{
    $roleDefinitionIds | ForEach-Object {
        $roleDefId = $_.Split("/") | Select-Object -Last 1
        New-AzRoleAssignment -Scope $resourceGroup.ResourceId -ObjectId $assignment.Identity.PrincipalId -RoleDefinitionId $roleDefId
    }
}
```

### <a name="grant-defined-roles-through-portal"></a>Zuweisen definierter Rollen über das Portal

Es gibt zwei Möglichkeiten, der verwalteten Identität einer Zuweisung die definierten Rollen über das Portal zuzuweisen: Durch die Verwendung der **Zugriffssteuerung (IAM)** oder durch die Bearbeitung der Richtlinie oder der Initiativzuweisung und dem anschließenden Auswählen von **Speichern**.

Führen Sie die folgenden Schritte aus, um eine Rolle zu der verwalteten Identität der Zuweisung hinzuzufügen:

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie die Option **Alle Dienste** auswählen und dann nach **Policy** suchen und die entsprechende Option auswählen.

1. Wählen Sie links auf der Seite „Azure Policy“ die Option **Zuweisungen**.

1. Suchen Sie die Zuweisung, die über eine verwaltete Identität verfügt, und wählen Sie den Namen aus.

1. Suchen Sie auf der Bearbeitungsseite nach der Eigenschaft **Zuweisungs-ID**. Die Zuweisungs-ID sieht wie folgt aus:

   ```output
   /subscriptions/{subscriptionId}/resourceGroups/PolicyTarget/providers/Microsoft.Authorization/policyAssignments/2802056bfc094dfb95d4d7a5
   ```

   Der letzte Teil der Ressourcen-ID der Zuweisung enthält den Namen der verwalteten Identität, in diesem Beispiel `2802056bfc094dfb95d4d7a5`. Kopieren Sie diesen Teil der Ressourcen-ID der Zuweisung.

1. Navigieren Sie zu der Ressource des übergeordneten Containers der Ressource (Ressourcengruppe, Abonnement, Verwaltungsgruppe). Dort muss die Rollendefinition manuell hinzugefügt werden.

1. Wählen Sie auf der Seite „Ressourcen“ den Link **Zugriffssteuerung (IAM)** und anschließend oben auf der Seite „Zugriffssteuerung“ die Option **+ Rollenzuweisung hinzufügen** aus.

1. Wählen Sie die entsprechende Rolle, die **roleDefinitionIds** aus der Richtliniendefinition entspricht.
   Lassen Sie für **Zugriff zuweisen zu** den Standardwert „Azure AD-Benutzer, -Gruppe oder -Anwendung“ festgelegt. Fügen Sie im Feld **Auswählen** den zuvor lokalisierten Teil der Ressourcen-ID der Zuweisung ein oder geben Sie diesen ein. Wählen Sie nach Abschluss der Suche das Objekt mit dem gleichen Namen aus, um die ID auszuwählen, und anschließend wählen Sie **Speichern** aus.

## <a name="create-a-remediation-task"></a>Erstellen eines Wartungstask

### <a name="create-a-remediation-task-through-portal"></a>Erstellen eines Wartungstasks über das Portal

Während der Auswertung wird durch die Richtlinienzuweisung mit den Effekten **deployIfNotExists** oder **modify** bestimmt, ob nicht konforme Ressourcen oder Abonnements vorhanden sind. Falls nicht konforme Ressourcen oder Abonnements gefunden werden, finden Sie auf der Seite **Wartung** weitere Einzelheiten. Neben der Liste der Richtlinien mit nicht konformen Ressourcen und Abonnements befindet sich die Option zum Auslösen eines **Wartungstasks**.
Dadurch wird eine Bereitstellung mithilfe der Vorlage **deployIfNotExists** oder den **modify**-Vorgängen erstellt.

Führen Sie die folgenden Schritte aus, um einen **Wartungstask** zu erstellen:

1. Starten Sie den Azure Policy-Dienst über das Azure-Portal, indem Sie die Option **Alle Dienste** auswählen und dann nach **Policy** suchen und die entsprechende Option auswählen.

   :::image type="content" source="../media/remediate-resources/search-policy.png" alt-text="Screenshot: Suchen nach „Policy“ unter „Alle Dienste“" border="false":::

1. Wählen Sie links auf der Seite „Azure Policy“ die Option **Wartung** aus.

   :::image type="content" source="../media/remediate-resources/select-remediation.png" alt-text="Screenshot: Knoten „Wartung“ auf der Seite „Policy“." border="false":::

1. Auf der Registerkarte **Policies to remediate** (Zu wartende Richtlinien) und in der Datentabelle sind sämtliche **deployIfNotExists**- und **modify**-Richtlinienzuweisungen mit nicht konformen Ressourcen enthalten. Wählen Sie eine Richtlinie mit Ressourcen aus, die nicht konform sind. Die Seite **Neuer Wiederherstellungstask** wird geöffnet.

   > [!NOTE]
   > Eine alternative Möglichkeit zum Öffnen der Seite **Wiederherstellungstask** besteht darin, über die Seite **Konformität** nach der Richtlinie zu suchen, diese und anschließend die Schaltfläche **Wartungstask erstellen** auszuwählen.

1. Filtern Sie auf der Seite **Neuer Wartungstask** die zu korrigierenden Ressourcen, indem Sie die über die Auslassungspunkte bei **Bereich** untergeordnete Ressourcen auswählen, von denen aus die Richtlinie zugewiesen wurde (bis hin zu den einzelnen Ressourcenobjekten). Darüber hinaus können Sie die Ressourcen über das Dropdownfeld **Standorte** weiter filtern. Nur in der Tabelle aufgeführte Ressourcen werden gewartet.

   :::image type="content" source="../media/remediate-resources/select-resources.png" alt-text="Screenshot: Knoten „Wartung“ und das Raster mit Ressourcen für die Wartung" border="false":::

1. Sobald die Ressourcen gefiltert wurden, können Sie den Wartungstask auslösen, indem Sie **Korrigieren** auswählen. Auf der Registerkarte **Wartungstasks** wird die Seite zur Richtlinienkonformität geöffnet, auf der der Fortschritt der Tasks angezeigt wird. Durch dem Wartungstask erstellte Bereitstellungen beginnen sofort.

   :::image type="content" source="../media/remediate-resources/task-progress.png" alt-text="Screenshot: Registerkarte „Wartungstasks“ und Fortschritt der vorhandenen Wartungstasks" border="false":::

1. Wählen Sie auf der Seite „Richtlinienkonformität“ die Option **Wartungstask** aus, um Einzelheiten zum Fortschritt abzurufen. Die für den Task verwendete Filterung wird zusammen mit einer Liste der Ressourcen angezeigt, die gerade korrigiert werden.

1. Klicken Sie auf der Seite **Wartungstasks** mit der rechten Maustaste auf eine Ressource, um die Bereitstellung des Wartungstasks oder die Ressource anzuzeigen. Wählen Sie am Ende der Zeile **Verknüpfte Ereignisse** aus, um Einzelheiten anzuzeigen, z. B. eine Fehlermeldung.

   :::image type="content" source="../media/remediate-resources/resource-task-context-menu.png" alt-text="Screenshot: Kontextmenü für eine Ressource auf der Registerkarte „Wartungstask“" border="false":::

Ressourcen, die über einen **Wartungstask** bereitgestellt werden, werden auf der Seite „Richtlinienkonformität“ zur Registerkarte **Bereitgestellte Ressourcen** hinzugefügt.

### <a name="create-a-remediation-task-through-azure-cli"></a>Erstellen eines Wartungstasks über die Azure CLI

Verwenden Sie zum Erstellen eines **Wartungstasks** mit der Azure-Befehlszeilenschnittstelle die `az policy remediation`-Befehle. Ersetzen Sie `{subscriptionId}` durch Ihre Abonnement-ID und `{myAssignmentId}` durch Ihre **deployIfNotExists**- oder **modify**-Richtlinienzuweisungs-ID.

```azurecli-interactive
# Login first with az login if not using Cloud Shell

# Create a remediation for a specific assignment
az policy remediation create --name myRemediation --policy-assignment '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Informationen zu anderen Wartungsbefehlen und Beispiele finden Sie im Artikel zu den [az policy remediation](/cli/azure/policy/remediation)-Befehlen.

### <a name="create-a-remediation-task-through-azure-powershell"></a>Erstellen eines Wartungstasks über Azure PowerShell

Verwenden Sie zum Erstellen eines **Wartungstasks** mit Azure PowerShell die `Start-AzPolicyRemediation`-Befehle. Ersetzen Sie `{subscriptionId}` durch Ihre Abonnement-ID und `{myAssignmentId}` durch Ihre **deployIfNotExists**- oder **modify**-Richtlinienzuweisungs-ID.

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell

# Create a remediation for a specific assignment
Start-AzPolicyRemediation -Name 'myRemedation' -PolicyAssignmentId '/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/{myAssignmentId}'
```

Andere Cmdlets zur Wartung und Beispiele finden Sie im [Az.PolicyInsights](/powershell/module/az.policyinsights/#policy_insights)-Modul.

### <a name="create-a-remediation-task-during-policy-assignment-in-the-azure-portal"></a>Erstellen eines Wartungstasks während der Richtlinienzuweisung im Azure-Portal

Eine optimierte Möglichkeit zum Erstellen eines Wartungstasks besteht während der Richtlinienzuweisung über das Azure-Portal. Wenn die zuzuweisende Richtliniendefinition ein **deployIfNotExists**- oder **modify**-Effekt ist, enthält der Assistent auf der Registerkarte **Wartung** eine Option _Wartungstask erstellen_. Bei Auswahl dieser Option wird ein Wartungstask gleichzeitig mit der Richtlinienzuweisung erstellt.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Beispiele unter [Azure Policy-Beispiele](../samples/index.md) an.
- Lesen Sie die Informationen unter [Struktur von Azure Policy-Definitionen](../concepts/definition-structure.md).
- Lesen Sie [Grundlegendes zu Richtlinienauswirkungen](../concepts/effects.md).
- Informieren Sie sich über das [programmgesteuerte Erstellen von Richtlinien](programmatically-create.md).
- Informieren Sie sich über das [Abrufen von Konformitätsdaten](get-compliance-data.md).
- Weitere Informationen zu Verwaltungsgruppen finden Sie unter [Organisieren Ihrer Ressourcen mit Azure-Verwaltungsgruppen](../../management-groups/overview.md).
