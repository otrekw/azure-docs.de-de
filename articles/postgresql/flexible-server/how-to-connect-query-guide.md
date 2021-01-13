---
title: Verbinden und Abfragen – flexible PostgreSQL-Server
description: Links zu Schnellstarts zeigen, wie Sie eine Verbindung mit Ihrem flexiblen Azure Database for PostgreSQL-Server herstellen und Abfragen ausführen.
services: postgresql
ms.service: postgresql
ms.topic: how-to
author: mksuni
ms.author: sumuth
ms.date: 12/08/2020
ms.openlocfilehash: ee3b1f7db8bdafb1233b32579e032e8c864c37a9
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2020
ms.locfileid: "97364531"
---
# <a name="connect-and-query-overview-for-azure-database-for-postgresql--flexible-server"></a>Übersicht über Verbindungen und Abfragen für Azure Database for PostgreSQL – flexibler Server

Das folgende Dokument enthält Links zu Beispielen, die zeigen, wie Sie eine Verbindung mit Azure Database for PostgreSQL-Einzelserver herstellen und Daten abfragen. Dieser Leitfaden enthält auch TLS-Empfehlungen und Erweiterungen, die Sie zum Herstellen einer Verbindung mit dem Server in den nachstehend aufgeführten unterstützten Sprachen verwenden können.

>[!IMPORTANT]
> Flexible Azure Database for PostgreSQL-Server befinden sich in der **Vorschau**.

## <a name="quickstarts"></a>Schnellstarts

| Schnellstart | BESCHREIBUNG |
|---|---|
|[Pgadmin](https://www.pgadmin.org/)|Mithilfe von „pgadmin“ können Sie eine Verbindung mit dem Server herstellen. Außerdem vereinfacht sie die Erstellung, Wartung und Verwendung von Datenbankobjekten.|
|[psql in Azure Cloud Shell](./quickstart-create-server-cli.md#connect-using-postgresql-command-line-client)|In diesem Artikel wird gezeigt, wie Sie [**psql**](https://www.postgresql.org/docs/current/static/app-psql.html) in [Azure Cloud Shell](../../cloud-shell/overview.md) ausführen, um eine Verbindung mit Ihrem Server herzustellen, und dann Anweisungen zum Abfragen, Einfügen, Aktualisieren und Löschen von Daten in der Datenbank ausführen. Sie können **psql** ausführen, wenn sie in Ihrer Entwicklungsumgebung installiert wurde.|
|[Python](connect-python.md)|In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Python eine Verbindung mit einer Datenbank herstellen und anschließend mithilfe von Work Daten bei Datenbankobjekten abfragen. |
|[Django mit App Service](tutorial-django-app-service-postgres.md)|In diesem Tutorial erfahren Sie, wie Sie mithilfe von Ruby ein Programm erstellen, das eine Verbindung mit einer Datenbank herstellt, und anschließend mithilfe von Work Daten bei Datenbankobjekten abfragen.|

## <a name="tls-considerations-for-database-connectivity"></a>Überlegungen zu TLS für Datenbankverbindungen

Transport Layer Security (TLS) wird von allen Treibern verwendet, die Microsoft zum Herstellen einer Verbindung mit Datenbanken in Azure Database for PostgreSQL bereitstellt oder unterstützt. Eine besondere Konfiguration ist nicht erforderlich, aber erzwingen Sie unbedingt TLS 1.2 für neu erstellte Server. Wenn Sie TLS 1.0 und 1.1 verwenden, empfehlen wir, die TLS-Version für Ihre Server zu aktualisieren. Weitere Informationen finden Sie unter [Konfigurieren von TLS](how-to-connect-tls-ssl.md).

## <a name="postgresql-extensions"></a>PostgreSQL-Erweiterungen

PostgreSQL bietet die Möglichkeit, die Funktionalität Ihrer Datenbank mithilfe von Erweiterungen zu erweitern. Bei Erweiterungen werden mehrere zusammengehörige SQL-Objekte zu einem Paket gebündelt und mit nur einem Befehl in die Datenbank geladen oder daraus entfernt. Nach dem Laden in die Datenbank funktionieren Erweiterungen genauso wie integrierte Features.

- [Postgres 12-Erweiterungen](./concepts-extensions.md#postgres-12-extensions)
- [Postgres 11-Erweiterungen](./concepts-extensions.md#postgres-11-extensions)
- [„dblink“ und „postgres_fdw“](./concepts-extensions.md#dblink-and-postgres_fdw)
- [pg_prewarm](./concepts-extensions.md#pg_prewarm)
- [pg_stat_statements](./concepts-extensions.md#pg_stat_statements)

Weitere Informationen finden Sie unter [Verwenden von PostgreSQL-Erweiterungen auf einem flexiblen Server](concepts-extensions.md).

## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von Daten durch Sicherungen und Wiederherstellungen](../howto-migrate-using-dump-and-restore.md)
- [Migrieren von Daten durch Import und Export](../howto-migrate-using-export-and-import.md)
