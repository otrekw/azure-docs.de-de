---
title: Erstellen einer Konfiguration für bereichsbezogene Ressourcensätze
description: Erfahren Sie, wie Sie eine Regel für bereichsbezogene Ressourcensätze erstellen, um zu überschreiben, wie Ressourcen in Ressourcensätzen gruppiert werden.
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 02/17/2021
ms.openlocfilehash: 10e925a84dbe187ccdf5e444cb8b3dd4b7bb4676
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "102608001"
---
# <a name="create-scoped-resource-set-configuration-rules"></a>Erstellen von Konfigurationsregeln für bereichsbezogene Ressourcensätze

In skalierbaren Datenverarbeitungssystemen wird eine einzelne Tabelle in der Regel in mehreren Dateien auf einem Datenträger gespeichert. Dieses Konzept wird in Azure Purview mithilfe von Ressourcensätzen dargestellt. Ein Ressourcensatz ist ein einzelnes Objekt im Datenkatalog, das eine große Anzahl von Ressourcen im Speicher darstellt. Weitere Informationen finden Sie unter [Grundlegendes zu Ressourcensätzen](concept-resource-sets.md).

Beim Überprüfen eines Speicherkontos verwendet Azure Purview eine Reihe definierter Muster, um zu bestimmen, ob eine Gruppe von Ressourcen ein Ressourcensatz ist. In einigen Fällen entspricht die Ressourcensatzgruppierung von Azure Purview möglicherweise nicht genau dem Datentyp. Mit Regeln für bereichsbezogene Ressourcensätze können Sie anpassen oder überschreiben, wie Azure Purview erkennt, welche Ressourcen als Ressourcensätze gruppiert werden, und wie sie im Katalog angezeigt werden.

## <a name="how-to-create-a-scoped-resource-set-configuration"></a>Erstellen einer Konfiguration für bereichsbezogene Ressourcensätze

Führen Sie die folgenden Schritte aus, um eine neue Konfiguration für bereichsbezogene Ressourcensätze zu erstellen:

1. Navigieren Sie zum Verwaltungscenter. Wählen Sie im Menü **Scoped resource sets** (Bereichsbezogene Ressourcensätze) aus. Wählen Sie **+ Neu** aus, um einen neuen Konfigurationsregelsatz zu erstellen.

   :::image type="content" source="media/how-to-scoped-resource-sets/create-new-scoped-resource-set-rule.png" alt-text="Erstellen einer neuen Regel für bereichsbezogene Ressourcensätze" border="true":::

1. Geben Sie den Bereich für die Konfiguration des bereichsbezogenen Ressourcensatzes ein. Wählen Sie den Speicherkontotyp und den Namen des Speicherkontos aus, für das Sie einen Regelsatz erstellen möchten. Jeder Regelsatz wird in Relation zu einem Ordnerpfadbereich angewendet, der im Feld **Ordnerpfad** angegeben ist.

   :::image type="content" source="media/how-to-scoped-resource-sets/create-new-scoped-resource-set-scope.png" alt-text="Erstellen von Konfigurationen für bereichsbezogene Ressourcensätze" border="true":::

1. Um eine Regel für einen Konfigurationsbereich einzugeben, wählen Sie **+ Neue Regel** aus.

