---
title: 'Beheben von Verbindungsproblemen – Hyperscale (Citus): Azure Database for PostgreSQL'
description: 'Dieser Artikel erläutert das Beheben von Verbindungsproblemen mit Azure Database for PostgreSQL: Hyperscale (Citus)'
keywords: PostgreSQL-Verbindung, Verbindungszeichenfolge, Verbindungsprobleme, vorübergehender Fehler, Verbindungsfehler
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 10/8/2019
ms.openlocfilehash: e1c6825820ae943d10157279dfe93922a7521b75
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91295616"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---hyperscale-citus"></a>Beheben von Verbindungsproblemen mit Azure Database for PostgreSQL: Hyperscale (Citus)

Verbindungsprobleme können verschiedene Ursachen haben. Beispiele:

* Firewalleinstellungen
* Verbindungstimeout
* Falsche Anmeldeinformationen
* Erreichtes Verbindungslimit für die Servergruppe
* Probleme mit der Infrastruktur des Diensts
* Dienstwartung
* Failover des Koordinatorknotens auf neue Hardware

Im Allgemeinen können Verbindungsprobleme mit Hyperscale (Citus) wie folgt klassifiziert werden:

* Vorübergehende Fehler (kurzlebige oder zeitweilige Fehler)
* Dauerhafte oder nicht vorübergehende Fehler (Fehler, die sich regelmäßig wiederholen)

## <a name="troubleshoot-transient-errors"></a>Problembehandlung bei vorübergehenden Fehlern

Vorübergehende Fehler treten aus verschiedenen Gründen auf. Am häufigsten sind Systemwartung, Hardware- oder Softwarefehler oder Upgrades der V-Kerne des Koordinatorknotens.

Durch Aktivieren der Hochverfügbarkeit für Servergruppenknoten von Hyperscale (Citus) können derartige Probleme automatisch gemindert werden. Die Anwendung sollte dennoch auf kurzfristige Verbindungsunterbrechungen vorbereitet sein. Das Mindern anderer Ereignisse kann gelegentlich länger dauern, z. B. wenn eine große Transaktion eine lang andauernde Wiederherstellung auslöst.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Schritte zum Beheben vorübergehender Verbindungsprobleme

1. Überprüfen Sie das [Microsoft Azure-Dienstdashboard](https://azure.microsoft.com/status) auf alle bekannten Ausfälle, die während des Zeitraums aufgetreten sind, während dessen die Anwendung Fehler gemeldet hat.
2. Anwendungen, die mit einem Clouddienst wie Hyperscale (Citus) verbunden sind, sollten auf vorübergehende Fehler eingestellt sein und ordnungsgemäß reagieren. Anwendungen sollten beispielsweise eine Wiederholungslogik zum Behandeln dieser Fehler implementieren, anstatt sie als Anwendungsfehler an die Benutzer auszugeben.
3. Wenn sich die Servergruppe ihren Ressourcenlimits nähert, kann dies wie ein vorübergehendes Verbindungsproblem aussehen. Hier können das Vergrößern des Arbeitsspeichers der Knoten oder das Hinzufügen von Workerknoten und das Ausgleichen von Daten hilfreich sein.
4. Wenn weiterhin Verbindungsprobleme auftreten, diese länger als 60 Sekunden vorliegen oder pro Tag mehrmals auftreten, senden Sie eine Azure-Supportanfrage, indem Sie auf der [Azure-Support-Website](https://azure.microsoft.com/support/options) die Option **Support anfordern** auswählen.

## <a name="troubleshoot-persistent-errors"></a>Problembehandlung bei dauerhaften Fehlern

Wenn die Anwendung dauerhaft keine Verbindung mit Hyperscale (Citus) herstellen kann, sind die häufigsten Ursachen eine falsche Firewallkonfiguration oder Benutzerfehler.

* Firewallkonfiguration für Koordinatorknoten: Stellen Sie sicher, dass die Serverfirewall von Hyperscale (Citus) so konfiguriert ist, dass Verbindungen von Ihrem Client zulässig sind, einschließlich Proxyserver und Gateways.
* Konfiguration der Clientfirewall: Die Firewall auf dem Client muss Verbindungen mit Ihrem Datenbankserver zulassen. Für einige Firewalls müssen Anwendungen nicht allein nach Namen zugelassen werden, sondern es müssen auch die IP-Adressen und Ports des Servers zugelassen werden.
* Benutzerfehler: Überprüfen Sie die Verbindungszeichenfolge. Möglicherweise verfügen Sie über falsch eingegebene Parameter wie den Servernamen. Sie können Verbindungszeichenfolgen für verschiedene Sprachframeworks und psql im Azure-Portal finden. Navigieren Sie zur Registerkarte **Verbindungszeichenfolgen** in Ihrer Hyperscale-Servergruppe (Citus). Beachten Sie außerdem, dass Hyperscale-Cluster (Citus) nur eine Datenbank aufweisen und der vordefinierte Name **citus** ist.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Schritte zum Beheben dauerhafter Verbindungsprobleme

1. Richten Sie [Firewallregeln](howto-hyperscale-manage-firewall-using-portal.md) so ein, dass die IP-Adresse des Clients zugelassen wird. Richten Sie nur zu Testzwecken vorübergehend eine Firewallregel mit 0.0.0.0 als erster IP-Adresse und 255.255.255.255 als letzter IP-Adresse fest. Durch diese Regel wird der Server für alle IP-Adressen geöffnet. Wird Ihr Konnektivitätsproblem durch die Regel behoben, entfernen Sie sie, und erstellen Sie eine Firewallregel für eine entsprechend eingeschränkte IP-Adresse bzw. einen entsprechend eingeschränkten IP-Adressbereich.
2. Stellen Sie für alle Firewalls zwischen Client und Internet sicher, dass Port 5432 für ausgehende Verbindungen geöffnet ist.
3. Überprüfen Sie die Verbindungszeichenfolge und andere Verbindungseinstellungen.
4. Überprüfen Sie im Dashboard den Dienststatus.

## <a name="next-steps"></a>Nächste Schritte

* Machen Sie sich mit den Konzepten von [Firewall-Regeln in Azure Database for PostgreSQL – Hyperscale (Citus)](concepts-hyperscale-firewall-rules.md) vertraut.
* Informieren Sie sich über das [Verwalten von Firewallregeln für Azure Database for PostgreSQL – Hyperscale (Citus)](howto-hyperscale-manage-firewall-using-portal.md).
