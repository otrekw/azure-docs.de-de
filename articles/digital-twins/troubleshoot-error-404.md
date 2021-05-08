---
title: 'Fehler bei Azure Digital Twins-Anforderung mit folgendem Status: 404 Unterdomäne nicht gefunden'
description: 'Ursachen und Behebungen für „Fehler bei Service Request. Status: 404 Unterdomäne nicht gefunden“ für Azure Digital Twins.'
ms.service: digital-twins
author: baanders
ms.author: baanders
ms.topic: troubleshooting
ms.date: 4/13/2021
ms.openlocfilehash: e3fe3ad22098d796faa309ff3509c318a7e1df4d
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107590542"
---
# <a name="service-request-failed-status-404-sub-domain-not-found"></a>Fehler bei Service Request. Status: 404 Unterdomäne nicht gefunden

In diesem Artikel werden die Ursachen sowie die Schritte zur Behebung im Falle eines 404-Fehlers bei Service Requests für Azure Digital Twins beschrieben. 

## <a name="symptoms"></a>Symptome

Dieser Fehler kann auftreten, wenn auf eine Azure Digital Twins-Instanz mit einem Dienstprinzipal oder Benutzerkonto zugegriffen wird, der bzw. das zu einem anderen [Azure AD-Mandanten (Azure Active Directory)](../active-directory/develop/quickstart-create-new-tenant.md) gehört. Der Identität scheinen die richtigen [Rollen](concepts-security.md) zugewiesen zu sein, aber bei API-Anforderungen tritt ein Fehler mit dem Status `404 Sub-Domain not found` auf.

## <a name="causes"></a>Ursachen

### <a name="cause-1"></a>Ursache 1

Azure Digital Twins erfordert, dass alle authentifizierenden Benutzer zum gleichen Azure AD-Mandanten gehören wie die Azure Digital Twins-Instanz.

[!INCLUDE [digital-twins-tenant-limitation](../../includes/digital-twins-tenant-limitation.md)]

## <a name="solutions"></a>Lösungen

### <a name="solution-1"></a>Lösung 1

Sie können dieses Problem beheben, indem Sie für jede Verbundidentität eines anderen Mandanten ein **Token** vom Basismandanten der Azure Digital Twins-Instanz anfordern. 

[!INCLUDE [digital-twins-tenant-solution-1](../../includes/digital-twins-tenant-solution-1.md)]

### <a name="solution-2"></a>Lösung 2

Wenn Sie in Ihrem Code die Klasse `DefaultAzureCredential` verwenden und dieses Problem nach dem Abrufen eines Tokens weiterhin auftritt, können Sie den Basismandanten in den Optionen vom Typ `DefaultAzureCredential` angeben, um den Mandanten zu identifizieren, auch wenn von der Authentifizierung standardmäßig ein anderer Typ verwendet wird.

[!INCLUDE [digital-twins-tenant-solution-2](../../includes/digital-twins-tenant-solution-2.md)]

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich weiter über die Sicherheit und Berechtigungen in Azure Digital Twins:
* [*Konzepte: Sicherheit für Azure Digital Twins-Lösungen*](concepts-security.md)
