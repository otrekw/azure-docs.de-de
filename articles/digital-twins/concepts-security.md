---
title: Sicherheit für Azure Digital Twins-Lösungen
titleSuffix: Azure Digital Twins
description: Grundlegendes zu bewährten Sicherheitsmethoden mit Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 6784ca9dbc32811a02f4454be94d220c634318f5
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92503316"
---
# <a name="secure-azure-digital-twins"></a>Schützen von Azure Digital Twins

Zur Gewährleistung der Sicherheit ermöglicht Azure Digital Twins eine exakte Zugriffssteuerung für bestimmte Daten, Ressourcen und Aktionen in Ihrer Bereitstellung. Hierzu wird eine präzise Rollen- und Rechteverwaltungsstrategie namens **rollenbasierte Zugriffssteuerung in Azure** (Role-Based Access Control, Azure RBAC) verwendet. Informationen zu den allgemeinen Prinzipien von Azure RBAC finden Sie [hier](../role-based-access-control/overview.md).

Azure Digital Twins unterstützt auch die Verschlüsselung ruhender Daten.

## <a name="roles-and-permissions-with-azure-rbac"></a>Rollen und Berechtigungen mit Azure RBAC

Azure RBAC wird in Azure Digital Twins über die Integration mit [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) bereitgestellt.

Mit Azure RBAC können einem *Sicherheitsprinzipal* Berechtigungen erteilt werden. Bei einem Sicherheitsprinzipal kann es sich um einen Benutzer, um eine Gruppe oder um einen Anwendungsdienstprinzipal handeln. Der Sicherheitsprinzipal wird durch Azure AD authentifiziert und erhält im Gegenzug ein OAuth 2.0-Token. Dieses Token kann verwendet werden, um eine an eine Azure Digital Twins-Instanz gerichtete Zugriffsanforderung zu autorisieren.

### <a name="authentication-and-authorization"></a>Authentifizierung und Autorisierung

Der Zugriff mit Azure AD ist ein zweistufiger Prozess. Wenn ein Sicherheitsprinzipal (ein Benutzer, eine Gruppe oder eine Anwendung) versucht, auf Azure Digital Twins zuzugreifen, muss die Anforderung *authentifiziert* und *autorisiert* werden. 

1. Zunächst wird die Identität des Sicherheitsprinzipals *authentifiziert* und ein OAuth 2.0-Token zurückgegeben.
2. Anschließend wird das Token als Teil einer Anforderung an den Azure Digital Twins-Dienst übergeben, um den Zugriff auf die angegebene Ressource zu *autorisieren*.

Für den Authentifizierungsschritt muss jede Anwendungsanforderung zur Laufzeit ein OAuth 2.0-Zugriffstoken enthalten. Wird eine Anwendung in einer Azure-Entität ausgeführt (beispielsweise in einer [Azure Functions](../azure-functions/functions-overview.md)-App), kann für den Zugriff auf die Ressourcen eine **verwaltete Identität** verwendet werden. Weitere Informationen zu verwalteten Identitäten finden Sie im nächsten Abschnitt.

Für den Autorisierungsschritt muss dem Sicherheitsprinzipal eine Azure-Rolle zugewiesen werden. Die möglichen Berechtigungen eines Sicherheitsprinzipals sind durch die Rollen vorgegeben, die dem Prinzipal zugewiesen sind. Azure Digital Twins stellt Azure-Rollen bereit, die Berechtigungssätze für Azure Digital Twins-Ressourcen enthalten. Diese Rollen werden weiter unten in diesem Artikel beschrieben.

Weitere Informationen zu in Azure unterstützten Rollen und Rollenzuweisungen finden Sie in der Azure RBAC-Dokumentation unter [*Administratorrollen für klassische Abonnements, Azure-Rollen und Azure AD-Rollen*](../role-based-access-control/rbac-and-directory-admin-roles.md).

#### <a name="authentication-with-managed-identities"></a>Authentifizierung mit verwalteten Identitäten

[Verwaltete Identitäten für Azure-Ressourcen](../active-directory/managed-identities-azure-resources/overview.md) ist ein Azure-übergreifendes Feature, mit dem Sie eine sichere Identität für die Bereitstellung erstellen können, in der Ihr Anwendungscode ausgeführt wird. Dieser Identität können dann Zugriffssteuerungsrollen zugeordnet werden, um benutzerdefinierte Berechtigungen für den Zugriff auf bestimmte Azure-Ressourcen zu erteilen, die Ihre Anwendung benötigt.

