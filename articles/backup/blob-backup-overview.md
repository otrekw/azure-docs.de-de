---
title: Übersicht über die operative Sicherung für Azure-Blobs
description: Erfahren Sie mehr über die operative Sicherung für Azure-Blobs (Vorschau).
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: b10191c8a01d3cc7a92dee8ca9bf59a506497a60
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101743302"
---
# <a name="overview-of-operational-backup-for-azure-blobs-in-preview"></a>Übersicht über die operative Sicherung für Azure-Blobs (Vorschau)

Die operative Sicherung für Blobs ist eine verwaltete, lokale Datensicherungslösung, mit der Sie Ihre Blockblobs vor verschiedenen Datenverlustszenarien wie Beschädigungen, Bloblöschungen und dem versehentlichen Löschen von Speicherkonten schützen können. Die Daten werden lokal im Quellspeicherkonto selbst gespeichert und können bei Bedarf zu einem ausgewählten Zeitpunkt wiederhergestellt werden. Die operative Sicherung bietet also eine einfache, sichere und kostengünstige Möglichkeit, Ihre Blobs zu schützen.

Die operative Sicherung für Blobs lässt sich neben anderen Sicherungsverwaltungsfunktionen in [Backup Center](backup-center-overview.md) integrieren und bietet so eine Kommandozentrale, in der Sie Sicherungen im großen Stil steuern, überwachen, betreiben und analysieren können.

## <a name="how-operational-backup-works"></a>So funktioniert die operative Sicherung

Die operative Sicherung von Blobs ist eine **lokale Sicherungslösung**. Die Sicherungsdaten werden also nicht an den Backup-Tresor übertragen, sondern im Quellspeicherkonto selbst gespeichert. Der Backup-Tresor dient jedoch weiterhin als Einheit zum Verwalten von Sicherungen. Es handelt sich außerdem um eine Lösung mit **fortlaufender Sicherung**. Das bedeutet, dass Sie keine Sicherungen planen müssen und alle Änderungen beibehalten werden und in dem Zustand wiederhergestellt werden können, der zu einem ausgewählten Zeitpunkt vorlag.

Die operative Sicherung verwendet Blobplattformfunktionen, um Ihre Daten zu schützen und eine Wiederherstellung nach Bedarf zu ermöglichen:

