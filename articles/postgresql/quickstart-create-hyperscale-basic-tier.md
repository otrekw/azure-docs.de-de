---
title: 'Schnellstart: Erstellen einer Servergruppe im Basic-Tarif – Hyperscale (Citus) – Azure Database for PostgreSQL'
description: Erste Schritte mit Azure Database for PostgreSQL und Hyperscale (Citus) im Basic-Tarif
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 04/07/2021
ms.openlocfilehash: 07ace217e5299662a1c3145a225abc25f4f1f337
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023879"
---
# <a name="create-a-hyperscale-citus-basic-tier-server-group-in-the-azure-portal"></a>Erstellen einer Hyperscale (Citus)-Servergruppe im Basic-Tarif im Azure-Portal

Azure Database for PostgreSQL Hyperscale (Citus) ist ein verwalteter Dienst, mit dem Sie hochverfügbare PostgreSQL-Datenbanken in der Cloud ausführen, verwalten und skalieren können. Der [Basic-Tarif](concepts-hyperscale-tiers.md) ist eine praktische Bereitstellungsoption für die anfängliche Entwicklung sowie für Eingangstests.

In dieser Schnellstartanleitung erfahren Sie, wie Sie über das Azure-Portal eine Hyperscale (Citus)-Servergruppe im Basic-Tarif erstellen. Sie stellen die Servergruppe bereit und überprüfen, ob Sie eine Verbindung mit ihr herstellen können, um Abfragen auszuführen.

> [!IMPORTANT]
> Der Basic-Tarif von Hyperscale (Citus) befindet sich derzeit in der öffentlichen Vorschau.  Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Vorschaufeatures für PostgreSQL – Hyperscale (Citus)](hyperscale-preview-features.md).

[!INCLUDE [azure-postgresql-hyperscale-create-basic-tier](../../includes/azure-postgresql-hyperscale-create-basic-tier.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie erfahren, wie Sie eine Hyperscale-Servergruppe (Citus) bereitstellen. Sie haben mithilfe von psql eine Verbindung mit ihr hergestellt, haben ein Schema erstellt und Daten verteilt.

- Tutorial zum [Erstellen skalierbarer mehrinstanzenfähiger Anwendungen](./tutorial-design-database-hyperscale-multi-tenant.md)
- Bestimmen der besten [Anfangsgröße](howto-hyperscale-scale-initial.md) für Ihre Servergruppe
