---
title: Verwenden von Bereitstellungsskripts in Vorlagen | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Bereitstellungsskripts in Azure Resource Manager-Vorlagen verwenden.
services: azure-resource-manager
author: mumian
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/20/2020
ms.author: jgao
ms.openlocfilehash: 24a0891b57f67bfb78cf3699bddbcf8d345ee679
ms.sourcegitcommit: a3c6efa4d4a48e9b07ecc3f52a552078d39e5732
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2020
ms.locfileid: "83708005"
---
# <a name="use-deployment-scripts-in-templates-preview"></a>Verwenden von Bereitstellungsskripts in Vorlagen (Vorschauversion)

Erfahren Sie, wie Sie Bereitstellungsskripts in Azure Resource Manager-Vorlagen verwenden. Bei einem neuen Ressourcentyp mit dem Namen `Microsoft.Resources/deploymentScripts` können Benutzer Bereitstellungsskripts in Vorlagenbereitstellungen ausführen und die Ausführungsergebnisse überprüfen. Diese Skripts können zum Ausführen benutzerdefinierter Schritte wie der folgenden verwendet werden:

- Hinzufügen von Benutzern zu einem Verzeichnis
- Ausführen von Vorgängen auf Datenebene, z. B. Kopieren von Blobs oder Ausführen von Datenbankseeding
- Suchen und Validieren eines Lizenzschlüssels
- Erstellen eines selbstsignierten Zertifikats
- Erstellen eines Objekts in Azure AD
- Suchen von IP-Adressblöcken auf einem benutzerdefinierten System

Vorteile von Bereitstellungsskripts:

- Einfach zu codieren, zu verwenden und zu debuggen. Sie können Bereitstellungsskripts in Ihren bevorzugten Entwicklungsumgebungen entwickeln. Die Skripts können in Vorlagen oder in externe Skriptdateien eingebettet werden.
- Sie können die Skriptsprache und die Plattform angeben. Derzeit werden Azure PowerShell- und Azure CLI-Bereitstellungsskripts in der Linux-Umgebung unterstützt.
- Sie können die Identitäten angeben, die zum Ausführen der Skripts verwendet werden. Zurzeit wird nur eine [benutzerseitig zugewiesene verwaltete Azure-Identität](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) unterstützt.
- Sie können Befehlszeilenargumente an das Skript übergeben.
- Sie können Skriptausgaben angeben und an die Bereitstellung zurückgeben.

Die Bereitstellungsskriptressource ist nur in den Regionen verfügbar, in denen Azure Container Instances verfügbar ist.  Informationen dazu finden Sie unter [Ressourcenverfügbarkeit für Azure Container Instances in Azure-Regionen](../../container-instances/container-instances-region-availability.md).

