---
title: Bedingte Bereitstellung mit Bicep
description: Beschreibt, wie eine Ressource in Bicep bedingt bereitgestellt werden kann.
author: mumian
ms.author: jgao
ms.topic: conceptual
ms.date: 06/01/2021
ms.openlocfilehash: 9636444af81b000443dc72cf6e3fc1d8d6da5093
ms.sourcegitcommit: bd65925eb409d0c516c48494c5b97960949aee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2021
ms.locfileid: "111537092"
---
# <a name="conditional-deployment-in-bicep"></a>Bedingte Bereitstellung in Bicep

Manchmal müssen Sie eine Ressource optional in Bicep bereitstellen. Verwenden Sie das `if`-Schlüsselwort, um anzugeben, ob die Ressource bereitgestellt wird. Der Wert für die Bedingung wird in „true“ oder „false“ aufgelöst. Wenn der Wert TRUE ist, wird die Ressource erstellt. Wenn der Wert FALSE ist, wird die Ressource nicht erstellt. Der Wert kann nur auf die gesamte Ressource angewandt werden.

> [!NOTE]
> Die bedingte Bereitstellung wird nicht an [untergeordnete Ressourcen](child-resource-name-type.md) weitergegeben. Wenn Sie eine Ressource und ihre untergeordneten Ressourcen bedingt bereitstellen möchten, müssen Sie dieselbe Bedingung auf jeden Ressourcentyp anwenden.

## <a name="deploy-condition"></a>Bereitstellungsbedingung

Sie können einen Parameterwert übergeben, der angibt, ob eine Ressource bereitgestellt wird. Das folgende Beispiel stellt bedingt eine DNS-Zone bereit:

```bicep
param deployZone bool

resource dnsZone 'Microsoft.Network/dnszones@2018-05-01' = if (deployZone) {
  name: 'myZone'
  location: 'global'
}
```

Bedingungen können mit Abhängigkeitsdeklarationen verwendet werden. Wenn der Bezeichner der bedingten Ressource in `dependsOn` einer anderen Ressource angegeben ist (explizite Abhängigkeit), wird die Abhängigkeit ignoriert, wenn die Bedingung zum Zeitpunkt der Vorlagenbereitstellung als „False“ ausgewertet wird. Wenn die Bedingung als „True“ ausgewertet wird, wird die Abhängigkeit berücksichtigt. Der Verweis auf eine Eigenschaft einer bedingten Ressource (implizite Abhängigkeit) ist zulässig, kann jedoch in einigen Fällen zu einem Laufzeitfehler führen.

## <a name="new-or-existing-resource"></a>Neue oder vorhandene Ressource

Sie können bedingte Bereitstellung verwenden, um eine neue Ressource zu erstellen oder eine vorhandene zu verwenden. Im folgenden Beispiel wird gezeigt, wie ein neues Speicherkonto bereitgestellt oder ein vorhandenes Speicherkonto verwendet wird.

```bicep
param storageAccountName string
param location string = resourceGroup().location

@allowed([
  'new'
  'existing'
])
param newOrExisting string = 'new'

resource sa 'Microsoft.Storage/storageAccounts@2019-06-01' = if (newOrExisting == 'new') {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
    tier: 'Standard'
  }
  kind: 'StorageV2'
  properties: {
    accessTier: 'Hot'
  }
}
```

Wenn der Parameter `newOrExisting` auf **new** festgelegt ist, wird die Bedingung zu „true“ ausgewertet. Das Speicherkonto wird bereitgestellt. Ist `newOrExisting` dagegen auf **existing** festgelegt, wird die Bedingung zu „false“ ausgewertet, und das Speicherkonto wird nicht bereitgestellt.

Eine vollständige Beispielvorlage, die das `condition`-Element verwendet, finden Sie unter [VM mit einem neuen oder vorhandenen virtuellen Netzwerk, Speicher und einer neuen oder vorhandenen öffentlichen IP-Adresse](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/vm-new-or-existing-conditions).

## <a name="runtime-functions"></a>Laufzeitfunktionen

Bei Verwendung einer Funktion vom Typ [reference](./bicep-functions-resource.md#reference) oder [list](./bicep-functions-resource.md#list) mit einer Ressource, die bedingt bereitgestellt wird, wird die Funktion auch dann ausgewertet, wenn die Ressource nicht bereitgestellt wird. Es wird eine Fehlermeldung angezeigt, wenn die Funktion auf eine nicht vorhandene Ressource verweist.

Verwenden Sie den Operator [Bedingter Ausdruck ?:](./operators-logical.md#conditional-expression--), um sicherzustellen, dass die Funktion nur für Bedingungen ausgewertet wird, wenn die Ressource bereitgestellt wird. In der folgenden Beispielvorlage wird gezeigt, wie Sie diese Funktion mit Ausdrücken verwenden, die nur bedingt gültig sind.

```bicep
param vmName string
param location string
param logAnalytics string = ''

resource vmName_omsOnboarding 'Microsoft.Compute/virtualMachines/extensions@2017-03-30' = if (!empty(logAnalytics)) {
  name: '${vmName}/omsOnboarding'
  location: location
  properties: {
    publisher: 'Microsoft.EnterpriseCloud.Monitoring'
    type: 'MicrosoftMonitoringAgent'
    typeHandlerVersion: '1.0'
    autoUpgradeMinorVersion: true
    settings: {
      workspaceId: ((!empty(logAnalytics)) ? reference(logAnalytics, '2015-11-01-preview').customerId : json('null'))
    }
    protectedSettings: {
      workspaceKey: ((!empty(logAnalytics)) ? listKeys(logAnalytics, '2015-11-01-preview').primarySharedKey : json('null'))
    }
  }
}

output mgmtStatus string = ((!empty(logAnalytics)) ? 'Enabled monitoring for VM!' : 'Nothing to enable')
```

Sie legen eine [Ressource als abhängig](./resource-declaration.md#set-resource-dependencies) von einer bedingten Ressource fest, wie Sie auch jede andere Ressource festlegen. Wenn eine bedingte Ressource nicht bereitgestellt wurde, entfernt Azure Resource Manager sie automatisch aus den erforderlichen Abhängigkeiten.

## <a name="next-steps"></a>Nächste Schritte

* Ein Microsoft Learn-Modul zu Bedingungen und Schleifen finden Sie unter [Erstellen flexibler Bicep-Vorlagen mithilfe von Bedingungen und Schleifen](/learn/modules/build-flexible-bicep-templates-conditions-loops/).
* Empfehlungen zum Erstellen von Bicep-Dateien finden Sie unter [Bewährte Methoden für Bicep](best-practices.md).
* Informationen, wie mehrere Instanzen einer Ressource erstellt werden, finden Sie unter [Ressourceniteration in Bicep](loop-resources.md).
