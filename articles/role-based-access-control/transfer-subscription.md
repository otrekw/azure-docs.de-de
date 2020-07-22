---
title: Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis (Vorschau)
description: Erfahren Sie, wie Sie ein Azure-Abonnement und bekannte zugehörige Ressourcen in ein anderes Azure Active Directory-Verzeichnis (Azure AD) übertragen.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.workload: identity
ms.date: 07/01/2020
ms.author: rolyon
ms.openlocfilehash: db1b030aed34498ade91a195d5ca68725b579ba3
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86230841"
---
# <a name="transfer-an-azure-subscription-to-a-different-azure-ad-directory-preview"></a>Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis (Vorschau)

> [!IMPORTANT]
> Dieses Verfahren zum Übertragen eines Abonnements in ein anderes Azure AD-Verzeichnis befindet sich zurzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Organisationen verfügen möglicherweise über mehrere Azure-Abonnements. Jedes Abonnement ist mit einem bestimmten Azure Active Directory-Verzeichnis (Azure AD) verknüpft. Um die Verwaltung zu vereinfachen, können Sie ein Abonnement in ein anderes Azure AD-Verzeichnis übertragen. Wenn Sie ein Abonnement in ein anderes Azure AD-Verzeichnis übertragen, werden einige Ressourcen nicht in das Zielverzeichnis übertragen. Beispielsweise werden alle Rollenzuweisungen und benutzerdefinierten Rollen in der rollenbasierten Zugriffssteuerung von Azure (Azure RBAC) **dauerhaft** aus dem Quellverzeichnis gelöscht und nicht in das Zielverzeichnis übertragen.

In diesem Artikel werden die grundlegenden Schritte beschrieben, die Sie befolgen können, um ein Abonnement in ein anderes Azure AD-Verzeichnis zu übertragen und einige der Ressourcen nach der Übertragung erneut zu erstellen.

## <a name="overview"></a>Übersicht

Das Übertragen eines Azure-Abonnements in ein anderes Azure AD-Verzeichnis ist ein komplexer Vorgang, der sorgfältig geplant und ausgeführt werden muss. Viele Azure-Dienste erfordern Sicherheitsprinzipale (Identitäten), um normal zu funktionieren oder andere Azure-Ressourcen zu verwalten. Dieser Artikel versucht, die meisten der Azure-Dienste abzudecken, die stark von Sicherheitsprinzipien abhängig sind, ist aber nicht umfassend.

> [!IMPORTANT]
> Das Übertragen eines Abonnements ist mit Ausfallzeiten verbunden, um den Vorgang abzuschließen.

Die folgende Abbildung zeigt die grundlegenden Schritte, die Sie befolgen müssen, wenn Sie ein Abonnement in ein anderes Verzeichnis übertragen.

1. Vorbereiten der Übertragung

1. Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto

1. Erneutes Erstellen von Ressourcen im Zielverzeichnis (z. B. Rollenzuweisungen, benutzerdefinierte Rollen und verwaltete Identitäten)

    ![Abbildung zum Übertragen eines Abonnements](./media/transfer-subscription/transfer-subscription.png)

### <a name="deciding-whether-to-transfer-a-subscription-to-a-different-directory"></a>Entscheiden, ob ein Abonnement in ein anderes Verzeichnis übertragen werden soll

Im Folgenden finden Sie einige Gründe, warum Sie ein Abonnement übertragen möchten:

- Aufgrund einer Firmenfusion oder -übernahme möchten Sie ein erworbenes Abonnement in Ihrem primären Azure AD-Verzeichnis verwalten.
- Eine Person in Ihrer Organisation hat ein Abonnement erstellt, und Sie möchten die Verwaltung in einem bestimmten Azure AD-Verzeichnis konsolidieren.
- Sie verfügen über Anwendungen, die von einer bestimmten Abonnement-ID oder URL abhängig sind, und es ist nicht einfach, die Konfiguration oder den Code der Anwendung zu ändern.
- Ein Teil Ihres Unternehmens wurde in ein separates Unternehmen aufgeteilt, und Sie müssen einige Ihrer Ressourcen in ein anderes Azure AD-Verzeichnis verschieben.
- Sie möchten einige Ihrer Ressourcen zur Sicherheitsisolierung in einem anderen Azure AD-Verzeichnis verwalten.

