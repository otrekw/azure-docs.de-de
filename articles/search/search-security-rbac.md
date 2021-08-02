---
title: Autorisieren des Zugriffs über Azure-Rollen
titleSuffix: Azure Cognitive Search
description: Rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) im Azure-Portal zum Steuern und Delegieren von administrativen Aufgaben für die Verwaltung von Azure Cognitive Search.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/28/2021
ms.openlocfilehash: 4e79af03f4545e4af799c660314212d18dbc91ff
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110459962"
---
# <a name="authorize-access-through-azure-roles-in-azure-cognitive-search"></a>Autorisieren des Zugriffs über Azure-Rollen in Azure Cognitive Search

In Azure wird ein [globales Modell für die rollenbasierte Autorisierung](../role-based-access-control/role-assignments-portal.md) für alle Dienste bereitgestellt, die über das Portal oder mit Resource Manager-APIs verwaltet werden. Das Autorisierungsmodell umfasst die Rollen „Besitzer“, „Mitwirkender“ und „Leser“, die die Ebene der *Dienstverwaltung* für Active Directory-Benutzer, -Gruppen und -Dienstprinzipale festlegen, die einer Rolle jeweils zugewiesen ist. Cognitive Search verwendet diese drei Rollen, um den Zugriff für die Suchdienstverwaltung zu autorisieren.

Cognitive Search bietet keine Unterstützung für Folgendes:

+ [Benutzerdefinierte Rollen](../role-based-access-control/custom-roles.md).
+ Die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC) für inhaltsbezogene Vorgänge (z. B. das Erstellen oder Abfragen eines Indexes oder eines anderen Objekts im Dienst).

  Für die Autorisierung zum Ausführen von Vorgängen für Inhalte ist entweder ein [Administrator-API-Schlüssel oder ein Abfrage-API-Schlüssel](search-security-api-keys.md) erforderlich.

> [!Note]
> Für den identitätsbasierten Zugriff über Suchergebnisse (manchmal als „Sicherheit auf Zeilenebene“ bezeichnet) können Sie Sicherheitsfilter erstellen, um die Ergebnisse anhand der Identität einzugrenzen. Auf diese Weise werden Dokumente entfernt, auf die der anfordernde Benutzer keinen Zugriff haben sollte. Weitere Informationen finden Sie unter [Sicherheitsfilter](search-security-trimming-for-azure-search.md).

## <a name="roles-used-in-cognitive-search"></a>In Cognitive Search verwendete Rollen

Für die kognitive Azure-Suche sind Rollen mit Berechtigungsebenen zugeordnet, die die folgenden Verwaltungsaufgaben unterstützen:

