---
title: Versionsrichtlinie –Azure Database for PostgreSQL – Einzelserver und flexibler Server (Vorschau)
description: Beschreibt die Richtlinie zu den unterstützten Haupt- und Nebenversionen von Postgres in Azure Database for PostgreSQL (Einzelserver).
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/03/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: b88d42071beba0ddd5a5627cefbe50229b4d27eb
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93294196"
---
# <a name="azure-database-for-postgresql-versioning-policy"></a>Versionsrichtlinie zu Azure Database for PostgreSQL

Auf dieser Seite wird die Versionsrichtlinie für Azure Database for PostgreSQL beschrieben, die auf die Bereitstellungsmodi für Azure Database for PostgreSQL (Einzelserver) und Azure Database for PostgreSQL (flexibler Server, Vorschau) angewendet werden kann.

## <a name="supported--postgresql-versions"></a>Unterstützte PostgreSQL-Versionen

Azure Database for PostgreSQL unterstützt die folgenden Datenbankversionen:

| Version | Einzelner Server | Flexible Server (Vorschau) |
| ----- | :------: | :----: |
| PostgreSQL 12 |  | X  | 
| PostgreSQL 11 | X | X |
| PostgreSQL 10 | X |  |
| PostgreSQL 9.6 | X |  |
| PostgreSQL 9.5 | X |  |

## <a name="major-version-support"></a>Support für die Hauptversion
Jede Hauptversion von PostgreSQL wird durch Azure Database for PostgreSQL ab dem Datum, an dem Azure mit dem Support für die Version beginnt, bis zu dem Datum unterstützt, an dem die Version durch die PostgreSQL-Community entsprechend der [Versionsrichtlinie für die PostgreSQL-Community](https://www.postgresql.org/support/versioning/) eingestellt wird.

## <a name="minor-version-support"></a>Support für die Nebenversionen
Azure Database for PostgreSQL führt im Rahmen regelmäßiger Wartungsarbeiten Upgrades auf Nebenversionen für die bevorzugte Azure PostgreSQL-Version automatisch durch. 

## <a name="major-version-retirement-policy"></a>Richtlinie zur Einstellung der Hauptversion
Die folgende Tabelle enthält die Details zur Einstellung von PostgreSQL-Hauptversionen. Die Datumsangaben folgen der [Versionsrichtlinie für die PostgreSQL-Community](https://www.postgresql.org/support/versioning/).

| Version | Neues | Startdatum des Azure-Supports | Deaktivierungstermin|
| ----- | ----- | ------ | ----- |
| PostgreSQL 9.5| [Funktionen](https://www.postgresql.org/docs/9.5/release-9-5.html)  | 18. April 2018    | 11. Februar 2021
| [PostgreSQL 9.6](https://www.postgresql.org/about/news/postgresql-96-released-1703/) | [Funktionen](https://wiki.postgresql.org/wiki/NewIn96) | 18. April 2018  | 11. November 2021
| [PostgreSQL 10](https://www.postgresql.org/about/news/postgresql-10-released-1786/) | [Funktionen](https://wiki.postgresql.org/wiki/New_in_postgres_10) | 4. Juni 2018  | 10. November 2022
| [PostgreSQL 11](https://www.postgresql.org/about/news/postgresql-11-released-1894/) | [Funktionen](https://www.postgresql.org/docs/11/release-11.html) | 24. Juli 2019  | 9\. November 2023
| [PostgreSQL 12](https://www.postgresql.org/about/news/postgresql-12-released-1976/) | [Funktionen](https://www.postgresql.org/docs/12/release-12.html) | 22. September 2020  | 14. November 2024

## <a name="retired-postgresql-engine-versions-not-supported-in-azure-postgresql"></a>Eingestellte Versionen der PostgreSQL-Engine werden in Azure PostgreSQL nicht unterstützt

Wenn Sie nach dem Einstellungsdatum für die einzelnen PostgreSQL-Datenbankversionen die eingestellte Version weiterhin ausführen, beachten Sie die folgenden Einschränkungen:
- Da die Community keine weiteren Fehlerbehebungen oder Sicherheitskorrekturen mehr veröffentlicht, patcht Azure for PostgreSQL die eingestellte Datenbank-Engine nicht bei Fehlern oder Sicherheitsproblemen und ergreift keinerlei Sicherheitsmaßnahmen im Hinblick auf die eingestellte Datenbank-Engine. Dies führt möglicherweise zu Sicherheitsrisiken oder anderen Problemen. Azure führt jedoch weiterhin regelmäßige Wartung und Patchen für den Host, das Betriebssystem, die Container sowie alle anderen dienstbezogenen Komponenten durch.
- Wenn ein möglicherweise auftretendes Supportproblem im Zusammenhang mit der PostgreSQL-Datenbank steht, können wir Ihnen keinen Support bieten. In solchen Fällen müssen Sie Ihre Datenbank aktualisieren, damit wir Ihnen Support dafür bieten können.
- Sie können für die eingestellte Version keine neuen Datenbankserver erstellen. Sie können jedoch Zeitpunktwiederherstellungen durchführen und Lesereplikate für Ihre vorhandenen Server erstellen.
- Die neuen von Azure Database for PostgreSQL entwickelten Dienstfunktionen stehen möglicherweise nur für unterstützte Datenbankserverversionen zur Verfügung.
- Betriebszeit-SLAs gelten ausschließlich für dienstbezogene Probleme bei Azure Database for PostgreSQL und nicht für Ausfallzeiten, die durch Fehler im Zusammenhang mit der Datenbank-Engine verursacht werden.  
- Bei einem schwerwiegenden Datenbanksicherheitsrisiko, das in der eingestellten Datenbankversion identifiziert wird, kann Azure ein Upgrade Ihrer Datenbank auf eine höhere Version automatisch durchführen.

## <a name="postgresql-version-syntax"></a>PostgreSQL-Versionssyntax
Vor PostgreSQL-Version 10 wurde ein _Hauptversionsupgrade_ von der [PostgreSQL-Versionsrichtlinie](https://www.postgresql.org/support/versioning/) als eine Erhöhung der ersten _oder_ zweiten Versionsziffer betrachtet. Beispielsweise wurde ein Upgrade von 9.5 auf 9.6 als _Hauptversionsupgrade_ angesehen. Ab Version 10 gilt nur eine Änderung der ersten Ziffer als Hauptversionsupgrade. So ist der Schritt von 10.0 auf 10.1 beispielsweise ein _Nebenreleaseupgrade_. Version 10 auf 11 ist ein _Hauptversionsupgrade_.

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie in „Azure Database for PostgreSQL – Einzelserver“ den Abschnitt [Unterstützte Versionen](./concepts-supported-versions.md).
- Lesen Sie in „Azure Database for PostgreSQL – flexibler Server (Vorschau)“ den Abschnitt [Unterstützte Versionen](flexible-server/concepts-supported-versions.md).
- Informationen dazu, wie Upgrades von Hauptversionen durchgeführt werden, finden Sie in der Dokumentation [Upgrades von Hauptversionen](how-to-upgrade-using-dump-and-restore.md).
- Informationen zu unterstützten PostgreSQL-Erweiterungen finden Sie im [Dokument zu Erweiterungen](concepts-extensions.md).
