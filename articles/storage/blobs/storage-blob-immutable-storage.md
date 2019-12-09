---
title: Unveränderlicher Blobspeicher – Azure Storage
description: Azure Storage bietet WORM-Unterstützung (Write Once, Read Many – Einmal schreiben, oft lesen) für Blobspeicher (Objektspeicher), der Benutzern das Speichern in einem nicht löschbaren und nicht änderbaren Zustand für einen angegebenen Zeitraum ermöglicht.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: tamram
ms.reviewer: hux
ms.subservice: blobs
ms.openlocfilehash: 92bfa4f13467763fd88b9ae993554aef69355d75
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555227"
---
# <a name="store-business-critical-blob-data-with-immutable-storage"></a>Speichern unternehmenskritischer Blobdaten mit unveränderlichem Speicher

Unveränderlicher Speicher für Azure-Blobspeicher ermöglicht es Benutzern, unternehmenskritische Datenobjekte im WORM-Zustand (Write Once, Read Many – Einmal schreiben, oft lesen) zu speichern. In diesem Zustand sind die Daten für einen vom Benutzer angegebenen Zeitraum nicht löschbar und nicht änderbar. Während des Aufbewahrungszeitraums können Blobs erstellt und gelesen, aber nicht geändert oder gelöscht werden. Unveränderlicher Speicher ist für Konten vom Typ „Allgemein v2“ sowie für Blobspeicherkonten in allen Azure-Regionen verfügbar.

Informationen zum Festlegen und Aufheben einer Aufbewahrung für juristische Zwecke sowie zum Erstellen einer zeitbasierten Aufbewahrungsrichtlinie über das Azure-Portal, per PowerShell oder mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher](storage-blob-immutability-policies-manage.md).

## <a name="about-immutable-blob-storage"></a>Informationen zu unveränderlichem Blobspeicher

Unveränderlicher Speicher unterstützt Organisationen im Gesundheitswesen sowie Finanzinstitute und ähnliche Branchen (insbesondere Broker-Dealer-Organisationen) bei der sicheren Speicherung von Daten. Darüber hinaus kann unveränderlicher Speicher in allen Szenarien verwendet werden, in denen kritische Daten vor Änderung oder Löschung geschützt werden sollen.

Beispiele für typische Anwendungen:

- **Einhaltung gesetzlicher Bestimmungen**: Unveränderlicher Speicher für Azure-Blobspeicher unterstützt Organisationen dabei, SEC 17a-4(f), CFTC 1.31(d), FINRA und weitere Bestimmungen einzuhalten. In einem technischen Whitepaper von Cohasset Associates ist ausführlich beschrieben, wie diese gesetzlichen Anforderungen mit unveränderlichem Speicher erfüllt werden können. Sie können das Whitepaper über das [Microsoft-Vertrauensstellungsportal](https://aka.ms/AzureWormStorage) herunterladen. Das [Azure Trust Center](https://www.microsoft.com/trustcenter/compliance/compliance-overview) enthält ausführliche Informationen zu den Compliancezertifizierungen.

- **Sichere Dokumentaufbewahrung**: Mit unveränderlichem Speicher für Azure-Blobspeicher wird sichergestellt, dass Daten von Benutzern auch dann nicht geändert oder gelöscht werden können, wenn diese über Berechtigungen für die Kontoverwaltung verfügen.

- **Gesetzliche Aufbewahrungspflicht**: Mit unveränderlichem Speicher für Azure-Blobspeicher können Benutzer sensible Informationen, die für Rechtsstreitigkeiten oder die geschäftliche Nutzung wichtig sind, für den gewünschten Zeitraum in einem manipulationsgeschütztem Zustand speichern, bis die Aufbewahrungspflicht nicht mehr gilt. Dieses Feature ist nicht allein auf rechtliche Anwendungsfälle beschränkt, sondern kann auch als ereignisbasierter Aufbewahrungsvorgang oder Unternehmenssperre angesehen werden, wenn Daten basierend auf Ereignisauslösern oder Unternehmensrichtlinien geschützt werden müssen.

Unveränderlicher Speicher unterstützt folgende Features:

- **[Unterstützung einer zeitbasierten Aufbewahrungsrichtlinie](#time-based-retention-policies)** : Benutzer können Richtlinien für die Speicherung von Daten für einen bestimmten Zeitraum festlegen. Wenn eine zeitbasierte Aufbewahrungsrichtlinie festgelegt wird, können Blobs erstellt und gelesen, aber nicht geändert oder gelöscht werden. Nach Ablauf des Aufbewahrungszeitraums können Blobs gelöscht, aber nicht überschrieben werden.

- **[Unterstützung einer Richtlinie zur gesetzlichen Aufbewahrungspflicht](#legal-holds)** : Ist der Aufbewahrungszeitraum nicht bekannt, können Benutzer eine Aufbewahrung für juristische Zwecke festlegen, um unveränderliche Daten zu speichern, bis die Aufbewahrung für juristische Zwecke aufgehoben wird.  Wenn eine Richtlinie für die gesetzliche Aufbewahrungspflicht festgelegt wird, können Blobs erstellt und gelesen, aber nicht geändert oder gelöscht werden. Jeder gesetzlichen Aufbewahrungspflicht ist ein benutzerdefiniertes alphanumerisches Tag zugeordnet (z. B. Fall-ID, Ereignisname usw.), das als Bezeichnerzeichenfolge verwendet wird. 

- **Unterstützung für alle Blobebenen**: WORM-Richtlinien sind unabhängig von der Azure-Blobspeicherebene und gelten für alle Ebenen: „Heiß“, „Kalt“ und „Archiv“. Benutzer können Daten in die Ebene überführen, für die die Workloadkosten bestmöglich optimiert sind, während gleichzeitig die Unveränderlichkeit der Daten sichergestellt ist.

- **Konfiguration auf Containerebene**: Benutzer können zeitbasierte Aufbewahrungsrichtlinien und Tags für gesetzliche Aufbewahrungspflichten auf Containerebene konfigurieren. Mit den Einstellungen auf Containerebene können Benutzer zeitbasierte Aufbewahrungsrichtlinien erstellen und sperren, Aufbewahrungszeiträume verlängern, Zeiträume für gesetzliche Aufbewahrungspflichten festlegen und aufheben usw. Diese Richtlinien gelten für alle Blobs im Container (vorhandene und neue).

- **Unterstützung der Überwachungsprotokollierung**: Jeder Container enthält ein Richtlinien-Überwachungsprotokoll. Es enthält bis zu sieben zeitbasierte Aufbewahrungsbefehle für gesperrte zeitbasierte Aufbewahrungsrichtlinien sowie die Benutzer-ID, den Befehlstyp, Zeitstempel und den Aufbewahrungszeitraum. Für Zeiträume zur gesetzlichen Aufbewahrungspflicht enthält das Protokoll Benutzer-ID, Befehlstyp, Zeitstempel und die entsprechenden Tags. Dieses Protokoll wird für die Lebensdauer der Richtlinie gemäß den SEC 17a-4(f)-Bestimmungsrichtlinien aufbewahrt. Im [Azure-Aktivitätsprotokoll](../../azure-monitor/platform/activity-logs-overview.md) werden umfassendere Protokolldaten mit allen Aktivitäten auf Steuerungsebene angezeigt. Wenn Sie [Azure-Diagnoseprotokolle](../../azure-monitor/platform/resource-logs-overview.md) aktivieren, werden dagegen nur Vorgänge auf Datenebene aufbewahrt und angezeigt. Der Benutzer ist für die dauerhafte Speicherung dieser Protokolle verantwortlich, die aus gesetzlichen oder anderen Gründen ggf. erforderlich ist.

## <a name="how-it-works"></a>So funktioniert's

Unveränderlicher Speicher für Azure-Blobspeicher unterstützt zwei Arten von WORM-Richtlinien bzw. Richtlinien für die unveränderliche Speicherung: zeitbasierte Aufbewahrung und gesetzliche Aufbewahrungspflicht. Wenn eine zeitbasierte Aufbewahrungsrichtlinie oder ein Zeitraum für die gesetzliche Aufbewahrungspflicht auf einen Container angewendet wird, werden alle vorhandenen Blobs innerhalb von weniger als 30 Sekunden in einen unveränderlichen WORM-Zustand versetzt. Alle neuen Blobs, die in diesen Container hochgeladen werden, werden ebenfalls in den unveränderlichen Zustand versetzt. Nachdem alle Blobs in den unveränderlichen Zustand versetzt wurden, wird die Unveränderlichkeitsrichtlinie bestätigt, und alle Überschreibungs- oder Löschvorgänge für vorhandene und neue Objekte im unveränderlichen Container sind unzulässig.

Container und Speicherkonten können nicht gelöscht werden, wenn der Container oder das Speicherkonto Blobs enthält, die durch eine Unveränderlichkeitsrichtlinie geschützt sind. Die Containerlöschung ist nicht erfolgreich, wenn mindestens ein Blob mit einer gesperrten zeitbasierten Aufbewahrungsrichtlinie oder einer Aufbewahrung für juristische Zwecke vorhanden ist. Die Speicherkontolöschung ist nicht erfolgreich, wenn mindestens ein WORM-Container mit Aufbewahrung für juristische Zwecke oder ein Blob mit einem aktiven Aufbewahrungszeitraum vorhanden ist.

### <a name="time-based-retention-policies"></a>Zeitbasierte Aufbewahrungsrichtlinien

> [!IMPORTANT]
> Eine zeitbasierte Aufbewahrungsrichtlinie muss *gesperrt* sein, damit das Blob zur Erzielung von Konformität mit SEC 17a-4(f) und anderen gesetzlichen Bestimmungen in einem konformen unveränderlichen Zustand ist (Schreib- und Löschschutz). Sie sollten die Richtlinie in einem angemessenen Zeitraum sperren – in der Regel weniger als 24 Stunden. Der ursprüngliche Zustand einer angewendeten zeitbasierten Aufbewahrungsrichtlinie lautet *Entsperrt*, damit Sie das Feature testen und Änderungen an der Richtlinie vornehmen können, bevor Sie das Sperren durchführen. Der Zustand *Entsperrt* bietet zwar Unveränderlichkeitsschutz, aber wir empfehlen Ihnen, *Entsperrt* nicht für andere Zwecke als für kurzfristige Featuretests zu verwenden. 

Wenn eine zeitbasierte Aufbewahrungsrichtlinie auf einen Container angewendet wird, bleiben alle Blobs im Container so lange im unveränderlichen Zustand, wie der Aufbewahrungszeitraum *gilt*. Die Gültigkeit des Aufbewahrungszeitraums für vorhandene Blobs entspricht der Differenz zwischen der Bloberstellung und dem vom Benutzer angegebenen Aufbewahrungszeitraum.

Für neue Blobs entspricht die Gültigkeit des Aufbewahrungszeitraums dem vom Benutzer angegebenen Aufbewahrungszeitraum. Da Benutzer den Aufbewahrungszeitraum verlängern können, nutzt unveränderlicher Speicher den letzten Wert des vom Benutzer angegebenen Aufbewahrungszeitraums, um den effektiven Aufbewahrungszeitraum zu berechnen.

Ein Beispiel: Angenommen, der Benutzer erstellt eine zeitbasierte Aufbewahrungsrichtlinie mit einem Aufbewahrungszeitraum von fünf Jahren. Ein in diesem Container vorhandenes Blob (_testblob1_) wurde vor einem Jahr erstellt. Für _testblob1_ gilt ein Aufbewahrungszeitraum von vier Jahren. Wenn ein neues Blob (_testblob2_) in den Container hochgeladen wird, beträgt der effektive Aufbewahrungszeitraum für das neue Blob fünf Jahre.

Eine zeitbasierte Aufbewahrungsrichtlinie im Zustand „Entsperrt“ wird nur für Featuretests empfohlen, und eine Richtlinie muss gesperrt sein, um mit SEC 17a-4(f) konform zu sein und andere gesetzliche Bestimmungen zu erfüllen. Nachdem eine zeitbasierte Aufbewahrungsrichtlinie gesperrt wurde, kann die Richtlinie nicht mehr entfernt werden, und es sind maximal fünf Erhöhungen des effektiven Aufbewahrungszeitraums zulässig. Weitere Informationen zum Festlegen und Sperren zeitbasierter Aufbewahrungsrichtlinien finden Sie unter [Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher](storage-blob-immutability-policies-manage.md).

Für Aufbewahrungsrichtlinien gelten folgende Grenzwerte:

- Ein Speicherkonto kann über maximal 1.000 Container mit gesperrten zeitbasierten Unveränderlichkeitsrichtlinien verfügen.
- Der Mindestwert für den Aufbewahrungszeitraum beträgt einen Tag. Der Höchstwert beträgt 146.000 Tage (400 Jahre).
- Für einen Container sind höchstens fünf Bearbeitungen zur Verlängerung eines Aufbewahrungszeitraums gesperrte zeitbasierte unveränderliche Richtlinien möglich.
- Für einen Container werden maximal sieben Überwachungsprotokolle der zeitbasierten Aufbewahrungsrichtlinie für eine gesperrte Richtlinie aufbewahrt.

### <a name="legal-holds"></a>Gesetzliche Aufbewahrungspflichten

Wenn Sie einen Zeitraum für die gesetzliche Aufbewahrungspflicht festlegen, verbleiben alle vorhandenen und neuen Blobs im unveränderlichen Zustand, bis die gesetzliche Aufbewahrungspflicht aufgehoben wird. Weitere Informationen zum Festlegen und Aufheben einer Aufbewahrung für juristische Zwecke finden Sie unter [Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher](storage-blob-immutability-policies-manage.md).

Für einen Container können gleichzeitig eine gesetzliche Aufbewahrungspflicht und eine zeitbasierte Aufbewahrungsrichtlinie gelten. Alle Blobs in diesem Container verbleiben so lange im unveränderlichen Zustand, bis alle gesetzlichen Aufbewahrungspflichten aufgehoben wurden. Dies gilt auch, wenn die effektive Aufbewahrungsdauer bereits abgelaufen ist. Dagegen verbleibt ein Blob auch dann so lange im unveränderlichen Zustand, bis der effektive Aufbewahrungszeitraum abgelaufen ist, wenn alle Zeiträume für die gesetzliche Aufbewahrungspflicht aufgehoben wurden.

Die folgende Tabelle gibt Aufschluss über die Arten von Blobspeichervorgängen, die für die verschiedenen Unveränderlichkeitsszenarien deaktiviert sind. Weitere Informationen finden Sie in der Dokumentation [Blob-Dienst-REST-API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

|Szenario  |Blobzustand  |Blobvorgänge sind nicht zulässig  |
|---------|---------|---------|
|Effektiver Aufbewahrungszeitraum für das Blob ist noch nicht abgelaufen bzw. ein Zeitraum für die gesetzliche Aufbewahrungspflicht wurde festgelegt     |Unveränderlich: Lösch- und Schreibschutz         | Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Delete Container, Delete Blob, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Effektiver Aufbewahrungszeitraum für das Blob abgelaufen     |Nur Schreibschutz (Löschvorgänge sind zulässig)         |Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Alle Zeiträume zur gesetzlichen Aufbewahrungspflicht wurden aufgehoben, und für den Container wurde keine zeitbasierte Aufbewahrungsrichtlinie festgelegt.     |Veränderlich         |Keine         |
|Keine WORM-Richtlinie erstellt (zeitbasierte Aufbewahrung oder gesetzliche Aufbewahrungspflicht)     |Veränderlich         |Keine         |

<sup>1</sup> Die Anwendung lässt diese Vorgänge zu, um einmalig ein neues Blob zu erstellen. Alle nachfolgenden Überschreibungsvorgänge in einem vorhandenen Blobpfad eines unveränderlichen Containers sind nicht zulässig.

Im Zusammenhang mit der Aufbewahrung für juristische Zwecke gelten folgende Grenzwerte:

- Ein Speicherkonto kann über maximal 1.000 Container mit einer Einstellung der gesetzlichen Aufbewahrungspflicht verfügen.
- Ein Container kann über maximal 10 Tags für die gesetzliche Aufbewahrungspflicht verfügen.
- Ein Tag für die Aufbewahrung für juristische Zwecke muss mindestens drei alphanumerische Zeichen lang sein. Die Höchstlänge beträgt 23 alphanumerische Zeichen.
- Für einen Container werden höchstens zehn Richtlinien-Überwachungsprotokolle für die gesetzliche Aufbewahrungspflicht für die Dauer der Richtlinie aufbewahrt.

## <a name="pricing"></a>Preise

Für die Nutzung dieses Features fallen keine zusätzlichen Gebühren an. Unveränderliche Daten werden auf die gleiche Weise abgerechnet wie änderbare Daten. Ausführliche Informationen zu Preisen für Azure Blob Storage finden Sie auf der [Seite mit den Preisen für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="faq"></a>Häufig gestellte Fragen

**Kann Dokumentation zum Thema WORM-Konformität bereitgestellt werden?**

Ja. Für die Dokumentation der Konformität hat Microsoft ein führendes Unternehmen für unabhängige Bewertungen (Cohasset Associates) beauftragt, das auf Datensatzverwaltung und Information Governance spezialisiert ist. Die Aufgabe bestand darin, unveränderlichen Blobspeicher und dessen Konformität mit den spezifischen Anforderungen der Finanzdienstleistungsbranche zu bewerten. Cohasset hat bestätigt, dass unveränderlicher Blobspeicher bei Verwendung zur Aufbewahrung von zeitbasierten Blobs im WORM-Zustand die relevanten Speicheranforderungen der CFTC-Regel 1.31(c)-(d), der FINRA-Regel 4511 und der SEC-Regel 17a-4 erfüllt. Microsoft hat die Einhaltung dieser Regeln angestrebt, da sie die weltweit strikteste Anleitung zur Aufbewahrung von Datensätzen für Finanzinstitute darstellen. Der Cohasset-Bericht ist im [Microsoft Service Trust Center](https://aka.ms/AzureWormStorage) verfügbar. Wenden Sie sich an den Azure-Support, um von Microsoft einen Nachweis der WORM-Konformität anzufordern.

**Gilt die Funktion nur für Blockblobs oder auch für Seiten- und Anfügeblobs?**

Unveränderlicher Speicher kann mit einem beliebigen Blobtyp verwendet werden, da er auf der Containerebene festgelegt wird. Für Container, in denen in erster Linie Blockblobs gespeichert werden, empfehlen wir jedoch die Verwendung von WORM. Im Gegensatz zu Blockblobs müssen neue Seiten- und Anfügeblobs außerhalb eines WORM-Containers erstellt und dann hineinkopiert werden. Nach dem Kopieren dieser Blobs in einen WORM-Container sind keine weiteren *Anfügevorgänge* an ein Anfügeblob oder Änderungen eines Seitenblobs zulässig. Es wird davon abgeraten, eine WORM-Richtlinie für einen Container festzulegen, in dem VHDs (Seitenblobs) für aktive virtuelle Computer gespeichert sind, da dadurch der VM-Datenträger gesperrt wird.

**Muss ich ein neues Speicherkonto erstellen, um dieses Feature zu nutzen?**

Nein. Sie können unveränderlichen Speicher mit allen bereits vorhandenen oder neu erstellten Konten vom Typ „Allgemein v2“ oder Blobspeicherkonten verwenden. Dieses Feature ist für die Verwendung mit Blockblobs in Konten vom Typ „Allgemein v2“ und in Blobspeicherkonten vorgesehen. Speicherkonten vom Typ „Universell v1“ werden nicht unterstützt, aber es kann problemlos ein Upgrade auf „Allgemein v2“ durchgeführt werden. Informationen zum Upgraden eines vorhandenen Kontos vom Typ „Universell v1“ finden Sie unter [Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Allgemein v2“](../common/storage-account-upgrade.md).

**Kann ich sowohl eine Richtlinie für eine gesetzliche Aufbewahrungspflicht als auch eine Richtlinie für die zeitbasierte Aufbewahrung anwenden?**

Ja, für einen Container können gleichzeitig eine gesetzliche Aufbewahrungspflicht und eine zeitbasierte Aufbewahrungsrichtlinie gelten. Alle Blobs in diesem Container verbleiben so lange im unveränderlichen Zustand, bis alle gesetzlichen Aufbewahrungspflichten aufgehoben wurden. Dies gilt auch, wenn die effektive Aufbewahrungsdauer bereits abgelaufen ist. Dagegen verbleibt ein Blob auch dann so lange im unveränderlichen Zustand, bis der effektive Aufbewahrungszeitraum abgelaufen ist, wenn alle Zeiträume für die gesetzliche Aufbewahrungspflicht aufgehoben wurden.

**Gelten Richtlinien für die gesetzliche Aufbewahrungspflicht nur für rechtliche Abläufe, oder gibt es auch andere Nutzungsszenarien?**

Nein. „Aufbewahrung für juristische Zwecke“ ist nur der allgemeine Ausdruck, der für eine nicht zeitbasierte Aufbewahrungsrichtlinie verwendet wird. Er bezieht sich nicht ausschließlich auf Vorgänge im Zusammenhang mit Rechtsstreitigkeiten. Richtlinien zur gesetzlichen Aufbewahrungspflicht sind nützlich, um Überschreibungen und Löschungen zum Schützen von wichtigen WORM-Unternehmensdaten zu deaktivieren, wenn der Aufbewahrungszeitraum unbekannt ist. Sie können sie als Unternehmensrichtlinie zum Schützen Ihrer unternehmenskritischen WORM-Workloads oder als Stagingrichtlinie verwenden, bevor für einen benutzerdefinierten Ereignisauslöser eine zeitbasierte Aufbewahrungsrichtlinie erforderlich ist. 

**Kann ich eine zeitbasierte Aufbewahrungsrichtlinie im Zustand _Gesperrt_ oder eine gesetzliche Aufbewahrungspflicht entfernen?**

Nur entsperrte zeitbasierte Aufbewahrungsrichtlinien können aus einem Container entfernt werden. Nachdem eine zeitbasierte Aufbewahrungsrichtlinie gesperrt wurde, kann sie nicht mehr entfernt werden. Es sind nur Verlängerungen des geltenden Aufbewahrungszeitraums zulässig. Tags für die gesetzliche Aufbewahrungspflicht können gelöscht werden. Wenn alle Tags für die gesetzliche Aufbewahrungspflicht gelöscht werden, kann die gesetzliche Aufbewahrungspflicht entfernt werden.

**Was passiert, wenn ich versuche, einen Container mit einer *gesperrten* zeitbasierten Aufbewahrungsrichtlinie oder gesetzlichen Aufbewahrungspflicht zu löschen?**

Beim Vorgang „Delete Container“ tritt ein Fehler auf, wenn mindestens ein Blob mit einer gesperrten zeitbasierten Aufbewahrungsrichtlinie oder einer gesetzlichen Aufbewahrungspflicht vorhanden ist. Der Vorgang „Delete Container“ ist erfolgreich, wenn kein Blob mit einem aktiven Aufbewahrungszeitraum vorhanden ist und keine gesetzlichen Aufbewahrungspflichten vorhanden sind. Sie müssen die Blobs löschen, bevor Sie den Container löschen können.

**Was passiert, wenn ich versuche, ein Speicherkonto mit einem WORM-Container zu löschen, das über eine *gesperrte* zeitbasierte Aufbewahrungsrichtlinie oder eine gesetzliche Aufbewahrungspflicht verfügt?**

Für das Löschen des Speicherkontos tritt ein Fehler auf, wenn mindestens ein WORM-Container mit einer gesetzlichen Aufbewahrungspflicht oder ein Blob mit einem aktiven Aufbewahrungszeitraum vorhanden ist. Sie müssen alle WORM-Container löschen, bevor Sie das Speicherkonto löschen können. Informationen zum Löschen des Containers finden Sie unter der vorherigen Frage.

**Kann ich die Daten zwischen unterschiedlichen Blobebenen („Hot“, „Cool“, „Cold“) verschieben, wenn sich das Blob im unveränderlichen Zustand befindet?**

Ja. Sie können den Befehl „Set Blob Tier“ nutzen, um Daten zwischen den Blobebenen zu verschieben, während die Daten im konformen, unveränderlichen Zustand verbleiben. Unveränderlicher Speicher wird für die Blobebenen „Hot“, „Cool“ und „Archiv“ unterstützt.

**Was passiert, wenn ich nicht bezahle und mein Aufbewahrungszeitraum abgelaufen ist?**

Bei einer fehlenden Zahlung gelten die üblichen Richtlinien der Datenaufbewahrung gemäß den Bedingungen Ihres Vertrags mit Microsoft.

**Bieten Sie einen Test- oder Karenzzeitraum an, in dem die Funktion ausprobiert werden kann?**

Ja. Wenn eine zeitbasierte Aufbewahrungsrichtlinie erstellt wird, befindet sie sich zunächst im *entsperrten* Zustand. In diesem Zustand können Sie alle gewünschten Änderungen am Aufbewahrungszeitraum vornehmen, z.B. diesen verlängern oder verkürzen und auch die Richtlinie löschen. Nach dem Sperren der Richtlinie bleibt diese gesperrt, bis der Aufbewahrungszeitraums abgelaufen ist. Mit dieser Sperrung der Richtlinie wird verhindert, dass der Aufbewahrungszeitraum gelöscht und geändert wird. Es wird dringend empfohlen, den *entsperrten* Zustand nur für Testzwecke zu nutzen und die Richtlinie innerhalb eines Zeitraums von 24 Stunden wieder zu sperren. Dieses Vorgehen erleichtert Ihnen, SEC 17a-4(f) und andere Bestimmungen einzuhalten.

**Kann ich neben Richtlinien für unveränderliche Blobs das vorläufige Löschen verwenden?**

Ja. Das [vorläufige Löschen für Azure-Blobspeicher](storage-blob-soft-delete.md) gilt für alle Container eines Speicherkontos, unabhängig von einer Richtlinie für die gesetzliche Aufbewahrungspflicht oder eine zeitbasierte Aufbewahrungsrichtlinie. Wir empfehlen Ihnen, das vorläufige Löschen als zusätzlichen Schutz zu verwenden, bevor Richtlinien für den unveränderlichen WORM-Zustand angewendet und bestätigt werden.

**Wo ist die Funktion verfügbar?**

Unveränderliche Speicher sind in den Regionen Azure Public, China und Behörden verfügbar. Sollte unveränderlicher Speicher in Ihrer Region nicht verfügbar sein, können Sie sich an den Support wenden und eine E-Mail an die folgende Adresse senden: azurestoragefeedback@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

[Festlegen und Verwalten von Unveränderlichkeitsrichtlinien für Blobspeicher](storage-blob-immutability-policies-manage.md)