> [!IMPORTANT]
> Für die Skriptausführung und Problembehandlung werden ein Speicherkonto und eine Containerinstanz benötigt. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto anzugeben. Andernfalls wird das Speicherkonto zusammen mit der Containerinstanz vom Skriptdienst automatisch erstellt. Die beiden automatisch erstellten Ressourcen werden normalerweise vom Skriptdienst gelöscht, wenn die Ausführung des Bereitstellungsskripts beendet ist. Die Ressourcen werden Ihnen in Rechnung gestellt, bis sie gelöscht werden. Weitere Informationen finden Sie unter [Bereinigen von Bereitstellungsskriptressourcen](#clean-up-deployment-script-resources).

## <a name="prerequisites"></a>Voraussetzungen

- **Benutzerseitig zugewiesene verwaltete Identität mit der Rolle „Mitwirkender“ für die Zielressourcengruppe.** . Diese Identität wird zum Ausführen von Bereitstellungsskripts verwendet. Zum Ausführen von Vorgängen außerhalb der Ressourcengruppe müssen Sie zusätzliche Berechtigungen erteilen. Weisen Sie die Identität beispielsweise der Abonnementebene zu, wenn Sie eine neue Ressourcengruppe erstellen möchten.

  > [!NOTE]
  > Der Skriptdienst erstellt im Hintergrund ein Speicherkonto (es sei denn, Sie geben ein vorhandenes Speicherkonto an) und eine Containerinstanz.  Eine benutzerseitig zugewiesene verwaltete Identität mit der Rolle „Mitwirkender“ auf Abonnementebene ist erforderlich, wenn das Abonnement nicht die Ressourcenanbieter Azure-Speicherkonto (Microsoft.Storage) und Azure-Containerinstanz (Microsoft.ContainerInstance) registriert hat.

  Informationen zum Erstellen einer Identität finden Sie unter [Erstellen einer benutzerseitig zugewiesenen verwalteten Identität mithilfe des Azure-Portals](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md), [... mithilfe der Azure-Befehlszeilenschnittstelle](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) und [... mithilfe von Azure PowerShell](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md). Sie benötigen die Identitäts-ID beim Bereitstellen der Vorlage. Das Format der Identität lautet:

  ```json
  /subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<IdentityID>
  ```

  Verwenden Sie das folgende CLI- oder PowerShell-Skript, um die ID abzurufen, indem Sie den Namen der Ressourcengruppe und den Identitätsnamen angeben.

  # <a name="cli"></a>[BEFEHLSZEILENSCHNITTSTELLE (CLI)](#tab/CLI)

  ```azurecli-interactive
  echo "Enter the Resource Group name:" &&
  read resourceGroupName &&
  echo "Enter the managed identity name:" &&
  read idName &&
  az identity show -g jgaoidentity1008rg -n jgaouami --query id
  ```

  # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

  ```azurepowershell-interactive
  $idGroup = Read-Host -Prompt "Enter the resource group name for the managed identity"
  $idName = Read-Host -Prompt "Enter the name of the managed identity"

  (Get-AzUserAssignedIdentity -resourcegroupname $idGroup -Name $idName).Id
  ```

  ---

- **Azure PowerShell** oder **Azure CLI**. Eine Liste der unterstützten Azure PowerShell-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list); eine Liste der unterstützten Azure CLI-Versionen finden Sie [hier](https://mcr.microsoft.com/v2/azuredeploymentscripts-powershell/tags/list).

    >[!IMPORTANT]
    > Das Bereitstellungsskript verwendet die verfügbaren CLI-Images von Microsoft Container Registry (MCR). Das Zertifizieren eines CLI-Images für das Bereitstellungsskript dauert ungefähr einen Monat. Verwenden Sie nicht die CLI-Versionen, die innerhalb von 30 Tagen veröffentlicht wurden. Die Veröffentlichungsdaten für die Images finden Sie unter [Versionshinweise für die Azure CLI](https://docs.microsoft.com/cli/azure/release-notes-azure-cli?view=azure-cli-latest). Wenn eine nicht unterstützte Version verwendet wird, werden in der Fehlermeldung die unterstützten Versionen aufgelistet.

    Sie benötigen diese Versionen nicht für Bereitstellungsvorlagen. Für das lokale Testen von Bereitstellungsskripts sind diese Versionen jedoch erforderlich. Siehe [Installieren des Azure PowerShell-Moduls](/powershell/azure/install-az-ps). Sie können ein vorkonfiguriertes Docker-Image verwenden.  Weitere Informationen finden Sie unter [Konfigurieren der Entwicklungsumgebung](#configure-development-environment).

## <a name="sample-templates"></a>Beispielvorlagen

Nachfolgend finden Sie ein JSON-Beispiel.  Das neueste Vorlagenschema finden Sie [hier](/azure/templates/microsoft.resources/deploymentscripts).

```json
{
  "type": "Microsoft.Resources/deploymentScripts",
  "apiVersion": "2019-10-01-preview",
  "name": "runPowerShellInline",
  "location": "[resourceGroup().location]",
  "kind": "AzurePowerShell", // or "AzureCLI"
  "identity": {
    "type": "userAssigned",
    "userAssignedIdentities": {
      "/subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourceGroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myID": {}
    }
  },
  "properties": {
    "forceUpdateTag": 1,
    "containerSettings": {
      "containerGroupName": "mycustomaci"
    },
    "storageAccountSettings": {
      "storageAccountName": "myStorageAccount",
      "storageAccountKey": "myKey"
    },
    "azPowerShellVersion": "3.0",  // or "azCliVersion": "2.0.80"
    "arguments": "[concat('-name ', parameters('name'))]",
    "environmentVariables": [
      {
        "name": "someSecret",
        "secureValue": "if this is really a secret, don't put it here... in plain text..."
      }
    ],
    "scriptContent": "
      param([string] $name)
      $output = 'Hello {0}' -f $name
      Write-Output $output
      $DeploymentScriptOutputs = @{}
      $DeploymentScriptOutputs['text'] = $output
    ", // or "primaryScriptUri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
    "supportingScriptUris":[],
    "timeout": "PT30M",
    "cleanupPreference": "OnSuccess",
    "retentionInterval": "P1D"
  }
}
```

> [!NOTE]
> Dieses Beispiel dient zu Demonstrationszwecken.  **scriptContent** und **primaryScriptUris** dürfen nicht zusammen in einer Vorlage vorkommen.

Details zu Eigenschaftswerten:

- **Identität**: Der Bereitstellungsskriptdienst verwendet eine benutzerseitig zugewiesene verwaltete Identität, um die Skripts auszuführen. Zurzeit wird nur eine benutzerseitig zugewiesene verwaltete Identität unterstützt.
- **kind:** Geben Sie den Typ des Skripts an. Zurzeit werden Azure PowerShell- und Azure CLI-Skripts unterstützt. Die Werte sind **AzurePowerShell** und **AzureCLI**.
- **forceUpdateTag**: Wenn Sie diesen Wert zwischen Vorlagenbereitstellungen ändern, wird das Bereitstellungsskript erneut ausgeführt. Verwenden Sie eine der Funktionen newGuid() oder utcNow(), die als defaultValue eines Parameters festgelegt werden muss. Weitere Informationen finden Sie unter [Mehrmaliges Ausführen des Skripts](#run-script-more-than-once).
- **containerSettings**: Geben Sie die Einstellungen zum Anpassen der Azure-Containerinstanz an.  **containerGroupName** dient zum Angeben des Namens der Containergruppe.  Falls nicht angegeben, wird der Gruppenname automatisch generiert.
- **storageAccountSettings**: Geben Sie die Einstellungen zur Verwendung eines vorhandenen Speicherkontos an. Falls nicht angegeben, wird ein Speicherkonto automatisch erstellt. Weitere Informationen finden Sie unter [Verwenden eines vorhandenen Speicherkontos](#use-an-existing-storage-account).
- **azPowerShellVersion**/**azCliVersion**: Geben Sie die zu verwendende Modulversion an. Eine Liste der unterstützten PowerShell- und CLI-Versionen finden Sie unter [Voraussetzungen](#prerequisites).
- **arguments:** Geben Sie die Parameterwerte an. Die Werte werden durch Leerzeichen voneinander getrennt.
- **environmentVariables**: Geben Sie die Umgebungsvariablen an, die an das Skript übergeben werden sollen. Weitere Informationen finden Sie unter [Entwickeln von Bereitstellungsskripts](#develop-deployment-scripts).
- **scriptContent**: Geben Sie den Skriptinhalt an. Wenn Sie ein externes Skript ausführen möchten, verwenden Sie stattdessen `primaryScriptUri`. Beispiele finden Sie unter [Verwenden von Inlineskripts](#use-inline-scripts) und [Verwenden externer Skripts](#use-external-scripts).
- **primaryScriptUri**: Geben Sie eine öffentlich zugängliche URL zum primären Bereitstellungsskript mit unterstützten Dateierweiterungen an.
- **supportingScriptUris**: Geben Sie ein Array öffentlich zugänglicher URLs zu unterstützenden Dateien an, die in `ScriptContent` oder `PrimaryScriptUri` aufgerufen werden.
- **timeout:** Geben Sie die maximal zulässige Ausführungsdauer für das Skript im [ISO 8601-Format](https://en.wikipedia.org/wiki/ISO_8601) an. Der Standardwert ist **P1D**.
- **cleanupPreference**. Geben Sie die Einstellung für das Bereinigen der Bereitstellungsressourcen an, nachdem die Skriptausführung beendet wurde. Die Standardeinstellung ist **Always**. Damit werden die Ressourcen unabhängig vom Endzustand (Erfolg, Fehler, Abbruch) gelöscht. Weitere Informationen finden Sie unter [Bereinigen von Bereitstellungsskriptressourcen](#clean-up-deployment-script-resources).
- **retentionInterval**: Geben Sie das Intervall an, das vom Dienst für die Aufbewahrung der Bereitstellungsskriptressourcen verwendet wird, nachdem das Bereitstellungsskript einen Beendigungszustand erreicht. Die Bereitstellungsskriptressourcen werden gelöscht, wenn dieser Zeitraum abgelaufen ist. Die Dauer basiert auf dem [ISO 8601-Muster](https://en.wikipedia.org/wiki/ISO_8601). Der Standardwert ist **P1D** und bedeutet sieben Tage. Diese Eigenschaft wird verwendet, wenn cleanupPreference auf *OnExpiration* festgelegt ist. Die *OnExpiration*-Eigenschaft ist derzeit nicht aktiviert. Weitere Informationen finden Sie unter [Bereinigen von Bereitstellungsskriptressourcen](#clean-up-deployment-script-resources).

### <a name="additional-samples"></a>Weitere Beispiele

- [Erstellen und Zuweisen eines Zertifikats zu einem Schlüsseltresor](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault.json)

- [Erstellen und Zuweisen einer benutzerseitig zugewiesenen verwalteten Identität zu einer Ressourcengruppe und Ausführen eines Bereitstellungsskripts](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-keyvault-mi.json).

> [!NOTE]
> Es wird empfohlen, eine benutzerseitig zugewiesene Identität zu erstellen und im Voraus Berechtigungen zu gewähren. Möglicherweise erhalten Sie anmeldungs- und berechtigungsbezogene Fehler, wenn Sie in derselben Vorlage, in der Sie Bereitstellungsskripts ausführen, die Identität erstellen und Berechtigungen gewähren. Es dauert einige Zeit, bis die Berechtigungen wirksam werden.

## <a name="use-inline-scripts"></a>Verwenden von Inlineskripts

Für die folgende Vorlage wurde eine Ressource mit dem Typ `Microsoft.Resources/deploymentScripts` definiert. Der hervorgehobene Teil ist das Inlineskript.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-helloworld.json" range="1-54" highlight="34-40":::

> [!NOTE]
> Da die Inlinebereitstellungsskripts in doppelte Anführungszeichen eingeschlossen sind, müssen Zeichenfolgen innerhalb der Bereitstellungsskripts stattdessen in einfache Anführungszeichen eingeschlossen werden. Das Escapezeichen für PowerShell ist **&#92;** . Sie können auch, wie im vorherigen JSON-Beispiel gezeigt, eine Zeichenfolgenersetzung in Erwägung ziehen. Sehen Sie sich den Standardwert des Parameters an.

Das Skript akzeptiert einen Parameter und gibt den Parameterwert aus. **DeploymentScriptOutputs** wird zum Speichern von Ausgaben verwendet.  Im Abschnitt „outputs“ zeigt die Zeile **value** an, wie auf die gespeicherten Werte zugegriffen wird. `Write-Output` wird zum Debuggen verwendet. Informationen zum Zugreifen auf die Ausgabedatei finden Sie unter [Debuggen von Bereitstellungsskripts](#debug-deployment-scripts).  Beschreibungen der Eigenschaften finden Sie unter [Beispielvorlagen](#sample-templates).

Wählen Sie zum Ausführen des Skripts **Jetzt testen** aus, um die Cloud Shell zu öffnen, und fügen Sie anschließend den folgenden Code in den Shellbereich ein.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the name of the resource group to be created"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$id = Read-Host -Prompt "Enter the user-assigned managed identity ID"

New-AzResourceGroup -Name $resourceGroupName -Location $location

New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.json" -identity $id

Write-Host "Press [ENTER] to continue ..."
```

Die Ausgabe sieht wie folgt aus:

![Resource Manager-Vorlage: Ausgabe des Hallo-Welt-Bereitstellungsskripts](./media/deployment-script-template/resource-manager-template-deployment-script-helloworld-output.png)

## <a name="use-external-scripts"></a>Verwenden externer Skripts

Neben Inlineskripts können Sie auch externe Skriptdateien verwenden. Es werden nur primäre PowerShell-Skripts mit der Dateierweiterung **ps1** unterstützt. Bei CLI-Skripts können die primären Skripts beliebige (oder gar keine) Erweiterungen besitzen, solange es sich bei den Skripts um gültige Bash-Skripts handelt. Um externe Skriptdateien zu verwenden, ersetzen Sie `scriptContent` durch `primaryScriptUri`. Beispiel:

```json
"primaryScriptURI": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/deploymentscript-helloworld.ps1",
```

Ein Beispiel finden Sie [hier](https://github.com/Azure/azure-docs-json-samples/blob/master/deployment-script/deploymentscript-helloworld-primaryscripturi.json).

Die externen Skriptdateien müssen zugänglich sein.  Informationen zum Schützen Ihrer Skriptdateien, die in Azure Storage-Konten gespeichert sind, finden Sie unter [Bereitstellen einer privaten ARM-Vorlage mit SAS-Token](./secure-template-with-sas-token.md).

Es ist Ihre Aufgabe, die Integrität der Skripts zu gewährleisten, auf die vom Bereitstellungsskript verwiesen wird, entweder **PrimaryScriptUri** oder **SupportingScriptUris**.  Verweisen Sie nur auf Skripts, denen Sie vertrauen.

## <a name="use-supporting-scripts"></a>Verwenden unterstützender Skripts

Sie können komplizierte Logik in unterstützende Skriptdateien aufteilen. Die `supportingScriptURI`-Eigenschaft ermöglicht Ihnen, bei Bedarf ein Array von URIs für die unterstützenden Skriptdateien bereitzustellen:

```json
"scriptContent": "
    ...
    ./Create-Cert.ps1
    ...
"

"supportingScriptUris": [
  "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/deployment-script/create-cert.ps1"
],
```

Unterstützende Skriptdateien können sowohl aus Inlineskripts als auch aus primären Skriptdateien aufgerufen werden. Unterstützende Skriptdateien unterliegen keinen Einschränkungen in Bezug auf die Dateierweiterung.

Die unterstützenden Dateien werden zur Laufzeit in „azscripts/azscriptinput“ kopiert. Verwenden Sie den relativen Pfad, um aus Inlineskripts und primären Skriptdateien auf die unterstützenden Dateien zu verweisen.

## <a name="work-with-outputs-from-powershell-script"></a>Arbeiten mit Ausgaben von PowerShell-Skripts

Die folgende Vorlage zeigt, wie Werte zwischen zwei deploymentScripts-Ressourcen übergeben werden:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic.json" range="1-84" highlight="39-40,66":::

In der ersten Ressource definieren Sie eine Variable mit dem Namen **$DeploymentScriptOutputs**, die Sie zum Speichern der Ausgabewerte verwenden. Um aus einer anderen Ressource in der Vorlage auf den Ausgabewert zuzugreifen, verwenden Sie Folgendes:

```json
reference('<ResourceName>').output.text
```

## <a name="work-with-outputs-from-cli-script"></a>Arbeiten mit Ausgaben von CLI-Skripts

Anders als das PowerShell-Bereitstellungsskript macht die CLI-/Bash-Unterstützung keine allgemeine Variable zum Speichern von Skriptausgaben verfügbar. Stattdessen gibt es eine Umgebungsvariable namens **AZ_SCRIPTS_OUTPUT_PATH**, in der der Speicherort der Skriptausgabedatei gespeichert wird. Wenn ein Bereitstellungsskript aus einer Resource Manager-Vorlage ausgeführt wird, wird diese Umgebungsvariable von der Bash-Shell automatisch für Sie festgelegt.

Bereitstellungsskriptausgaben müssen am AZ_SCRIPTS_OUTPUT_PATH-Speicherort gespeichert werden, und bei den Ausgaben muss es sich um ein gültiges JSON-Zeichenfolgenobjekt handeln. Der Inhalt der Datei muss als Schlüssel-Wert-Paar gespeichert werden. Beispielsweise wird ein Array von Zeichenfolgen als { "MyResult": [ "foo", "bar"] } gespeichert.  Das ausschließliche Speichern der Arrayergebnisse, z. B. [ "foo", "bar" ], ist ungültig.

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-basic-cli.json" range="1-44" highlight="32":::

Im vorherigen Beispiel wird [jq](https://stedolan.github.io/jq/) verwendet. Dies ist in den Containerimages enthalten. Weitere Informationen finden Sie unter [Konfigurieren der Entwicklungsumgebung](#configure-development-environment).

## <a name="develop-deployment-scripts"></a>Entwickeln von Bereitstellungsskripts

### <a name="handle-non-terminating-errors"></a>Behandeln von Fehlern ohne Abbruch

Sie können steuern, wie PowerShell auf Fehler ohne Abbruch reagiert, indem Sie die Variable [ **$ErrorActionPreference**](/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7#erroractionpreference
) in Ihrem Bereitstellungsskript verwenden. Der Wert wird vom Skriptdienst nicht festgelegt bzw. geändert.  Trotz des Werts, den Sie für „$ErrorActionPreference“ festgelegt haben, legt das Bereitstellungsskript den Bereitstellungsstatus der Ressource auf *Fehler* fest, wenn bei dem Skript ein Fehler auftritt.

### <a name="pass-secured-strings-to-deployment-script"></a>Übergeben von sicheren Zeichenfolgen an Bereitstellungsskripts

Das Festlegen von Umgebungsvariablen (EnvironmentVariable) in Ihren Containerinstanzen ermöglicht es Ihnen, eine dynamische Konfiguration der Anwendung oder des Skripts bereitzustellen, die bzw. das vom Container ausgeführt wird. Das Bereitstellungsskript verarbeitet nicht gesicherte und gesicherte Umgebungsvariablen auf dieselbe Weise wie Azure Container Instance. Weitere Informationen finden Sie unter [Festlegen von Umgebungsvariablen in Container Instances](../../container-instances/container-instances-environment-variables.md#secure-values).

Die maximal zulässige Größe für Umgebungsvariablen beträgt 64 KB.

## <a name="debug-deployment-scripts"></a>Debuggen von Bereitstellungsskripts

Der Skriptdienst erstellt für die Ausführung von Skripts im Hintergrund ein [Speicherkonto](../../storage/common/storage-account-overview.md) (es sei denn, Sie geben ein vorhandenes Speicherkonto an) und eine [Containerinstanz](../../container-instances/container-instances-overview.md). Wenn diese Ressourcen automatisch vom Skriptdienst erstellt werden, haben die Namen der Ressourcen das Suffix **azscripts**.

![Resource Manager-Vorlage: Namen von Bereitstellungsskriptressourcen](./media/deployment-script-template/resource-manager-template-deployment-script-resources.png)

Das Benutzerskript, die Ausführungsergebnisse und die stdout-Datei werden in den Dateifreigaben des Speicherkontos gespeichert. Es gibt einen Ordner namens **azscripts**. In diesem Ordner sind zwei weitere Ordner für die Eingabe- und Ausgabedateien enthalten: **azscriptinput** und **azscriptoutput**.

Der Ausgabeordner enthält die Datei **executionresult.json** und die Skriptausgabedatei. Sie können die Fehlermeldung der Skriptausführung in **executionresult.json** anzeigen. Die Ausgabedatei wird nur erstellt, wenn das Skript erfolgreich ausgeführt wurde. Der Eingabeordner enthält eine PowerShell-Skriptdatei des Systems und die Bereitstellungsskriptdateien der Benutzer. Sie können die Skriptdatei für die Benutzerbereitstellung durch eine überarbeitete Version ersetzen und das Bereitstellungsskript erneut aus der Azure-Containerinstanz ausführen.

Mithilfe der REST-API können Sie die Bereitstellungsinformationen der Bereitstellungsskriptressourcen auf Ressourcengruppenebene und Abonnementebene abrufen:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>?api-version=2019-10-01-preview
```

```rest
/subscriptions/<SubscriptionID>/providers/microsoft.resources/deploymentScripts?api-version=2019-10-01-preview
```

Im folgenden Beispiel wird [ARMClient](https://github.com/projectkudu/ARMClient) verwendet:

```azurepowershell
armclient login
armclient get /subscriptions/01234567-89AB-CDEF-0123-456789ABCDEF/resourcegroups/myrg/providers/microsoft.resources/deploymentScripts/myDeployementScript?api-version=2019-10-01-preview
```

Die Ausgabe sieht in etwa wie folgt aus:

:::code language="json" source="~/resourcemanager-templates/deployment-script/deploymentscript-status.json" range="1-37" highlight="15,34":::

Die Ausgabe zeigt den Bereitstellungsstatus und die IDs der Bereitstellungsskriptressourcen.

Die folgende REST-API gibt das Protokoll zurück:

```rest
/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/microsoft.resources/deploymentScripts/<DeploymentScriptResourceName>/logs?api-version=2019-10-01-preview
```

Dies funktioniert nur, bevor die Bereitstellungsskriptressourcen gelöscht werden.

Wählen Sie im Portal die Option **Ausgeblendete Typen anzeigen** aus, um die deploymentScripts-Ressource anzuzeigen.

![Resource Manager-Vorlage: Bereitstellungsskript, „Ausgeblendete Typen anzeigen“, Portal](./media/deployment-script-template/resource-manager-deployment-script-portal-show-hidden-types.png)

## <a name="use-an-existing-storage-account"></a>Verwenden eines vorhandenen Speicherkontos

Für die Skriptausführung und Problembehandlung werden ein Speicherkonto und eine Containerinstanz benötigt. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto anzugeben. Andernfalls wird das Speicherkonto zusammen mit der Containerinstanz vom Skriptdienst automatisch erstellt. Die Voraussetzungen für die Verwendung eines vorhandenen Speicherkontos:

- Unterstützte Speicherkontotypen: Universell V2, Universell V1 und FileStorage. Premium-SKUs werden nur von FileStorage-Konten unterstützt. Weitere Informationen finden Sie unter [Speicherkontentypen](../../storage/common/storage-account-overview.md).
- Firewallregeln für Speicherkonten werden noch nicht unterstützt. Weitere Informationen finden Sie unter [Konfigurieren von Firewalls und virtuellen Netzwerken in Azure Storage](../../storage/common/storage-network-security.md).
- Die vom Benutzer zugewiesene verwaltete Identität des Bereitstellungsskripts muss über Berechtigungen zum Verwalten des Speicherkontos verfügen, was das Lesen, Erstellen und Löschen von Dateifreigaben umfasst.

Um ein vorhandenes Speicherkonto anzugeben, fügen Sie den folgenden JSON-Code zum Eigenschaftselement von `Microsoft.Resources/deploymentScripts` hinzu:

```json
"storageAccountSettings": {
  "storageAccountName": "myStorageAccount",
  "storageAccountKey": "myKey"
},
```

- **storageAccountName**: Geben Sie den Namen des Speicherkontos an.
- **storageAccountKey**: Geben Sie einen der Speicherkontoschlüssel an. Sie können den Schlüssel mit der [`listKeys()`](./template-functions-resource.md#listkeys)-Funktion abrufen. Beispiel:

    ```json
    "storageAccountSettings": {
        "storageAccountName": "[variables('storageAccountName')]",
        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), '2019-06-01').keys[0].value]"
    }
    ```

Ein vollständiges Beispiel für die Definition von `Microsoft.Resources/deploymentScripts` finden Sie unter [Beispielvorlagen](#sample-templates).

Wenn ein vorhandenes Speicherkonto zum Einsatz kommt, erstellt der Skriptdienst eine Dateifreigabe mit einem eindeutigen Namen. Informationen dazu, wie der Skriptdienst die Dateifreigabe bereinigt, finden Sie unter [Bereinigen von Ressourcen für Bereitstellungsskripts](#clean-up-deployment-script-resources).

## <a name="clean-up-deployment-script-resources"></a>Bereinigen von Bereitstellungsskriptressourcen

Für die Skriptausführung und Problembehandlung werden ein Speicherkonto und eine Containerinstanz benötigt. Sie haben die Möglichkeit, ein vorhandenes Speicherkonto anzugeben. Andernfalls wird ein Speicherkonto zusammen mit einer Containerinstanz vom Skriptdienst automatisch erstellt. Die beiden automatisch erstellten Ressourcen werden vom Skriptdienst gelöscht, sobald die Ausführung des Bereitstellungsskripts beendet ist. Die Ressourcen werden Ihnen in Rechnung gestellt, bis sie gelöscht werden. Preisinformationen finden Sie unter [Container Instances – Preise](https://azure.microsoft.com/pricing/details/container-instances/) und [Azure Storage – Preise](https://azure.microsoft.com/pricing/details/storage/).

Der Lebenszyklus dieser Ressourcen wird durch die folgenden Eigenschaften in der Vorlage gesteuert:

- **cleanupPreference**: Dies ist die Einstellung für die Bereinigung, wenn die Skriptausführung einen Beendigungszustand erreicht. Die unterstützten Werte sind:

  - **Always:** Die Ressourcen werden gelöscht, sobald die Skriptausführung beendet ist. Wenn ein vorhandenes Speicherkonto zum Einsatz kommt, löscht der Skriptdienst die im Speicherkonto erstellte Dateifreigabe. Da die Ressource deploymentScripts auch nach Bereinigung der Ressourcen noch vorhanden sein kann, speichern die Skriptdienste die Ergebnisse der Skriptausführung, z. B. stdout, Ausgaben, Rückgabewerte usw., ehe die Ressourcen gelöscht werden.
  - **OnSuccess:** Die Ressourcen werden automatisch gelöscht, wenn die Skriptausführung erfolgreich war. Wenn ein vorhandenes Speicherkonto verwendet wird, entfernt der Skriptdienst die Dateifreigabe nur bei erfolgreicher Skriptausführung. Sie können weiterhin auf die Ressourcen zugreifen, um Debuginformationen zu finden.
  - **OnExpiration:** Die Ressourcen werden automatisch gelöscht, wenn die Einstellung **retentionInterval** abgelaufen ist. Wenn ein vorhandenes Speicherkonto zum Einsatz kommt, entfernt der Skriptdienst die Dateifreigabe und behält das Speicherkonto bei.

- **retentionInterval**: Geben Sie das Zeitintervall an, für das eine Skriptressource aufbewahrt wird und nach dem sie als abgelaufen gilt und gelöscht wird.

> [!NOTE]
> Es wird nicht empfohlen, das Speicherkonto und die Containerinstanz zu verwenden, die vom Skriptdienst für andere Zwecke generiert werden. Die beiden Ressourcen werden abhängig vom Lebenszyklus des Skripts möglicherweise entfernt.

## <a name="run-script-more-than-once"></a>Mehrmaliges Ausführen des Skripts

Die Ausführung des Bereitstellungsskripts ist ein idempotenter Vorgang. Wenn keine der deploymentScripts-Ressourceneigenschaften (einschließlich des Inlineskripts) geändert werden, wird das Skript nicht ausgeführt, wenn Sie die Vorlage erneut bereitstellen. Der Bereitstellungsskriptdienst vergleicht die Ressourcennamen in der Vorlage mit den vorhandenen Ressourcen in derselben Ressourcengruppe. Wenn Sie dasselbe Bereitstellungsskript mehrmals ausführen möchten, haben Sie zwei Möglichkeiten:

- Ändern Sie den Namen Ihrer deploymentScripts-Ressource. Verwenden Sie z. B. die Vorlagenfunktion [utcNow](./template-functions-date.md#utcnow) als Ressourcennamen oder als Teil des Ressourcennamens. Wenn Sie den Ressourcennamen ändern, wird eine neue deploymentScripts-Ressource erstellt. Es ist sinnvoll, den Verlauf der Skriptausführung zu protokollieren.

    > [!NOTE]
    > Die utcNow-Funktion kann nur für den Standardwert eines Parameters verwendet werden.

- Geben Sie einen anderen Wert in der `forceUpdateTag`-Eigenschaft der Vorlage an.  Verwenden Sie beispielsweise als Wert „utcNow“.

> [!NOTE]
> Schreiben Sie idempotente Bereitstellungsskripts. Dadurch wird sichergestellt, dass bei versehentlicher erneuter Ausführung keine Systemänderungen auftreten. Wenn das Bereitstellungsskript z. B. zum Erstellen einer Azure-Ressource verwendet wird, vergewissern Sie sich, dass die Ressource vor dem Erstellen nicht vorhanden ist, damit das Skript erfolgreich ausgeführt werden kann und Sie die Ressource nicht erneut erstellen.

## <a name="configure-development-environment"></a>Konfigurieren der Entwicklungsumgebung

Sie können ein vorkonfiguriertes Docker-Containerimage als Entwicklungsumgebung für Ihr Bereitstellungsskript verwenden. Im folgenden Verfahren wird gezeigt, wie Sie das Docker-Image unter Windows konfigurieren. Für Linux und Mac finden Sie die entsprechenden Informationen im Internet.

1. Installieren Sie [Docker Desktop](https://www.docker.com/products/docker-desktop) auf dem Entwicklungscomputer.
1. Öffnen Sie Docker Desktop.
1. Wählen Sie auf der Taskleiste das Symbol für Docker Desktop und dann **Settings** (Einstellungen) aus.
1. Wählen Sie **Shared Drives** (Freigegebene Laufwerke) und dann ein lokales Laufwerk aus, das für Ihre Container verfügbar sein soll. Wählen Sie dann **Apply** (Anwenden) aus.

    ![Resource Manager-Vorlage: Bereitstellungsskript, Docker-Laufwerk](./media/deployment-script-template/resource-manager-deployment-script-docker-setting-drive.png)

1. Geben Sie an der Eingabeaufforderung Ihre Windows-Anmeldeinformationen ein.
1. Öffnen Sie ein Terminalfenster (entweder die Eingabeaufforderung oder Windows PowerShell – verwenden Sie nicht die PowerShell ISE).
1. Pullen Sie das Containerimage für das Bereitstellungsskript auf den lokalen Computer:

    ```command
    docker pull mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    In diesem Beispiel wird Version 2.7.0 der PowerShell verwendet.

    So pullen Sie ein CLI-Image aus einer Microsoft Container Registry (MCR)

    ```command
    docker pull mcr.microsoft.com/azure-cli:2.0.80
    ```

    In diesem Beispiel wird Version 2.0.80 der CLI verwendet. Das Bereitstellungsskript verwendet die standardmäßigen CLI-Containerimages, die Sie [hier](https://hub.docker.com/_/microsoft-azure-cli) finden.

1. Führen Sie das Docker-Image lokal aus.

    ```command
    docker run -v <host drive letter>:/<host directory name>:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    Ersetzen Sie **&lt;host drive letter>** und **&lt;host directory name>** durch einen vorhandenen Ordner auf dem freigegebenen Laufwerk.  Der Ordner wird dem Ordner **/data** im Container zugeordnet. So ordnen Sie z. B. „D:\docker“ zu

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azuredeploymentscripts-powershell:az2.7
    ```

    **-it** bedeutet, dass das Containerimage aktiv bleibt.

    Ein CLI-Beispiel:

    ```command
    docker run -v d:/docker:/data -it mcr.microsoft.com/azure-cli:2.0.80
    ```

1. Wählen Sie nach Aufforderung **Share it** (Freigeben) aus.
1. Im folgenden Screenshot wird gezeigt, wie Sie ein PowerShell-Skript ausführen (sofern der Ordner „D:\docker“ die Datei „helloworld.ps1“ enthält).

    ![Resource Manager-Vorlage: Bereitstellungsskript, Docker-Befehle](./media/deployment-script-template/resource-manager-deployment-script-docker-cmd.png)

Nachdem das Skript erfolgreich getestet wurde, können Sie es als Bereitstellungsskript verwenden.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie Bereitstellungsskripts verwenden. Ein Tutorial zu Bereitstellungsskripts finden Sie unter:

> [!div class="nextstepaction"]
> [Tutorial: Verwenden von Bereitstellungsskripts in Azure Resource Manager-Vorlagen](./template-tutorial-deployment-script.md).