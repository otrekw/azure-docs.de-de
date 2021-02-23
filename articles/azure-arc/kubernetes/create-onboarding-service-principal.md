---
title: Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 02/09/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: 'Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals '
keywords: Kubernetes, Arc, Azure, Container
ms.openlocfilehash: 8772cf7634d9a833af120784e3e7868b41d202c4
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100390486"
---
# <a name="create-an-azure-arc-enabled-onboarding-service-principal-preview"></a>Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals (Vorschauversion)

## <a name="overview"></a>Übersicht

Sie können Kubernetes-Cluster mithilfe von Dienstprinzipalen mit Rollenzuweisungen mit eingeschränkten Berechtigungen in Azure Arc integrieren. Diese Funktion ist für CI/CD-Pipelines (Continuous Integration/Continuous Deployment) wie Azure Pipelines und GitHub Actions nützlich.

In den folgenden Schritten lernen Sie, Dienstprinzipale für das Onboarding von Kubernetes-Clustern in Azure Arc zu verwenden.

## <a name="create-a-new-service-principal"></a>Erstellen eines neuen Dienstprinzipals

Erstellen Sie einen neuen Dienstprinzipal mit einem aussagekräftigen Namen, der für Ihren Azure Active Directory-Mandanten eindeutig ist.

```console
az ad sp create-for-RBAC --skip-assignment --name "https://azure-arc-for-k8s-onboarding"
```

**Ausgabe:**

```console
{
  "appId": "22cc2695-54b9-49c1-9a73-2269592103d8",
  "displayName": "azure-arc-for-k8s-onboarding",
  "name": "https://azure-arc-for-k8s-onboarding",
  "password": "09d3a928-b223-4dfe-80e8-fed13baa3b3d",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

## <a name="assign-permissions"></a>Zuweisen von Berechtigungen

Weisen Sie dem neu erstellten Dienstprinzipal die Rolle „Kubernetes-Cluster – Azure Arc-Onboarding“ zu. Diese integrierte Azure-Rolle mit eingeschränkten Berechtigungen gestattet dem Prinzipal ausschließlich das Registrieren von Clustern in Azure. Der Prinzipal kann mit dieser zugewiesenen Rolle keine anderen Cluster oder Ressourcen im Abonnement aktualisieren, löschen oder ändern.

Aufgrund der begrenzten Möglichkeiten können Kunden diesen Prinzipal problemlos wiederverwenden, um mehrere Cluster zu integrieren.

Sie können Berechtigungen weiter einschränken, indem Sie beim Zuweisen der Rolle das entsprechende `--scope`-Argument übergeben. Dies ermöglicht es Kunden, die Clusterregistrierung einzuschränken. Die folgenden Szenarien werden durch verschiedene `--scope`-Parameter unterstützt:

| Resource  | `scope`-Argument| Wirkung |
| ------------- | ------------- | ------------- |
| Subscription | `--scope /subscriptions/0b1f6471-1bf0-4dda-aec3-111122223333` | Der Dienstprinzipal kann jeden Cluster in einer vorhandenen Ressourcengruppe im angegebenen Abonnement registrieren. |
| Ressourcengruppe | `--scope /subscriptions/0b1f6471-1bf0-4dda-aec3-111122223333/resourceGroups/myGroup`  | Der Dienstprinzipal kann __ausschließlich__ Cluster in der Ressourcengruppe `myGroup` registrieren. |

```console
az role assignment create \
    --role 34e09817-6cbe-4d01-b1a2-e0eac5743d41 \      # this is the id for the built-in role
    --assignee 22cc2695-54b9-49c1-9a73-2269592103d8 \  # use the appId from the new SP
    --scope /subscriptions/<<SUBSCRIPTION_ID>>         # apply the appropriate scope
```

**Ausgabe:**

```console
{
  "canDelegate": null,
  "id": "/subscriptions/<<SUBSCRIPTION_ID>>/providers/Microsoft.Authorization/roleAssignments/fbd819a9-01e8-486b-9eb9-f177ba400ba6",
  "name": "fbd819a9-01e8-486b-9eb9-f177ba400ba6",
  "principalId": "ddb0ddb4-ba84-4cde-b936-affc272a4b90",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<<SUBSCRIPTION_ID>>/providers/Microsoft.Authorization/roleDefinitions/34e09817-6cbe-4d01-b1a2-e0eac5743d41",
  "scope": "/subscriptions/<<SUBSCRIPTION_ID>>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="use-service-principal-with-the-azure-cli"></a>Verwenden eines Dienstprinzipals mit der Azure-Befehlszeilenschnittstelle

Verweisen Sie mit den folgenden Befehlen auf den neu erstellten Dienstprinzipal:

```azurecli
az login --service-principal -u mySpnClientId -p mySpnClientSecret --tenant myTenantID
az connectedk8s connect -n myConnectedClusterName -g myResoureGroupName
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)
