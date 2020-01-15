---
title: Bereitstellen einer Service Fabric-App mit systemseitig zugewiesener verwalteter Identität
description: In diesem Artikel erfahren Sie, wie Sie einer Azure Service Fabric-Anwendung eine systemseitig zugewiesene verwaltete Identität zuweisen.
ms.topic: article
ms.date: 07/25/2019
ms.openlocfilehash: d5a14722363d642957904f9c7c699d3cf1d66c0f
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614824"
---
# <a name="deploy-service-fabric-application-with-system-assigned-managed-identity-preview"></a>Bereitstellen einer Service Fabric-Anwendung mit einer systemseitig zugewiesenen verwalteten Identität (Vorschau)

Um auf das Feature für verwaltete Identitäten für Azure Service Fabric-Anwendungen zugreifen zu können, müssen Sie zunächst den Tokendienst für verwaltete Identitäten im Cluster aktivieren. Dieser Dienst ist für die Authentifizierung von Service Fabric-Anwendungen anhand ihrer verwalteten Identitäten und für den Abruf von Zugriffstoken in deren Auftrag zuständig. Nachdem der Dienst aktiviert wurde, wird er in Service Fabric Explorer im Abschnitt **System** im linken Bereich unter dem Namen **fabric:/System/ManagedIdentityTokenService** neben anderen Systemdiensten angezeigt.

> [!NOTE] 
> Die Bereitstellung von Service Fabric-Anwendungen mit verwalteten Identitäten wird ab der API-Version `"2019-06-01-preview"` unterstützt. Sie können auch die gleiche API-Version für den Anwendungstyp, die Anwendungstypversion und die Dienstressourcen verwenden. Die unterstützte Mindestversion der Service Fabric-Runtime ist 6.5 CU2. Zusätzlich sollte die Build-/Paketumgebung auch über das SF .NET SDK der Version Cu2 oder höher verfügen.

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

### <a name="application-template"></a>Anwendungsvorlage

Wenn Sie eine Anwendung mit einer systemseitig zugewiesenen verwalteten Identität aktivieren möchten, fügen Sie der Anwendungsressource die Eigenschaft **identity** mit dem Typ **systemAssigned** hinzu, wie im folgenden Beispiel zu sehen:

```json
    {
      "apiVersion": "2019-06-01-preview",
      "type": "Microsoft.ServiceFabric/clusters/applications",
      "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
      ],
      "identity": {
        "type" : "systemAssigned"
      },
      "properties": {
        "typeName": "[parameters('applicationTypeName')]",
        "typeVersion": "[parameters('applicationTypeVersion')]",
        "parameters": {
        }
      }
    }
```
Diese Eigenschaft deklariert (gegenüber Azure Resource Manager sowie gegenüber dem Anbieter der verwalteten Identität bzw. dem Service Fabric-Ressourcenanbieter), dass diese Ressource über eine implizite verwaltete Identität (vom Typ `system assigned`) verfügen soll.

### <a name="application-and-service-package"></a>Anwendungs- und Dienstpaket

1. Aktualisieren Sie das Anwendungsmanifest, um im Abschnitt **Principals** ein Element vom Typ **ManagedIdentity** mit einem einzelnen Eintrag hinzuzufügen, wie im Anschluss zu sehen:

    **ApplicationManifest.xml**

    ```xml
    <Principals>
      <ManagedIdentities>
        <ManagedIdentity Name="SystemAssigned" />
      </ManagedIdentities>
    </Principals>
    ```
    Dadurch wird die der Anwendung zugewiesene Identität als Ressource einem Anzeigenamen zugeordnet, um die weitere Zuweisung zu den Diensten zu ermöglichen, aus denen sich die Anwendung zusammensetzt. 

2. Fügen Sie im Abschnitt **ServiceManifestImport** für den Dienst, dem die verwaltete Identität zugewiesen wird, ein Element vom Typ **IdentityBindingPolicy** hinzu, wie hier gezeigt:

    **ApplicationManifest.xml**

      ```xml
        <ServiceManifestImport>
          <Policies>
            <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="SystemAssigned" />
          </Policies>
        </ServiceManifestImport>
      ```

    Dieses Element weist die Identität der Anwendung dem Dienst zu. Ohne diese Zuweisung kann der Dienst nicht auf die Identität der Anwendung zugreifen. Im obigen Codeausschnitt wird die Identität `SystemAssigned` (reserviertes Schlüsselwort) der Definition des Diensts unter dem Anzeigenamen `WebAdmin` zugeordnet.

3. Aktualisieren Sie das Dienstmanifest, um im Abschnitt **Resources** ein Element vom Typ **ManagedIdentity** hinzuzufügen, dessen Name dem Wert der Einstellung `ServiceIdentityRef` aus der Definition `IdentityBindingPolicy` im Anwendungsmanifest entspricht:

    **ServiceManifest.xml**

    ```xml
      <Resources>
        ...
        <ManagedIdentities DefaultIdentity="WebAdmin">
          <ManagedIdentity Name="WebAdmin" />
        </ManagedIdentities>
      </Resources>
    ```
    Dies ist die gleiche Zuordnung einer Identität zu einem Dienst, die oben beschrieben wurde, diesmal allerdings aus der Perspektive der Dienstdefinition. Auf die Identität wird hier anhand ihres im Anwendungsmanifest deklarierten Anzeigenamens (`WebAdmin`) verwiesen.

## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich mit der [Unterstützung der verwalteten Identität](./concepts-managed-identity.md) in Azure Service Fabric vertraut.
* [Bereitstellen eines neuen](./configure-new-azure-service-fabric-enable-managed-identity.md) Azure Service Fabric-Clusters mit Unterstützung verwalteter Identitäten 
* [Aktivieren von verwalteten Identitäten](./configure-existing-cluster-enable-managed-identity-token-service.md) in einem vorhandenen Azure Service Fabric-Cluster
* Nutzen der [verwalteten Identität einer Service Fabric-Anwendung aus dem Quellcode](./how-to-managed-identity-service-fabric-app-code.md)
* [Bereitstellen einer Azure Service Fabric-Anwendung mit einer benutzerseitig zugewiesenen verwalteten Identität](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Gewähren des Zugriffs auf andere Azure-Ressourcen für eine Azure Service Fabric-Anwendung](./how-to-grant-access-other-resources.md)
