---
title: Versionshinweise zu Azure Database for PostgreSQL – Flexible Server
description: Hier finden Sie Versionshinweise zu Azure Database for PostgreSQL – Flexible Server.
author: sr-msft
ms.author: srranga
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 04/26/2021
ms.openlocfilehash: ad417bc44de7d13594f003f7aa1e39b2308204ed
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108064861"
---
# <a name="release-notes---azure-database-for-postgresql---flexible-server"></a>Versionshinweise: Azure Database for PostgreSQL – Flexible Server

Auf dieser Seite finden Sie aktuelle Neuigkeiten und Updates zu Featureergänzungen, Unterstützung von Engine-Versionen und Erweiterungen sowie weitere Ankündigungen, die für Flexible Server – PostgreSQL relevant sind.

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

## <a name="release-april-26-2021"></a>Release: 26. April 2021

* Unterstützung für die [aktuellen PostgreSQL-Nebenversionen](./concepts-supported-versions.md) 12.6 und 11.11 bei neuen Servererstellungen
* Unterstützung für die [private DNS-Zone](./concepts-networking.md#private-access-vnet-integration) im virtuellen Netzwerk (VNet)
* Unterstützung für die Auswahl der Verfügbarkeitszone während der Zeitpunktwiederherstellung
* Unterstützung für neue [Regionen](./overview.md#azure-regions) wie „Australien, Osten“, „Kanada, Mitte“ und „Frankreich, Mitte“
* Unterstützung für die [integrierte PgBouncer-Verbindungspoolfunktion](./concepts-pgbouncer.md) 
* Unterstützung für Version 2.3.2 der [pglogical](https://github.com/2ndQuadrant/pglogical)-Erweiterung
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