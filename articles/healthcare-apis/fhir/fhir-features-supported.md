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
ms.openlocfilehash: e012bc2bc3c9ec1ab9351ed937e2c5049eef20d5
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2021
ms.locfileid: "108316009"
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
| delete                         | Ja       | Ja       | Ja       |  Siehe Hinweis weiter unten.                                   |
| delete (bedingt)           | Nein        | Nein        | Nein        |                                                     |
| history                        | Ja       | Ja       | Ja       |                                                     |
| create                         | Ja       | Ja       | Ja       | Unterstützung für POST/PUT                               |
| create (bedingt)           | Ja       | Ja       | Ja       | Issue [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Teilweise   | Teilweise   | Teilweise   | Weitere Informationen finden Sie [unter Übersicht über die FHIR-Suche.](overview-of-search.md)                           |
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

  Im Azure API for FHIR und auf dem Open-Source-FHIR-Server, der von Cosmos unterstützt wird, ist die verkettete Suche und umgekehrte verkettete Suche eine MVP-Implementierung. Um die verkettete Suche auf Cosmos DB durchzuführen, führt die Implementierung den Suchausdruck durch und gibt Unterabfragen aus, um die übereinstimmenden Ressourcen zu beheben. Dies erfolgt für jede Ebene des Ausdrucks. Wenn eine Abfrage mehr als 100 Ergebnisse zurückgibt, wird ein Fehler ausgelöst. Standardmäßig befindet sich die verkettete Suche hinter einem Featureflag. Verwenden Sie den Header , um die verkettete Suche für Cosmos DB zu `x-ms-enable-chained-search: true` verwenden. Weitere Informationen finden Sie unter [PR 1695](https://github.com/microsoft/fhir-server/pull/1695).

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

* [**Anforderungseinheiten (Request Units, RUs):**](../../cosmos-db/concepts-limits.md) Sie können bis zu 10.000 RUs im Portal für Azure API for FHIR konfigurieren. Sie benötigen mindestens 400 RUs oder 40 RUs/GB, je nachdem, welcher Wert größer ist. Wenn Sie mehr als 10.000 RUs benötigen, können Sie über ein Supportticket eine Erhöhung des Werts anfordern. Maximal sind 1.000.000 RUs verfügbar.

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