Das Übertragen eines Abonnements ist mit Ausfallzeiten verbunden, um den Vorgang abzuschließen. Abhängig von Ihrem Szenario kann es besser sein, die Ressourcen einfach erneut zu erstellen und die Daten in das Zielverzeichnis und das Abonnement zu kopieren.

### <a name="understand-the-impact-of-transferring-a-subscription"></a>Verstehen der Auswirkungen der Übertragung eines Abonnements

Mehrere Azure-Ressourcen weisen eine Abhängigkeit von einem Abonnement oder einem Verzeichnis auf. Abhängig von Ihrer Situation listet die folgende Tabelle die bekannten Auswirkungen der Übertragung eines Abonnements auf. Wenn Sie die Schritte in diesem Artikel ausführen, können Sie einige der Ressourcen, die vor der Übertragung des Abonnements vorhanden waren, erneut erstellen.

> [!IMPORTANT]
> In diesem Abschnitt werden die bekannten Azure-Dienste oder -Ressourcen aufgelistet, die von Ihrem Abonnement abhängig sind. Da Ressourcentypen in Azure ständig weiterentwickelt werden, sind möglicherweise zusätzliche Abhängigkeiten vorhanden, die hier nicht aufgelistet werden, die jedoch zu einem Breaking Change Ihrer Umgebung führen können. 

