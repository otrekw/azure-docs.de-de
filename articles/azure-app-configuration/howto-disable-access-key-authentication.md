---
title: Deaktivieren der Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Instanz (Vorschau)
titleSuffix: Azure App Configuration
description: Hier erfahren Sie, wie Sie die Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Instanz (Vorschau) deaktivieren.
ms.service: azure-app-configuration
author: jimmyca15
ms.author: jimmyca
ms.topic: how-to
ms.date: 5/14/2021
ms.openlocfilehash: 451d9701b62cf46fe81bdd17f4eded63a38d1dce
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110482764"
---
# <a name="disable-access-key-authentication-for-an-azure-app-configuration-instance-preview"></a>Deaktivieren der Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Instanz (Vorschau)

Jede Anforderung einer Azure App Configuration-Ressource muss authentifiziert werden. Anforderungen können standardmäßig entweder mit Azure Active Directory-Anmeldeinformationen (Azure AD) oder mithilfe eines Zugriffsschlüssels authentifiziert werden. Von diesen beiden Authentifizierungsschemas bietet Azure AD eine höhere Sicherheit und einfachere Verwendung gegenüber Zugriffsschlüsseln und wird daher von Microsoft empfohlen. Sie können die Verwendung von Zugriffsschlüsseln für eine Azure App Configuration-Ressource deaktivieren, sodass Clients Azure AD für das Authentifizieren von Anforderungen verwenden müssen.

Wenn Sie die Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Ressource deaktivieren, werden alle vorhandenen Zugriffsschlüssel für diese Ressource gelöscht. Alle nachfolgenden Anforderungen der Ressource mit den zuvor vorhandenen Zugriffsschlüsseln werden abgelehnt. Nur die mithilfe von Azure AD authentifizierten Anforderungen sind erfolgreich. Weitere Informationen zur Verwendung von Azure AD finden Sie unter [Autorisieren des Zugriffs auf Azure App Configuration mittels Azure Active Directory](./concept-enable-rbac.md).

## <a name="disable-access-key-authentication"></a>Deaktivieren der Zugriffsschlüsselauthentifizierung

Wenn Sie die Zugriffsschlüsselauthentifizierung deaktivieren, werden alle Zugriffsschlüssel gelöscht. Wenn ausgeführte Anwendungen Zugriffsschlüssel für die Authentifizierung verwenden, treten Fehler auf, sobald die Zugriffsschlüsselauthentifizierung deaktiviert ist. Wenn Sie die Zugriffsschlüsselauthentifizierung wieder aktivieren, werden neue Zugriffsschlüsseln generiert, und bei allen Anwendungen, die versuchen, die alten Zugriffsschlüssel zu verwenden, treten weiterhin Fehler auf.

