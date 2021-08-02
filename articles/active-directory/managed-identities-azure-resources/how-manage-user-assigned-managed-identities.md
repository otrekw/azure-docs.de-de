---
title: Verwalten von benutzerseitig zugewiesenen verwalteten Identitäten - Azure AD
description: Erstellen von benutzerseitig zugewiesenen verwalteten Identitäten
services: active-directory
author: barclayn
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: how-to
ms.workload: identity
ms.date: 06/08/2021
ms.author: barclayn
zone_pivot_groups: identity-mi-methods
ms.openlocfilehash: 46f6b6354bed2ef0e2fc531f00b8380b04f6f0b9
ms.sourcegitcommit: 8bca2d622fdce67b07746a2fb5a40c0c644100c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2021
ms.locfileid: "111758112"
---
# <a name="manage-user-assigned-managed-identities"></a>Verwalten von benutzerseitig zugewiesenen verwalteten Identitäten


Verwaltete Identitäten für Azure-Ressourcen machen die Verwaltung von Anmeldeinformationen im Code überflüssig. Sie ermöglichen es Ihnen, ein Azure Active Directory-Token abzurufen, das Ihre Anwendungen beim Zugriff auf Ressourcen verwenden können, die die Azure Active Directory-Authentifizierung unterstützen. Azure nimmt Ihnen die Verwaltung der Identität ab. Es gibt zwei Arten von verwalteten Identitäten: systemseitig und benutzerseitig zugewiesene Identitäten. Der Hauptunterschied zwischen den beiden Arten ist, dass der Lebenszyklus systemseitig zugewiesener verwalteter Identitäten mit der Ressource verknüpft ist, in der sie verwendet werden. Benutzerseitig zugewiesene verwaltete Identitäten können für mehrere Ressourcen verwendet werden. Weitere Informationen zu verwalteten Identitäten finden Sie in der [Übersicht](overview.md) über verwaltete Identitäten. 

::: zone pivot="identity-mi-methods-azp"

In diesem Artikel erfahren Sie, wie Sie eine benutzerseitig zugewiesene verwaltete Identität über das Azure-Portal erstellen, auflisten, löschen oder zuweisen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesene verwaltete Identität zu erstellen.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter **Dienste** auf **Verwaltete Identitäten**.
3. Klicken Sie auf **Hinzufügen**, und geben Sie im Bereich **Vom Benutzer zugewiesene verwaltete Identität erstellen** in den folgenden Feldern Werte ein:
    - **Abonnement**: Wählen Sie das Abonnement aus, in dem die benutzerseitig zugewiesene verwaltete Identität erstellt werden soll.
    - **Ressourcengruppe**: Wählen Sie eine Ressourcengruppe aus, in der die benutzerseitig zugewiesene verwaltete Identität erstellt werden soll, oder klicken Sie auf **Neu erstellen**, um eine neue Ressourcengruppe zu erstellen.
    - **Region**: Wählen Sie eine Region aus, in der die benutzerseitig zugewiesene verwaltete Identität bereitgestellt werden soll, z. B. **USA, Westen**.
    - **Name**: Dies ist der Name für die vom Benutzer zugewiesene verwaltete Identität, z.B. „UAI1“.
    ![Erstellen einer benutzerseitig zugewiesenen verwalteten Identität](media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)
4. Klicken Sie auf **Überprüfen + erstellen**, um die Änderungen zu überprüfen.
5. Klicken Sie auf **Erstellen**.

## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Ihrem Konto muss die Rolle [Operator für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-operator) oder [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität aufgelistet/gelesen werden kann.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesenen verwalteten Identitäten aufzulisten.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter „Dienste“ auf **Verwaltete Identitäten**.
3. Eine Liste der vom Benutzer zugewiesenen verwalteten Identitäten für Ihr Abonnement wird zurückgegeben.  Durch Klicken auf den entsprechenden Namen können Sie die Details der vom Benutzer zugewiesenen verwalteten Identität anzeigen.

![Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten](media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität gelöscht werden kann.

Durch das Löschen wird eine vom Benutzer zugewiesene Identität nicht von der VM oder Ressource entfernt, der sie zugewiesen wurde.  Informationen zum Entfernen einer vom Benutzer zugewiesenen Identität von einem virtuellen Computer finden Sie unter [Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer VM](qs-configure-portal-windows-vm.md#remove-a-user-assigned-managed-identity-from-a-vm).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um eine vom Benutzer zugewiesene verwaltete Identität zu löschen.
2. Wählen Sie die vom Benutzer zugewiesene verwaltete Identität aus, und klicken Sie auf **Löschen**.
3. Wählen Sie im Bestätigungsfeld **Ja** aus.

![Löschen einer vom Benutzer zugewiesenen verwalteten Identität](media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Zuweisen einer Rolle zu einer vom Benutzer zugewiesenen verwalteten Identität 

Ihrem Konto muss die Rolle [Benutzerzugriffsadministrator](../../role-based-access-control/built-in-roles.md#user-access-administrator) zugewiesen sein, damit einer benutzerseitig zugewiesenen verwalteten Identität eine Rolle zugewiesen werden kann.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) mit einem Konto an, das dem Azure-Abonnement zugeordnet ist, um die vom Benutzer zugewiesenen verwalteten Identitäten aufzulisten.
2. Geben Sie *Verwaltete Identitäten* im Suchfeld ein, und klicken Sie unter „Dienste“ auf **Verwaltete Identitäten**.
3. Eine Liste der vom Benutzer zugewiesenen verwalteten Identitäten für Ihr Abonnement wird zurückgegeben.  Wählen Sie die vom Benutzer zugewiesene verwaltete Identität aus, der Sie eine Rolle zuweisen möchten.
4. Wählen Sie **Zugriffssteuerung (IAM)** und anschließend **Rollenzuweisung hinzufügen** aus.

   ![Vom Benutzer zugewiesene verwaltete Identität – Start](media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. Konfigurieren Sie auf dem Blatt „Rollenzuweisung hinzufügen“ die folgenden Werte, und klicken Sie dann auf **Speichern**:
   - **Rolle**: Zuzuweisende Rolle
   - **Zugriff zuweisen zu**: Ressource, der die vom Benutzer zugewiesene verwaltete Identität zugewiesen wird
   - **Auswählen**: Mitglied, dem der Zugriff zugewiesen wird
   
   ![Vom Benutzer zugewiesene verwaltete Identität – IAM](media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)



::: zone-end

::: zone pivot="identity-mi-methods-azcli"

In diesem Artikel erfahren Sie, wie Sie eine benutzerseitig zugewiesene verwaltete Identität über die Azure-Befehlszeilenschnittstelle erstellen, auflisten, löschen oder zuweisen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../../includes/azure-cli-prepare-your-environment-no-header.md)]

> [!IMPORTANT]  
> Um Benutzerberechtigungen bei Verwendung eines App Service-Prinzipals mithilfe der Befehlszeilenschnittstelle zu ändern, müssen Sie dem Dienstprinzipal zusätzliche Berechtigungen in der Azure AD Graph-API zuweisen, da Teile der GET-Anforderungen in der Befehlszeilenschnittstelle für die Graph-API ausgeführt werden. Andernfalls erhalten Sie möglicherweise eine Meldung mit einem Hinweis auf unzureichende Berechtigungen zum Durchführen des Vorgangs. Dazu müssen Sie die App-Registrierung in Azure Active Directory aufrufen, Ihre App auswählen, auf „API-Berechtigungen“ klicken, nach unten scrollen und „Azure Active Directory Graph“ auswählen. Wählen Sie anschließend „Anwendungsberechtigungen“ aus, und fügen Sie dann die entsprechenden Berechtigungen hinzu. 

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität 

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

Verwenden Sie den Befehl [az identity create](/cli/azure/identity#az_identity_create), um eine benutzerseitig zugewiesene verwaltete Identität zu erstellen. Der Parameter `-g` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene verwaltete Identität erstellt werden soll, während der Parameter `-n` den Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Ihrem Konto muss die Rolle [Operator für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-operator) oder [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität aufgelistet/gelesen werden kann.

Verwenden Sie den Befehl [az identity list](/cli/azure/identity#az_identity_list), um vom Benutzer zugewiesene verwaltete Identitäten aufzulisten. Ersetzen Sie `<RESOURCE GROUP>` durch Ihren eigenen Wert:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```

In der JSON-Antwort wird bei vom Benutzer zugewiesenen verwalteten Identitäten der Wert `"Microsoft.ManagedIdentity/userAssignedIdentities"` für den Schlüssel `type` zurückgegeben.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität gelöscht werden kann.

Um eine vom Benutzer zugewiesene verwaltete Identität zu löschen, verwenden Sie den Befehl [az identity delete](/cli/azure/identity#az_identity_delete).  Der Parameter „-n“ gibt den Namen an, während der Parameter „-g“ die Ressourcengruppe angibt, in der die vom Benutzer zugewiesene verwaltete Identität erstellt wurde. Ersetzen Sie die Parameterwerte `<USER ASSIGNED IDENTITY NAME>` und `<RESOURCE GROUP>` durch Ihre eigenen Werte:

```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen verwalteten Identität wird der Verweis nicht für andere Ressourcen entfernt, denen sie zugewiesen wurde. Entfernen Sie diese mit dem Befehl `az vm/vmss identity remove` von der VM/VMSS.

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Liste der Identitätsbefehle für die Azure CLI finden Sie unter [az identity](/cli/azure/identity).

Informationen zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einer Azure-VM finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe der Azure-Befehlszeilenschnittstelle](qs-configure-cli-windows-vm.md#user-assigned-managed-identity).


::: zone-end

::: zone pivot="identity-mi-methods-powershell"

In diesem Artikel erfahren Sie, wie Sie eine benutzerseitig zugewiesene verwaltete Identität mithilfe von PowerShell erstellen, auflisten und löschen.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Zum Ausführen der Beispielskripts haben Sie zwei Möglichkeiten:
    - Verwenden Sie die [Azure Cloud Shell](../../cloud-shell/overview.md), die Sie mithilfe der Schaltfläche **Testen** in der oberen rechten Ecke der Codeblöcke öffnen können.
    - Führen Sie Skripts lokal mit Azure PowerShell aus, wie im nächsten Abschnitt beschrieben.

### <a name="configure-azure-powershell-locally"></a>Lokales Konfigurieren von Azure PowerShell

Wenn Sie Azure PowerShell in diesem Artikel lokal verwenden möchten (anstelle von Cloud Shell), führen Sie die folgenden Schritte aus:

1. Installieren Sie [die aktuelle Version von Azure PowerShell](/powershell/azure/install-az-ps), sofern noch nicht geschehen.

1. Melden Sie sich bei Azure an:

    ```azurepowershell
    Connect-AzAccount
    ```

1. Installieren Sie die [neueste Version von PowerShellGet](/powershell/scripting/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).

    ```azurepowershell
    Install-Module -Name PowerShellGet -AllowPrerelease
    ```

    Sie müssen die aktuelle PowerShell-Sitzung unter Umständen beenden (`Exit`), nachdem Sie diesen Befehl für den nächsten Schritt ausgeführt haben.

1. Installieren Sie die Vorabversion des `Az.ManagedServiceIdentity`-Moduls, um die Vorgänge für benutzerseitig zugewiesene verwaltete Identitäten in diesem Artikel auszuführen:

    ```azurepowershell
    Install-Module -Name Az.ManagedServiceIdentity -AllowPrerelease
    ```

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

Um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, verwenden Sie den Befehl `New-AzUserAssignedIdentity`. Der Parameter `ResourceGroupName` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene verwaltete Identität erstellt werden soll, während der Parameter `-Name` den Namen angibt. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```azurepowershell-interactive
New-AzUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```

## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Ihrem Konto muss die Rolle [Operator für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-operator) oder [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität aufgelistet/gelesen werden kann.

Um die vom Benutzer zugewiesenen verwalteten Identitäten aufzulisten, verwenden Sie den Befehl [Get-AzUserAssigned].  Der Parameter `-ResourceGroupName` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene verwaltete Identität erstellt wurde. Ersetzen Sie `<RESOURCE GROUP>` durch Ihren eigenen Wert:

```azurepowershell-interactive
Get-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```

In der Antwort wird bei vom Benutzer zugewiesenen verwalteten Identitäten der Wert `"Microsoft.ManagedIdentity/userAssignedIdentities"` für den Schlüssel `Type` zurückgegeben.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität gelöscht werden kann.

Um eine vom Benutzer zugewiesene verwaltete Identität zu löschen, verwenden Sie den Befehl `Remove-AzUserAssignedIdentity`.  Der Parameter `-ResourceGroupName` gibt die Ressourcengruppe an, in der die vom Benutzer zugewiesene Identität erstellt wurde, und der Parameter `-Name` gibt den Namen an. Ersetzen Sie die Parameterwerte `<RESOURCE GROUP>` und `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

```azurepowershell-interactive
Remove-AzUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```

> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen verwalteten Identität wird der Verweis nicht für andere Ressourcen entfernt, denen sie zugewiesen wurde. Zuweisungen von Identitäten müssen separat entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Liste der Azure PowerShell-Befehle für verwaltete Identitäten für Azure-Ressourcen und weitere Details hierzu finden Sie unter [Az.ManagedServiceIdentity](/powershell/module/az.managedserviceidentity#managed_service_identity).


::: zone-end


::: zone pivot="identity-mi-methods-arm"

In diesem Artikel erstellen Sie eine vom Benutzer zugewiesene verwaltete Identität mithilfe des Azure Resource Manager.

Das Auflisten und Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe einer Azure Resource Manager-Vorlage ist nicht möglich.  Informationen zum Erstellen und Auflisten einer vom Benutzer zugewiesenen verwalteten Identität finden Sie in den folgenden Artikeln:

- [Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten](how-to-manage-ua-identity-cli.md#list-user-assigned-managed-identities)
- [Löschen einer vom Benutzer zugewiesenen verwalteten Identität](how-to-manage-ua-identity-cli.md#delete-a-user-assigned-managed-identity)

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.

## <a name="template-creation-and-editing"></a>Erstellung und Bearbeitung von Vorlagen

Wie beim Azure-Portal und der Skripterstellung bieten Azure Resource Manager-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

- Verwenden einer [benutzerdefinierten Vorlage aus Azure Marketplace](../../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage zu verwenden, die auf einer vorhandenen, häufig genutzten Vorlage oder einer [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
- Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates) oder aus dem [aktuellen Status der Bereitstellung](../../azure-resource-manager/management/manage-resource-groups-portal.md#export-resource-groups-to-templates) exportiert wird.
- Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
- Verwenden des [Azure-Ressourcengruppenprojekts](../../azure-resource-manager/templates/create-visual-studio-deployment-project.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage. 

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität 

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

Um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen, verwenden Sie die folgende Vorlage. Ersetzen Sie den Wert `<USER ASSIGNED IDENTITY NAME>` durch Ihre eigenen Werte:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "resourceName": {
          "type": "string",
          "metadata": {
            "description": "<USER ASSIGNED IDENTITY NAME>"
          }
        }
  },
  "resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('resourceName')]",
      "apiVersion": "2018-11-30",
      "location": "[resourceGroup().location]"
    }
  ],
  "outputs": {
      "identityName": {
          "type": "string",
          "value": "[parameters('resourceName')]"
      }
  }
}
```
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe einer Vorlage](qs-configure-template-windows-vm.md).




::: zone-end


::: zone pivot="identity-mi-methods-rest"

In diesem Artikel erfahren Sie, wie Sie mithilfe von cURL zum Durchführen von REST-API-Aufrufen eine vom Benutzer zugewiesene verwaltete Identität erstellen, auflisten und löschen können.

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit verwalteten Identitäten für Azure-Ressourcen vertraut sind, helfen Ihnen die Informationen in der [Übersicht](overview.md) weiter. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#managed-identity-types)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Sie können alle Befehle in diesem Artikel in der Cloud oder lokal ausführen:
    - Verwenden Sie für die Ausführung in der Cloud [Azure Cloud Shell](../../cloud-shell/overview.md).
    - Installieren Sie für die lokale Ausführung [curl](https://curl.haxx.se/download.html) und die [Azure CLI](/cli/azure/install-azure-cli).

## <a name="obtain-a-bearer-access-token"></a>Abrufen eines Bearerzugriffstokens

1. Wenn Sie die Befehle lokal ausführen, melden Sie sich über die Azure CLI bei Azure an:

    ```
    az login
    ```

1. Rufen Sie mit [az account get-access-token](/cli/azure/account#az_account_get_access_token) ein Zugriffstoken ab.

    ```azurecli-interactive
    az account get-access-token
    ```

## <a name="create-a-user-assigned-managed-identity"></a>Erstellen einer benutzerseitig zugewiesenen verwalteten Identität 

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität erstellt werden kann.

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
PUT https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```

**Anforderungsheader**

|Anforderungsheader  |BESCHREIBUNG  |
|---------|---------|
|*Content-Type*     | Erforderlich. Legen Sie diese Option auf `application/json` fest.        |
|*Autorisierung*     | Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer`-Zugriffstoken fest.        |

**Anforderungstext**

|Name  |BESCHREIBUNG  |
|---------|---------|
|location     | Erforderlich. Ressourcenspeicherort        |

## <a name="list-user-assigned-managed-identities"></a>Auflisten der vom Benutzer zugewiesenen verwalteten Identitäten

Ihrem Konto muss die Rolle [Operator für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-operator) oder [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität aufgelistet/gelesen werden kann.

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
GET https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview HTTP/1.1
```

|Anforderungsheader  |BESCHREIBUNG  |
|---------|---------|
|*Content-Type*     | Erforderlich. Legen Sie diese Option auf `application/json` fest.        |
|*Autorisierung*     | Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer`-Zugriffstoken fest.        |

## <a name="delete-a-user-assigned-managed-identity"></a>Löschen einer vom Benutzer zugewiesenen verwalteten Identität

Ihrem Konto muss die Rolle [Mitwirkender für verwaltete Identität](../../role-based-access-control/built-in-roles.md#managed-identity-contributor) zugewiesen sein, damit eine benutzerseitig zugewiesene verwaltete Identität gelöscht werden kann.

> [!NOTE]
> Durch das Löschen einer vom Benutzer zugewiesenen verwalteten Identität wird der Verweis nicht für andere Ressourcen entfernt, denen sie zugewiesen wurde. Informationen zum Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einem virtuellen Computer mithilfe von cURL finden Sie unter [Entfernen einer vom Benutzer zugewiesenen Identität von einem virtuellen Azure-Computer](qs-configure-rest-vm.md#remove-a-user-assigned-managed-identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

```HTTP
DELETE https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview HTTP/1.1
```
|Anforderungsheader  |BESCHREIBUNG  |
|---------|---------|
|*Content-Type*     | Erforderlich. Legen Sie diese Option auf `application/json` fest.        |
|*Autorisierung*     | Erforderlich. Legen Sie diese Option auf ein gültiges `Bearer`-Zugriffstoken fest.        |

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einer Azure-VM/-VMSS mithilfe von cURL finden Sie unter [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe von REST-API-Aufrufen](qs-configure-rest-vm.md#user-assigned-managed-identity) und [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen in einer VM-Skalierungsgruppe mithilfe von REST-API-Aufrufen](qs-configure-rest-vmss.md#user-assigned-managed-identity).

::: zone-end


