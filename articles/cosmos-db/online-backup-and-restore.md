---
title: Onlinesicherung und bedarfsgesteuerte Wiederherstellung in Azure Cosmos DB
description: In diesem Artikel wird beschrieben, wie die automatische Sicherung und die bedarfsgesteuerte Datenwiederherstellung funktionieren und wie Sie das Sicherungsintervall und die Aufbewahrung in Azure Cosmos DB konfigurieren.
author: kanshiG
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/24/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: f8ec215458e8ebfafb87209516f167d628e98389
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89047627"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Onlinesicherung und bedarfsgesteuerte Wiederherstellung in Azure Cosmos DB

Azure Cosmos DB erstellt in regelmäßigen Abständen automatisch Sicherungen Ihrer Daten. Die automatischen Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit des Datenbankbetriebs. Alle Sicherungskopien werden in einem Speicherdienst getrennt gespeichert, und diese Sicherungen werden zum besseren Schutz vor regionalen Ausfällen global repliziert. Automatische Sicherungen sind in Situationen hilfreich, in denen Sie versehentlich Ihr Azure Cosmos-Konto, die Datenbank oder einen Container löschen oder aktualisieren und später eine Datenwiederherstellung durchführen möchten.

## <a name="automatic-and-online-backups"></a>Automatische Onlinesicherungen

Azure Cosmos DB sorgt nicht nur dafür, dass Ihre Daten, sondern auch die Sicherungen Ihrer Daten überaus redundant und gegen regionale Katastrophen geschützt sind. Die folgenden Schritte zeigen, wie Azure Cosmos DB eine Datensicherung ausführt:

* Azure Cosmos DB erstellt alle 4 Stunden automatisch eine Sicherung Ihrer Datenbank, und es werden standardmäßig immer nur die neuesten beiden Sicherungen gespeichert. Wenn die Standardintervalle für Ihre Workloads nicht ausreichen, können Sie das Sicherungsintervall und den Aufbewahrungszeitraum im Azure-Portal ändern. Sie können die Konfiguration der Sicherung während oder nach der Erstellung des Azure Cosmos-Kontos ändern. Wenn der Container bzw. die Datenbank gelöscht wird, bewahrt Azure Cosmos DB die vorhandenen Momentaufnahmen des angegebenen Containers bzw. der Datenbank 30 Tage lang auf.

* Azure Cosmos DB speichert diese Sicherungen in Azure Blob-Speicher, während sich die eigentlichen Daten lokal in Azure Cosmos DB befinden.

* Um niedrige Latenz zu gewährleisten, wird die Momentaufnahme Ihrer Sicherung in Azure Blob Storage in der Region gespeichert, in der sich auch die aktuelle Schreibregion befindet (bzw. bei einer Multimaster-Konfiguration in **einer** der Schreibregionen). Zum besseren Schutz vor regionalen Katastrophen wird jede Momentaufnahme der Sicherungsdaten in Azure Blob Storage nochmals mithilfe von georedundantem Speicher (GRS) in eine andere Region repliziert. Die Region, in die die Sicherung repliziert wird, richtet sich nach Ihrer Quellregion und dem Regionspaar, das der Quellregion zugeordnet ist. Weitere Informationen finden Sie im Artikel mit der [Liste der georedundanten Azure-Regionspaare](../best-practices-availability-paired-regions.md). Sie können auf diese Sicherung nicht direkt zugreifen. Das Team von Azure Cosmos DB stellt Ihre Sicherung wieder her, wenn Sie dies über eine Supportanfrage anfordern.

   Die folgende Abbildung zeigt, wie ein Azure Cosmos DB-Container (mit allen drei primären physischen Partitionen in „USA, Westen“) in einem Azure Blob Storage-Remotekonto in „USA, Westen“ gesichert und anschließend nach „USA, Osten“ repliziert wird:

  :::image type="content" source="./media/online-backup-and-restore/automatic-backup.png" alt-text="Regelmäßige vollständige Sicherungen aller Cosmos DB-Entitäten in georedundantem Azure Storage" border="false":::

