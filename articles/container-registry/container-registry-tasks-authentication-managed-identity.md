---
title: Verwaltete Identität in ACR Tasks
description: Aktivieren Sie eine verwaltete Identität für Azure-Ressourcen in einem Azure Container Registry-Task (ACR-Task), damit der Task auf andere Azure-Ressourcen, einschließlich anderer privater Containerregistrierungen, zugreifen kann.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 01/14/2020
ms.author: danlep
ms.openlocfilehash: 8f2749a18a5ac6aed0822553d59beaacc9060228
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98915946"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Verwenden einer verwalteten Azure-Identität in ACR Tasks 

Aktivieren Sie eine [verwaltete Identität für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) in einem [ACR-Task](container-registry-tasks-overview.md), damit der Task auf andere Azure-Ressourcen zugreifen kann, ohne Anmeldeinformationen bereitstellen oder verwalten zu müssen. Verwenden Sie beispielsweise eine verwaltete Identität, damit es möglich wird, Containerimages mit einem Taskschritt in eine andere Registrierung zu pullen oder zu pushen.

In diesem Artikel erfahren Sie, wie Sie über die Azure-Befehlszeilenschnittstelle (Azure CLI) eine benutzerseitig oder vom System zugewiesene verwaltete Identität für einen ACR-Task aktivieren. Dazu können Sie die Azure Cloud Shell oder eine lokale Installation der Azure CLI verwenden. Wenn Sie es lokal verwenden möchten, ist die Version 2.0.68 oder höher erforderlich. Führen Sie `az --version` aus, um die Version zu ermitteln. Informationen zum Durchführen einer Installation oder eines Upgrades finden Sie bei Bedarf unter [Installieren der Azure CLI][azure-cli-install].

Zur Veranschaulichung verwenden die in diesem Artikel verwendeten Beispielbefehle [az acr task create][az-acr-task-create], um einen grundlegenden Imageerstellungstask zu erstellen, der eine verwaltete Identität aktiviert. Beispielszenarien für den Zugriff auf gesicherte Ressourcen aus einem ACR-Task mithilfe einer verwalteten Identität finden Sie unter:

* [Registrierungsübergreifende Authentifizierung](container-registry-tasks-cross-registry-authentication.md)
* [Zugreifen auf externe Ressourcen mit in Azure Key Vault gespeicherten Geheimnissen ](container-registry-tasks-authentication-key-vault.md)

## <a name="why-use-a-managed-identity"></a>Gründe für die Verwendung einer verwalteten Identität

Über eine verwaltete Identität für Azure-Ressourcen wird für ausgewählte Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereitgestellt. Sie können einen ACR-Task mit einer verwalteten Identität konfigurieren, sodass der Task auf andere gesicherte Azure-Ressourcen zugreifen kann, ohne Anmeldeinformationen in den Taskschritten zu übergeben.

Es gibt zwei Arten von verwalteten Identitäten:

* *Benutzerseitig zugewiesene Identitäten* können mehreren Ressourcen zugewiesen werden und werden für die gewünschte Dauer beibehalten. Benutzerseitig zugewiesene Identitäten befinden sich zurzeit in der Vorschau.

* Vom *System zugewiesene Identitäten* sind für eine spezifische Ressource wie einen ACR-Task eindeutig und für die Lebensdauer dieser Ressource gültig.

Sie können in einem ACR-Task entweder einen Identitätstyp oder beide Typen aktivieren. Gewähren Sie der Identität Zugriff auf eine andere Ressource, so wie jeder beliebige Sicherheitsprinzipal. Wenn der Task ausgeführt wird, verwendet er die Identität für den Zugriff auf die Ressource in allen Taskschritten, die Zugriff erfordern.

## <a name="steps-to-use-a-managed-identity"></a>Schritte für die Verwendung einer verwalteten Identität

Führen Sie die folgenden allgemeinen Schritte aus, um eine verwaltete Identität bei einem ACR-Task zu verwenden.

### <a name="1-optional-create-a-user-assigned-identity"></a>1. (Optional) Erstellen einer benutzerseitig zugewiesenen Identität

Wenn Sie beabsichtigen, eine vom Benutzer zugewiesene Identität zu verwenden, verwenden Sie eine vorhandene Identität, oder erstellen Sie die Identität mithilfe der Azure CLI oder anderer Azure-Tools. Verwenden Sie beispielsweise den Befehl [az identity create][az-identity-create]. 

Wenn Sie nur eine vom System zugewiesene Identität verwenden möchten, überspringen Sie diesen Schritt. Sie erstellen eine vom System zugewiesene Identität, wenn Sie den ACR-Task erstellen.

### <a name="2-enable-identity-on-an-acr-task"></a>2. Aktivieren der Identität für einen ACR-Task

Wenn Sie einen ACR-Task erstellen, aktivieren Sie optional eine benutzerseitig zugewiesene Identität, eine vom System zugewiesene Identität oder beide Identitäten. Übergeben Sie beispielsweise den Parameter `--assign-identity`, wenn Sie den Befehl [az acr task create][az-acr-task-create] in der Azure CLI ausführen.

Wenn Sie eine vom System zugewiesene Identität aktivieren möchten, übergeben Sie `--assign-identity` (ohne einen Wert) oder `assign-identity [system]`. Der folgende Beispielbefehl erstellt einen Linux-Task aus einem öffentlichen GitHub-Repository, der das `hello-world`-Image erstellt und eine vom System zugewiesene verwaltete Identität aktiviert:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git#main \
    --file Dockerfile \
    --commit-trigger-enabled false \
    --assign-identity
