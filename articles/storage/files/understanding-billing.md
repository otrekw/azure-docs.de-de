---
title: Grundlegendes zur Abrechnung für Azure Files | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Abrechnungsmodelle „Bereitgestellt“ und „Nutzungsbasierte Zahlung“ für Azure-Dateifreigaben interpretieren können.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 01/20/2021
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 19ecbea70d9cb6b8cc31c72ed3c1294cd137ce93
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632477"
---
# <a name="understanding-azure-files-billing"></a>Grundlegendes zur Abrechnung für Azure Files
Für Azure Files gibt es zwei Abrechnungsmodelle: „Bereitgestellt“ und „Nutzungsbasierte Zahlung“. Das Modell „Bereitgestellt“ ist nur für Premium-Dateifreigaben verfügbar, d. h. für Dateifreigaben, die in einem Speicherkonto des Typs **FileStorage** bereitgestellt werden. Das Modell „Nutzungsbasierte Zahlung“ ist nur für Standarddateifreigaben verfügbar, d. h. für Dateifreigaben, die in einem Speicherkonto des Typs **Universell, Version 2** bereitgestellt werden. In diesem Artikel wird die Funktionsweise beider Modelle erklärt, um Ihnen zu helfen, Ihre monatliche Azure Files-Rechnung zu verstehen.

