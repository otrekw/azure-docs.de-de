---
title: Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals (Vorschauversion)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: 'Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals '
keywords: Kubernetes, Arc, Azure, Container
ms.openlocfilehash: 3c95c6bb85c7c1bc097b7751a560a658863c0afd
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83725600"
---
# <a name="create-an-azure-arc-enabled-onboarding-service-principal-preview"></a>Erstellen eines Azure Arc-fähigen Onboardingdienstprinzipals (Vorschauversion)

## <a name="overview"></a>Übersicht

Es ist möglich, Dienstprinzipale mit einer Rollenzuweisung mit eingeschränkten Berechtigungen zum Integrieren von Kubernetes-Clustern in Azure Arc zu verwenden. Dies ist für CI/CD-Pipelines (Continuous Integration/Continuous Deployment) wie Azure Pipelines und GitHub Actions nützlich.

Die folgenden Schritte bieten eine exemplarische Vorgehensweise zur Verwendung von Dienstprinzipalen für das Onboarding von Kubernetes-Clustern in Azure Arc.

## <a name="create-a-new-service-principal"></a>Erstellen eines neuen Dienstprinzipals

Erstellen Sie einen neuen Dienstprinzipal mit einem aussagekräftigen Namen. Beachten Sie, dass dieser Name in Ihrem Azure Active Directory-Mandanten eindeutig sein muss:

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

Nachdem Sie den neuen Dienstprinzipal erstellt haben, weisen Sie ihm die Rolle „Azure Arc for Kubernetes Onboarding“ zu. Dabei handelt es sich um eine integrierte Azure-Rolle mit eingeschränkten Berechtigungen, die dem Prinzipal ausschließlich das Registrieren von Clustern in Azure gestattet. Der Prinzipal kann keine anderen Cluster oder Ressourcen im Abonnement aktualisieren, löschen oder ändern.

Aufgrund der begrenzten Möglichkeiten können Kunden diesen Prinzipal problemlos wiederverwenden, um mehrere Cluster zu integrieren.

Die Berechtigungen können weiter eingeschränkt werden, indem beim Zuweisen der Rolle das entsprechende Argument `--scope` übergeben wird. Dies ermöglicht es Kunden, die Clusterregistrierung einzuschränken. Die folgenden Szenarien werden durch verschiedene `--scope`-Parameter unterstützt:

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

Verweisen auf den neu erstellten Dienstprinzipal:

```console
az login --service-principal -u mySpnClientId -p mySpnClientSecret --tenant myTenantID
az connectedk8s connect -n myConnectedClusterName -g myResoureGroupName
```

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden von Azure Policy zum Steuern der Clusterkonfiguration](./use-azure-policy.md)