- **Zeitpunktwiederherstellung**: Die [Zeitpunktwiederherstellung für Blobs](https://docs.microsoft.com/azure/storage/blobs/point-in-time-restore-overview) ermöglicht die Wiederherstellung von Blobdaten in einem früheren Zustand. Dafür wiederum werden die Features **vorläufiges Löschen**, **Änderungsfeed** und **Blobversionsverwaltung** verwendet, um Daten für den angegebenen Zeitraum aufzubewahren. Die operative Sicherung ermöglicht sowohl Zeitpunktwiederherstellungen als auch die zugrunde liegenden Funktionen, mit denen sichergestellt wird, dass Daten für den angegebenen Zeitraum aufbewahrt werden.

- **Löschsperre**: Eine Löschsperre verhindert, dass das Speicherkonto versehentlich durch nicht autorisierte Benutzer gelöscht wird. Die operative Sicherung wendet auch automatisch eine Löschsperre an (sofern diese konfiguriert wurde), um Datenverluste zu verringern, die auftreten können, wenn das Speicherkontos gelöscht wird.

Informationen zu den Einschränkungen der aktuellen Lösung finden Sie in der [Supportmatrix](blob-backup-support-matrix.md).

### <a name="protection"></a>Schutz

Die operative Sicherung wird auf Ebene des **Speicherkontos** konfiguriert und verwaltet und gilt für alle Blockblobs im Speicherkonto. Die operative Sicherung verwendet eine **Sicherungsrichtlinie**, um den Zeitraum zu verwalten, für den die Sicherungsdaten (einschließlich älterer Versionen und gelöschter Blobs) aufbewahrt werden sollen. Auf diese Weise wird der maximale Zeitraum definiert, aus dem Sie Ihre Daten wiederherstellen können. In der Sicherungsrichtlinie kann eine maximale Aufbewahrungsdauer von 360 Tagen bzw. der entsprechenden Anzahl von ganzen Wochen (51) oder Monaten (11) definiert werden.

Wenn Sie die Sicherung für ein Speicherkonto konfigurieren und eine Sicherungsrichtlinie mit „n“ Tagen zuweisen, werden die zugrunde liegenden Eigenschaften wie folgt festgelegt. Sie können diese Eigenschaften auf der Registerkarte **Datenschutz** des Blobdiensts in Ihrem Speicherkonto anzeigen.

- **Zeitpunktwiederherstellung**: Ist auf „n“ Tage festgelegt, wie in der Sicherungsrichtlinie definiert. Wenn für das Speicherkonto vor dem Konfigurieren der Sicherung bereits eine Zeitpunktwiederherstellung mit einer Aufbewahrung von beispielsweise x  Tagen aktiviert wurde, wird der Zeitraum der Zeitpunktwiederherstellung auf den größeren der beiden Werte festgelegt, also „max(n,x)“. Wenn Sie bereits eine Zeitpunktwiederherstellung aktiviert und die Aufbewahrungsdauer auf einen größeren Wert festgelegt haben, als in der Sicherungsrichtlinie angegeben ist, bleibt der Wert unverändert.

- **Vorläufiges Löschen**: Ist auf „n+5“ Tage festgelegt, also fünf Tage zusätzlich zu dem Zeitraum, der in der Sicherungsrichtlinie festgelegt ist. Wenn für das Speicherkonto, das für die operative Sicherung konfiguriert wird, das vorläufige Löschen bereits mit einem Aufbewahrungszeitraum von beispielsweise „y“ Tagen aktiviert wurde, wird der Aufbewahrungszeitraum für das vorläufige Löschen auf den größeren der beiden Werte festgelegt, also „max(n+5,y)“. Wenn Sie das vorläufige Löschen bereits aktiviert und die Aufbewahrungsdauer auf einen größeren Wert festgelegt haben, als in der Sicherungsrichtlinie angegeben ist, bleibt der Wert unverändert.

- **Versionsverwaltung und Änderungsfeed für Blobs**: Versionsverwaltung und Änderungsfeed sind für Speicherkonten aktiviert, die für die operative Sicherung konfiguriert wurden.

- **Löschsperre**: Durch Konfigurieren der operativen Sicherung in einem Speicherkonto wird auch eine Löschsperre auf das Konto angewendet. Die von Backup angewendete Löschsperre kann auf der Registerkarte **Sperren** des Speicherkontos angezeigt werden.

Damit Backup diese Eigenschaften in den zu schützenden Speicherkonten aktivieren kann, muss dem Backup-Tresor die Rolle **Mitwirkender für Speicherkontosicherung** in den jeweiligen Speicherkonten zugewiesen werden.

>[!NOTE]
>Die operative Sicherung unterstützt nur Vorgänge in Blockblobs. Vorgänge in Containern können nicht wiederhergestellt werden. Wenn Sie einen Container aus dem Speicherkonto löschen, indem Sie den Vorgang **Container löschen** aufrufen, kann dieser Container nicht mit einem Wiederherstellungsvorgang wiederhergestellt werden. Es empfiehlt sich, das vorläufige Löschen zu aktivieren, um Datenschutz und -wiederherstellung zu ergänzen.

### <a name="management"></a>Verwaltung

Sobald Sie die Sicherung in einem Speicherkonto aktiviert haben, wird eine dem Speicherkonto entsprechende Backup-Instanz im Backup-Tresor erstellt. Sie können sämtliche sicherungsbezogenen Vorgänge für ein Speicherkonto – z. B. Initiieren von Wiederherstellungen, Überwachen, Beenden des Schutzes usw. – über die entsprechende Backup-Instanz durchführen.

Die operative Sicherung lässt sich auch direkt in Backup Center integrieren, sodass Sie den Schutz all Ihrer Speicherkonten zentral und zusammen mit allen anderen Workloads verwalten können, die von Backup unterstützt werden. Backup Center ist Ihre Kommandozentrale für sämtliche Anforderungen von Backup: Überwachen von Aufträgen und des Zustands von Sicherungen und Wiederherstellungen, Sicherstellen von Compliance und Governance, Analysieren der Sicherungsverwendung und Ausführen von Vorgängen in Zusammenhang mit der Sicherung und Wiederherstellung von Daten.

### <a name="restore"></a>Restore

Sie können Daten von jedem Zeitpunkt wiederherstellen, für den ein Wiederherstellungspunkt vorhanden ist. Ein Wiederherstellungspunkt wird erstellt, wenn sich ein Speicherkonto im geschützten Zustand befindet, und kann zum Wiederherstellen von Daten verwendet werden, solange er in dem Aufbewahrungszeitraum liegt, der von der Sicherungsrichtlinie festgelegt wurde (also gemäß der Zeitpunktwiederherstellung des Blobdiensts im Speicherkonto). Die operative Sicherung verwendet die Zeitpunktwiederherstellung von Blobs, um Daten aus einem Wiederherstellungspunkt wiederherzustellen.

Bei der operativen Sicherung können Sie auswählen, ob Sie alle Blockblobs im Speicherkonto wiederherstellen, bestimmte Container suchen und wiederherstellen oder den Präfixabgleich verwenden möchten, um eine Teilmenge der Blobs wiederherzustellen. Alle Wiederherstellungen können nur für das Quellspeicherkonto ausgeführt werden.

## <a name="pricing"></a>Preise

Für die Verwendung der operativen Sicherung für Blobs fallen keine Verwaltungs- oder Instanzgebühren an. Allerdings werden Ihnen folgende Gebühren berechnet:

- Wiederherstellungen erfolgen über Zeitpunktwiederherstellungen für Blobs, daher fallen Gebühren basierend auf der Menge der verarbeiteten Daten an. Weitere Informationen finden Sie unter [Zeitpunktwiederherstellung – Preise](https://docs.microsoft.com/azure/storage/blobs/point-in-time-restore-overview#pricing-and-billing).

- Datenaufbewahrung aufgrund folgender Features: [Vorläufiges Löschen für Blobs](https://docs.microsoft.com/azure/storage/blobs/soft-delete-blob-overview), [Unterstützung für den Änderungsfeed in Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-change-feed) und [Blobversionsverwaltung](https://docs.microsoft.com/azure/storage/blobs/versioning-overview).

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren und Verwalten von Azure Blob-Sicherungen](blob-backup-configure-manage.md)
