---
title: 'Server in Azure Database for PostgreSQL: Flexible Server (Vorschau)'
description: Dieser Artikel enthält Überlegungen und Richtlinien zum Konfigurieren und Verwalten von Azure Database for PostgreSQL Flexible Server.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 5cd4454d5b77d18940f6693a25a691a01f4fedec
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90931732"
---
# <a name="servers---azure-database-for-postgresql---flexible-server"></a>Server: Azure Database for PostgreSQL Flexible Server

> [!IMPORTANT]
> Azure Database for PostgreSQL – Flexible Server befindet sich in der Vorschau.

Dieser Artikel enthält Überlegungen und Richtlinien für die Arbeit mit Azure Database for PostgreSQL Flexible Server.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>Was ist ein Azure Database for PostgreSQL-Server?

Ein Server der Bereitstellungsoption Azure Database for PostgreSQL Flexible Server fungiert als zentraler Verwaltungspunkt für mehrere Datenbanken. Sie weist dasselbe PostgreSQL-Serverkonstrukt auf, das Sie möglicherweise aus lokalen Umgebungen kennen. Im Besonderen ist der PostgreSQL-Dienst verwaltet, bietet Leistungsgarantien und stellt Zugriff sowie Funktionen auf Serverebene bereit.

Ein Azure-Datenbank für PostgreSQL-Server weist folgende Eigenschaften auf:

- Sie wird im Rahmen eines Azure-Abonnements erstellt.
- Sie stellt die übergeordnete Ressource für Datenbanken dar.
- Sie stellt einen Namespace für Datenbanken bereit.
- Sie ist ein Container mit einer Semantik von hoher Lebensdauer. Beim Löschen eines Servers werden die enthaltenen Datenbanken gelöscht.
- Sie stellt Ressourcen in einer Region zusammen.
- Sie stellt einen Verbindungsendpunkt für den Server- und Datenbankzugriff bereit.
- Sie stellt den Bereich für Verwaltungsrichtlinien bereit, die auf die jeweiligen Datenbanken angewendet werden (Anmeldungen, Firewalls, Benutzer, Rollen, Konfigurationen etc.).
- Sie ist in mehreren Versionen verfügbar. Weitere Informationen finden Sie unter [Unterstützte PostgreSQL-Datenbankversionen](concepts-supported-versions.md).
- Sie kann von Benutzern erweitert werden. Weitere Informationen finden Sie im Artikel zu [PostgreSQL-Erweiterungen](concepts-extensions.md).

Sie können in einer Instanz von Azure-Datenbank für PostgreSQL-Server eine oder mehrere Datenbanken erstellen. Sie können eine Einzeldatenbank pro Server erstellen, die alle Ressourcen nutzt, oder Sie können mehrere Datenbanken erstellen, um die Ressourcen gemeinsam zu verwenden. Die Preise gelten pro Server und basieren auf der Konfiguration des Tarifs, der virtuellen Kerne und des Speichers (GB). Weitere Informationen finden Sie unter [Compute- und Speicheroptionen](concepts-compute-storage.md).

## <a name="how-do-i-connect-and-authenticate-to-the-database-server"></a>Wie verbinde ich mich mit einem und authentifiziere mich für einen Datenbankserver?

Mithilfe der folgenden Elemente kann ein sicherer Zugriff auf Ihre Datenbank sichergestellt werden:

|||
|:--|:--|
| **Authentifizierung und Autorisierung** | Der Azure-Datenbank für PostgreSQL-Server unterstützt die systemeigene PostgreSQL-Authentifizierung. Mithilfe der Anmeldeinformationen für Serveradministrator können Sie eine Verbindung zum Server herstellen und diesen authentifizieren. |
| **Protokoll** | Der Dienst unterstützt ein nachrichtenbasiertes Protokoll, das von PostgreSQL verwendet wird. |
| **TCP/IP** | Das Protokoll wird über TCP/IP- und Unix-Domänensockets unterstützt. |
| **Firewall** | Zum Schutz Ihrer Daten verhindert eine Firewallregel jeglichen Zugriff auf Ihren Server und auf dessen Datenbanken, solange Sie nicht angeben, welche Computer zugriffsberechtigt sind. Weitere Informationen finden Sie unter [Firewallregeln für Azure-Datenbank für PostgreSQL-Server](how-to-manage-firewall-portal.md). |

## <a name="managing-your-server"></a>Verwalten Ihres Servers

Sie können Azure Database for PostgreSQL-Server mithilfe des [Azure-Portals](https://portal.azure.com) oder der [Azure CLI](/cli/azure/postgres) verwalten.

Beim Erstellen eines Servers richten Sie die Anmeldeinformationen für Ihren Administratorbenutzer ein. Der Administratorbenutzer ist der Benutzer mit den höchsten Berechtigungen für den Server. Er gehört der Rolle „azure_pg_admin“ an. Diese Rolle verfügt nicht über vollständige Administratorrechte. 

Das PostgreSQL-Superuser-Attribut ist dem „azure_superuser“ zugewiesen, der dem verwalteten Dienst angehört. Sie haben keinen Zugriff auf diese Rolle.

Ein Azure Database for PostgreSQL-Server verfügt über Standarddatenbanken: 

- **postgres**: Eine Standarddatenbank, mit der Sie nach der Erstellung Ihres Servers eine Verbindung herstellen können.
- **azure_maintenance**: Diese Datenbank wird verwendet, um die Prozesse, die den verwalteten Dienst bereitstellen, von den Benutzeraktionen zu trennen. Sie haben keinen Zugriff auf diese Datenbank.

## <a name="server-parameters"></a>Serverparameter

Die PostgreSQL-Serverparameter bestimmen die Konfiguration des Servers. In Azure Database for PostgreSQL kann die Liste der Parameter über das Azure-Portal oder die Azure CLI angezeigt und bearbeitet werden.

Da es sich um einen verwalteten Dienst für Postgres handelt, stellen die konfigurierbaren Parameter in Azure Database for PostgreSQL eine Teilmenge der Parameter in einer lokalen Postgres-Instanz dar. (Weitere Informationen zu Postgres-Parametern finden Sie in der [PostgreSQL-Dokumentation](https://www.postgresql.org/docs/12/static/runtime-config.html).) Bei der Erstellung wird der Azure Database for PostgreSQL-Server mit Standardwerten für die einzelnen Parameter konfiguriert. Einige Parameter, die einen Serverneustart oder Administratorenzugriff erfordern, damit die Änderungen wirksam werden, können nicht vom Benutzer konfiguriert werden.

## <a name="next-steps"></a>Nächste Schritte

- Einen Überblick über den Dienst finden Sie unter [Azure-Datenbank für PostgreSQL – Überblick](overview.md).
- Weitere Informationen zu den speziellen Ressourcenkontingenten und Einschränkungen für Ihre **Konfiguration** finden Sie unter [Compute- und Speicheroptionen](concepts-compute-storage.md).
- Sie können Serverparameter über das [Azure-Portal](howto-configure-server-parameters-using-portal.md) oder die [Azure CLI](howto-configure-server-parameters-using-cli.md) anzeigen und bearbeiten.