```

Wenn Sie eine benutzerseitig zugewiesene Identität aktivieren möchten, übergeben Sie `--assign-identity` mit einem Wert der *Ressourcen-ID* der Identität. Der folgende Beispielbefehl erstellt einen Linux-Task aus einem öffentlichen GitHub-Repository, der das `hello-world`-Image erstellt und eine vom Benutzer zugewiesene verwaltete Identität aktiviert:

```azurecli
az acr task create \
    --image hello-world:{{.Run.ID}} \
    --name hello-world --registry MyRegistry \
    --context https://github.com/Azure-Samples/acr-build-helloworld-node.git#main \
    --file Dockerfile \
    --commit-trigger-enabled false
    --assign-identity <resourceID>
```

Sie können die Ressourcen-ID der Identität durch Ausführen des Befehls [az identity show][az-identity-show] abrufen. Die Ressourcen-ID für die ID *myUserAssignedIdentity* in der Ressourcengruppe *myResourceGroup* hat das folgende Format: 

```
"/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myUserAssignedIdentity"
```

### <a name="3-grant-the-identity-permissions-to-access-other-azure-resources"></a>3. Erteilen der Identitätsberechtigungen für den Zugriff auf andere Azure-Ressourcen

Gewähren Sie den Identitätsberechtigungen – je nach den Anforderungen ihres Tasks – Zugriff auf andere Azure-Ressourcen. Beispiele:

* Weisen Sie der verwalteten Identität eine Rolle mit Pullberechtigungen, Push- und Pullberechtigungen oder anderen Berechtigungen für eine Zielcontainerregistrierung in Azure zu. Eine vollständige Liste der Registrierungsrollen finden Sie unter [Azure Container Registry – Rollen und Berechtigungen](container-registry-roles.md). 
* Weisen Sie der verwalteten Identität eine Rolle zu, um Geheimnisse in einem Azure-Schlüsseltresor zu lesen.

Verwenden Sie die [Azure CLI](../role-based-access-control/role-assignments-cli.md) oder andere Azure-Tools, um den rollenbasierten Zugriff auf Ressourcen zu verwalten. Verwenden Sie beispielsweise den Befehl [az role assignment create][az-role-assignment-create], um der Identität eine Rolle zur Ressource zuzuweisen. 

Im folgenden Beispiel werden einer verwalteten Identität die Berechtigungen zum Pullen aus einer Containerregistrierung zugewiesen. Der Befehl gibt die *Prinzipal-ID* der Aufgabenidentität und die *Ressourcen-ID* der Zielregistrierung an.


```azurecli
az role assignment create \
  --assignee <principalID> \
  --scope <registryID> \
  --role acrpull
```

### <a name="4-optional-add-credentials-to-the-task"></a>4. (Optional) Hinzufügen von Anmeldeinformationen zum Task

Wenn Ihr Task Anmeldeinformationen benötigt, um Images per Pull oder Push in eine andere benutzerdefinierte Registrierung zu übertragen oder um auf andere Ressourcen zuzugreifen, fügen Sie dem Task Anmeldeinformationen hinzu. Führen Sie den Befehl [az acr task credential add][az-acr-task-credential-add] aus, um Anmeldeinformationen hinzuzufügen, und übergeben Sie den Parameter `--use-identity`, um anzugeben, dass die Identität auf die Anmeldeinformationen zugreifen kann. 

Wenn Sie beispielsweise Anmeldeinformationen für eine vom System zugewiesene Identität hinzufügen möchten, um eine Authentifizierung mit der Azure Container Registry *targetregistry* durchzuführen, übergeben Sie `use-identity [system]`:

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity [system]
```

Wenn Sie Anmeldeinformationen für eine benutzerseitig zugewiesene Identität hinzufügen möchten, um eine Authentifizierung mit der Registrierung *targetregistry* durchzuführen, übergeben Sie `use-identity` mit dem Wert *client ID* der Identität. Zum Beispiel:

```azurecli
az acr task credential add \
    --name helloworld \
    --registry myregistry \
    --login-server targetregistry.azurecr.io \
    --use-identity <clientID>
```

Sie können die Client-ID der Identität abrufen, indem Sie den Befehl [az identity show][az-identity-show] ausführen. Die Client-ID ist eine GUID mit den Format `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.

### <a name="5-run-the-task"></a>5. Ausführen des Tasks

Nachdem Sie einen Task mit einer verwalteten Identität konfiguriert haben, führen Sie den Task aus. Wenn Sie z. B. einen der in diesem Artikel erstellten Tasks testen möchten, lösen Sie ihn mit dem Befehl [az acr task run][az-acr-task-run] manuell aus. Wenn Sie zusätzliche automatisierte Tasktrigger konfiguriert haben, wird der Task bei automatischer Auslösung ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie eine benutzerseitig oder vom System zugewiesene verwaltete Identität für einen ACR-Task aktivieren und verwenden. Szenarien für den Zugriff auf gesicherte Ressourcen aus einem ACR-Task mithilfe einer verwalteten Identität finden Sie unter:

* [Registrierungsübergreifende Authentifizierung](container-registry-tasks-cross-registry-authentication.md)
* [Zugreifen auf externe Ressourcen mit in Azure Key Vault gespeicherten Geheimnissen ](container-registry-tasks-authentication-key-vault.md)


<!-- LINKS - Internal -->
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[azure-cli-install]: /cli/azure/install-azure-cli
