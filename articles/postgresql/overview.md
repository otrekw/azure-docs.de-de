---
title: Übersicht über den relationalen Datenbankdienst Azure Database for PostgreSQL
description: Bietet eine Übersicht über den relationalen Datenbankdienst Azure-Datenbank für PostgreSQL.
author: jonels-msft
ms.author: jonels
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 11/25/2019
ms.openlocfilehash: 9ea0610811f6906526afe55d577e04a8decd5f49
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74481666"
---
# <a name="what-is-azure-database-for-postgresql"></a>Was ist Azure-Datenbank für PostgreSQL?
Azure Database for PostgreSQL ist ein relationaler Datenbankdienst in der Microsoft Cloud für Entwickler. Er basiert auf der Communityversion der Open-Source-[PostgreSQL](https://www.postgresql.org/)-Datenbank-Engine und wird mit zwei Bereitstellungsoptionen angeboten: „Einzelserver“ und „Hyperscale (Citus)“

## <a name="azure-database-for-postgresql---single-server"></a>Azure Database for PostgreSQL (Einzelserver)
Die Bereitstellungsoption „Einzelserver“ bietet Folgendes:

- Integrierte [Hochverfügbarkeit](concepts-high-availability.md) ohne zusätzliche Kosten (SLA mit 99,99 %)
- Vorhersagbare Leistung inklusive nutzungsbasierter Bezahlung
- [Vertikale Skalierung nach Bedarf](concepts-pricing-tiers.md) in Sekundenschnelle
- [Überwachung und Warnung](concepts-monitoring.md) zum Bewerten Ihres Servers
- Sicherheit und Konformität auf Unternehmensniveau
- [Schutz](concepts-security.md) von sensiblen Daten im ruhenden Zustand und während der Übertragung
- [Automatische Sicherungen und Point-in-Time-Wiederherstellung](concepts-business-continuity.md) für bis zu 35 Tage


All diese Funktionen erfordern fast keine Administration und werden alle ohne zusätzliche Kosten bereitgestellt. Sie ermöglichen es Ihnen, sich auf die schnelle Anwendungsentwicklung und die Beschleunigung der Markteinführung zu konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von VMs und der Infrastruktur aufzuwenden. Sie können Ihre Anwendung weiterhin mit den Open-Source-Tools und der Plattform Ihrer Wahl entwickeln, ohne sich neue Fähigkeiten aneignen zu müssen.

Für die Bereitstellungsoption „Einzelserver“ stehen drei Tarife zur Verfügung: „Basic“, „Allgemein“ und „Arbeitsspeicheroptimiert“. Jeder Tarif bietet unterschiedliche Ressourcenfunktionen für Ihre Datenbankworkloads. Sie können Ihre erste App mit einer kleinen Datenbank für wenige USD im Monat erstellen und die Skalierung dann so ändern, dass sie den Anforderungen Ihrer Lösung entspricht. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie bezahlen nur für die benötigten Ressourcen und auch nur dann, wenn Sie sie verwenden. Einzelheiten hierzu finden Sie unter  [Tarife](concepts-pricing-tiers.md).

## <a name="azure-database-for-postgresql---hyperscale-citus"></a>Azure Database for PostgreSQL: Hyperscale (Citus)
Bei der Option „Hyperscale (Citus)“ werden Abfragen mithilfe von Sharding horizontal über mehrere Computer hinweg skaliert. Die Abfrage-Engine parallelisiert eingehende SQL-Abfragen auf diesen Servern, um die Antwortzeiten bei großen Datasets zu verkürzen. Sie eignet sich für Anwendungen, die eine größere Skalierbarkeit und höhere Leistung erfordern, und wird im Allgemeinen für Workloads mit bis zu 100 GB an Daten (oder auch mehr) eingesetzt.

Die Bereitstellungsoption „Hyperscale (Citus)“ bietet Folgendes:

- Horizontale Skalierung auf mehreren Computern mithilfe von Sharding
- Parallelisierung von Abfragen auf den Servern für kürzere Antwortzeiten bei großen Datasets
- Hervorragende Unterstützung für mehrinstanzenfähige Anwendungen, operative Echtzeitanalyse und Transaktionsworkloads mit hohem Durchsatz

Bei der Option „Hyperscale (Citus)“ können für PostgreSQL erstellte Anwendungen verteilte Abfragen mit [Standardverbindungsbibliotheken](./concepts-connection-libraries.md) und minimalen Änderungen ausführen.

## <a name="contacts"></a>Kontakte
Sollten Sie Fragen oder Vorschläge zur Verwendung von Azure Database for PostgreSQL haben, senden Sie eine E-Mail an das Azure Database for PostgreSQL-Team ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Wenden Sie sich bei allgemeinen Fragen an diese Adresse, anstatt ein Supportticket zu öffnen.

Weitere Kontaktmöglichkeiten:
- Wenn Sie den Azure-Support kontaktieren oder ein Problem mit Ihrem Konto beheben lassen möchten, [erstellen Sie im Azure-Portal ein Ticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).

## <a name="next-steps"></a>Nächste Schritte
- Auf der Seite mit der [Preisgestaltung](https://azure.microsoft.com/pricing/details/postgresql/) finden Sie Kostenvergleiche und Rechner.
- Erstellen Sie Ihre erste [Einzelserverinstanz](./quickstart-create-server-database-portal.md) von Azure Database for PostgreSQL oder [Hyperscale (Citus)](./quickstart-create-hyperscale-portal.md)
- Erstellen Ihrer ersten App in Python, PHP, Ruby, C\#, Java oder Node.js: [Verbindungsbibliotheken](./concepts-connection-libraries.md)