1. Füllen Sie die folgenden Felder aus, um eine Regel zu erstellen:

   1. **Regelname:** Der Name der Konfigurationsregel. Dieses Feld hat keine Auswirkung auf die Ressourcen, für die die Regel gilt.

   1. **Qualifizierter Name:** Ein qualifizierter Pfad, in dem eine Kombination aus Text, dynamischen Ersetzungsfunktionen und statischen Ersetzungsfunktionen verwendet wird, um Ressourcen mit der Konfigurationsregel zu vergleichen. Dieser Pfad ist relativ zum Bereich der Konfigurationsregel. Ausführliche Anweisungen zum Angeben qualifizierter Namen finden Sie im Abschnitt [Syntax](#syntax) weiter unten.

   1. **Anzeigename:** Der Anzeigename der Ressource. Dieses Feld ist optional. Verwenden Sie Nur-Text und statische Ersetzungsfunktionen, um die Darstellung von Ressourcen im Katalog anzupassen. Ausführlichere Anweisungen finden Sie im Abschnitt [Syntax](#syntax) weiter unten.

   1. **Do not group as resource set** (Nicht als Ressourcensatz gruppieren): Wenn diese Option aktiviert ist, wird die übereinstimmende Ressource nicht in einem Ressourcensatz gruppiert.

      :::image type="content" source="media/how-to-scoped-resource-sets/scoped-resource-set-rule-example.png" alt-text="Erstellen einer neuen Konfigurationsregel." border="true":::

1. Speichern Sie die Regel, indem Sie auf **Hinzufügen** klicken.

## <a name="scoped-resource-set-syntax"></a><a name="syntax"></a> Syntax für bereichsbezogene Ressourcensätze

Verwenden Sie beim Erstellen von Regeln für bereichsbezogenen Ressourcensätze die folgende Syntax, um anzugeben, auf welche Ressourcen die Regeln angewendet werden sollen.

### <a name="dynamic-replacers-single-brackets"></a>Dynamische Ersetzungsfunktionen (einfache geschweifte Klammern)

In Regeln für bereichsbezogene Ressourcensätze werden für **dynamische Ersetzungsfunktionen** einfache geschweifte Klammern verwendet. Geben Sie eine dynamische Ersetzungsfunktion im qualifizierten Namen mit dem Format `{<replacerName:<replacerType>}` an. Wenn eine Übereinstimmung vorliegt, werden dynamische Ersetzungsfunktionen als Gruppierungsbedingung verwendet, die angibt, dass Ressourcen als Ressourcensatz dargestellt werden sollen. Wenn die Ressourcen in einem Ressourcensatz gruppiert sind, enthält der qualifizierte Pfad des Ressourcensatzes `{replacerName}`, um die Ersetzungsfunktion anzugeben.

Wenn beispielsweise die beiden Ressourcen `folder1/file-1.csv` und `folder2/file-2.csv` die Regel `{folder:string}/file-{NUM:int}.csv` erfüllen, ist der Ressourcensatz die einzelne Entität `{folder}/file-{NUM}.csv`.

#### <a name="special-case-dynamic-replacers-when-not-grouping-into-resource-set"></a>Sonderfall: dynamische Ersetzungsfunktionen, wenn keine Gruppierung im Ressourcensatz erfolgt

Wenn für eine Regel für bereichsbezogene Ressourcensätze *Do not group as resource set* (Nicht als Ressourcensatz gruppieren) aktiviert ist, ist der Name der Ersetzungsfunktion ein optionales Feld. `{:<replacerType>}` ist gültige Syntax. Beispielsweise stimmt `file-{:int}.csv` mit `file-1.csv` und `file-2.csv` überein, und anstelle eines Ressourcensatzes werden zwei verschiedene Ressourcen erstellt.

### <a name="static-replacers-double-brackets"></a>Statische Ersetzungsfunktionen (doppelte geschweifte Klammern)

Doppelte geschweifte Klammern werden für **statische Ersetzungsfunktionen**  im qualifizierten Namen einer Regel für bereichsbezogene Ressourcensätze verwendet. Geben Sie eine statische Ersetzungsfunktion im qualifizierten Namen mit dem Format `{{<replacerName>:<replacerType>}}` an. Wenn die Regel erfüllt wird, werden durch jeden Satz eindeutiger statischer Ersetzungsfunktionswerte unterschiedliche Ressourcensatzgruppierungen erstellt.

Wenn z. B. die beiden Ressourcen `folder1/file-1.csv` und `folder2/file-2.csv` die Regel `{{folder:string}}/file-{NUM:int}.csv` erfüllen, werden die beiden Ressourcensätze `folder1/file-{NUM}.csv` und `folder2/file-{NUM}.csv` erstellt.

Mit statischen Ersetzungsfunktionen kann der Anzeigename einer Ressource angegeben werden, die eine Regel für bereichsbezogene Ressourcensätze erfüllt. Wenn der Anzeigename einer Regel `{{<replacerName>}}` enthält, wird im Ressourcennamen der übereinstimmende Wert verwendet.

### <a name="available-replacement-types"></a>Verfügbare Ersetzungstypen

Im Folgenden finden Sie die Typen, die in statischen und dynamischen Ersetzungsfunktionen verwendet werden können:

| type | Struktur |
| ---- | --------- |
| Zeichenfolge | Ein oder mehrere Unicode-Zeichen, die Trennzeichen, z. B. Leerzeichen, enthalten. |
| INT | Ein oder mehrere ASCII-Zeichen von 0 bis 9. 0 kann als Präfix verwendet werden (Beispiel: 0001). |
| guid | Eine Zeichenfolgendarstellung einer UUID im Format 32-Bit oder im Format 8-4-4-4-12 gemäß der Definition in [RFC 4122](https://tools.ietf.org/html/rfc4122). |
| date | 6 oder 8 ASCII-Zeichen von 0 bis 9 mit optionalen Trennzeichen: yyyymmdd, yyyy-mm-dd, yymmdd, yy-mm-dd, gemäß der Definition in [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| time | 4 oder 6 ASCII-Zeichen von 0 bis 9 mit optionalen Trennzeichen: HHmm, HH:mm, HHmmss, HH:mm:ss, gemäß der Definition in [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| timestamp | 12 oder 14 ASCII-Zeichen von 0 bis 9 mit optionalen Trennzeichen: yyyy-mm-ddTHH:mm, yyyymmddhhmm, yyyy-mm-ddTHH:mm:ss, yyyymmddHHmmss, gemäß der Definition in [RFC 3339](https://tools.ietf.org/html/rfc3339). |
| boolean | Kann „true“ oder „false“ enthalten, keine Beachtung von Groß-/Kleinschreibung. |
| number | 0 oder mehr ASCII-Zeichen von 0 bis 9 mit optional vorangestellter 0 (z. B. 0001) und gefolgt von einem optionalen Punkt „.“ sowie 1 oder mehr ASCII-Zeichen von 0 bis 9 mit optional nachgestellter 0 (z. B. .100). |
| hex | Ein oder mehrere ASCII-Zeichen aus dem Satz 0–1 und A–F. Dem Wert kann 0 vorangestellt werden. |
| locale | Eine Zeichenfolge, die der in [RFC 5646](https://tools.ietf.org/html/rfc5646) angegebenen Syntax entspricht. |

## <a name="order-of-scoped-resource-set-rules-getting-applied"></a>Die Reihenfolge, in der Regeln für bereichsbezogene Ressourcensätze angewendet werden

Im Folgenden wird die Reihenfolge der Vorgänge zum Anwenden von Regeln für bereichsbezogene Ressourcensätze angegeben:

1. Wenn eine Ressource zwei Regeln erfüllt, haben die spezielleren Bereiche Vorrang. Beispielsweise werden Regeln im Bereich `container/folder` vor Regeln im Bereich `container` angewendet.

1. Die Reihenfolge der Regeln in einem bestimmten Bereich. Diese kann auf der Benutzeroberfläche bearbeitet werden.

1. Wenn eine Ressource keine angegebene Regel erfüllt, wird die standardmäßige Ressourcensatzheuristik angewendet.

## <a name="examples"></a>Beispiele

### <a name="example-1"></a>Beispiel 1

SAP-Datenextraktion in vollständige Ladevorgänge und Deltaladevorgänge

#### <a name="inputs"></a>Eingaben

Dateien:

- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/13/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/13/saptable_customer_20200101_20200102_02.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/delta/2020/01/15/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/17/saptable_customer_20200101_20200102_01.txt`
- `https://myazureblob.blob.core.windows.net/bar/customer/full/2020/01/17/saptable_customer_20200101_20200102_02.txt`

#### <a name="scoped-resource-set-rule"></a>Regel für bereichsbezogene Ressourcensätze

**Bereich:** `https://myazureblob.blob.core.windows.net/bar/`

**Anzeigename:** 'External Customer'

**Qualifizierter Name:** `customer/{extract:string}/{year:int}/{month:int}/{day:int}/saptable_customer_{date_from:date}_{date_to:time}_{sequence:int}.txt`

**Ressourcensatz:** true

#### <a name="output"></a>Ausgabe

Eine Ressourcengruppenressource

**Anzeigename:** External Customer

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/customer/{extract}/{year}/{month}/{day}/saptable_customer_{date_from}_{date_to}_{sequence}.txt`

### <a name="example-2"></a>Beispiel 2

IoT-Daten im Avro-Format

#### <a name="inputs"></a>Eingaben

Dateien:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="scoped-resource-set-rules"></a>Regeln für bereichsbezogene Ressourcensätze

**Bereich:** `https://myazureblob.blob.core.windows.net/bar/`

Regel 1

**Anzeigename:** 'machine-89'

**Qualifizierter Name:** `raw/machinename-89/{date:date}/{time:time}-{id:int}.avro`

**Ressourcensatz:** true

Regel 2

**Anzeigename:** 'machine-90'

**Qualifizierter Name:** `raw/machinename-90/{date:date}/{time:time}-{id:int}.avro`

#### <a name="resource-set-true"></a>*Ressourcensatz:* true

#### <a name="outputs"></a>Ausgaben

2 Ressourcensätze

Ressourcensatz 1

**Anzeigename:** machine-89

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/{date}/{time}-{id}.avro`

Ressourcensatz 2

**Anzeigename:** machine-90

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/{date}/{time}-{id}.avro`

### <a name="example-3"></a>Beispiel 3

IoT-Daten im Avro-Format

#### <a name="inputs"></a>Eingaben

Dateien:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.netbar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="scoped-resource-set-rule"></a>Regel für bereichsbezogene Ressourcensätze

**Bereich:** `https://myazureblob.blob.core.windows.net/bar/`

**Anzeigename:** 'Machine-{{machineid}}'

**Qualifizierter Name:** `raw/machinename-{{machineid:int}}/{date:date}/{time:time}-{id:int}.avro`

**Ressourcensatz:** true

#### <a name="outputs"></a>Ausgaben

Ressourcensatz 1

**Anzeigename:** machine-89

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/{date}/{time}-{id}.avro`

Ressourcensatz 2

**Anzeigename:** machine-90

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/{date}/{time}-{id}.avro`

### <a name="example-4"></a>Beispiel 4

Nicht in Ressourcensätzen gruppieren

#### <a name="inputs"></a>Eingaben

Dateien:

- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`
- `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

#### <a name="scoped-resource-set-rule"></a>Regel für bereichsbezogene Ressourcensätze

**Bereich:** `https://myazureblob.blob.core.windows.net/bar/`

**Anzeigename:** `Machine-{{machineid}}`

**Qualifizierter Name:** `raw/machinename-{{machineid:int}}/{{:date}}/{{:time}}-{{:int}}.avro`

**Ressourcensatz:** false

#### <a name="outputs"></a>Ausgaben

4 einzelne Ressourcen

Ressource 1

**Anzeigename:** machine-89

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-001.avro`

Ressource 2

**Anzeigename:** machine-89

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/01-01-2020/22:33:22-002.avro`

Ressource 3

**Anzeigename:** machine-89

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-89/02-01-2020/22:33:22-001.avro`

Ressource 4

**Anzeigename:** machine-90

**Qualifizierter Name:** `https://myazureblob.blob.core.windows.net/bar/raw/machinename-90/01-01-2020/22:33:22-001.avro`

## <a name="next-steps"></a>Nächste Schritte

[Registrieren und überprüfen Sie zunächst ein Azure Data Lake Gen2-Speicherkonto](register-scan-adls-gen2.md).