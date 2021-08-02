---
title: Neuerungen in Azure Database for MySQL Single Server
description: Hier erfahren Sie mehr zu Azure Database for MySQL Single Server, einem relationalen Datenbankdienst in der Microsoft-Cloud, der auf MySQL Community Edition basiert.
author: hjtoland3
ms.service: mysql
ms.author: jtoland
ms.custom: mvc
ms.topic: conceptual
ms.date: 05/05/2021
ms.openlocfilehash: 6b77ede348073dd09c0ba44bfe282d3bf546a092
ms.sourcegitcommit: c072eefdba1fc1f582005cdd549218863d1e149e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111958537"
---
# <a name="whats-new-in-azure-database-for-mysql---single-server"></a>Neuerungen in Azure Database for MySQL Single Server

Azure Database for MySQL ist ein relationaler Datenbankdienst in der Microsoft-Cloud. Der Dienst basiert auf der [Datenbank-Engine MySQL Community Edition](https://www.mysql.com/products/community/) (verfügbar unter der GPLv2-Lizenz) und unterstützt die Versionen 5.6, 5.7 und 8.0. Bei [Azure Database for MySQL Single Server](./overview.md#azure-database-for-mysql---single-server) handelt es sich um einen Bereitstellungsmodus, der einen vollständig verwalteten Datenbankdienst mit minimalen Anforderungen für die Anpassung der Datenbank bietet. Die Single Server-Plattform kann die meisten Funktionen zur Datenbankverwaltung bereitstellen, z. B. Patching, Sicherungen, Hochverfügbarkeit sowie Sicherheit, alle mit minimaler Benutzerkonfiguration und -steuerung.

In diesem Artikel werden neue Releases und Features in Azure Database for MySQL Single Server ab Januar 2021 zusammengefasst. Auflistungen werden in umgekehrter chronologischer Reihenfolge angezeigt, die neuesten Updates zuerst.

## <a name="february-2021"></a>Februar 2021

Dieses Release von Azure Database for MySQL Single Server enthält die folgenden Updates.

- Neue gespeicherte Prozeduren wurden hinzugefügt, um den globalen Transaktionsbezeichner (GTID) für eingehende Daten für den Großspeicherserver der Version 5.7 und 8.0 zu unterstützen.
- Mit dem Update werden die MySQL-Versionen bis 5.6.50 und 5.7.32 unterstützt.

## <a name="january-2021"></a>Januar 2021

Dieses Release von Azure Database for MySQL Single Server enthält die folgenden Updates.

- „Kennwort zurücksetzen“ wurde aktiviert, um die erste Administratorberechtigung automatisch zu korrigieren.
- Der Serverparameter `auto_increment_increment/auto_increment_offset` und `session_track_gtids` wurden verfügbar gemacht.
- Neue gespeicherte Prozeduren zum Steuern der Sicherung/Wiederherstellung des InnoDB-Pufferpools wurden hinzugefügt.
- Der InnoDB-Aufwärmparameter für einen großen Speicherserver wurde verfügbar gemacht.

## <a name="contacts"></a>Kontakte

Wenn Sie Fragen oder Vorschläge zur Arbeit mit Azure Database for MySQL haben, wenden Sie sich an das Azure Database for MySQL-Team ([@AskAzure DB for MySQL](mailto:AskAzureDBforMySQL@service.microsoft.com)). Bei dieser E-Mail-Adresse handelt es sich nicht um einen Alias für technischen Support.

Weitere Kontaktmöglichkeiten:

- Wenn Sie den Azure-Support kontaktieren möchten, [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Um ein Problem mit Ihrem Konto zu beheben, richten Sie im Azure-Portal eine [Anfrage an den Support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
- Wenn Sie Feedback abgeben oder Vorschläge für neue Features einreichen möchten, erstellen Sie einen Eintrag über [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu den [Preisen für Azure Database for MySQL](https://azure.microsoft.com/pricing/details/mysql/server/).
- Suchen Sie in der [öffentlichen Dokumentation](./single-server/index.yml) nach Azure Database for MySQL Single Server.
- Lesen Sie Details zur [Problembehandlung für häufige Fehler](./howto-troubleshoot-common-errors.md).