* Die Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit Ihrer Anwendungen. Azure Cosmos DB erstellt die Datensicherung im Hintergrund, ohne Ihren bereitgestellten Durchsatz (Anforderungseinheiten, RUs) zu beanspruchen bzw. die Leistung oder Verfügbarkeit Ihrer Datenbank zu beeinträchtigen.

## <a name="options-to-manage-your-own-backups"></a>Optionen für das Verwalten Ihrer eigenen Sicherungen

Mithilfe von Azure Cosmos DB-SQL-API-Konten können Sie auch Ihre eigenen Sicherungen verwalten. Nutzen Sie dazu eine der folgenden Vorgehensweisen:

* Verwenden Sie [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) zum Verschieben von Daten in einen Speicher Ihrer Wahl in regelmäßigen Abständen.

* Verwenden Sie den [Änderungsfeed](change-feed.md) von Azure Cosmos DB, um Daten regelmäßig für eine vollständige Sicherung oder für inkrementelle Änderungen zu lesen und in Ihrem eigenen Speicher zu speichern.

## <a name="modify-the-backup-interval-and-retention-period"></a>Ändern des Sicherungsintervalls und des Aufbewahrungszeitraums

Azure Cosmos DB erstellt alle 4 Stunden automatisch eine Sicherung Ihrer Daten, und es bleiben jeweils die neuesten beiden Sicherungen gespeichert. Diese Konfiguration ist die Standardoption, die ohne Zusatzkosten angeboten wird. Sie können das Standardsicherungsintervall und den Aufbewahrungszeitraum während der Erstellung des Azure Cosmos-Kontos oder nach dem Erstellen des Kontos ändern. Die Sicherungskonfiguration wird auf der Ebene des Azure Cosmos-Kontos festgelegt, und Sie müssen sie für jedes Konto vornehmen. Nachdem Sie die Sicherungsoptionen für ein Konto konfiguriert haben, wird es auf alle Container innerhalb dieses Kontos angewandt. Derzeit können Sie die Sicherungsoptionen nur im Azure-Portal ändern.

Wenn Sie Ihre Daten versehentlich gelöscht oder beschädigt haben, stellen Sie **vor dem Erstellen einer Supportanfrage zum Wiederherstellen der Daten sicher, den Aufbewahrungszeitraum für Ihr Konto auf mindestens sieben Tage zu erhöhen. Es empfiehlt sich, den Aufbewahrungszeitraum innerhalb von 8 Stunden nach diesem Ereignis zu erhöhen.** Auf diese Weise hat das Azure Cosmos DB-Team genug Zeit, um Ihr Konto wiederherzustellen.