| Dienst oder Ressource | Betroffen | Wiederherstellbar | Sind Sie betroffen? | Ihre Möglichkeiten |
| --------- | --------- | --------- | --------- | --------- |
| Rollenzuweisungen | Ja | Ja | [Auflisten von Rollenzuweisungen](#save-all-role-assignments) | Alle Rollenzuweisungen werden dauerhaft gelöscht. Sie müssen den entsprechenden Objekten im Zielverzeichnis Benutzer, Gruppen und Dienstprinzipale zuordnen. Sie müssen die Rollenzuweisungen erneut erstellen. |
| Benutzerdefinierte Rollen | Ja | Ja | [Auflisten benutzerdefinierter Rollen](#save-custom-roles) | Alle benutzerdefinierten Rollen werden dauerhaft gelöscht. Die benutzerdefinierten Rollen und Rollenzuweisungen müssen erneut erstellt werden. |
| Vom System zugewiesene verwaltete Identitäten | Ja | Ja | [Auflisten verwalteter Identitäten](#list-role-assignments-for-managed-identities) | Sie müssen die verwalteten Identitäten deaktivieren und erneut aktivieren. Sie müssen die Rollenzuweisungen erneut erstellen. |
| Vom Benutzer zugewiesene verwaltete Identitäten | Ja | Ja | [Auflisten verwalteter Identitäten](#list-role-assignments-for-managed-identities) | Sie müssen die verwalteten Identitäten löschen, erneut erstellen und an die entsprechende Ressource anfügen. Sie müssen die Rollenzuweisungen erneut erstellen. |
| Azure-Schlüsseltresor | Ja | Ja | [Auflisten von Key Vault-Zugriffsrichtlinien](#list-other-known-resources) | Sie müssen die Mandanten-ID aktualisieren, die den Schlüsseltresoren zugeordnet ist. Sie müssen Zugriffsrichtlinien entfernen und neue Zugriffsrichtlinien hinzufügen. |
| Azure SQL-Datenbanken mit Azure AD-Authentifizierung | Ja | Nein | [Überprüfen von Azure SQL-Datenbanken mit Azure AD Authentifizierung](#list-other-known-resources) |  |  |
| Azure Storage und Azure Data Lake Storage Gen2 | Ja | Ja |  | ACLs müssen erneut erstellt werden. |
| Azure Data Lake Storage Gen1 | Ja |  |  | ACLs müssen erneut erstellt werden. |
| Azure Files | Ja | Ja |  | ACLs müssen erneut erstellt werden. |
| Azure-Dateisynchronisierung | Ja | Ja |  |  |
| Azure Managed Disks | Ja | – |  |  |
| Azure Container Services für Kubernetes | Ja | Ja |  |  |
| Azure Active Directory Domain Services | Ja | Nein |  |  |
| App-Registrierungen | Ja | Ja |  |  |

Wenn Sie die Verschlüsselung ruhender Daten für eine Ressource verwenden (z. B. ein Speicherkonto oder eine SQL-Datenbank), die von einem Schlüsseltresor abhängig ist, der sich NICHT im selben Abonnement befindet, das übertragen wird, kann dies zu einem nicht wiederherstellbaren Szenario führen. In dieser Situation sollten Sie Maßnahmen ergreifen, um einen anderen Schlüsseltresor zu verwenden oder kundenseitig verwaltete Schlüssel vorübergehend zu deaktivieren, um dieses nicht wiederherstellbare Szenario zu vermeiden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen dieser Schritte benötigen Sie Folgendes:

- [Bash in der Azure Cloud Shell](/azure/cloud-shell/overview) oder [Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure)
- Kontoadministrator des Abonnements, das Sie im Quellverzeichnis übertragen möchten
- Rolle [Besitzer](built-in-roles.md#owner) im Zielverzeichnis

## <a name="step-1-prepare-for-the-transfer"></a>Schritt 1: Vorbereiten der Übertragung

### <a name="sign-in-to-source-directory"></a>Anmelden beim Quellverzeichnis

1. Melden Sie sich bei Azure als Administrator an.

1. Rufen Sie eine Liste Ihrer Abonnements mithilfe des Befehls [az account list](/cli/azure/account#az-account-list) ab.

    ```azurecli
    az account list --output table
    ```

1. Verwenden Sie [az account set](https://docs.microsoft.com/cli/azure/account#az-account-set), um das aktive Abonnement festzulegen, das Sie übertragen möchten.

    ```azurecli
    az account set --subscription "Marketing"
    ```

### <a name="install-the-resource-graph-extension"></a>Installieren der Ressourcengrapherweiterung

 Die Resourcengrapherweiterung ermöglicht es Ihnen, mit dem Befehl [az graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) Ressourcen abzufragen, die von Azure Resource Manager verwaltet werden. Sie verwenden diesen Befehl in späteren Schritten.

1. Verwenden Sie [az extension list](https://docs.microsoft.com/cli/azure/extension#az-extension-list), um festzustellen, ob die Erweiterung *resource-graph* installiert ist.

    ```azurecli
    az extension list
    ```

1. Wenn dies nicht der Fall ist, installieren Sie die Erweiterung *resource-graph*.

    ```azurecli
    az extension add --name resource-graph
    ```

### <a name="save-all-role-assignments"></a>Speichern aller Rollenzuweisungen

1. Verwenden Sie [az role assignment list](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-list), um alle Rollenzuweisungen (einschließlich geerbter Rollenzuweisungen) aufzulisten.

    Um die Überprüfung der Liste zu vereinfachen, können Sie die Ausgabe als JSON, TSV oder Tabelle exportieren. Weitere Informationen finden Sie unter [Auflisten von Rollenzuweisungen mithilfe von Azure RBAC und der Azure CLI](role-assignments-list-cli.md).

    ```azurecli
    az role assignment list --all --include-inherited --output json > roleassignments.json
    az role assignment list --all --include-inherited --output tsv > roleassignments.tsv
    az role assignment list --all --include-inherited --output table > roleassignments.txt
    ```

1. Speichern Sie die Liste der Rollenzuweisungen.

    Wenn Sie ein Abonnement übertragen, werden alle Rollenzuweisungen **dauerhaft** gelöscht, sodass es wichtig ist, eine Kopie zu speichern.

1. Überprüfen Sie die Liste der Rollenzuweisungen. Es gibt möglicherweise Rollenzuweisungen, die Sie im Zielverzeichnis nicht benötigen.

### <a name="save-custom-roles"></a>Speichern benutzerdefinierter Rollen

1. Verwenden Sie [az role definition list](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-list), um Ihre benutzerdefinierten Rollen aufzulisten. Weitere Informationen finden Sie unter [Erstellen oder Aktualisieren von benutzerdefinierten Rollen für Azure-Ressourcen mithilfe der Azure CLI](custom-roles-cli.md).

    ```azurecli
    az role definition list --custom-role-only true --output json --query '[].{roleName:roleName, roleType:roleType}'
    ```

1. Speichern Sie jede benutzerdefinierte Rolle, die Sie im Zielverzeichnis benötigen, als separate JSON-Datei.

    ```azurecli
    az role definition list --name <custom_role_name> > customrolename.json
    ```

1. Erstellen Sie Kopien der benutzerdefinierten Rollendateien.

1. Ändern Sie die einzelnen Kopien so, dass das folgende Format verwendet wird.

    Sie verwenden diese Dateien später zum erneuten Erstellen der benutzerdefinierten Rollen im Zielverzeichnis.

    ```json
    {
      "Name": "",
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": []
    }
    ```

### <a name="determine-user-group-and-service-principal-mappings"></a>Bestimmen von Benutzer-, Gruppen- und Dienstprinzipalzuordnungen

1. Bestimmen Sie basierend auf der Liste der Rollenzuweisungen die Benutzer, Gruppen und Dienstprinzipale, für die im Zielverzeichnis eine Zuordnung erfolgt.

    Sie können den Typ des Prinzipals ermitteln, indem Sie die `principalType`-Eigenschaft in jeder Rollenzuweisung untersuchen.

1. Erstellen Sie ggf. im Zielverzeichnis Benutzer, Gruppen oder Dienstprinzipale, die Sie benötigen.

### <a name="list-role-assignments-for-managed-identities"></a>Auflisten von Rollenzuweisungen für verwaltete Identitäten

Verwaltete Identitäten werden nicht aktualisiert, wenn ein Abonnement in ein anderes Verzeichnis übertragen wird. Infolgedessen tritt für alle vorhandenen system- oder benutzerseitig zugewiesenen verwalteten Identitäten ein Fehler auf. Nach der Übertragung können Sie alle vom System zugewiesenen verwalteten Identitäten erneut aktivieren. Benutzerseitig zugewiesene verwaltete Identitäten müssen Sie sie im Zielverzeichnis erneut erstellen und anfügen.

1. Überprüfen Sie die [Liste der Azure-Dienste, die verwaltete Identitäten unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), um zu erfahren, wo Sie möglicherweise verwaltete Identitäten verwenden können.

1. Verwenden Sie [az ad sp list](/cli/azure/identity?view=azure-cli-latest#az-identity-list), um systemseitig zugewiesene und benutzerseitig zugewiesene verwaltete Identitäten aufzulisten.

    ```azurecli
    az ad sp list --all --filter "servicePrincipalType eq 'ManagedIdentity'"
    ```

1. Ermitteln Sie in der Liste der verwalteten Identitäten, welche Identitäten systemseitig zugewiesen und welche benutzerseitig zugewiesen sind. Zum Ermitteln des Typs können Sie die folgenden Kriterien verwenden.

    | Kriterien | Typ der verwalteten Identität |
    | --- | --- |
    | `alternativeNames`-Eigenschaft enthält `isExplicit=False` | Systemseitig zugewiesen |
    | `alternativeNames`-Eigenschaft enthält nicht `isExplicit` | Systemseitig zugewiesen |
    | `alternativeNames`-Eigenschaft enthält `isExplicit=True` | Benutzerseitig zugewiesen |

    Sie können auch [az identity list](https://docs.microsoft.com/cli/azure/identity#az-identity-list) verwenden, um nur benutzerseitig zugewiesene verwaltete Identitäten aufzulisten. Weitere Informationen finden Sie unter [Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md).

    ```azurecli
    az identity list
    ```

1. Rufen Sie eine Liste der `objectId`-Werte für Ihre verwalteten Identitäten ab.

1. Durchsuchen Sie die Liste der Rollenzuweisungen, um zu ermitteln, ob für Ihre verwalteten Identitäten Rollenzuweisungen vorhanden sind.

### <a name="list-key-vaults"></a>Auflisten von Schlüsseltresoren

Wenn Sie einen Schlüsseltresor erstellen, wird er automatisch an die standardmäßige Azure Active Directory-Mandanten-ID für das Abonnement gebunden, in dem er erstellt wurde. Außerdem werden auch alle Zugriffsrichtlinieneinträge an diese Mandanten-ID gebunden. Weitere Informationen finden Sie unter [Verschieben einer Azure Key Vault-Instanz in ein anderes Abonnement](../key-vault/general/keyvault-move-subscription.md).

> [!WARNING]
> Wenn Sie die Verschlüsselung ruhender Daten für eine Ressource verwenden (z. B. ein Speicherkonto oder eine SQL-Datenbank), die von einem Schlüsseltresor abhängig ist, der sich NICHT im selben Abonnement befindet, das übertragen wird, kann dies zu einem nicht wiederherstellbaren Szenario führen. In dieser Situation sollten Sie Maßnahmen ergreifen, um einen anderen Schlüsseltresor zu verwenden oder kundenseitig verwaltete Schlüssel vorübergehend zu deaktivieren, um dieses nicht wiederherstellbare Szenario zu vermeiden.

- Wenn Sie über einen Schlüsseltresor verfügen, verwenden Sie [az keyvault show](https://docs.microsoft.com/cli/azure/keyvault#az-keyvault-show), um die Zugriffsrichtlinien aufzulisten. Weitere Informationen finden Sie unter [Bereitstellen von Key Vault-Authentifizierung mit einer Zugriffssteuerungsrichtlinie](../key-vault/key-vault-group-permissions-for-apps.md).

    ```azurecli
    az keyvault show --name MyKeyVault
    ```

### <a name="list-azure-sql-databases-with-azure-ad-authentication"></a>Auflisten von Azure SQL-Datenbanken mit Azure AD-Authentifizierung

- Verwenden Sie [az sql server ad-admin list](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) und die Erweiterung [az graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph), um festzustellen, ob Sie Azure SQL-Datenbanken mit Azure AD-Authentifizierung verwenden. Weitere Informationen finden Sie unter [Konfigurieren und Verwalten von Azure Active Directory-Authentifizierung mit SQL](../sql-database/sql-database-aad-authentication-configure.md).

    ```azurecli
    az sql server ad-admin list --ids $(az graph query -q 'resources | where type == "microsoft.sql/servers" | project id' -o tsv | cut -f1)
    ```

### <a name="list-acls"></a>Auflisten von ACLs

1. Wenn Sie Azure Data Lake Storage Gen1 verwenden, können Sie die ACLs auflisten, die auf eine beliebige Datei angewendet werden, indem Sie das Azure-Portal oder PowerShell verwenden.

1. Wenn Sie Azure Data Lake Storage Gen2 verwenden, können Sie die ACLs auflisten, die auf eine beliebige Datei angewendet werden, indem Sie das Azure-Portal oder PowerShell verwenden.

1. Wenn Sie Azure Files verwenden, listen Sie die ACLs auf, die auf eine beliebige Datei angewendet werden.

### <a name="list-other-known-resources"></a>Auflisten anderer bekannter Ressourcen

1. Verwenden Sie [az account show](https://docs.microsoft.com/cli/azure/account#az-account-show), um Ihre Abonnement-ID abzurufen.

    ```azurecli
    subscriptionId=$(az account show --query id | sed -e 's/^"//' -e 's/"$//')
    ```

1. Verwenden Sie die Erweiterung [az graph-](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph), um andere Azure-Ressourcen mit bekannten Abhängigkeiten vom Azure AD-Verzeichnis aufzulisten.

    ```azurecli
    az graph query -q \
    'resources | where type != "microsoft.azureactivedirectory/b2cdirectories" | where  identity <> "" or properties.tenantId <> "" or properties.encryptionSettingsCollection.enabled == true | project name, type, kind, identity, tenantId, properties.tenantId' \
    --subscriptions $subscriptionId --output table
    ```

## <a name="step-2-transfer-billing-ownership"></a>Schritt 2: Übertragen des Abrechnungsbesitzes

In diesem Schritt übertragen Sie den Abrechnungsbesitz des Abonnements aus dem Quellverzeichnis an das Zielverzeichnis.

> [!WARNING]
> Wenn Sie den Abrechnungsbesitz des Abonnements übertragen, werden alle Rollenzuweisungen im Quellverzeichnis **dauerhaft** gelöscht und können nicht wiederhergestellt werden. Sie können den Vorgang nicht mehr rückgängig machen, nachdem Sie den Abrechnungsbesitz des Abonnements übertragen haben. Stellen Sie sicher, dass Sie die vorherigen Schritte ausgeführt haben, bevor Sie diesen Schritt ausführen.

1. Führen Sie die Schritte unter [Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto](../cost-management-billing/manage/billing-subscription-transfer.md) aus. Wenn Sie das Abonnement in ein anderes Azure AD Verzeichnis übertragen möchten, müssen Sie das Kontrollkästchen **Abonnement Azure AD-Mandant** aktivieren.

1. Nachdem Sie die Übertragung des Besitzes abgeschlossen haben, kehren Sie zu diesem Artikel zurück, um die Ressourcen im Zielverzeichnis erneut zu erstellen.

## <a name="step-3-re-create-resources"></a>Schritt 3: Erneutes Erstellen der Ressourcen

### <a name="sign-in-to-target-directory"></a>Anmelden beim Zielverzeichnis

1. Melden Sie sich im Zielverzeichnis als der Benutzer an, der die Übertragungsanforderung angenommen hat.

    Nur der Benutzer des neuen Kontos, der die Übertragungsanforderung angenommen hat, besitzt Zugriff auf die Verwaltung der Ressourcen.

1. Rufen Sie eine Liste Ihrer Abonnements mithilfe des Befehls [az account list](https://docs.microsoft.com/cli/azure/account#az-account-list) ab.

    ```azurecli
    az account list --output table
    ```

1. Verwenden Sie [az account set](https://docs.microsoft.com/cli/azure/account#az-account-set), um das aktive Abonnement festzulegen, das Sie verwenden möchten.

    ```azurecli
    az account set --subscription "Contoso"
    ```

### <a name="create-custom-roles"></a>Erstellen von benutzerdefinierten Rollen
        
- Verwenden Sie [az role definition create](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-create), um jede benutzerdefinierte Rolle aus den zuvor erstellten Dateien zu erstellen. Weitere Informationen finden Sie unter [Erstellen oder Aktualisieren von benutzerdefinierten Rollen für Azure-Ressourcen mithilfe der Azure CLI](custom-roles-cli.md).

    ```azurecli
    az role definition create --role-definition <role_definition>
    ```

### <a name="create-role-assignments"></a>Erstellen von Rollenzuweisung

- Verwenden Sie [az role assignment create](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create), um die Rollenzuweisungen für Benutzer, Gruppen und Dienstprinzipale zu erstellen. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der Azure CLI](role-assignments-cli.md).

    ```azurecli
    az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
    ```

### <a name="update-system-assigned-managed-identities"></a>Aktualisieren von systemseitig zugewiesenen verwalteten Identitäten

1. Deaktivieren Sie systemseitig zugewiesene verwaltete Identitäten, und aktivieren Sie sie dann erneut.

    | Azure-Dienst | Weitere Informationen | 
    | --- | --- |
    | Virtuelle Computer | [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe der Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#system-assigned-managed-identity) |
    | VM-Skalierungsgruppen | [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen in einer VM-Skalierungsgruppe mit der Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#system-assigned-managed-identity) |
    | Sonstige Dienste | [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) |

1. Verwenden Sie [az role assignment create](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create), um die Rollenzuweisungen für systemseitig zugewiesene verwaltete Identitäten zu erstellen. Weitere Informationen finden Sie unter [Zuweisen des Zugriffs einer verwalteten Identität auf eine Ressource mit der Azure CLI](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-user-assigned-managed-identities"></a>Aktualisieren benutzerseitig zugewiesener verwalteter Identitäten

1. Löschen Sie benutzerseitig zugewiesene verwaltete Identitäten, erstellen Sie sie erneut, und fügen Sie sie an.

    | Azure-Dienst | Weitere Informationen | 
    | --- | --- |
    | Virtuelle Computer | [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe der Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity) |
    | VM-Skalierungsgruppen | [Konfigurieren von verwalteten Identitäten für Azure-Ressourcen in einer VM-Skalierungsgruppe mit der Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#user-assigned-managed-identity) |
    | Sonstige Dienste | [Dienste, die verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)<br/>[Erstellen, Auflisten oder Löschen einer vom Benutzer zugewiesenen verwalteten Identität mithilfe der Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) |

1. Verwenden Sie [az role assignment create](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create), um die Rollenzuweisungen für benutzerseitig zugewiesene verwaltete Identitäten zu erstellen. Weitere Informationen finden Sie unter [Zuweisen des Zugriffs einer verwalteten Identität auf eine Ressource mit der Azure CLI](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-key-vaults"></a>Aktualisieren von Schlüsseltresoren

In diesem Abschnitt werden die grundlegenden Schritte zum Aktualisieren von Schlüsseltresoren beschrieben. Weitere Informationen finden Sie unter [Verschieben einer Azure Key Vault-Instanz in ein anderes Abonnement](../key-vault/general/keyvault-move-subscription.md).

1. Aktualisieren Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren im Abonnement zugeordnet ist, in das Zielverzeichnis.

1. Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.

1. Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die dem Zielverzeichnis zugeordnet sind.

### <a name="update-acls"></a>Aktualisieren von ACLs

1. Wenn Sie Azure Data Lake Storage Gen1 verwenden, weisen Sie die entsprechenden ACLs zu. Weitere Informationen finden Sie unter [Schützen von Daten, die in Azure Data Lake Storage Gen1 gespeichert sind](../data-lake-store/data-lake-store-secure-data.md).

1. Wenn Sie Azure Data Lake Storage Gen2 verwenden, weisen Sie die entsprechenden ACLs zu. Weitere Informationen finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

1. Wenn Sie Azure Files verwenden, weisen Sie die entsprechenden ACLs zu.

### <a name="review-other-security-methods"></a>Überprüfen anderer Sicherheitsmethoden

Auch wenn Rollenzuweisungen während der Übertragung entfernt werden, haben Benutzer im ursprünglichen Besitzerkonto möglicherweise über andere Sicherheitsmethoden weiterhin Zugriff auf das Abonnement. Dazu zählen:

- Tastenkombinationen für Dienste wie Storage.
- [Verwaltungszertifikate](../cloud-services/cloud-services-certs-create.md), die dem Benutzer Administratorzzugriff für Abonnementressourcen gewähren.
- Anmeldeinformationen für den Remotezugriff für Dienste wie Azure Virtual Machines.

Wenn Sie beabsichtigen, Benutzern im Quellverzeichnis den Zugriff zu entziehen, sodass sie im Zielverzeichnis keinen Zugriff mehr haben, sollten Sie erwägen, alle Anmeldeinformationen zu rotieren. Bis die Anmeldeinformationen aktualisiert wurden, besitzen Benutzer nach der Übertragung weiterhin Zugriff.

1. Rotieren Sie die Speicherkonto-Zugriffsschlüssel. Weitere Informationen finden Sie unter [Verwalten von Speicherkonto-Zugriffsschlüsseln](../storage/common/storage-account-keys-manage.md).

1. Wenn Sie Zugriffsschlüssel für andere Dienste (z. B. Azure SQL-Datenbank oder Azure Service Bus-Messaging) verwenden, rotieren Sie die Zugriffsschlüssel.

1. Für Ressourcen, die Geheimnisse verwenden, öffnen Sie die Einstellungen für die Ressource, und aktualisieren Sie das Geheimnis.

1. Für Ressourcen, die Zertifikate verwenden, aktualisieren Sie das Zertifikat.

## <a name="next-steps"></a>Nächste Schritte

- [Übertragen des Abrechnungsbesitzes eines Azure-Abonnements an ein anderes Konto](../cost-management-billing/manage/billing-subscription-transfer.md)
- [Übertragen von Azure-Abonnements zwischen Abonnenten und CSPs](../cost-management-billing/manage/transfer-subscriptions-subscribers-csp.md)
- [Zuordnen oder Hinzufügen eines Azure-Abonnements zu Ihrem Azure Active Directory-Mandanten](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
