---
title: Unterstützte FHIR-Features in Azure – Azure API for FHIR
description: In diesem Artikel wird erläutert, welche Features der FHIR-Spezifikation in Azure API for FHIR implementiert sind.
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 4/15/2021
ms.author: cavoeg
ms.openlocfilehash: 56e3ba46ffb43aec907d729a2e74cdf6f7a62c32
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2021
ms.locfileid: "107530643"
---
# <a name="features"></a>Features

Azure API for FHIR bietet eine vollständig verwaltete Bereitstellung des Microsoft FHIR-Servers für Azure. Der Server ist eine Implementierung des [FHIR](https://hl7.org/fhir)-Standards. In diesem Dokument sind die wichtigsten Features des FHIR-Servers aufgeführt.

## <a name="fhir-version"></a>FHIR-Version

Letzte unterstützte Version: `4.0.1`

Derzeit ebenfalls unterstützte vorherige Versionen: `3.0.2`

## <a name="rest-api"></a>REST-API

| API                            | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment                                             |
|--------------------------------|-----------|-----------|-----------|-----------------------------------------------------|
| Lesen                           | Ja       | Ja       | Ja       |                                                     |
| vread                          | Ja       | Ja       | Ja       |                                                     |
| aktualisieren                         | Ja       | Ja       | Ja       |                                                     |
| Update mit optimistischer Sperre | Ja       | Ja       | Ja       |                                                     |
| update (bedingt)           | Ja       | Ja       | Ja       |                                                     |
| patch                          | Nein        | Nein        | Nein        |                                                     |
| delete                         | Ja       | Ja       | Ja       |  Siehe Hinweis unten.                                   |
| delete (bedingt)           | Nein        | Nein        | Nein        |                                                     |
| history                        | Ja       | Ja       | Ja       |                                                     |
| create                         | Ja       | Ja       | Ja       | Unterstützung für POST/PUT                               |
| create (bedingt)           | Ja       | Ja       | Ja       | Issue [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Teilweise   | Teilweise   | Teilweise   | Siehe Abschnitt "Suche" weiter unten.                           |
| Verkettete Suche                 | Teilweise       | Ja       | Teilweise   | Siehe Hinweis 2 weiter unten.                                   |
| Umgekehrte verkettete Suche         | Teilweise       | Ja       | Teilweise   | Siehe Hinweis 2 weiter unten.                                   |
| capabilities                   | Ja       | Ja       | Ja       |                                                     |
| Batch                          | Ja       | Ja       | Ja       |                                                     |
| transaction                    | Nein        | Ja       | Nein        |                                                     |
| paging                         | Teilweise   | Teilweise   | Teilweise   | `self` und `next` werden unterstützt                     |
| intermediaries                 | Nein        | Nein        | Nein        |                                                     |

> [!Note]
> Der von der FHIR-Spezifikation definierte Löschvorgang erfordert, dass nach dem Löschen nachfolgende nicht versionsspezifische Lesevorgänge einer Ressource den HTTP-Statuscode 410 zurückgeben und die Ressource beim Suchen nicht mehr gefunden wird. Die Azure-API für FHIR ermöglicht Ihnen außerdem das vollständige Löschen der Ressource (einschließlich des gesamten Verlaufs). Zum vollständigen Löschen der Ressource können Sie eine Einstellung des Parameters `hardDelete` auf TRUE (`DELETE {server}/{resource}/{id}?hardDelete=true`) übergeben. Wenn Sie diesen Parameter nicht übergeben oder für `hardDelete` FALSE festlegen, sind die früheren Versionen der Ressource weiterhin verfügbar.


 **Hinweis 2**
* Fügt MVP-Unterstützung für verkettete und umgekehrt verkettete FHIR-Suche in CosmosDB hinzu. 

  In der Azure API for FHIR und dem von Cosmos unterstützten Open-Source-FHIR-Server ist die verkettete Suche und die umgekehrte verkettete Suche eine MVP-Implementierung. Um eine verkettete Suche auf Cosmos DB zu ermöglichen, durchsucht die Implementierung den Suchausdruck und gibt Unterabfragen aus, um die übereinstimmenden Ressourcen zu beheben. Dies erfolgt für jede Ebene des Ausdrucks. Wenn eine Abfrage mehr als 100 Ergebnisse zurückgibt, wird ein Fehler ausgelöst. Standardmäßig befindet sich die verkettete Suche hinter einem Featureflag. Verwenden Sie den Header , um die verkettete Cosmos DB zu `x-ms-enable-chained-search: true` verwenden. Weitere Informationen finden Sie unter [PR 1695](https://github.com/microsoft/fhir-server/pull/1695).

## <a name="search"></a>Suchen,

Es werden alle Suchparametertypen unterstützt. 

| Suchparametertyp | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-----------------------|-----------|-----------|-----------|---------|
| Number                | Ja       | Ja       | Ja       |         |
| Date/DateTime         | Ja       | Ja       | Ja       |         |
| String                | Ja       | Ja       | Ja       |         |
| Token                 | Ja       | Ja       | Ja       |         |
| Verweis             | Ja       | Ja       | Ja       |         |
| Composite             | Ja       | Ja       | Ja       |         |
| Menge              | Ja       | Ja       | Ja       |         |
| URI                   | Ja       | Ja       | Ja       |         |
| Sonderfunktionen               | Nein        | Nein        | Nein        |         |


| Modifizierer             | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-----------------------|-----------|-----------|-----------|---------|
|`:missing`             | Ja       | Ja       | Ja       |         |
|`:exact`               | Ja       | Ja       | Ja       |         |
|`:contains`            | Ja       | Ja       | Ja       |         |
|`:text`                | Ja       | Ja       | Ja       |         |
|`:[type]` (Referenz)  | Ja       | Ja       | Ja       |         |
|`:not`                 | Ja       | Ja       | Ja       |         |
|`:below` (URI)         | Ja       | Ja       | Ja       |         |
|`:above` (URI)         | Nein        | Nein        | Nein        | Problem [#158](https://github.com/Microsoft/fhir-server/issues/158) |
|`:in` (Token)          | Nein        | Nein        | Nein        |         |
|`:below` (Token)       | Nein        | Nein        | Nein        |         |
|`:above` (Token)       | Nein        | Nein        | Nein        |         |
|`:not-in` (Token)      | Nein        | Nein        | Nein        |         |

| Allgemeiner Suchparameter | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-------------------------| ----------| ----------| ----------|---------|
| `_id`                   | Ja       | Ja       | Ja       |         |
| `_lastUpdated`          | Ja       | Ja       | Ja       |         |
| `_tag`                  | Ja       | Ja       | Ja       |         |
| `_list`                 | Ja       | Ja       | Ja       |         |
| `_type`                 | Ja       | Ja       | Ja       | Problem [Nr. 1562](https://github.com/microsoft/fhir-server/issues/1562)        |
| `_security`             | Ja       | Ja       | Ja       |         |
| `_profile`              | Teilweise   | Teilweise   | Teilweise   | Unterstützt in STU3. Wenn Sie Ihre Datenbank **nach** dem 20. Februar 2021 erstellt haben, haben Sie auch Unterstützung in R4. Wir arbeiten daran, „_profile“ für Datenbanken zu aktivieren, die vor dem 20. Februar 2021 erstellt wurden. |
| `_text`                 | Nein        | Nein        | Nein        |         |
| `_content`              | Nein        | Nein        | Nein        |         |
| `_has`                  | Nein        | Nein        | Nein        |         |
| `_query`                | Nein        | Nein        | Nein        |         |
| `_filter`               | Nein        | Nein        | Nein        |         |

| Suchergebnisparameter | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|-------------------------|-----------|-----------|-----------|---------|
| `_elements`             | Ja       | Ja       | Ja       | Problem [Nr. 1256](https://github.com/microsoft/fhir-server/issues/1256)        |
| `_count`                | Ja       | Ja       | Ja       | `_count` ist auf 1.000 Zeichen beschränkt. Wenn hierfür ein höherer Wert als 1.000 festgelegt ist, wird nur 1.000 zurückgegeben, und im Paket wird eine Warnung zurückgegeben. |
| `_include`              | Ja       | Ja       | Ja       |Enthaltene Elemente sind auf 100 beschränkt. Einschließen in PaaS und OSS in Cosmos DB umfasst keine Unterstützung von :iterate.|
| `_revinclude`           | Ja       | Ja       | Ja       | Enthaltene Elemente sind auf 100 beschränkt. Einschließen in PaaS und OSS in Cosmos DB umfasst [keine Unterstützung von :iterate](https://github.com/microsoft/fhir-server/issues/1313). Problem [Nr. 1319](https://github.com/microsoft/fhir-server/issues/1319)|
| `_summary`              | Teilweise   | Teilweise   | Teilweise   | `_summary=count` wird unterstützt |
| `_total`                | Partial   | Teilweise   | Teilweise   | `_total=none` und `_total=accurate`      |
| `_sort`                 | Teilweise   | Teilweise   | Teilweise   |   `_sort=_lastUpdated` wird unterstützt       |
| `_contained`            | Nein        | Nein        | Nein        |         |
| `containedType`         | Nein        | Nein        | Nein        |         |
| `_score`                | Nein        | Nein        | Nein        |         |

## <a name="extended-operations"></a>Erweiterte Vorgänge

Alle unterstützten Vorgänge zur Erweiterung der RESTful-API.

| Suchparametertyp | Unterstützt: PaaS | Unterstützt: OSS (SQL) | Unterstützt: OSS (Cosmos DB) | Comment |
|------------------------|-----------|-----------|-----------|---------|
| $export (gesamtes System) | Ja       | Ja       | Ja       |         |
| Patient/$export        | Ja       | Ja       | Ja       |         |
| Gruppe/$export          | Ja       | Ja       | Ja       |         |
| $convert-data          | Ja       | Ja       | Ja       |         |


## <a name="persistence"></a>Persistenz

Der Microsoft FHIR-Server verfügt über ein austauschbares Persistenzmodul (siehe [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Derzeit umfasst der Open-Source-Code für FHIR-Server eine Implementierung für [Azure Cosmos DB](../../cosmos-db/index-overview.md) und [SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

Cosmos DB ist eine global verteilte Datenbank, die mehrere Modelle (SQL-API, MongoDB-API, usw.) unterstützt. Es unterstützt verschiedene [Konsistenzebenen](../../cosmos-db/consistency-levels.md). Die standardmäßige Bereitstellungsvorlage konfiguriert den FHIR-Server mit `Strong`-Konsistenz, aber die Konsistenzrichtlinie kann auf Anforderungsbasis unter Verwendung des Anforderungsheaders `x-ms-consistency-level` geändert (allgemein gelockert) werden.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Der FHIR-Server verwendet [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Zugriffssteuerung. Insbesondere wird die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) erzwungen, wenn der Konfigurationsparameter `FhirServer:Security:Enabled` auf `true` festgelegt ist, und bei allen Anforderungen (außer `/metadata`) an den FHIR-Server muss der Anforderungsheader `Authorization` auf `Bearer <TOKEN>` festgelegt sein. Das Token muss eine oder mehrere Rollen gemäß der Definition im `roles`-Anspruch enthalten. Eine Anforderung ist zulässig, wenn der Token eine Rolle enthält, die die angegebene Aktion für die angegebene Ressource gestattet.

Derzeit werden die zulässigen Aktionen für eine bestimmte Rolle *global* auf die API angewendet.

## <a name="service-limits"></a>Diensteinschränkungen

* [**Anforderungseinheiten (Request Units, RUs):**](../../cosmos-db/concepts-limits.md) Sie können bis zu 10.000 RUs im Portal für Azure API for FHIR konfigurieren. Sie benötigen mindestens 400 RUs oder 40 RUs/GB, je nach Größe. Wenn Sie mehr als 10.000 RUs benötigen, können Sie über ein Supportticket eine Erhöhung des Werts anfordern. Maximal sind 1.000.000 RUs verfügbar.

* **Gleichzeitige Verbindungen** und **Instanzen**: Standardmäßig stehen Ihnen fünf gleichzeitige Verbindungen in zwei Instanzen im Cluster (und somit insgesamt zehn gleichzeitige Anforderungen) zur Verfügung. Wenn Sie weitere gleichzeitige Anforderungen benötigen, öffnen Sie ein Supportticket mit Details zu Ihren Anforderungen.

* **Paketgröße:** Jedes Paket ist auf 500 Elemente beschränkt.

* **Datengröße:** Daten/Dokumente müssen jeweils etwas kleiner als 2 MB sein.

## <a name="performance-expectations"></a>Erwartung an die Leistung

Die Leistung des Systems hängt von der Anzahl der RUs, den gleichzeitigen Verbindungen und der Art der ausgeführten Vorgänge ab (PUT, POST usw.). Im Folgenden finden Sie einige allgemeine Bereiche, die auf der Grundlage der konfigurierten RUs zu erwarten sind. Im Allgemeinen wird die Leistung linear mit einer Erhöhung der RUs skaliert:

| Anzahl der RUs | Ressourcen/s |    Max. Speicher (GB)*    |
|----------|---------------|--------|                 
| 400      | 5-10          |     10   |
| 1\.000    | 100–150       |      25  |
| 10.000   | 225–400       |      250  |
| 100.000  | 2\.500–4.000   |      2\.500  |

Hinweis: Pro Cosmos DB Anforderung ist ein Mindestdurchsatz von 40 RU/s pro GB Speicher erforderlich. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den unterstützten FHIR-Features in Azure API for FHIR erhalten. Stellen Sie als Nächstes die Azure API for FHIR bereit.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Azure API for FHIR](fhir-paas-portal-quickstart.md)
