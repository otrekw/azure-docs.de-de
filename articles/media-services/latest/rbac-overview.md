---
title: Rollenbasierte Zugriffssteuerung für Media Services-Konten
description: In diesem Artikel wird die rollenbasierte Zugriffssteuerung (RBAC) in Azure für Azure Media Services-Konten erläutert.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 7c9edce71e0d0565de80153bfd5b3d5f1edb2e8f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98955155"
---
# <a name="azure-role-based-access-control-azure-rbac-for-media-services-accounts"></a>Rollenbasierte Zugriffssteuerung (RBAC) in Azure für Media Services-Konten

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Derzeit definiert Azure Media Services keine benutzerdefinierten dienstspezifischen Rollen. Um vollen Zugriff auf das Media Services-Konto zu erhalten, können Kunden die integrierten Rollen von **Besitzer** oder **Mitwirkender** verwenden. Der Hauptunterschied zwischen diesen Rollen ist, dass der **Besitzer** im Gegensatz zu einem **Mitwirkenden** steuern kann, wer Zugriff auf eine Ressource hat. Die integrierte Rolle **Leser** kann ebenfalls verwendet werden, aber der Benutzer oder die Anwendung hat nur Lesezugriff auf die Media Services-APIs. 

## <a name="design-principles"></a>Entwurfsprinzipien

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Der Benutzer muss eine separate Aktionsmethode zum Abrufen von Geheimnissen oder Anmeldeinformationen aufrufen. Die **Leser**-Rolle kann keine Vorgänge wie „Asset.ListContainerSas“, „StreamingLocator.ListContentKeys“ oder „ContentKeyPolicies.GetPolicyPropertiesWithSecrets“ aufrufen. Getrennte Aktionen ermöglichen Ihnen, bei Bedarf noch genauer abgestimmte Azure RBAC-Sicherheitsberechtigungen in einer benutzerdefinierten Rolle festzulegen.

So listen Sie die von Media Services unterstützten Vorgänge auf

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

Im Artikel zu den [integrierten Rollendefinitionen](../../role-based-access-control/built-in-roles.md) erfahren Sie genau, was die Rolle gewährt. 

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../../role-based-access-control/rbac-and-directory-admin-roles.md)
- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)
- [Hinzufügen oder Entfernen von Rollenzuweisungen mithilfe von Azure RBAC und der REST-API](../../role-based-access-control/role-assignments-rest.md)
- [Media Services-Ressourcenanbietervorgänge](../../role-based-access-control/resource-provider-operations.md#microsoftmedia)

## <a name="next-steps"></a>Nächste Schritte

- [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md)
- [Abrufen von Richtlinien für symmetrische Schlüssel mithilfe von Media Services .NET](get-content-key-policy-dotnet-howto.md)
