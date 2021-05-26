---
title: Azure SQL-Datenbank-Ledger – Übersicht
description: Übersicht über Azure SQL-Datenbank-Ledger
ms.custom: ''
ms.date: 05/25/2021
ms.service: sql-database
ms.subservice: security
ms.reviewer: vanto
ms.topic: conceptual
author: JasonMAnderson
ms.author: janders
ms.openlocfilehash: 36d7c5d6b97b54392be8c85d1b6a6088eda5f983
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386306"
---
# <a name="azure-sql-database-ledger"></a>Azure SQL-Datenbank-Ledger

[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

> [!NOTE]
> Der Azure SQL-Datenbank-Ledger ist zurzeit als **Public Preview** (Öffentliche Vorschau) verfügbar.

Das Einrichten einer Vertrauensstellung rund um die Integrität von in Datenbanksystemen gespeicherten Daten war ein seit langem bestehendes Problem bei allen Organisationen, die Finanzdaten, Patientendaten oder andere vertrauliche Daten verwalten. Das Ledgerfeature von [Azure SQL-Datenbank](sql-database-paas-overview.md) bietet Manipulationsbeweisfunktionen in Ihrer Datenbank, sodass Sie anderen Parteien, z. B. Prüfern oder anderen Geschäftspartnern, kryptografisch nachweisen können, dass Ihre Daten nicht manipuliert wurden.

Ledger hilft beim Schutz von Daten vor Angreifern oder Benutzern mit umfangreichen Berechtigungen, einschließlich Datenbankadministratoren (Database Administrators, DBAs) sowie System- und Cloudadministratoren. Wie bei einem herkömmlichen Ledger werden Verlaufsdaten beibehalten, sodass bei der Aktualisierung einer Zeile in der Datenbank der vorherige Wert in einer Verlaufstabelle beibehalten und geschützt wird. Der Ledger stellt eine Chronik aller Änderungen bereit, die im Laufe der Zeit an der Datenbank vorgenommen wurden. Der Ledger und die Verlaufsdaten werden transparent verwaltet und bieten so Schutz ohne Anwendungsänderungen. Verlaufsdaten werden in einer relationalen Form verwaltet, um SQL-Abfragen für Überwachung, Forensik und andere Zwecke zu unterstützen. Ledger bietet kryptografische Datenintegritätsgarantien, während die Leistungsfähigkeit, Flexibilität und Leistung von Azure SQL-Datenbank erhalten bleibt.

:::image type="content" source="media/ledger/ledger-table-architecture.png" alt-text="Architektur der Ledgertabelle":::

## <a name="use-case-for-azure-sql-database-ledger"></a>Anwendungsfall für Azure SQL-Datenbank-Ledger 

### <a name="streamlining-audits"></a>Optimieren von Überwachungen

Der Wert jedes Produktionssystems basiert auf der Fähigkeit, den vom System genutzten und erzeugten Daten zu vertrauen. Wenn die Daten in Ihrer Datenbank von einem böswilligen Benutzer manipuliert wurden, kann dies zu katastrophalen Ergebnissen in den Geschäftsprozessen führen, die auf diesen Daten basieren. Die Aufrechterhaltung des Vertrauens in Ihre Daten erfordert eine Kombination aus der Aktivierung der richtigen Sicherheitskontrollen, um potenzielle Angriffe, Sicherungs- und Wiederherstellungsmethoden sowie gründliche Notfallwiederherstellungsverfahren zu reduzieren. Die Sicherstellung, dass diese Methoden aktiviert werden, wird oft durch externe Parteien überwacht. Überwachungsprozesse sind sehr zeitintensive Aktivitäten. Für die Überwachung ist eine Überprüfung der implementierten Methoden vor Ort erforderlich, beispielsweise das Überprüfen von Überwachungsprotokollen sowie das Überprüfen der Authentifizierung und der Zugriffssteuerungen, um nur wenige zu nennen. Diese manuellen Prozesse können zwar potenzielle Sicherheitslücken offen legen, aber sie können keinen belegbaren Nachweis bieten, dass die Daten nicht böswillig geändert wurden. Ledger bietet Prüfern den kryptografischen Nachweis der Datenintegrität, der nicht nur bei einer Optimierung des Überwachungsprozesses helfen kann, sondern auch Unleugbarkeit im Hinblick auf die Integrität der Systemdaten bietet.

### <a name="multi-party-business-processes"></a>Geschäftsprozesse mit mehreren Parteien

Systeme, bei denen es in mehreren Organisationen einen Geschäftsprozess gibt, der den Status miteinander teilen muss, z. B. Lieferketten-Verwaltungssysteme, stehen vor der Herausforderung, Daten miteinander zu teilen und einander zu vertrauen. Viele Organisationen nutzen herkömmliche Blockchains wie Ethereum oder Hyperledger Fabric, um ihre Geschäftsprozesse mit mehreren Parteien digital zu transformieren. Blockchain ist eine hervorragende Lösung für Netzwerke mit mehreren Parteien, bei denen das Vertrauen zwischen am Netzwerk beteiligten Parteien gering ist. Viele dieser Netzwerke sind jedoch grundsätzlich zentralisierte Lösungen, bei denen Vertrauen wichtig ist. Eine vollständig dezentralisierte Infrastruktur ist aber eine „schwergewichtige“ Lösung. Ledger bietet eine Lösung für diese Netzwerke, bei der Teilnehmer die Datenintegrität der zentral gespeicherten Daten überprüfen können, statt mit der Komplexität und den Auswirkungen auf die Leistung konfrontiert zu sein, die der Netzwerkkonsens in einem Blockchain-Netzwerk mit sich bringt.

### <a name="trusted-off-chain-storage-for-blockchain"></a>Vertrauenswürdiger Off-Chain-Speicher für Blockchain

Dort, wo ein Blockchain-Netzwerk für einen Geschäftsprozess mit mehreren Parteien erforderlich ist, ist es eine Herausforderung, die Daten in der Blockchain ohne Leistungseinbußen abfragen zu können. Typische Muster zur Lösung dieses Problems umfassen das Replizieren von Daten aus der Blockchain in einen Off-Chain-Speicher, z. B. eine Datenbank. Sobald die Daten jedoch aus der Blockchain in die Datenbank repliziert wurden, garantiert die Datenintegrität, dass ein Blockchainangebot verloren geht. Ledger bietet die für die Off-Chain-Speicherung von Blockchain-Netzwerken erforderliche Datenintegrität und stellt so eine vollständige Datenvertrauensstellung über das gesamte System sicher.

## <a name="how-it-works"></a>Funktionsweise

Jede von der Datenbank empfangene Transaktion wird kryptografisch mit SHA-256 gehasht. Die Hashfunktion verwendet den Wert der Transaktion (einschließlich Hashes der darin enthaltenen Zeilen) zusammen mit dem Hash der vorherigen Transaktion als Eingabe. Die Funktion verknüpft alle Transaktionen kryptografisch, ähnlich wie eine Blockchain. Kryptografische Hashes [(Datenbankhashes)](#database-digests), die den Status der Datenbank darstellen, werden regelmäßig außerhalb von Azure SQL-Datenbank an einem manipulationssicheren Speicherort generiert und gespeichert. Ein Beispiel für einen Speicherort wären [unveränderliche Azure Storage-Blobs](../../storage/blobs/storage-blob-immutable-storage.md) oder [Azure Confidential Ledger](/azure/confidential-ledger/). Datenbankhashes werden dann später zur Überprüfung der Datenbankintegrität verwendet, indem der Wert des Hashes im Digest mit den berechneten Hashes in der Datenbank verglichen wird. 

Ledgerfunktionen werden in Azure SQL-Datenbank-Tabellen in zwei Formen eingeführt:

- [**Aktualisierbare Ledgertabellen**](ledger-updatable-ledger-tables.md), bei denen Sie Zeilen in Ihren Tabellen aktualisieren und löschen können.
- [**Ledgertabellen, die nur Anfügevorgänge unterstützen**](ledger-append-only-ledger-tables.md), bei denen Sie in Ihren Tabellen nur einfügen können.

Sowohl **aktualisierbare Ledgertabellen** als auch **Ledgertabellen, die nur Anfügevorgänge unterstützen,** bieten Funktionen für Manipulationsbeweis und digitale Forensik. Es ist wichtig zu verstehen, welche Transaktionen von welchen Benutzern übermittelt wurden, die zu Änderungen an der Datenbank geführt haben, wenn beide potenzielle Manipulationsereignisse beheben oder dritten Parteien nachweisen, dass an das System übermittelte Transaktionen von autorisierten Benutzern stammen. Mithilfe des Ledgerfeatures können Benutzer, deren Partner oder Prüfer alle Vorgänge im Verlauf analysieren und potenzielle Manipulationen erkennen. Jeder Zeilenvorgang wird von der ID der Transaktion begleitet, die sie ausgeführt hat. So können Benutzer weitere Informationen zum Zeitpunkt der Transaktionsausführung sowie zur Identität des Benutzers, der sie ausgeführt hat, abrufen und sie mit anderen Vorgängen korrelieren, die durch diese Transaktion ausgeführt wurden.

Es gibt einige Einschränkungen bei Ledgertabellen, die Sie beachten sollten. Weitere Informationen zu Einschränkungen bei Ledgertabellen finden Sie unter [Einschränkungen beim Azure SQL-Datenbank-Ledger](ledger-limits.md).

### <a name="ledger-database"></a>Ledgerdatenbank

Eine Ledgerdatenbank ist eine Datenbank, in der alle Benutzerdaten manipulationssicher sind und in Ledgertabellen gespeichert werden. Eine Ledgerdatenbank kann nur Ledgertabellen enthalten, und jede Tabelle wird standardmäßig als aktualisierbare Ledgertabelle erstellt. Ledgerdatenbanken bieten eine benutzerfreundliche Lösung für Anwendungen, die die Integrität aller zu schützenden Daten erfordern. 

### <a name="updatable-ledger-tables"></a>Aktualisierbare Ledgertabellen

[Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md) eignen sich ideal für Anwendungsmuster, die eine Aktualisierung von Problemen und Löschungen bei Tabellen in Ihrer Datenbank erwarten, z. B. SOR-Anwendungen (System of Record). Dies bedeutet, dass vorhandene Datenmuster bei Ihrer Anwendung nicht geändert werden müssen, um Ledgerfunktionen zu aktivieren.  

In [aktualisierbaren Ledgertabellen](ledger-updatable-ledger-tables.md) wird der Verlauf von Änderungen an beliebigen Zeilen in Ihrer Datenbank nachverfolgt, wenn es zu Transaktionen kommt, die Updates oder Löschungen durchführen. Eine aktualisierbare Ledgertabelle ist eine Tabelle mit Systemversionsverwaltung, die einen Verweis auf eine andere Tabelle mit einem gespiegelten Schema enthält. Das System verwendet diese Tabelle, um bei jeder Aktualisierung oder Löschung einer Zeile in der Ledgertabelle die vorherige Version der Zeile automatisch zu speichern. Diese andere Tabelle wird als „Verlaufstabelle“ bezeichnet. Die Verlaufstabelle wird bei der Erstellung einer aktualisierbaren Ledgertabelle automatisch erstellt. Die Werte in der aktualisierbaren Ledgertabelle und der zugehörigen Verlaufstabelle stellen eine Chronik der Werte Ihrer Datenbank im zeitlichen Verlauf zur Verfügung. Zum problemlosen Abfragen dieser Chronik Ihrer Datenbank wird eine vom System generierte Ledgeransicht erstellt, die die aktualisierbare Ledgertabelle mit der Verlaufstabelle verknüpft.

Weitere Informationen zum Erstellen und Verwenden von aktualisierbaren Ledgertabellen finden Sie unter [Erstellen von aktualisierbaren Ledgertabellen](ledger-how-to-updatable-ledger-tables.md).

### <a name="append-only-ledger-tables"></a>Ledgertabellen, die nur Anfügevorgänge unterstützen

[Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md) eignen sich ideal für Anwendungsmuster, in denen nur Einfügungen erfolgen, z. B. SEIM-Anwendungen (Security Information & Event Management). Ledgertabellen, die nur Anfügevorgänge unterstützen, blockieren Aktualisierungen und Löschungen auf API-Ebene (Application Programming Interface), und bieten so einen weiteren Manipulationsschutz vor privilegierten Benutzern wie Systemadministratoren und DBAs. Da nur Einfügungen in das System zulässig sind, gibt es bei Ledgertabellen, die nur Anfügevorgänge unterstützen, keine entsprechende Verlaufstabelle, weil kein Verlauf erfasst werden muss. So wie bei aktualisierbaren Ledgertabellen wird eine Ledgeransicht erstellt. Sie bietet Einblicke in die Transaktion, mit der Zeilen in die Tabelle (die nur Anfügevorgänge unterstützt) eingefügt wurden, und den Benutzer, der die Einfügung ausgeführt hat.

Weitere Informationen zum Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen, finden Sie unter [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md).

### <a name="database-ledger"></a>Datenbankledger

Der Datenbankledger besteht aus Systemtabellen, in denen die kryptografischen Hashes der im System verarbeiteten Transaktionen gespeichert werden. Da Transaktionen die [Unteilbarkeit](/windows/win32/cossdk/acid-properties)-Einheit bei der Datenbank-Engine sind, ist dies die im Datenbankledger erfasste Arbeitseinheit. Insbesondere bei einem Transaktionscommit wird der SHA-256-Hash aller Zeilen, die durch die Transaktion in der Ledgertabelle geändert wurden, – zusammen mit einigen Metadaten für die Transaktion, z. B. der Identität des Benutzers, der sie ausgeführt hat, und dessen Commitzeitstempel – als *Transaktionseintrag* im Datenbankledger angefügt. Alle 30 Sekunden werden die von der Datenbank verarbeiteten Transaktionen mithilfe einer Merkle-Baumdatenstruktur mit SHA-256 zusammen gehasht und so ein Stammhash erzeugt. Dadurch wird ein Block gebildet, der dann mithilfe seines Stammhashes zusammen mit dem Stammhash des vorherigen Blocks als Eingabe für die Hashfunktion mit SHA-256 gehasht wird und eine Blockchain bildet.

Weitere Informationen zum Datenbankledger finden Sie unter [Datenbankledger](ledger-database-ledger.md).

### <a name="database-digests"></a>Datenbankhashes

Der Hash des letzten Blocks im Datenbankledger wird als „Datenbankhash“ bezeichnet und stellt den Status aller Ledgertabellen in der Datenbank zum Zeitpunkt der Blockgenerierung dar. Bei der Bildung eines Blocks wird dessen zugeordneter Datenbankhash veröffentlicht und außerhalb von Azure SQL-Datenbank in einem manipulationsgeschützten Speicher gespeichert. Da Datenbankhashes den Status der Datenbank zum Zeitpunkt ihrer Generierung darstellen, ist der Schutz der Hashes vor Manipulationen von entscheidender Bedeutung. Ein Angreifer, der Zugriff zum Ändern der Digests hat, könnte die Daten in der Datenbank manipulieren, die Hashes generieren, die die Datenbank mit den manipulierten Änderungen darstellen, und dann die Digests so ändern, dass sie den aktualisierten Hash der Transaktionen im Block darstellen. Ledger bietet die Möglichkeit zum automatischen Generieren von Datenbankhashes und deren Speicherung in [unveränderlichen Azure Storage-Blobs](../../storage/blobs/storage-blob-immutable-storage.md) oder in [Azure Confidential Ledger](/azure/confidential-ledger/), um Manipulationen zu verhindern. Alternativ können Benutzer Datenbankhashes manuell generieren und am Speicherort ihrer Wahl speichern. Datenbankhashes werden verwendet, um später zu überprüfen, ob die in Ledgertabellen gespeicherten Daten manipuliert wurden.

Weitere Informationen zu den Datenbankhashes finden Sie unter [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md).

### <a name="ledger-verification"></a>Ledgerüberprüfung

Das Ledgerfeature erlaubt nicht, dass Benutzer den Inhalt des Ledgers ändern können. Ein Angreifer oder Systemadministrator, der Kontrolle über den Computer hat, kann jedoch alle Systemüberprüfungen umgehen und die Daten direkt manipulieren. So kann beispielsweise ein Angreifer oder Systemadministrator die Datenbankdateien im Speicher bearbeiten. Ledger kann solche Angriffe nicht verhindern, garantiert aber, dass Manipulationen bei einer Überprüfung der Ledgerdaten erkannt werden. Der Ledgerüberprüfungsprozess verwendet einen oder mehrere zuvor generierte Datenbankhashes als Eingabe und berechnet die im Datenbankledger gespeicherten Hashes – basierend auf dem aktuellen Status der Ledgertabellen – neu. Wenn die berechneten Hashes mit den Eingabehashes nicht übereinstimmen, schlägt die Überprüfung fehl (was auf eine Manipulation der Daten hinweist) und meldet alle erkannten Inkonsistenzen.

Da die Ledgerüberprüfung alle Hashes für Transaktionen in der Datenbank neu berechnet, kann dies bei Datenbanken mit großen Datenmengen ein ressourcenintensiver Prozess sein. Die Ledgerüberprüfung sollte nur ausgeführt werden, wenn Benutzer die Integrität ihrer Datenbank überprüfen müssen (statt dass dies kontinuierlich geschieht). Idealerweise sollte die Ledgerüberprüfung nur dann ausgeführt werden, wenn die Organisation, die die Daten hostet, eine Überwachung durchläuft und kryptografische Beweise zur Integrität ihrer Daten für eine andere Partei bereitstellen muss. Zur Senkung der Überprüfungskosten macht Ledger Optionen verfügbar, um einzelne Ledgertabellen oder nur eine Teilmenge des Ledgers zu überprüfen.

Weitere Informationen zur Ledgerüberprüfung finden Sie unter [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md).

## <a name="next-steps"></a>Nächste Schritte
 
- [Schnellstart: Erstellen einer Azure SQL-Datenbank mit aktiviertem Ledger](ledger-create-a-single-database-with-ledger-enabled.md)
- [Aktualisierbare Ledgertabellen](ledger-updatable-ledger-tables.md)   
- [Ledger-Tabellen, die nur Anfügevorgänge unterstützen](ledger-append-only-ledger-tables.md)   
- [Datenbankledger](ledger-database-ledger.md)   
- [Digestverwaltung und Datenbanküberprüfung](ledger-digest-management-and-database-verification.md)   
- [Einschränkungen beim Azure SQL-Datenbank-Ledger](ledger-limits.md)
- [Erstellen und Verwenden aktualisierbarer Ledgertabellen](ledger-how-to-updatable-ledger-tables.md)
- [Erstellen und Verwenden von Ledgertabellen, die nur Anfügevorgänge unterstützen](ledger-how-to-append-only-ledger-tables.md)
- [Zugreifen auf die in Azure Confidential Ledger (ACL) gespeicherten Digests](ledger-how-to-access-acl-digest.md)
- [Überprüfen einer Ledgertabelle zum Erkennen von Manipulationen](ledger-verify-database.md)
