---
title: Unterstützte Versionen – Hyperscale (Citus) – Azure Database for PostgreSQL
description: PostgreSQL-Versionen in Azure Database for PostgreSQL – Hyperscale (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: d8a584b6ba752e8f9220defa575f519828ba07e6
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023812"
---
# <a name="supported-database-versions-in-azure-database-for-postgresql--hyperscale-citus"></a>Unterstützte Datenbankversionen in Azure Database for PostgreSQL – Hyperscale (Citus)

## <a name="postgresql-versions"></a>PostgreSQL-Versionen

> [!IMPORTANT]
> Anpassbare PostgreSQL-Versionen in Hyperscale (Citus) befindet sich zurzeit in der Vorschau.  Diese Vorschau wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Vorschaufeatures für Hyperscale (Citus)](hyperscale-preview-features.md).

Die PostgreSQL-Version, die in einer Hyperscale (Citus)-Servergruppe ausgeführt wird, kann während der Erstellung angepasst werden. Die Auswahl einer anderen Version als Version 11 ist zurzeit ein Vorschaufeature.

Hyperscale (Citus) unterstützt zurzeit die folgenden Hauptversionen:

### <a name="postgresql-version-13-preview"></a>PostgreSQL, Version 13 (Vorschau)

Das aktuelle Nebenrelease ist 13.2. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/13/static/release-13-2.html).

### <a name="postgresql-version-12-preview"></a>PostgreSQL, Version 12 (Vorschau)

Das aktuelle Nebenrelease ist 12.6. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-6.html).

### <a name="postgresql-version-11"></a>PostgreSQL Version 11

Das aktuelle Nebenrelease ist 11.11. Informationen zu Verbesserungen und Fehlerbehebungen in diesem Nebenrelease finden Sie in der [Dokumentation zu PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-11.html).

### <a name="postgresql-version-10-and-older"></a>PostgreSQL Version 10 und früher

PostgreSQL, Version 10 und frühere Versionen werden bei Azure Database for PostgreSQL – Hyperscale (Citus) nicht unterstützt.

## <a name="citus-and-other-extension-versions"></a>Citus und andere Erweiterungsversionen

Je nachdem, welche PostgreSQL-Version in einer Servergruppe ausgeführt wird, werden auch verschiedene [Versionen von Postgres-Erweiterungen](concepts-hyperscale-extensions.md) installiert.  Insbesondere enthält Postgres 13 Citus 10, und frühere Postgres-Versionen enthalten Citus 9.5.

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich an, welche [Erweiterungen](concepts-hyperscale-extensions.md) in welchen Versionen installiert sind.
* Informieren Sie sich, wie [eine Hyperscale (Citus)-Servergruppe erstellt wird](quickstart-create-hyperscale-portal.md).
