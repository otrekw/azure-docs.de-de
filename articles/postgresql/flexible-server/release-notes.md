---
title: Versionshinweise zu Azure Database for PostgreSQL – Flexible Server
description: Hier finden Sie Versionshinweise zu Azure Database for PostgreSQL – Flexible Server.
author: sr-msft
ms.author: srranga
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 05/25/2021
ms.openlocfilehash: 4ed84ab94173961412a7769da89136c39413a92e
ms.sourcegitcommit: e39ad7e8db27c97c8fb0d6afa322d4d135fd2066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111983076"
---
# <a name="release-notes---azure-database-for-postgresql---flexible-server"></a>Versionshinweise: Azure Database for PostgreSQL – Flexible Server

Auf dieser Seite finden Sie aktuelle Neuigkeiten und Updates zu Featureergänzungen, Unterstützung von Engine-Versionen und Erweiterungen sowie weitere Ankündigungen, die für Flexible Server – PostgreSQL relevant sind.

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

## <a name="release-may-25-2021"></a>Release: 25. Mai 2021

* Unterstützung für [PostgreSQL-Hauptversion 13](./concepts-supported-versions.md)
* Unterstützung für Erweiterungen, pg_partman, pg_cron und pgaudit. Auf der [Seite „Erweiterungen“](./concepts-extensions.md) finden Sie Versionen, die von den einzelnen Hauptversionen unterstützt werden.
* Verschiedene Fehlerbehebungen, Stabilitäts- und Leistungsverbesserungen

## <a name="release-april-26-2021"></a>Release: 26. April 2021

* Unterstützung für die [aktuellen PostgreSQL-Nebenversionen](./concepts-supported-versions.md) 12.6 und 11.11 bei neuen Servererstellungen
* Unterstützung für die [private DNS-Zone](./concepts-networking.md#private-access-vnet-integration) im virtuellen Netzwerk (VNet)
* Unterstützung für die Auswahl der Verfügbarkeitszone während der Zeitpunktwiederherstellung
* Unterstützung für neue [Regionen](./overview.md#azure-regions) wie „Australien, Osten“, „Kanada, Mitte“ und „Frankreich, Mitte“
* Unterstützung für die [integrierte PgBouncer-Verbindungspoolfunktion](./concepts-pgbouncer.md) 
<!--- * Support for [pglogical](https://github.com/2ndQuadrant/pglogical) extension version 2.3.2. -->
* [Intelligente Leistung](concepts-query-store.md) in der öffentlichen Vorschauversion
* Verschiedene Fehlerbehebungen, Stabilitäts- und Leistungsverbesserungen

## <a name="contacts"></a>Kontakte

Sollten Sie Fragen oder Vorschläge im Zusammenhang mit Azure Database for PostgreSQL Flexible Server haben, senden Sie eine E-Mail an das zuständige Azure Database for PostgreSQL-Team ([@Ask Azure DB for PostgreSQL](mailto:AskAzureDBforPostgreSQL@service.microsoft.com)). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für den technischen Support.

Weitere Kontaktmöglichkeiten:

- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597976-azure-database-for-postgresql).
  

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun eine Einführung in den Bereitstellungsmodus für Azure Database for PostgreSQL – Flexible Server gelesen haben, sind Sie bereit, Ihren ersten Server zu erstellen: [Erstellen einer Instanz von Azure Database for PostgreSQL – Flexible Server im Azure-Portal](./quickstart-create-server-portal.md)