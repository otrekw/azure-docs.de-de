---
title: Azure App Configuration-REST-API – Azure Active Directory-Autorisierung
description: Verwenden von Azure Active Directory zur Autorisierung bei Azure App Configuration über die REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: bebab7c06062726f7b5c7868f984cadda3b4c98e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423725"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Azure Active Directory-Autorisierung – REST-API-Referenz

Bei Verwendung der Azure Active Directory-Authentifizierung (Azure AD) erfolgt die Autorisierung über die rollenbasierte Zugriffssteuerung (RBAC) in Azure. In Azure RBAC müssen Benutzer Rollen zugewiesen werden, damit Zugriff auf Ressourcen gewährt wird. Jede Rolle enthält eine Reihe von Aktionen, die Benutzer ausführen können, die der Rolle zugewiesen sind.

## <a name="roles"></a>Rollen

Die folgenden integrierten Rollen sind standardmäßig in Azure-Abonnements verfügbar:

- **Azure App Configuration-Datenbesitzer:** Diese Rolle ermöglicht Vollzugriff auf alle Vorgänge.
- **Azure App Configuration-Datenleser:** Diese Rolle ermöglicht Lesevorgänge.

## <a name="actions"></a>Aktionen

Rollen enthalten eine Liste von Aktionen, die Benutzer ausführen können, die der jeweiligen Rolle zugewiesen sind. Azure App Configuration unterstützt die folgenden Aktionen:

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`: Diese Aktion ermöglicht Lesezugriff auf App Configuration-Schlüsselwertressourcen, z. B. „/kv“ und „/labels“.
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`: Diese Aktion ermöglicht Schreibzugriff auf App Configuration-Schlüsselwertressourcen.
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`: Diese Aktion ermöglicht das Löschen von App Configuration-Schlüsselwertressourcen. Beachten Sie, dass beim Löschen einer Ressource der Schlüsselwert zurückgegeben wird, der gelöscht wurde.

## <a name="errors"></a>Errors

```http
HTTP/1.1 403 Forbidden
```

**Grund:** Der Prinzipal, der die Anforderung durchführt, verfügt nicht über die erforderlichen Berechtigungen für den angeforderten Vorgang.
**Lösung:** Weisen Sie dem Prinzipal, der die Anforderung durchführt, die zum Ausführen des angeforderten Vorgangs erforderliche Rolle zu.

## <a name="managing-role-assignments"></a>Verwalten von Rollenzuweisungen

Die Verwaltung von Rollenzuweisungen erfolgt mithilfe von [Azure RBAC](https://docs.microsoft.com/azure/role-based-access-control/overview)-Prozeduren, die in allen Azure-Diensten als Standard gelten. Dies kann über die Azure-Befehlszeilenschnittstelle, über PowerShell, über das Azure-Portal usw. durchgeführt werden. Die offizielle Dokumentation zum Erstellen von Rollenzuweisungen finden Sie [hier](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
