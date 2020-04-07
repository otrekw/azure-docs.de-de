---
title: Speichern und Verwalten von Anwendungsdaten in der Cloud mit Visual Studio App Center und Azure-Diensten
description: Erfahren Sie mehr über Dienste wie Visual Studio App Center, mit denen Sie die Daten einer mobilen Anwendung in der Cloud speichern und verwalten können.
author: codemillmatt
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 166847325fa9094136f1c2a143f1751420f05f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240913"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Speichern, Synchronisieren und Abfragen von Daten einer mobilen Anwendung aus der Cloud
Unabhängig von der Art der Anwendung, die Sie erstellen, werden Sie wahrscheinlich Daten erzeugen und verarbeiten. Die Benutzer Ihrer Anwendung haben hohe Erwartungen. Sie möchten, dass die Anwendung unter allen Umständen schnell und reibungslos funktioniert. Die meisten Anwendungen funktionieren außerdem auf mehreren Geräten. Möglicherweise greifen Sie von einem Desktop oder einem mobilen Gerät aus auf Ihre Anwendung zu. Mehrere Benutzer könnten die Anwendung gleichzeitig nutzen und Daten teilen, wobei sie erwarten, dass sie sofortigen Zugriff in Echtzeit auf die Daten erhalten.

Die Benutzer Ihrer Anwendung werden nicht immer über eine Internetverbindung verfügen. Anwendungen sind so konzipiert, und von Anwendungen wird erwartet, dass sie mit und ohne Internetverbindung funktionieren. Entwickler müssen die richtige Lösung für das Speichern und Synchronisieren ihrer Daten in der Cloud auswählen, um ein hervorragendes Kundenerlebnis für ihre Anwendung zu bieten.

Microsoft stellt eine Vielzahl von Diensten bereit, mit denen es überflüssig wird, Server einzurichten, Datenbanken auszuwählen oder sich Gedanken über die Skalierbarkeit oder Sicherheit zu machen. Vor diesem Hintergrund können Sie größtmögliche Benutzerfreundlichkeit bereitstellen. Diese Dienste bieten eine großartige Entwicklererfahrung, mit der Sie Anwendungsdaten mithilfe von SQL- oder NoSQL-APIs in der Cloud speichern können. Sie können außerdem Daten auf allen Geräten synchronisieren und die Anwendung für die Zusammenarbeit mit oder ohne Netzwerkverbindung einrichten, um das Erstellen von skalierbaren und robusten Anwendungen zu unterstützen.

Verwenden Sie die folgenden Dienste, um Daten einer mobilen Anwendung in der Cloud zu verwalten und zu speichern.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Data](/appcenter/data/) ist ein Datenverwaltungsdienst. Er ermöglicht es Anwendungen, Anwendungsdaten in der Cloud auf verschiedenen Geräten und Plattformen in Online- und Offlineszenarien zu verwalten, zu speichern und zu synchronisieren. Basierend auf Azure Cosmos DB wird App Center Data skaliert, wenn Ihre Benutzerbasis und die Anzahl der Anwendungen wachsen. Dadurch werden niedrige Wartezeiten, Hochverfügbarkeit und eine hohe Skalierbarkeit für alle Ihre Daten sichergestellt.

**Wichtige Features**
- Einfache Bereitstellung in einer neuen Azure Cosmos DB-Datenbank oder in einer vorhandenen Azure Cosmos DB-Datenbank aus dem Visual Studio App Center-Portal.
- Unterstützung von NoSQL-Datenbanken, um Anwendungsdaten problemlos zu speichern, zu synchronisieren und abzufragen.
- Dieser Dienst, der auf Azure Cosmos DB aufsetzt, erbt die meisten der wichtigen Features, die von Azure Cosmos DB bereitgestellt werden, und kann global skalieren, damit Ihre Geschäftsanforderungen erfüllt werden.
- Funktionalität für Online- und Offlinesynchronisierung, um Daten auf Geräten zu synchronisieren.
- Für mobile Clients vorgesehene SDKs, mit denen Sie private Anwendungsdaten problemlos verwalten können.
- Plattformunterstützung für iOS, Android, Xamarin und React Native.

