---
title: Lesereplikate – Azure Database for PostgreSQL – Hyperscale (Citus)
description: In diesem Artikel wird das Feature „Lesereplikat“ in Azure Database for PostgreSQL – Hyperscale (Citus) beschrieben.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 04/07/2021
ms.openlocfilehash: cf8c64bf3858f0d3b7d633b94bc7302fbe563f87
ms.sourcegitcommit: 6ed3928efe4734513bad388737dd6d27c4c602fd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2021
ms.locfileid: "107023816"
---
# <a name="read-replicas-in-azure-database-for-postgresql---hyperscale-citus"></a>Lesereplikate in Azure Database for PostgreSQL – Hyperscale (Citus)

> [!IMPORTANT]
> Lesereplikate in Hyperscale (Citus) sind zurzeit als Vorschauversion verfügbar. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
>
> Eine vollständige Liste der anderen neuen Features finden Sie unter [Vorschaufeatures für Hyperscale (Citus)](hyperscale-preview-features.md).

Das Feature „Lesereplikat“ ermöglicht es Ihnen, Daten aus einer Hyperscale (Citus)-Servergruppe in eine schreibgeschützte Servergruppe zu replizieren. Replikate werden mithilfe der physischen Replikationstechnologie von PostgreSQL **asynchron** aktualisiert. Sie können vom primären Server in eine unbegrenzte Anzahl von Replikaten replizieren.

Replikate sind neue Servergruppen, die Sie ähnlich wie reguläre Hyperscale (Citus)-Servergruppen verwalten. Für jedes Lesereplikat werden Ihnen die bereitgestellten Computeressourcen in Form von virtuellen Kernen sowie der Speicher in GB/Monat in Rechnung gestellt.

Erfahren Sie, wie Sie [Replikate erstellen und verwalten](howto-hyperscale-read-replicas-portal.md).

## <a name="when-to-use-a-read-replica"></a>Einsatzmöglichkeiten von Lesereplikaten

Das Feature für Lesereplikate kann die Leistung und Skalierung von leseintensiven Workloads verbessern. Leseworkloads können in den Replikaten isoliert werden, während Schreibworkloads an den primären Server weitergeleitet werden können.

In einem häufig anzutreffenden Szenario verwenden BI- und Analyseworkloads das Lesereplikat als Datenquelle für die Berichterstellung.

Da Replikate schreibgeschützt sind, führen sie nicht direkt zu einer verringerten Auslastung der Schreibkapazität auf dem primären Server.

### <a name="considerations"></a>Überlegungen

Das Feature ist für Szenarien vorgesehen, in denen die Verzögerung bei der Replikation akzeptabel und für das Auslagern von Abfragen vorgesehen ist. Es ist nicht für Szenarien mit synchroner Replikation gedacht, in denen die Replikatdaten auf dem neuesten Stand sein sollen. Zwischen dem primären Server und dem Replikat entsteht eine messbare Verzögerung. Diese Verzögerung kann – abhängig von der Workload und der Latenz zwischen primärem Server und Replikat – Minuten oder sogar Stunden dauern.  Letztendlich sind die Daten auf dem Replikat mit den Daten auf dem primären Server konsistent. Verwenden Sie das Feature für Workloads, für die diese Verzögerung akzeptabel ist. 

## <a name="create-a-replica"></a>Erstellen eines Replikats

Wenn Sie mit dem Workflow zum Erstellen eines Replikats beginnen, wird eine leere Hyperscale (Citus)-Servergruppe erstellt. Die neue Gruppe wird dann mit den Daten aus der primären Servergruppe gefüllt. Die Erstellungszeit hängt von der Datenmenge auf dem primären Server und der verstrichenen Zeit seit der letzten wöchentlichen vollständigen Sicherung ab. Dieser Zeitraum kann wenige Minuten bis zu mehrere Stunden umfassen.

Das Feature für Lesereplikate verwendet die physische Replikation von PostgreSQL, nicht die logische Replikation. Der Standardmodus ist Streamingreplikation mithilfe von Replikationsslots.
Bei Bedarf wird zum Aufholen der Protokollversand verwendet.

[Erfahren Sie, wie Sie ein Lesereplikat im Azure-Portal erstellen](howto-hyperscale-read-replicas-portal.md).

## <a name="connect-to-a-replica"></a>Herstellen einer Verbindung mit einem Replikat

Wenn Sie ein Replikat erstellen, erbt es keine Firewallregeln der primären Servergruppe. Diese Regeln müssen separat für das Replikat eingerichtet werden.

Das Replikat erbt das Administratorkonto („citus“) von der primären Servergruppe.
Alle Benutzerkonten werden in die Lesereplikate repliziert. Sie können nur mit denjenigen Benutzerkonten eine Verbindung mit einem Lesereplikat herstellen, die auf dem primären Server verfügbar sind.

Sie können eine Verbindung mit dem Koordinatorknoten des Replikats (unter Angabe von dessen Hostnamen und eines gültigen Benutzerkontos) genauso herstellen wie bei einer regulären Hyperscale (Citus)-Servergruppe.
Beim Server **my replica** (mein replikat) mit dem Administratorbenutzernamen **citus** können Sie mithilfe von psql eine Verbindung mit dem Koordinatorknoten so herstellen:

```bash
psql -h c.myreplica.postgres.database.azure.com -U citus@myreplica -d postgres
```

Geben Sie an der Eingabeaufforderung das Kennwort für das Benutzerkonto ein.

## <a name="considerations"></a>Überlegungen

In diesem Abschnitt werden Aspekte des Features für Lesereplikate zusammengefasst.

### <a name="new-replicas"></a>Neue Replikate

Ein Lesereplikat wird als neue Hyperscale (Citus)-Servergruppe erstellt. Eine vorhandene Servergruppe kann nicht in ein Replikat umgewandelt werden. Es kann kein Replikat eines anderen Lesereplikats erstellt werden.

### <a name="replica-configuration"></a>Replikatkonfiguration

Ein Replikat wird mit denselben Compute-, Speicher- und Workerknoteneinstellungen wie der primäre Server erstellt. Nach der Erstellung eines Replikats können mehrere Einstellungen geändert werden, darunter der Aufbewahrungszeitraum für Speicher und Sicherungen. Andere Einstellungen, z. B. die Speichergröße und die Anzahl von Workerknoten, können in Replikaten nicht geändert werden.

Denken Sie daran, die Replikate so stark zu machen, dass sie mit Änderungen, die vom primären Server eintreffen, Schritt halten können. Sie müssen beispielsweise die Computeleistung in Replikaten hochskalieren, wenn Sie sie auf dem primären Server hochskalieren.

Firewallregeln und Parametereinstellungen werden beim Erstellen des Replikats oder danach vom primären Server nicht geerbt.

### <a name="regions"></a>Regions

Hyperscale (Citus)-Servergruppen unterstützen nur die Replikation in derselben Region.

## <a name="next-steps"></a>Nächste Schritte

* Erhalten Sie Informationen zum [Erstellen und Verwalten von Lesereplikaten im Azure-Portal](howto-hyperscale-read-replicas-portal.md).
