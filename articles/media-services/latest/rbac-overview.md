---
title: Rollenbasierte Zugriffssteuerung für Media Services-Konten – Azure | Microsoft-Dokumentation
description: Dieser Artikel beschreibt die rollenbasierte Zugriffssteuerung (RBAC) für Azure Media Services-Konten.
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
ms.openlocfilehash: d6bc37a8aaddfb48e6d06eb46d9c1648e815b5ad
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89289238"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Rollenbasierte Zugriffssteuerung (RBAC) für Media Services-Konten

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Derzeit definiert Azure Media Services keine benutzerdefinierten dienstspezifischen Rollen. Um vollen Zugriff auf das Media Services-Konto zu erhalten, können Kunden die integrierten Rollen von **Besitzer** oder **Mitwirkender** verwenden. Der Hauptunterschied zwischen diesen Rollen ist, dass der **Besitzer** im Gegensatz zu einem **Mitwirkenden** steuern kann, wer Zugriff auf eine Ressource hat. Die integrierte Rolle **Leser** kann ebenfalls verwendet werden, aber der Benutzer oder die Anwendung hat nur Lesezugriff auf die Media Services-APIs. 

## <a name="design-principles"></a>Entwurfsprinzipien

Eines der wichtigsten Entwurfsprinzipien der v3-API ist es, die API sicherer zu machen. v3-APIs geben bei einem **Get**- oder **List**-Vorgang keine geheimen Schlüssel oder Anmeldeinformationen zurück. Die Schlüssel sind immer NULL, leer oder aus der Antwort bereinigt. Der Benutzer muss eine separate Aktionsmethode zum Abrufen von Geheimnissen oder Anmeldeinformationen aufrufen. Die **Leser**-Rolle kann keine Vorgänge wie „Asset.ListContainerSas“, „StreamingLocator.ListContentKeys“ oder „ContentKeyPolicies.GetPolicyPropertiesWithSecrets“ aufrufen. Getrennte Aktionen ermöglichen es Ihnen, bei Bedarf noch genauer abgestimmte RBAC-Sicherheitsberechtigungen in einer benutzerdefinierten Rolle festzulegen.

So listen Sie die von Media Services unterstützten Vorgänge auf

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

Im Artikel zu den [integrierten Rollendefinitionen](../../role-based-access-control/built-in-roles.md) erfahren Sie genau, was die Rolle gewährt. 

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Administratorrollen](../../role-based-access-control/rbac-and-directory-admin-roles.md)
- [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../../role-based-access-control/overview.md)
- [Verwenden der rollenbasierten Zugriffssteuerung (RBAC) zum Verwalten des Zugriffs](../../role-based-access-control/role-assignments-rest.md)
- [Media Services-Ressourcenanbietervorgänge](../../role-based-access-control/resource-provider-operations.md#microsoftmedia)

## <a name="next-steps"></a>Nächste Schritte

- [Entwickeln mit Media Services v3-APIs](media-services-apis-overview.md)
- [Abrufen von Richtlinien für symmetrische Schlüssel mithilfe von Media Services .NET](get-content-key-policy-dotnet-howto.md)
