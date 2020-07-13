---
title: Festlegen von RBAC-Rollen für den Azure-Administratorzugriff
titleSuffix: Azure Cognitive Search
description: Rollenbasierte Verwaltungsfunktionen (RBAC) im Azure-Portal zum Steuern und Delegieren von administrativen Aufgaben für die Verwaltung der kognitiven Azure-Suche.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/03/2020
ms.openlocfilehash: 402fae5622219b14cfdab921ebe1a78ad5dd111e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84462837"
---
# <a name="set-rbac-roles-for-administrative-access-to-azure-cognitive-search"></a>Festlegen von RBAC-Rollen für den Administratorzugriff auf die kognitive Azure-Suche

In Azure wird ein [globales Modell für die rollenbasierte Autorisierung](../role-based-access-control/role-assignments-portal.md) für alle Dienste bereitgestellt, die über das Portal oder mit Resource Manager-APIs verwaltet werden. Die Rollen „Besitzer“, „Mitwirkender“ und „Leser“ bestimmen die Ebene der *Dienstverwaltung* für Active Directory-Benutzer, -Gruppen und -Dienstprinzipale, die einer Rolle jeweils zugewiesen sind. 

> [!Note]
> Es gibt keine rollenbasierte Zugriffssteuerung zum Schutz der Teile eines Indexes oder einer Teilmenge von Dokumenten. Für den identitätsbasierten Zugriff über Suchergebnisse können Sie Sicherheitsfilter erstellen, um die Ergebnisse anhand der Identität einzugrenzen. Auf diese Weise werden Dokumente entfernt, auf die der Anforderer keinen Zugriff haben sollte. Weitere Informationen finden Sie unter [Sicherheitsfilter](search-security-trimming-for-azure-search.md) und [Sichern mit Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Verwaltungsaufgaben nach Rolle

Für die kognitive Azure-Suche sind Rollen mit Berechtigungsebenen zugeordnet, die die folgenden Verwaltungsaufgaben unterstützen:

| Role | Aufgabe |
| --- | --- |
| Besitzer |Erstellen oder löschen Sie den Dienst bzw. jedes Objekt im Dienst, einschließlich API-Schlüsseln, Indizes, Indexern sowie Datenquellen und Zeitplänen für Indexer.<p>Zeigen Sie den Dienststatus an, einschließlich Anzahl und Speichergröße.<p>Hinzufügen oder Löschen von Rollenmitgliedschaften (Nur ein Besitzer kann die Rollenmitgliedschaften verwalten.)<p>Abonnementadministratoren und Dienstbesitzer sind automatisch Mitglieder der Besitzerrolle. |
| Mitwirkender |Die gleiche Ebene wie „Besitzer“, mit Ausnahme der RBAC-Rollenverwaltung. So kann z.B. ein Mitwirkender Objekte erstellen oder löschen bzw. [API-Schlüssel](search-security-api-keys.md) anzeigen und neu generieren, aber nicht die Rollenmitgliedschaften ändern. |
| [Mitwirkender von Suchdienst](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor) | Entspricht der Rolle „Mitwirkender“. |
| Leser |Anzeigen von Dienstzusammenfassungen und Metriken. Mitglieder dieser Rolle können Index, Indexer, Datenquelle oder Schlüsselinformationen nicht anzeigen.  |

Rollen erteilen keine Zugriffsrechte für den Dienstendpunkt. Suchdienstoperationen, wie z. B. die Indexverwaltung, Auffüllung des Indexes und Abfragen von Suchdaten werden durch die API-Schlüssel und nicht durch Rollen gesteuert. Weitere Informationen finden Sie unter [Verwalten von API-Schlüsseln](search-security-api-keys.md).

## <a name="permissions-table"></a>Berechtigungstabelle

Die folgende Tabelle enthält eine Zusammenfassung der in der kognitiven Azure-Suche zulässigen Vorgänge sowie Informationen dazu, welcher Schlüssel für den jeweiligen Vorgang erforderlich ist.

| Vorgang | Berechtigungen |
|-----------|-------------------------|
| Erstellen von Diensten | Azure-Abonnementinhaber |
| Skalieren von Diensten | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource  |
| Löschen von Diensten | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource |
| Erstellen/Ändern/Löschen von Objekten für den Dienst: <br>Indizes und Komponententeile (einschließlich Analysedefinitionen, Bewertungsprofile und CORS-Optionen), Indexer, Datenquellen, Synonyme und Vorschläge | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource |
| Abfragen von Indizes | Administrator- oder Abfrageschlüssel (RBAC nicht verfügbar) |
| Abfragen von Systeminformationen (etwa zum Zurückgeben von Statistiken, Anzahlen und Objektlisten) | Administratorschlüssel, RBAC-Rolle für die Ressource („Besitzer“, „Mitwirkender“ oder „Leser“) |
| Verwalten von Administratorschlüsseln | Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource |
| Verwalten von Abfrageschlüsseln |  Administratorschlüssel, RBAC-Rolle „Besitzer“ oder „Mitwirkender“ für die Ressource  |

## <a name="see-also"></a>Weitere Informationen

+ [Verwalten mit PowerShell](search-manage-powershell.md) 
+ [Leistung und Optimierung in der kognitiven Azure-Suche](search-performance-optimization.md)
+ [Erste Schritte mit der rollenbasierten Zugriffssteuerung im Azure-Portal](../role-based-access-control/overview.md)