Die aktuellen Preise für Azure Files finden Sie auf der [Seite mit den Preisen für Azure Files](https://azure.microsoft.com/pricing/details/storage/files/).

## <a name="provisioned-model"></a>Bereitgestelltes Modell
Azure Files verwendet für Premium-Dateifreigaben das Modell „Bereitgestellt“. Beim diesem Geschäftsmodell geben Sie dem Azure Files-Dienst aktiv an, wie hoch Ihr Speicherbedarf ist, anstatt nach Inanspruchnahme abgerechnet zu werden. Dies ist vergleichbar mit dem Kauf lokaler Hardware. Wenn Sie eine Azure-Dateifreigabe mit einer bestimmten Menge Speicherplatz bereitstellen, zahlen Sie für diesen Speicher unabhängig davon, ob Sie ihn nutzen oder nicht, genauso wie Sie nicht anfangen, Kosten für lokale physische Medien zu zahlen, wenn Sie beginnen, Speicherplatz zu belegen. Im Gegensatz zum Kauf lokaler physischer Medien können bereitgestellte Dateifreigaben je nach Speicher- und E/A-Leistungsmerkmalen dynamisch hoch- oder herunterskaliert werden.

Wenn Sie eine Premium-Dateifreigabe bereitstellen, geben Sie an, wie viele GiBs Ihre Workload benötigt. Jedes GiB, das Sie bereitstellen, berechtigt Sie zu mehr IOPS und Durchsatz in einem festen Verhältnis. Zusätzlich zum garantierten IOPS-Grundwert unterstützt jede Premium-Dateifreigabe Bursting nach dem Prinzip der bestmöglichen Leistung. Die Formeln für IOPS und Durchsatz sind wie folgt:

- IOPS-Grundwert = 400 + 1 * bereitgestellte GiB. (Bis zu 100.000 IOPS).
- Burstgrenzwert = MAX(4000, 3 * IOPS-Grundwert).
- Ausgangsrate = 60 MiB/s + 0,06 * bereitgestellte GiB.
- Eingangsrate = 40 MiB/s + 0,04 * bereitgestellte GiB.

Die bereitgestellte Größe der Dateifreigabe kann jederzeit heraufgesetzt, jedoch erst 24 Stunden nach der letzten Heraufsetzung herabgesetzt werden. Wenn in einer 24-stündigen Wartezeit keine Heraufsetzung aufgetreten ist, können Sie das Freigabekontingent beliebig oft herabsetzen, bis Sie es erneut heraufsetzen. Änderungen an IOPS und Durchsatz werden innerhalb weniger Minuten nach Änderung der bereitgestellten Größe wirksam.

Es ist möglich, die Größe Ihrer bereitgestellten Freigabe unter Ihre verbrauchten GiB zu reduzieren. Wenn Sie dies tun, gehen Ihnen keine Daten verloren, sondern es wird Ihnen weiterhin die verwendete Größe in Rechnung gestellt. Sie erhalten die Leistung (IOPS-Grundwert, Durchsatz und Burst-IOPS) der bereitgestellten Freigabe, nicht die der verwendeten Größe.

Die folgende Tabelle zeigt einige Beispiele dieser Formeln für die bereitgestellten Freigabengrößen:

|Kapazität (GiB) | IOPS-Grundwert | Burst-IOPS | Ausgehend (MiB/s) | Eingehend (MiB/s) |
|---------|---------|---------|---------|---------|
|100         | 500     | Bis zu 4.000     | 66   | 44   |
|500         | 900     | Bis zu 4.000  | 90   | 60   |
|1\.024       | 1\.424   | Bis zu 4.000   | 122   | 81   |
|5\.120       | 5\.520   | Bis zu 15.360  | 368   | 245   |
|10.240      | 10.640  | Bis zu 30.720  | 675   | 450   |
|33.792      | 34.192  | Bis zu 100.000 | 2\.088 | 1\.392   |
|51.200      | 51.600  | Bis zu 100.000 | 3\.132 | 2\.088   |
|102.400     | 100.000 | Bis zu 100.000 | 6\.204 | 4\.136   |

Die effektive Leistung der Dateifreigabe hängt u. a. von den Grenzwerten des Computernetzwerks, der verfügbaren Netzwerkbandbreite, den E/A-Größen und der Parallelität ab. Beispielsweise kann ein einzelner virtueller Windows-Computer ohne aktivierte Funktion SMB Multichannel namens *Standard F16s_v2*, der mit einer Premium-Dateifreigabe über SMB verbunden ist, laut internen Tests mit Lese-/Schreibvorgängen mit einer E/A-Größe von 8 KiB 20 K Lese-IOPS und 15 K Schreib-IOPS erzielen. Bei Lese-/Schreibvorgängen mit einer E/A-Größe von 512 MiB kann derselbe virtuelle Computer einen Durchsatz von 1,1 GiB/s ausgehend und 370 MiB/s eingehend erzielen. Der gleiche Client kann eine bis zu \~dreifache Leistung erzielen, wenn SMB Multichannel für die Premium-Freigaben aktiviert ist. Um eine maximale Leistung zu erreichen, [aktivieren Sie SMB Multichannel](storage-files-enable-smb-multichannel.md), und verteilen Sie die Last auf mehrere VMs. Weitere Informationen zur [Leistung von SMB Multichannel](storage-troubleshooting-files-performance.md) und zu gängigen Leistungsproblemen sowie deren Lösungen finden Sie im [Leitfaden zur Problembehandlung](storage-files-smb-multichannel-performance.md).

### <a name="bursting"></a>Bursting
Wenn Ihre Workload die zusätzliche Leistung benötigt, um den Spitzenbedarf zu erfüllen, kann Ihre Freigabe Burstgutschriften verwenden, um den IOPS-Grundwert für die Freigabe zu überschreiten und die erforderliche Freigabeleistung bereitzustellen. Premium-Dateifreigaben können ihren IOPS-Wert bis auf 4.000 oder um den Faktor drei erhöhen (je nachdem, welcher Wert höher ist). Bursting wird automatisiert und funktioniert auf Basis eines Guthabensystems. Die Burstübertragung funktioniert auf Best-Effort-Basis, und der Burstgrenzwert ist keine Garantie. Bei Dateifreigaben ist eine Burstübertragung *bis zum* Grenzwert für eine maximale Dauer von 60 Minuten möglich.

Guthaben sammeln sich in einem Burstbucket an, wenn Datenverkehr für Ihre Dateifreigabe unterhalb des IOPS-Grundwerts liegt. Beispielsweise weist eine Freigabe mit 100 GiB 500 IOPS-Grundwerte auf. Wenn der tatsächliche Datenverkehr auf der Freigabe 100 IOPS für ein bestimmtes 1-Sekunden-Intervall betrug, werden die 400 nicht verwendeten IOPS einem Burstbucket gutgeschrieben. Auf ähnliche Weise fällt eine Burstgutschrift in Höhe von 1.424 IOPS für eine 1-TiB-Leerlauffreigabe an. Diese Guthaben werden dann später verwendet, wenn Vorgänge die IOPS-Grundwerte überschreiten.

Wenn eine Freigabe den IOPS-Grundwert überschreitet und Guthaben in einem Burstbucket hat, führt sie Burstübertragungen mit der maximal zulässigen Spitzenburstrate durch. Freigaben können bis zu einer Dauer von maximal 60 Minuten weiterhin Bursts ausführen, solange Guthaben verbleibt. Dies hängt aber von der Höhe des aufgelaufenen Burstguthabens ab. Jede E/A über dem IOPS-Grundwert verbraucht ein Guthaben, und wenn alle Guthaben verbraucht sind, kehrt die Freigabe zum IOPS-Grundwert zurück.

Freigabeguthaben können drei Zustände aufweisen:

- Anwachsend, wenn die Dateifreigabe weniger als den IOPS-Grundwert verwendet.
- Abnehmend, wenn die Dateifreigabe mehr als den IOPS-Grundwert verwendet und sich im Burstmodus befindet.
- Konstant, wenn die Dateifreigabe genau den IOPS-Grundwert verwendet. Guthaben sind weder aufgelaufen noch werden sie in Anspruch genommen.

Neue Dateifreigaben beginnen mit der vollen Anzahl von Guthaben im Burstbucket. Burstguthaben werden nicht angesammelt, wenn der Freigabe-IOPS aufgrund einer Einschränkung durch den Server unter den IOPS-Grundwert fällt.

## <a name="pay-as-you-go-model"></a>Modell für die nutzungsbasierte Bezahlung
Azure Files verwendet für Standarddateifreigaben das Geschäftsmodell „Nutzungsbasierte Zahlung“. Bei diesem Geschäftsmodell wird der zu zahlende Betrag dadurch bestimmt, wie viel Sie tatsächlich in Anspruch nehmen, und nicht auf Grundlage einer bereitgestellten Menge. Grundsätzlich zahlen Sie die Kosten einer auf Datenträgern gespeicherte Datenmenge und dann für zusätzliche Transaktionen, die auf der Nutzung dieser Daten basieren. Ein Modell mit nutzungsbasierter Zahlung kann wirtschaftlich sein, da Sie keine Überdimensionierung vornehmen müssen, um künftiges Wachstum oder Leistungsanforderungen zu berücksichtigen. Es muss auch keine Aufhebung von Bereitstellung erfolgen, wenn sich die Workload oder der Datenspeicherbedarf im Laufe der Zeit ändert. Andererseits kann ein Modell mit nutzungsbasierter Zahlung im Rahmen eines Budgetierungsprozesses schwierig zu planen sein, da das Abrechnungsmodell mit nutzungsbasierter Zahlung der Inanspruchnahme durch Endbenutzer unterliegt.

### <a name="differences-in-standard-tiers"></a>Unterschiede bei den Standardtarifen
Wenn Sie eine Standarddateifreigabe erstellen, haben Sie die Wahl zwischen den Speicherebenen „Transaktionsoptimiert“, „Heiß“ und „Kalt“. Alle drei Ebenen werden auf exakt derselben Standardspeicherhardware gespeichert. Der Hauptunterschied dieser drei Ebenen besteht im Preis für die Speicherung ruhender Daten (bei kälteren Ebenen niedriger) und im Transaktionspreis (bei kälteren Ebenen höher). Dies bedeutet Folgendes:

- Wie der Name bereits vermuten lässt, ist der Preis der transaktionsoptimierten Ebene für Workloads mit hohem Transaktionsaufkommen optimiert. Die transaktionsoptimierte Ebene hat zwar den höchsten Preis für die Speicherung ruhender Daten, bietet aber die niedrigsten Transaktionspreise.
- Die heiße Ebene ist für aktive Workloads ohne hohes Transaktionsaufkommen vorgesehen. Verglichen mit der transaktionsoptimierten Ebene hat sie einen etwas niedrigeren Preis für die Speicherung ruhender Daten und etwas höhere Transaktionspreise. Stellen Sie sie sich als Mittelweg zwischen den Ebenen „Transaktionsoptimiert“ und „Kalt“ vor.
- Bei der kalten Speicherebene sind die Kosten für Workloads mit geringer Aktivität optimiert. Diese Ebene weist die niedrigsten Kosten für die Speicherung ruhender Daten, aber die höchsten Kosten für Transaktionen auf.

Wenn Sie für eine Workload, auf die nur selten zugegriffen wird, die Speicherebene „Transaktionsoptimiert“ wählen, zahlen Sie für die wenigen Transaktionen in der Freigabe in einem Monat fast nichts, aber einen hohen Betrag für die Datenspeicherung. Wenn Sie dieselbe Freigabe auf die kalte Speicherebene verlagern, würden Sie immer noch fast keine Transaktionskosten zahlen, einfach weil für diese Workload nur sehr selten Transaktionen erfolgen, aber die kalte Speicherebene bietet viel günstigere Datenspeicherungskosten. Bei Wahl der optimalen Speicherebene für Ihren Anwendungsfall können Sie erheblich Kosten sparen. Bei Wahl der optimalen Speicherebene für Ihren Anwendungsfall können Sie erheblich Kosten sparen.

Ähnlich verhält es sich, wenn Sie eine Workload mit häufigem Zugriff auf die kalte Speicherebene verlagern. Dann zahlen Sie viel mehr für Transaktionen, aber weniger für Datenspeicherung. Dies kann dazu führen, dass die gestiegenen Kosten für Transaktionen die Einsparungen aufgrund niedrigerer Kosten für Datenspeicherung überwiegen. Dies kann bewirken, dass Sie für die kalte Speicherebene mehr zahlen als bei Wahl von „transaktionsoptimiert“. Es ist möglich, dass bei bestimmten Nutzungsgraden die heiße Speicherebene am wirtschaftlichsten ist, während die kalte Speicherebene teurer als „transaktionsoptimiert“ ist.

Welche Ebene für Ihre Standarddateifreigabe am kostengünstigsten ist, hängt von Ihrer Workload und von der Aktivitätsebene ab. In der Praxis lässt sich die wirtschaftlichste Speicherebene am besten ermitteln, indem der tatsächliche Inanspruchnahmen von Ressourcen der Freigabe (gespeicherte Daten, Schreibtransaktionen usw.) untersucht wird.

### <a name="what-are-transactions"></a>Was sind Transaktionen?
Transaktionen sind Vorgänge oder Anforderungen an Azure Files, den Inhalt der Dateifreigabe hoch- oder herunterzuladen oder anderweitig zu ändern. Jede Aktion in einer Dateifreigabe führt zu einer oder mehreren Transaktionen. Bei Standardfreigaben mit dem Abrechnungsmodell „Nutzungsbasierte Zahlung“ führt dies zu Transaktionskosten.

Es gibt fünf grundlegende Transaktionskategorien: „Schreiben“, „Auflisten“, „Lesen“, „Sonstige“ und „Löschen“. Alle Vorgänge, die über die REST-API oder SMB erfolgen, werden wie folgt einer dieser vier Kategorien zugeordnet:

| Vorgangsart | Schreibtransaktionen | Listentransaktionen | Lesetransaktionen | Sonstige Transaktionen | Löschtransaktionen |
|-|-|-|-|-|-|
| Verwaltungsvorgänge | <ul><li>`CreateShare`</li><li>`SetFileServiceProperties`</li><li>`SetShareMetadata`</li><li>`SetShareProperties`</li></ul> | <ul><li>`ListShares`</li></ul> | <ul><li>`GetFileServiceProperties`</li><li>`GetShareAcl`</li><li>`GetShareMetadata`</li><li>`GetShareProperties`</li><li>`GetShareStats`</li></ul> | | <ul><li>`DeleteShare`</li></ul> |
| Datenvorgänge | <ul><li>`CopyFile`</li><li>`Create`</li><li>`CreateDirectory`</li><li>`CreateFile`</li><li>`PutRange`</li><li>`PutRangeFromURL`</li><li>`SetDirectoryMetadata`</li><li>`SetFileMetadata`</li><li>`SetFileProperties`</li><li>`SetInfo`</li><li>`SetShareACL`</li><li>`Write`</li><li>`PutFilePermission`</li></ul> | <ul><li>`ListFileRanges`</li><li>`ListFiles`</li><li>`ListHandles`</li></ul>  | <ul><li>`FilePreflightRequest`</li><li>`GetDirectoryMetadata`</li><li>`GetDirectoryProperties`</li><li>`GetFile`</li><li>`GetFileCopyInformation`</li><li>`GetFileMetadata`</li><li>`GetFileProperties`</li><li>`QueryDirectory`</li><li>`QueryInfo`</li><li>`Read`</li><li>`GetFilePermission`</li></ul> | <ul><li>`AbortCopyFile`</li><li>`Cancel`</li><li>`ChangeNotify`</li><li>`Close`</li><li>`Echo`</li><li>`Ioctl`</li><li>`Lock`</li><li>`Logoff`</li><li>`Negotiate`</li><li>`OplockBreak`</li><li>`SessionSetup`</li><li>`TreeConnect`</li><li>`TreeDisconnect`</li><li>`CloseHandles`</li><li>`AcquireFileLease`</li><li>`BreakFileLease`</li><li>`ChangeFileLease`</li><li>`ReleaseFileLease`</li></ul> | <ul><li>`ClearRange`</li><li>`DeleteDirectory`</li></li>`DeleteFile`</li></ul> |

> [!Note]  
> NFS 4.1 ist nur für Premium-Dateifreigaben mit dem Abrechnungsmodell „Bereitgestellt“ verfügbar. Transaktionen haben keinen Einfluss auf die Abrechnung für Premium-Dateifreigaben.

## <a name="see-also"></a>Weitere Informationen
- [Azure Files – Preise](https://azure.microsoft.com/pricing/details/storage/files/)
- [Planung einer Azure Files-Bereitstellung](./storage-files-planning.md) und [Planung für die Bereitstellung einer Azure-Dateisynchronisierung](./storage-sync-files-planning.md)
- [Erstellen einer Dateifreigabe](./storage-how-to-create-file-share.md) und [Bereitstellen der Azure-Dateisynchronisierung](./storage-sync-files-deployment-guide.md)