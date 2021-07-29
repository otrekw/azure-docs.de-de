---
title: Unterstützte Versionen für Azure Database for PostgreSQL – Flexible Server
description: In diesem Artikel werden die unterstützten Haupt- und Nebenversionen von PostgreSQL in Azure Database for PostgreSQL – Flexible Server beschrieben.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/19/2021
ms.openlocfilehash: 07663ac14dcfb3e323a2edeadf886a715ecac3a0
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/26/2021
ms.locfileid: "110463722"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Unterstützte Haupt- und Nebenversionen von PostgreSQL für Azure Database for PostgreSQL – Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Azure Database for PostgreSQL – Flexible Server unterstützt derzeit die folgenden Hauptversionen:

## <a name="postgresql-version-13"></a>PostgreSQL Version 13

Das aktuelle Nebenrelease ist **13.2**. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Release finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/12/static/release-13-2.html). Mit dieser Nebenversion werden neue Server erstellt. 

## <a name="postgresql-version-12"></a>PostgreSQL Version 12

Die aktuelle Nebenversion ist **12.6**. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Release finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-6.html). Mit dieser Nebenversion werden neue Server erstellt. Ihre vorhandenen Server werden automatisch auf die neueste unterstützte Nebenversion in Ihrem künftigen geplanten Wartungsfenster aktualisiert.

## <a name="postgresql-version-11"></a>PostgreSQL Version 11

Das aktuelle Nebenrelease ist **11.11**. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Release finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-11.html). Mit dieser Nebenversion werden neue Server erstellt. Ihre vorhandenen Server werden automatisch auf die neueste unterstützte Nebenversion in Ihrem künftigen geplanten Wartungsfenster aktualisiert.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL Version 10 und früher

PostgreSQL Version 10 und frühere Versionen werden für Azure Database for PostgreSQL – Flexible Server nicht unterstützt. Verwenden Sie die Bereitstellungsoption [Einzelserver](../concepts-supported-versions.md), wenn Sie eine ältere Version benötigen.

## <a name="managing-upgrades"></a>Verwalten von Upgrades

Das PostgreSQL-Projekt gibt regelmäßig kleinere Releases aus, um gemeldete Fehler zu beheben. Azure Database for PostgreSQL führt automatische Patches der Server mit diesen Nebenreleases während der monatlichen Dienstbereitstellung durch.

Automation für ein Upgrade auf die Hauptversion wird noch nicht unterstützt. Beispielsweise erfolgt derzeit kein automatisches Upgrade von PostgreSQL 11 auf PostgreSQL 12.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->