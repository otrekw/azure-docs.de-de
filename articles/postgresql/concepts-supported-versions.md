---
title: Unterstützte Versionen – Azure Database for PostgreSQL (Einzelserver)
description: Beschreibung der unterstützten Haupt- und Nebenversionen von Postgres in Azure Database for PostgreSQL (Einzelserver)
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/03/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 61dd98028b7342290984615ea19b561b48aaeadb
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792241"
---
# <a name="supported-postgresql-major-versions"></a>Unterstützte PostgreSQL-Hauptversionen
Microsoft strebt in Azure Database for PostgreSQL (Einzelserver) die Unterstützung von n-2 Versionen der PostgreSQL-Engine an. Das bedeutet, dass die aktuelle Hauptversion (n) und die beiden vorherigen Hauptversionen (-2) unterstützt werden.

Azure Database for PostgreSQL unterstützt derzeit die folgenden Hauptversionen:

## <a name="postgresql-version-11"></a>PostgreSQL Version 11
Das aktuelle Nebenrelease ist 11.5. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-5.html).

## <a name="postgresql-version-10"></a>PostgreSQL Version 10
Das aktuelle Nebenrelease ist 10.10. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/10/static/release-10-10.html).

## <a name="postgresql-version-96"></a>PostgreSQL Version 9.6
Das aktuelle Nebenrelease ist 9.6.15. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.6/static/release-9-6-15.html).

## <a name="postgresql-version-95"></a>PostgreSQL Version 9.5
Das aktuelle Nebenrelease ist 9.5.19. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/9.5/static/release-9-5-19.html).

## <a name="managing-upgrades"></a>Verwalten von Upgrades
Das PostgreSQL-Projekt gibt regelmäßig kleinere Releases aus, um gemeldete Fehler zu beheben. Azure Database for PostgreSQL führt automatische Patches der Server mit diesen Nebenreleases während der monatlichen Dienstbereitstellung durch. 

Automatische Upgrades von Hauptversionen werden nicht unterstützt. Es erfolgt beispielsweise kein automatisches Upgrade von PostgreSQL 9.5 auf PostgreSQL 9.6. Um ein Upgrade auf die nächste Hauptversion durchzuführen, erstellen Sie ein [Datenbankspeicherabbild, und führen Sie eine Wiederherstellung](./howto-migrate-using-dump-and-restore.md) auf einem Server aus, der mit der neuen Engine-Version erstellt wurde.

### <a name="version-syntax"></a>Versionssyntax
Vor PostgreSQL-Version 10 wurde ein _Hauptversionsupgrade_ von der [PostgreSQL-Versionsrichtlinie](https://www.postgresql.org/support/versioning/) als eine Erhöhung der ersten _oder_ zweiten Versionsziffer betrachtet. Beispielsweise wurde ein Upgrade von 9.5 auf 9.6 als _Hauptversionsupgrade_ angesehen. Ab Version 10 gilt nur eine Änderung der ersten Ziffer als Hauptversionsupgrade. So ist der Schritt von 10.0 auf 10.1 beispielsweise ein _Nebenreleaseupgrade_. Version 10 auf 11 ist ein _Hauptversionsupgrade_.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu unterstützten PostgreSQL-Erweiterungen finden Sie im [Dokument zu Erweiterungen](concepts-extensions.md).
