---
title: Bereich für Erweiterungsressourcentypen (Bicep)
description: Hier wird die Verwendung der Bereichseigenschaft beim Bereitstellen von Erweiterungsressourcentypen mit Bicep erläutert.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 59b6576dfb1bd5e0ac4f56e6a59b6ea4d5c4b5f4
ms.sourcegitcommit: 7f59e3b79a12395d37d569c250285a15df7a1077
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2021
ms.locfileid: "111026466"
---
# <a name="set-scope-for-extension-resources-in-bicep"></a>Festlegen des Bereichs für Erweiterungsressourcen in Bicep

Eine Erweiterungsressource ist eine Ressource, die eine andere Ressource ändert. Sie können beispielsweise einer Ressource eine Rolle zuweisen. Die Rollenzuweisung ist ein Erweiterungsressourcentyp.

Eine vollständige Liste der Erweiterungsressourcentypen finden Sie unter [Ressourcentypen, die Funktionen anderer Ressourcen erweitern](../management/extension-resource-types.md).

In diesem Artikel wird gezeigt, wie Sie den Bereich für einen Erweiterungsressourcentyp festlegen, wenn dieser mit einer Bicep-Datei bereitgestellt wird. Außerdem wird die Bereichseigenschaft beschrieben, die für Erweiterungsressourcen beim Anwenden auf eine Ressource verfügbar ist.

> [!NOTE]
> Die Bereichseigenschaft ist nur für Erweiterungsressourcentypen verfügbar. Wenn Sie einen anderen Bereich für einen Ressourcentyp angeben möchten, der kein Erweiterungstyp ist, verwenden Sie ein [Modul](modules.md).

## <a name="apply-at-deployment-scope"></a>Anwenden im Bereitstellungsumfang

Fügen Sie die Ressource wie bei jedem anderen Ressourcentyp zu Ihrer Vorlage hinzu, um einen Erweiterungsressourcentyp im Zielbereitstellungsumfang anzuwenden. Die verfügbaren Bereiche sind [Ressourcengruppe](deploy-to-resource-group.md), [Abonnement](deploy-to-subscription.md), [Verwaltungsgruppe](deploy-to-management-group.md) und [Mandant](deploy-to-tenant.md). Der Bereitstellungsbereich muss den Ressourcentyp unterstützen.

Mit der folgenden Vorlage wird eine Sperre bereitgestellt.

```bicep
resource createRgLock 'Microsoft.Authorization/locks@2016-09-01' = {
  name: 'rgLock'
  properties: {
    level: 'CanNotDelete'
    notes: 'Resource group should not be deleted.'
  }
}
```

Im nächsten Beispiel wird eine Rolle zugewiesen.

```bicep
targetScope = 'subscription'

@description('The principal to assign the role to')
param principalId string

@allowed([
  'Owner'
  'Contributor'
  'Reader'
])
@description('Built-in role to assign')
param builtInRoleType string

@description('A new GUID used to identify the role assignment')
param roleNameGuid string = newGuid()

var role = {
  Owner: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635'
  Contributor: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c'
  Reader: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7'
}

resource roleAssignSub 'Microsoft.Authorization/roleAssignments@2020-04-01-preview' = {
  name: roleNameGuid
  properties: {
    roleDefinitionId: role[builtInRoleType]
    principalId: principalId
  }
}
```

## <a name="apply-to-resource"></a>Anwenden auf die Ressource

Verwenden Sie die `scope`-Eigenschaft, um eine Erweiterungsressource auf eine Ressource anzuwenden. Legen Sie die Bereichseigenschaft auf den Namen der Ressource fest, der Sie die Erweiterung hinzufügen möchten. Die Bereichseigenschaft ist eine Stammeigenschaft für den Erweiterungsressourcentyp.

Im folgenden Beispiel wird ein Speicherkonto erstellt und eine Rolle darauf angewendet.

```bicep
@description('The principal to assign the role to')
param principalId string

@allowed([
  'Owner'
  'Contributor'
  'Reader'
])
@description('Built-in role to assign')
param builtInRoleType string

@description('A new GUID used to identify the role assignment')
param roleNameGuid string = newGuid()
param location string = resourceGroup().location

var role = {
  Owner: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635'
  Contributor: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c'
  Reader: '/subscriptions/${subscription().subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7'
}
var uniqueStorageName = 'storage${uniqueString(resourceGroup().id)}'

resource storageName 'Microsoft.Storage/storageAccounts@2019-04-01' = {
  name: uniqueStorageName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'Storage'
  properties: {}
}

resource roleAssignStorage 'Microsoft.Authorization/roleAssignments@2020-04-01-preview' = {
  name: roleNameGuid
  properties: {
    roleDefinitionId: role[builtInRoleType]
    principalId: principalId
  }
  scope: storageName
  dependsOn: [
    storageName
  ]
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Bereitstellung in Bereichen finden Sie unter:

* [Bereitstellungen von Ressourcengruppen](deploy-to-resource-group.md)
* [Abonnementbereitstellungen](deploy-to-subscription.md)
* [Bereitstellungen von Verwaltungsgruppen](deploy-to-management-group.md)
* [Mandantenbereitstellungen](deploy-to-tenant.md)