**Referenzen**
- [Registrieren bei Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [Erste Schritte mit App Center Data](/appcenter/data/getting-started)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ist ein global verteilter Datenbankdienst mit Unterstützung mehrerer Modelle. Sie können ihn verwenden, um global skalierbare Anwendungen zu erstellen. Mit Azure Cosmos DB können Sie Durchsatz und Speicher elastisch und unabhängig voneinander über eine beliebige Anzahl von Azure-Regionen weltweit skalieren. Unter Verwendung Ihrer bevorzugten API-Oberflächen können Sie den schnellen Datenzugriff im einstelligen Millisekundenbereich nutzen. Zu diesen Oberflächen zählen SQL, MongoDB, Cassandra, Tables oder Gremlin. Azure Cosmos DB bietet einzigartig umfassende Vereinbarungen zum Service (Service Level Agreements, SLAs) für Durchsatz, Latenz, Verfügbarkeit und Konsistenz.

**Wichtige Features**
- Unterstützt eine Vielzahl von APIs einschließlich SQL (Core)-API, Cassandra-API, MongoDB-API, Gremlin-API und Tabellen-API.
- Die globale, sofort einsetzbare Verteilung repliziert Ihre Daten an jeden Ort, an dem sich Ihre Benutzer befinden. Ihre Benutzer können mit einem Replikat der Daten interagieren, das ihnen räumlich am nächsten liegt.
- Keine Schema- oder Indexverwaltung, da die Datenbank-Engine vollständig schemaunabhängig ist.
- Universelle regionale Präsenz, weil Azure Cosmos DB in allen Azure-Regionen weltweit verfügbar ist, einschließlich 54+ Regionen in der Public Cloud.
- Mehrere genau definierte Konsistenzoptionen, denn das Azure Cosmos DB-Protokoll für die Multimasterreplikation bietet fünf klar definierte Konsistenzoptionen. Diese fünf Optionen sind „Stark“, „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „Letztlich“.
- 99,999 % Verfügbarkeit für Lese- und Schreibvorgänge.
- Programmgesteuertes (oder über das Azure-Portal) Aufrufen des regionalen Failovers Ihres Azure Cosmos DB-Kontos, um sicherzustellen, dass Ihre Anwendung so konzipiert ist, dass sie auch nach einem regionalen Notfall weiter funktioniert.
- Weltweit garantiert niedrige Latenz beim 99. Perzentil.

**Referenzen**
- [Azure portal](https://portal.azure.com) 
- [Dokumentation für Azure Cosmos DB](/azure/cosmos-db/introduction)

## <a name="azure-sql-database"></a>Azure SQL-Datenbank
 [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) ist ein relationaler verwalteter Datenbankdienst für allgemeine Zwecke. Sie können ihn verwenden, um eine hochverfügbare und hochleistungsfähige Datenspeicherebene für Anwendungen und Lösungen in der Azure-Cloud zu erstellen.

**Wichtige Features**
- **Modelle und Tools für elastische Datenbanken:** Mit einer elastischen Datenbank können Entwickler Ressourcen zwischen einer Gruppe von Datenbanken zum Zwecke der Skalierung in einem Pool bündeln. Um diese Ressourcen administrativ zu verwalten, senden Sie ein Skript als Auftrag. Die SQL-Datenbank führt dann das Skript in den Datenbanken aus.
- **Hohe Leistung:** Anwendungen mit hohem Durchsatz können die neueste Version nutzen. Sie liefert 25 % mehr Premium-Datenbankleistung.
- **Sicherungen, Replikation und Hochverfügbarkeit:** Integrierte Replikation und eine von Microsoft unterstützte Vereinbarung zum Servicelevel (SLA) bieten Anwendungskontinuität und Schutz vor katastrophalen Ereignissen. Mit der aktiven Georeplikation können Sie Failover und Self-Service-Wiederherstellung konfigurieren, die vollständige Kontrolle über die „Hoppla-Wiederherstellung“ bieten. Die Datenwiederherstellung aus Datensicherungen ist bis zu 35 Tage verfügbar.
- **Wartung gegen null:** Automatische Software ist Bestandteil des Diensts. Integrierte Systemreplikate tragen dazu bei, inhärenten Datenschutz, inhärente Datenbankbetriebszeit und inhärente Systemstabilität zu gewährleisten. Systemreplikate werden automatisch auf neue Computer verschoben. Sie werden bei Ausfällen älterer Computer sofort bereitgestellt.
- **Sicherheit**: SQL-Datenbank bietet eine Reihe von Sicherheitsfunktionen, mit denen Sie sowohl Organisations- als auch branchenspezifische Konformitätsrichtlinien einhalten:
    - Überwachung bietet Entwicklern die Möglichkeit, konformitätsbezogene Aufgaben auszuführen und sich über Aktivitäten zu informieren.
    - Entwickler und IT-Experten können Richtlinien auf Datenbankebene implementieren, um den Zugriff auf vertrauliche Daten mithilfe der Funktionen für Sicherheit auf Zeilenebene, dynamische Datenmaskierung und Transparent Data Encryption für Azure SQL-Datenbank einzuschränken.
    - Azure SQL-Datenbank wird von anerkannten Cloudauditoren im Rahmen wichtiger Azure-Konformitätszertifizierungen und -genehmigungen überprüft, z. B. HIPAA-Geschäftspartnerverträge, ISO/IEC 27001:2005, FedRAMP und EU-Modellklauseln.

**Referenzen**
- [Azure portal](https://portal.azure.com) 
- [Dokumentation zu Azure SQL-Datenbank](/azure/sql-database/) 