Gehen Sie anhand der folgenden Schritte vor, um die Standardsicherungsoptionen für ein vorhandenes Azure Cosmos-Konto zu ändern:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie den Bereich **Sichern und Wiederherstellen**. Aktualisieren Sie das Sicherungsintervall und den Aufbewahrungszeitraum für Sicherungen nach Bedarf.

   * **Sicherungsintervall:** das Intervall, in dem Azure Cosmos DB versucht, eine Sicherung der Daten zu erstellen. Die Sicherung nimmt immer eine gewisse Zeit in Anspruch und kann in einigen Fällen aufgrund von Downstreamabhängigkeiten möglicherweise zu einem Fehler führen. Azure Cosmos DB versucht, eine Sicherung im konfigurierten Intervall zu erstellen, es kann jedoch nicht garantiert werden, dass die Sicherung innerhalb dieses Zeitintervalls abgeschlossen wird. Sie können diesen Wert in Stunden oder Minuten konfigurieren. Das Sicherungsintervall darf nicht kürzer als 1 Stunde und nicht länger als 24 Stunden sein. Wenn Sie dieses Intervall ändern, wird das neue Intervall ab dem Zeitpunkt wirksam, zu dem die letzte Sicherung erstellt wurde.

   * **Beibehaltung der Sicherung:** Dies stellt den Aufbewahrungszeitraum für die einzelnen Sicherungen dar. Sie können die Zeit in Stunden oder Tagen konfigurieren. Der minimale Aufbewahrungszeitraum darf nicht kürzer als das Zweifache des Sicherungsintervalls (in Stunden) und nicht länger als 720 Stunden sein.

   * **Aufbewahrte Kopien der Daten:** Standardmäßig werden zwei Sicherungskopien Ihrer Daten kostenlos angeboten. Wenn Sie mehr als zwei Kopien benötigen, fallen zusätzliche Gebühr an. Im Abschnitt „Speichernutzung“ auf der Seite [Preise](https://azure.microsoft.com/pricing/details/cosmos-db/) finden Sie den genauen Preis für zusätzliche Kopien.

   :::image type="content" source="./media/online-backup-and-restore/configure-backup-interval-retention.png" alt-text="Konfigurieren des Sicherungsintervalls und des Aufbewahrungszeitraums für ein vorhandenes Azure Cosmos-Konto" border="true":::

Wenn Sie während der Kontoerstellung Sicherungsoptionen konfigurieren, können Sie die **Sicherungsrichtlinie** konfigurieren, die **regelmäßig** oder **fortlaufend** sein kann. Bei einer Richtlinie für regelmäßige Sicherungen können Sie das Sicherungsintervall und den Aufbewahrungszeitraum für Sicherungen konfigurieren. Die Richtlinie für fortlaufende Sicherungen ist zurzeit nur bei der Registrierung verfügbar. Das Team von Azure Cosmos DB bewertet Ihre Workload und genehmigt Ihre Anforderung.

:::image type="content" source="./media/online-backup-and-restore/configure-periodic-continuous-backup-policy.png" alt-text="Konfigurieren von Richtlinien für regelmäßige oder fortlaufende Sicherungen für neue Azure Cosmos-Konten" border="true":::

## <a name="restore-data-from-an-online-backup"></a>Wiederherstellen von Daten aus einer Onlinesicherung

In den folgenden Szenarien werden Ihre Daten möglicherweise versehentlich gelöscht oder geändert:  

* Das gesamte Azure Cosmos-Konto wird gelöscht.

* Azure Cosmos-Datenbanken werden gelöscht.

* Ein oder mehrere Azure Cosmos-Container werden gelöscht.

* Azure Cosmos-Elemente (z. B. Dokumente) in einem Container werden gelöscht oder geändert. Dieser spezifische Fall wird in der Regel als „Datenbeschädigung“ bezeichnet.

* Eine freigegebene Datenbank oder Container in einer freigegebenen Datenbank werden gelöscht oder beschädigt.

Azure Cosmos DB können die Daten in allen oben genannten Szenarien wiederherstellen. Beim Wiederherstellen wird immer ein neues Azure Cosmos-Konto zum Speichern der wiederhergestellten Daten erstellt. Wenn kein Name für das neue Konto angegeben wird, hat dieser das Format `<Azure_Cosmos_account_original_name>-restored1`. Die letzte Ziffer wird nach und nach erhöht, wenn mehrere Wiederherstellungen durchgeführt werden. Sie können keine Daten in einem vorab erstellten Azure Cosmos-Konto wiederherstellen.

Wenn ein Azure Cosmos-Konto versehentlich gelöscht wird, können wir die Daten in einem neuen Konto mit demselben Namen wiederherstellen, sofern der Kontoname zurzeit nicht verwendet wird. Daher wird empfohlen, das Konto nach dem Löschen nicht neu zu erstellen. Damit wird nicht nur verhindert, dass die wiederhergestellten Daten denselben Namen erhalten, sondern es macht es auch schwieriger, das richtige Konto zum Wiederherstellen zu erkennen.

Wenn eine Azure Cosmos-Datenbank gelöscht wird, ist es möglich, die gesamte Datenbank oder eine Teilmenge der Container innerhalb der Datenbank wiederherzustellen. Es ist auch möglich, bestimmte Container aus mehreren Datenbanken auszuwählen und in einem neuen Azure Cosmos-Konto wiederherzustellen.

Wenn Elemente in einem Container versehentlich gelöscht oder geändert werden (der Fall der Datenbeschädigung), müssen Sie den Zeitpunkt angeben, der wiederhergestellt werden soll. Die Zeit ist bei einer Datenbeschädigung sehr wichtig. Da der Container aktiv ist, wird die Sicherung weiterhin ausgeführt. Wenn Sie also länger als den Aufbewahrungszeitraum warten (der Standardwert ist acht Stunden), werden die Sicherungen überschrieben. **Um zu verhindern, dass die Sicherung überschrieben wird, erhöhen Sie den Aufbewahrungszeitraum der Sicherung für Ihr Konto auf mindestens sieben Tage. Es empfiehlt sich, den Aufbewahrungszeitraum innerhalb von 8 Stunden nach der Datenbeschädigung zu erhöhen.**

Wenn Sie Ihre Daten versehentlich gelöscht oder beschädigt haben, sollten Sie sich innerhalb von 8 Stunden an den [Azure-Support](https://azure.microsoft.com/support/options/) wenden, damit das Azure Cosmos DB-Team Sie beim Wiederherstellen der Daten aus den Sicherungen unterstützen kann. Auf diese Weise hat das Supportteam von Azure Cosmos DB genug Zeit, Ihr Konto wiederherzustellen.

Wenn Sie den Durchsatz auf Datenbankebene bereitstellen, erfolgen die Sicherungs- und Wiederherstellungsprozesse auf der Ebene der gesamten Datenbank und nicht der einzelnen Container. In solchen Fällen können Sie keine Teilmenge der Container für die Wiederherstellung auswählen.

## <a name="migrate-data-to-the-original-account"></a>Migrieren von Daten zum ursprünglichen Konto

Das Hauptziel der Datenwiederherstellung ist die Wiederherstellung der Daten, die Sie versehentlich gelöscht oder geändert haben. Es wird daher empfohlen, dass Sie zunächst den Inhalt der wiederhergestellten Daten untersuchen, um sicherzustellen, dass die gewünschten Daten enthalten sind. Sie können die Daten später zum primären Konto migrieren. Es ist zwar möglich, das wiederhergestellte Konto als neues aktives Konto zu verwenden, dies wird für Produktionsworkloads jedoch nicht empfohlen.  

Im Folgenden werden verschiedene Möglichkeiten zum Migrieren von Daten zurück zum ursprünglichen Azure Cosmos-Konto beschrieben:

* Mithilfe des [Azure Cosmos DB-Datenmigrationstools](import-data.md)
* Mithilfe von [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md)
* Mithilfe des [Änderungsfeeds](change-feed.md) in Azure Cosmos DB
* Durch Schreiben von eigenem benutzerdefiniertem Code

Löschen Sie unbedingt die wiederhergestellten Konten, sobald Sie mit der Migration Ihrer Daten fertig sind, da hierfür laufende Gebühren anfallen.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich als Nächstes darüber, wie Sie Daten aus einem Azure Cosmos-Konto wiederherstellen oder Daten zu einem Azure Cosmos-Konto migrieren.

* Wenn Sie eine Wiederherstellungsanforderung übermitteln möchten, kontaktieren Sie den Azure-Support, und [erstellen Sie im Azure-Portal ein Ticket](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
* [Wiederherstellung von Daten aus einem Azure Cosmos-Konto](how-to-backup-and-restore.md)
* [Verwenden des Änderungsfeeds von Cosmos DB](change-feed.md) zum Verschieben von Daten in Azure Cosmos DB
* [Verwenden von Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) zum Verschieben von Daten in Azure Cosmos DB