Die Azure-Plattform verwaltet diese Laufzeitidentität mit verwalteten Identitäten. Sie müssen keine Zugriffsschlüssel in Ihrem Anwendungscode speichern und schützen – weder für die Identität selbst noch für die Ressourcen, auf die zugegriffen werden muss. Von einer Azure Digital Twins-Client-App, die innerhalb einer Azure App Service-Anwendung ausgeführt wird, müssen keine SAS-Regeln und -Schlüssel oder andere Zugriffstoken verarbeitet werden. Die Client-App benötigt nur die Endpunktadresse des Azure Digital Twins-Namespace. Wenn die App eine Verbindung herstellt, bindet Azure Digital Twins den Kontext der verwalteten Entität an den Client. Nach der Zuordnung zu einer verwalteten Identität können von Ihrem Azure Digital Twins-Client sämtliche autorisierten Vorgänge ausgeführt werden. Zur Autorisierung wird dann eine verwaltete Entität einer Azure Digital Twins-Rolle zugeordnet, wie im Anschluss beschrieben.

#### <a name="authorization-azure-roles-for-azure-digital-twins"></a>Autorisierung: Azure-Rollen für Azure Digital Twins

Azure bietet **zwei integrierte Azure-Rollen** für die Autorisierung des Zugriffs auf die [Datenebenen-APIs](how-to-use-apis-sdks.md#overview-data-plane-apis) von Azure Digital Twins. Sie können auf die Rollen entweder über den Namen oder über die ID verweisen:

| Integrierte Rolle | BESCHREIBUNG | id | 
| --- | --- | --- |
| Azure Digital Twins Data Owner (Azure Digital Twins-Datenbesitzer) | Bietet Vollzugriff auf Azure Digital Twins-Ressourcen | bcd981a7-7f74-457b-83e1-cceb9e632ffe |
| Azure Digital Twins Data Reader (Azure Digital Twins-Datenleser) | Bietet schreibgeschützten Zugriff auf Azure Digital Twins-Ressourcen | d57506d4-4c8d-48b1-8587-93c323f6a5a3 |

>[!NOTE]
> Diese Rollen wurden kürzlich in der Vorschau umbenannt:
> * *Azure Digital Twins Data Owner* (Azure Digital Twins-Datenbesitzer) war zuvor *Azure Digital Twins-Besitzer (Vorschau)* .
> * *Azure Digital Twins Data Reader* (Azure Digital Twins-Datenleser) war zuvor *Azure Digital Twins-Leser (Vorschau)* .

Rollen können auf zwei Arten zugewiesen werden:
* Über den Bereich „Zugriffssteuerung (IAM)“ für Azure Digital Twins im Azure-Portal (weitere Informationen finden Sie unter [*Hinzufügen oder Entfernen von Azure-Rollenzuweisungen über das Azure-Portal*](../role-based-access-control/role-assignments-portal.md))
* Mithilfe von CLI-Befehlen zum Hinzufügen oder Entfernen einer Rolle

Ausführlichere Informationen hierzu finden Sie im [*Azure Digital Twins-Tutorial: Erstellen einer End-to-End-Lösung*](tutorial-end-to-end.md).

Weitere Informationen zur Definition integrierter Rollen finden Sie in der Azure RBAC-Dokumentation unter [*Grundlegendes zu Azure-Rollendefinitionen*](../role-based-access-control/role-definitions.md). Informationen zum Erstellen von benutzerdefinierten Azure-Rollen finden Sie unter [*Benutzerdefinierte Azure-Rollen*](../role-based-access-control/custom-roles.md).

##### <a name="automating-roles"></a>Automatisieren von Rollen

Wenn Sie sich auf Rollen in automatisierten Szenarien beziehen, empfiehlt es sich, auf diese mit ihren **IDs** und nicht mit ihren Namen zu verweisen. Die Namen können sich zwischen den einzelnen Versionen ändern, die IDs jedoch nicht, was sie zu einer beständigeren Referenz in der Automation macht.

> [!TIP]
> Wenn Sie Rollen mit einem Cmdlet zuweisen, z. B. `New-AzRoleAssignment` ([Verweis](/powershell/module/az.resources/new-azroleassignment?view=azps-4.8.0)), können Sie den Parameter `-RoleDefinitionId` anstelle von `-RoleDefinitionName` verwenden, um eine ID anstelle eines Namens für die Rolle zu übergeben.

### <a name="permission-scopes"></a>Berechtigungsbereiche

Bevor Sie einem Sicherheitsprinzipal eine Azure-Rolle zuweisen, legen Sie den Zugriffsbereich fest, den der Sicherheitsprinzipal haben soll. Es empfiehlt sich, immer nur den kleinstmöglichen Umfang zu gewähren.

In der folgenden Liste werden die Ebenen beschrieben, auf denen Sie den Zugriff auf Azure Digital Twins-Ressourcen einschränken können:
* Modelle: Die Aktionen für diese Ressource geben die Kontrolle über [Modelle](concepts-models.md) vor, die in Azure Digital Twins hochgeladen werden.
* Abfragen des Digital Twins-Graphen: Die Aktionen für diese Ressource bestimmen die Fähigkeit, [Abfragevorgänge](concepts-query-language.md) für digitale Zwillinge innerhalb des Azure Digital Twins-Graphen auszuführen.
* Digitaler Zwilling: Die Aktionen für diese Ressource ermöglichen die Steuerung von CRUD-Vorgängen für [digitale Zwillinge](concepts-twins-graph.md) im Zwillingsgraphen.
* Digitale Zwillingsbeziehung: Die Aktionen für diese Ressource definieren die Steuerung von CRUD-Vorgängen für [Beziehungen](concepts-twins-graph.md) zwischen digitalen Zwillingen im Zwillingsgraphen.
* Ereignisroute: Die Aktionen für diese Ressource bestimmen Berechtigungen zum [Weiterleiten von Ereignissen](concepts-route-events.md) aus Azure Digital Twins an einen Endpunktdienst wie [Event Hub](../event-hubs/event-hubs-about.md), [Event Grid](../event-grid/overview.md) oder [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

### <a name="troubleshooting-permissions"></a>Problembehandlung bei Berechtigungen

Wenn ein Benutzer versucht, eine Aktion auszuführen, die für seine Rolle nicht zulässig ist, wird möglicherweise der folgende Fehler von der Dienstanforderung zurückgegeben: `403 (Forbidden)`. Weitere Informationen und Hilfe bei der Problembehandlung finden Sie unter [*Problembehandlung: Fehler bei Azure Digital Twins-Anforderung mit dem Status ''403 (Forbidden)'' (403 (Unzulässig))*](troubleshoot-error-403.md).

## <a name="encryption-of-data-at-rest"></a>Verschlüsselung für ruhende Daten

Azure Digital Twins bietet Verschlüsselung von ruhenden Daten und bei der Übertragung, während sie in unseren Rechenzentren geschrieben werden, und entschlüsselt die Daten, wenn Sie auf sie zugreifen. Diese Verschlüsselung erfolgt mithilfe eines von Microsoft verwalteten Verschlüsselungsschlüssels.

## <a name="cross-origin-resource-sharing-cors"></a>Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS)

**CORS (Cross-Origin Resource Sharing)** wird in Azure Digital Twins derzeit nicht unterstützt. Wenn Sie eine REST-API über eine Browser-App, eine [API Management (APIM)](../api-management/api-management-key-concepts.md)-Schnittstelle oder einen [Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview)-Connector aufrufen, wird daher möglicherweise ein Richtlinienfehler angezeigt.

Zum Beheben dieses Fehlers können Sie eine der folgenden Maßnahmen ergreifen:
* Entfernen Sie den CORS-Header `Access-Control-Allow-Origin` aus der Nachricht. Dieser Header gibt an, ob die Antwort freigegeben werden kann. 
* Alternativ können Sie einen CORS-Proxy erstellen und über diesen die REST-API-Anforderung von Azure Digital Twins stellen. 

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu diesen Konzepten finden Sie in der Anleitung [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung*](how-to-set-up-instance-portal.md).

* Weitere Informationen zur Interaktion mit diesen Konzepten über den Clientanwendungscode finden Sie unter [*Vorgehensweise: Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md).

* Weitere Informationen zu [Azure RBAC](../role-based-access-control/overview.md).