> [!WARNING]
> Wenn Clients derzeit mit Zugriffsschlüsseln auf Daten in Ihrer Azure App Configuration-Ressource zugreifen, empfiehlt Microsoft, dass Sie diese Clients zu [Azure AD](./concept-enable-rbac.md) migrieren, bevor Sie die Zugriffsschlüsselauthentifizierung deaktivieren.
> Darüber hinaus wird empfohlen, den Abschnitt mit den [Einschränkungen](#limitations) weiter unten zu lesen, um sicherzustellen, dass sich die Einschränkungen nicht auf die beabsichtigte Nutzung der Ressource auswirken.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um die Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Ressource im Azure-Portal zu deaktivieren:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure App Configuration-Ressource.
2. Suchen Sie unter **Einstellungen** die Einstellung **Zugriffsschlüssel**.

    :::image type="content" border="true" source="./media/access-keys-blade.png" alt-text="Screenshot: Blatt „Zugriffsschlüssel“ zum Zugreifen auf Azure App Configuration-Ressourcen":::

3. Legen Sie den Umschalter **Enable access keys** (Zugriffsschlüssel aktivieren) auf **Deaktiviert** fest.

    :::image type="content" border="true" source="./media/disable-access-keys.png" alt-text="Screenshot: Deaktivieren der Zugriffsschlüsselauthentifizierung für Azure App Configuration":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Die Funktion zum Deaktivieren der Zugriffsschlüsselauthentifizierung mithilfe der Azure CLI befindet sich in der Entwicklungsphase.

---

### <a name="verify-that-access-key-authentication-is-disabled"></a>Überprüfen, ob die Zugriffsschlüsselauthentifizierung deaktiviert ist

Wenn Sie überprüfen möchten, ob die Zugriffsschlüsselauthentifizierung nicht mehr zulässig ist, können Sie eine Anforderung zum Auflisten der Zugriffsschlüssel für die Azure App Configuration-Ressource senden. Wenn die Zugriffsschlüsselauthentifizierung deaktiviert ist, sind keine Zugriffsschlüssel vorhanden, und beim Auflistungsvorgang wird eine leere Liste zurückgegeben.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob die Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Ressource im Azure-Portal deaktiviert ist:

1. Navigieren Sie im Azure-Portal zu Ihrer Azure App Configuration-Ressource.
2. Suchen Sie unter **Einstellungen** die Einstellung **Zugriffsschlüssel**.

    :::image type="content" border="true" source="./media/access-keys-blade.png" alt-text="Screenshot: Blatt „Zugriffsschlüssel“ zum Zugreifen auf Azure App Configuration-Ressourcen":::

3. Vergewissern Sie sich, dass keine Zugriffsschlüssel angezeigt werden und **Enable access keys** (Zugriffsschlüssel aktivieren) auf **Deaktiviert** festgelegt ist.

    :::image type="content" border="true" source="./media/access-keys-disabled-portal.png" alt-text="Screenshot: Deaktivierte Zugriffsschlüssel für eine Azure App Configuration-Ressource":::

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Verwenden Sie den folgenden Befehl, um zu überprüfen, ob die Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Ressource im Azure-Portal deaktiviert ist. Der Befehl listet die Zugriffsschlüssel für eine Azure App Configuration-Ressource auf. Wenn die Zugriffsschlüsselauthentifizierung deaktiviert ist, ist die Liste leer.

```azurecli-interactive
az appconfig credential list \
    --name <app-configuration-name> \
    --resource-group <resource-group>
```

Wenn die Zugriffsschlüsselauthentifizierung deaktiviert ist, wird eine leere Liste zurückgegeben.

```
C:\Users\User>az appconfig credential list -g <resource-group> -n <app-configuration-name>
[]
```

---

## <a name="permissions-for-allowing-or-disallowing-access-key-authentication"></a>Berechtigungen zum Zulassen oder Untersagen der Zugriffsschlüsselauthentifizierung

Ein Benutzer muss über Berechtigungen zum Erstellen und Verwalten von Azure App Configuration-Ressourcen verfügen, um den Status der Zugriffsschlüsselauthentifizierung für eine Azure App Configuration-Ressource zu ändern. Die Rollen der rollenbasierten Zugriffssteuerung in Azure (Azure RBAC), die diese Berechtigungen bieten, umfassen die Aktion **Microsoft.AppConfiguration/configurationStores/write** oder **Microsoft.AppConfiguration/configurationStores/\*** . In diese Aktion sind folgende Rollen integriert:

- Die Azure Resource Manager-Rolle [Besitzer](../role-based-access-control/built-in-roles.md#owner)
- Die Azure Resource Manager-Rolle [Mitwirkender](../role-based-access-control/built-in-roles.md#contributor)

Diese Rollen bieten mithilfe von Azure Active Directory (Azure AD) keinen Zugriff auf Daten in einer Azure App Configuration-Ressource. Sie umfassen jedoch die Berechtigung für die Aktion **Microsoft.AppConfiguration/configurationStores/listKeys/action**, mit der der Zugriff auf die Zugriffsschlüssel der Ressource gewährt wird. Mit dieser Berechtigung kann ein Benutzer die Zugriffsschlüssel verwenden, um auf alle Daten in der Ressource zuzugreifen.

Rollenzuweisungen müssen auf die Ebene der Azure App Configuration-Ressource oder höher festgelegt werden, damit ein Benutzer die Zugriffsschlüsselauthentifizierung für die Ressource zulassen oder untersagen kann. Weitere Informationen zum Rollenbereich finden Sie unter [Grundlegendes zum Bereich für Azure RBAC](../role-based-access-control/scope-overview.md).

Beschränken Sie die Zuweisung dieser Rollen unbedingt nur auf diejenigen Benutzer, denen es möglich sein muss, eine App Configuration-Ressource zu erstellen oder deren Eigenschaften zu aktualisieren. Verwenden Sie das Prinzip der geringsten Rechte, um sicherzustellen, dass Benutzer die geringsten Berechtigungen haben, die sie zum Ausführen ihrer Aufgaben benötigen. Weitere Informationen zum Verwalten des Zugriffs mit Azure RBAC finden Sie unter [Bewährte Methoden für Azure RBAC](../role-based-access-control/best-practices.md).

> [!NOTE]
> Die zu „Administrator für klassisches Abonnement“ gehörigen Rollen „Dienstadministrator“ und „Co-Administrator“ schließen die Entsprechung der Azure Resource Manager-Rolle [Besitzer](../role-based-access-control/built-in-roles.md#owner) ein. Da die Rolle **Besitzer** alle Aktionen einschließt, kann ein Benutzer mit einer dieser administrativen Rollen auch App Configuration-Ressourcen erstellen und verwalten. Weitere Informationen finden Sie unter [Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen](../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

## <a name="limitations"></a>Einschränkungen

Die Funktion zum Deaktivieren der Zugriffsschlüsselauthentifizierung ist als Vorschau verfügbar. Derzeit gelten die folgenden Einschränkungen.

### <a name="arm-template-access"></a>Zugriff auf ARM-Vorlagen

Wenn die Zugriffsschlüsselauthentifizierung deaktiviert ist, wird auch die Funktion zum Lesen bzw. Schreiben von Schlüsselwerten in einer [ARM-Vorlage](./quickstart-resource-manager.md) deaktiviert. Dies liegt daran, dass für den Zugriff auf die in ARM-Vorlagen verwendete Ressource „Microsoft.AppConfiguration/configurationStores/keyValues“ eine Azure Resource Manager-Rolle wie „Mitwirkender“ oder „Besitzer“ erforderlich ist. Wenn die Zugriffsschlüsselauthentifizierung deaktiviert ist, ist für den Zugriff auf die Ressource eine der [Azure App Configuration-Datenebenenrollen](concept-enable-rbac.md) erforderlich, weshalb auch der Zugriff mit ARM-Vorlagen abgelehnt wird.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von kundenseitig verwalteten Schlüsseln zum Verschlüsseln Ihrer App Configuration-Daten](concept-customer-managed-keys.md)
- [Verwenden privater Endpunkte für Azure App Configuration](concept-private-endpoint.md)