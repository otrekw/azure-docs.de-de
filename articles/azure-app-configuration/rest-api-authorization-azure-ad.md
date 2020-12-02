---
title: Azure App Configuration-REST-API – Azure Active Directory-Autorisierung
description: Verwenden von Azure Active Directory zur Autorisierung bei Azure App Configuration über die REST-API
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 89c51e69a4274affcecb4d967deb96dcebcfd70f
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2020
ms.locfileid: "95253369"
---
# <a name="azure-active-directory-authorization---rest-api-reference"></a>Azure Active Directory-Autorisierung – REST-API-Referenz

Bei Verwendung der Azure Active Directory-Authentifizierung (Azure AD) erfolgt die Autorisierung über die rollenbasierte Zugriffssteuerung (RBAC). Für die RBAC müssen Benutzer Rollen zugewiesen werden, damit ihnen Zugriff auf Ressourcen gewährt wird. Jede Rolle enthält eine Reihe von Aktionen, die alle Benutzer ausführen können, die der Rolle zugewiesen sind.

## <a name="roles"></a>Rollen

Die folgenden Rollen sind standardmäßig in Azure-Abonnements verfügbar:

- **Azure App Configuration-Datenbesitzer:** Diese Rolle ermöglicht Vollzugriff auf alle Vorgänge.
- **Azure App Configuration-Datenleser:** Diese Rolle ermöglicht Lesevorgänge.

## <a name="actions"></a>Aktionen

Rollen enthalten eine Liste von Aktionen, die Benutzer ausführen können, die der jeweiligen Rolle zugewiesen sind. Azure App Configuration unterstützt die folgenden Aktionen:

- `Microsoft.AppConfiguration/configurationStores/keyValues/read`: Diese Aktion ermöglicht Lesezugriff auf App Configuration-Schlüsselwertressourcen, z. B. „/kv“ und „/labels“.
- `Microsoft.AppConfiguration/configurationStores/keyValues/write`: Diese Aktion ermöglicht Schreibzugriff auf App Configuration-Schlüsselwertressourcen.
- `Microsoft.AppConfiguration/configurationStores/keyValues/delete`: Diese Aktion ermöglicht das Löschen von App Configuration-Schlüsselwertressourcen. Beachten Sie, dass beim Löschen einer Ressource der Schlüsselwert zurückgegeben wird, der gelöscht wurde.

## <a name="error"></a>Fehler

```http
HTTP/1.1 403 Forbidden
```

**Grund:** Der Prinzipal, der die Anforderung durchführt, verfügt nicht über die erforderlichen Berechtigungen für den angeforderten Vorgang.
**Lösung:** Weisen Sie dem Prinzipal, der die Anforderung durchführt, die zum Ausführen des angeforderten Vorgangs erforderliche Rolle zu.

## <a name="managing-role-assignments"></a>Verwalten von Rollenzuweisungen

Sie können Rollenzuweisungen mithilfe von [RBAC-Prozeduren](https://docs.microsoft.com/azure/role-based-access-control/overview) verwalten, die in allen Azure-Diensten als Standard gelten. Sie können hierfür die Azure-Befehlszeilenschnittstelle, PowerShell und das Azure-Portal verwenden. Weitere Informationen finden Sie unter [Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal).