| Role | Aufgabe |
| --- | --- |
| Besitzer |Erstellen oder Löschen des Diensts. Erstellen, Aktualisieren oder Löschen von Objekten im Dienst: API-Schlüssel, Indizes, Synonymzuordnungen, Indexer, Indexerdatenquellen und Qualifikationsgruppen. </br></br>Vollzugriff auf alle Dienstinformationen, die im Portal oder über die Verwaltungs-REST-API, Azure PowerShell oder die Azure CLI verfügbar gemacht werden. </br></br>Zuweisen der Rollenmitgliedschaft.</br></br>Abonnementadministratoren und Dienstbesitzer sind automatisch Mitglieder der Besitzerrolle. |
| Mitwirkender | Gleiche Zugriffsebene wie „Besitzer“, aber ohne Rollenzuweisungen. [Mitwirkender von Suchdienst](../role-based-access-control/built-in-roles.md#search-service-contributor) entspricht der allgemeinen integrierten Rolle „Mitwirkender“. |
| Leser | Eingeschränkter Zugriff auf partielle Dienstinformationen. Im Portal kann die Rolle „Leser“ auf der Seite „Übersicht“ des Diensts im Abschnitt „Zusammenfassung“ und auf der Registerkarte „Monitor“ auf Informationen zugreifen. Der Zugriff auf alle anderen Registerkarten und Seiten ist nicht möglich. </br></br>Im Abschnitt „Zusammenfassung“: Ressourcengruppe, Status, Standort, Abonnementname und Abonnement-ID, Tags, URL, Tarif, Replikate, Partitionen und Sucheinheiten. </br></br>Zeigen Sie auf der Registerkarte „Monitor“ die Dienstmetriken an: Suchlatenz, Prozentsatz der gedrosselten Anforderungen, durchschnittliche Abfragen pro Sekunde. </br></br>Der Zugriff auf die Registerkarte „Nutzung“ (Speicher, Anzahl der im Dienst erstellten Indizes oder Indexer) oder auf Informationen auf den Registerkarten „Indizes“, „Indexer“, „Datenquellen“, „Qualifikationsgruppen“ oder „Debug sessions“ (Debugsitzungen) ist nicht möglich. |

Rollen erteilen keine Zugriffsrechte für den Dienstendpunkt. Suchdienstvorgänge wie die Indexverwaltung, das Auffüllen des Indexes und das Abfragen von Suchdaten werden durch die API-Schlüssel und nicht durch Rollen gesteuert. Weitere Informationen finden Sie unter [Verwalten von API-Schlüsseln](search-security-api-keys.md).

## <a name="tasks-and-permission-requirements"></a>Aufgaben und Berechtigungsanforderungen

In der folgenden Tabelle sind die Vorgänge zusammengefasst, die in Azure Cognitive Search zulässig sind. Außerdem erfahren Sie, welche Rolle oder welcher Schlüssel den Zugriff auf einen bestimmten Vorgang ermöglicht.

+ Die Azure RBAC-Mitgliedschaft garantiert den Zugriff auf Portalseiten und Dienstverwaltungsaufgaben (Erstellen, Löschen oder Ändern eines Diensts oder dessen API-Schlüssel).

+ API-Schlüssel werden erstellt, wenn der Dienst bereits vorhanden ist, und gelten für Inhaltsvorgänge im Dienst.

Darüber hinaus wird für inhaltsbezogene Vorgänge im Portal wie das Erstellen oder Löschen von Objekten der Vollzugriff auf alle Vorgänge und Informationen durch die explizite Rollenmitgliedschaft („Besitzer“ oder „Mitwirkender“) sowie die interne Verwendung eines Administratorschlüssels im Portal unterstützt. Anders ausgedrückt: Wenn Sie einen Index im Portal erstellen oder laden, erhalten Sie durch Ihre RBAC-Mitgliedschaft Zugriff auf die Seiten, aber das Portal selbst verwendet einen Administratorschlüssel, um den Vorgang im Dienst zu authentifizieren.

| Vorgang | Gesteuert von |
|-----------|-------------------------|
| Erstellen oder Löschen eines Diensts | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“ |
| Konfigurieren der Netzwerksicherheit (IP-Firewall) | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“ |
| Erstellen oder Löschen eines privaten Endpunkts | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“ |
| Implementieren von kundenseitig verwalteten Schlüsseln | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“ |
| Anpassen von Replikaten oder Partitionen | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“|
| Verwalten von Administrator- oder Abfrageschlüsseln | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“|
| Anzeigen von Dienstinformationen im Portal oder einer Verwaltungs-API | Azure RBAC-Berechtigungen: „Besitzer“, „Mitwirkender“ oder „Leser“  |
| Anzeigen von Objektinformationen und Metriken im Portal oder einer Verwaltungs-API | Azure RBAC-Berechtigungen: „Besitzer“ oder „Mitwirkender“ |
| Erstellen/Ändern/Löschen von Objekten für den Dienst: <br>Indizes und Komponententeile (einschließlich Analysedefinitionen, Bewertungsprofile und CORS-Optionen), Indexer, Datenquellen, Qualifikationsgruppen, Synonyme und Vorschläge | Administratorschlüssel bei Verwendung einer API sowie „Azure RBAC-Besitzer“ oder „Azure RBAC-Mitwirkender“ bei Verwendung des Portals |
| Abfragen von Indizes | Administrator- oder Abfrageschlüssel bei Verwendung einer API sowie „Azure RBAC-Besitzer“ oder „Azure RBAC-Mitwirkender“ bei Verwendung des Portals |
| Abfragen von Systeminformationen zu Objekten, beispielsweise zum Zurückgeben von Statistiken, Zählungen und Objektlisten | Administratorschlüssel bei Verwendung einer API sowie „Azure RBAC-Besitzer“ oder „Azure RBAC-Mitwirkender“ bei Verwendung des Portals |

## <a name="next-steps"></a>Nächste Schritte

+ [Verwalten mit PowerShell](search-manage-powershell.md) 
+ [Leistung und Optimierung in der kognitiven Azure-Suche](search-performance-optimization.md)
+ [Was ist die rollenbasierte Zugriffssteuerung in Azure (Azure Role-Based Access Control, Azure RBAC)?](../role-based-access-control/overview.md)
