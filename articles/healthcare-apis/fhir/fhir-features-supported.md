---
title: Unterstützte FHIR-Features in Azure – Azure API for FHIR
description: In diesem Artikel wird erläutert, welche Features der FHIR-Spezifikation in Azure API for FHIR implementiert sind.
services: healthcare-apis
author: caitlinv39
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 6/16/2021
ms.author: cavoeg
ms.openlocfilehash: 1a417d452a7db67cbcf392bb44233f9117f3f8e6
ms.sourcegitcommit: 6a3096e92c5ae2540f2b3fe040bd18b70aa257ae
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2021
ms.locfileid: "112321313"
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
| delete (bedingt)           | Ja       | Ja        | Ja        |                                                     |
| history                        | Ja       | Ja       | Ja       |                                                     |
| create                         | Ja       | Ja       | Ja       | Unterstützung für POST/PUT                               |
| create (bedingt)           | Ja       | Ja       | Ja       | Issue [#1382](https://github.com/microsoft/fhir-server/issues/1382) |
| search                         | Partial   | Partial   | Partial   | Weitere Informationen [finden Sie unter Übersicht über die FHIR-Suche.](overview-of-search.md)                           |
| Verkettete Suche                 | Partial       | Ja       | Teilweise   | Siehe Hinweis 2 weiter unten.                                   |
| Umgekehrte verkettete Suche         | Partial       | Ja       | Teilweise   | Siehe Hinweis 2 weiter unten.                                   |
| capabilities                   | Ja       | Ja       | Ja       |                                                     |
| Batch                          | Ja       | Ja       | Ja       |                                                     |
| transaction                    | Nein        | Ja       | Nein        |                                                     |
| paging                         | Teilweise   | Partial   | Teilweise   | `self` und `next` werden unterstützt                     |
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
| $validate              | Ja       | Ja       | Ja       |         |
| $member-Match          | Ja       | Ja       | Ja       |         |
| $patient alles    | Ja       | Nein        | Ja       |         |

## <a name="persistence"></a>Persistenz

Der Microsoft FHIR-Server verfügt über ein austauschbares Persistenzmodul (siehe [`Microsoft.Health.Fhir.Core.Features.Persistence`](https://github.com/Microsoft/fhir-server/tree/master/src/Microsoft.Health.Fhir.Core/Features/Persistence)).

Derzeit umfasst der Open-Source-Code für FHIR-Server eine Implementierung für [Azure Cosmos DB](../../cosmos-db/index-overview.md) und [SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

Cosmos DB ist eine global verteilte Datenbank, die mehrere Modelle (SQL-API, MongoDB-API, usw.) unterstützt. Es unterstützt verschiedene [Konsistenzebenen](../../cosmos-db/consistency-levels.md). Die standardmäßige Bereitstellungsvorlage konfiguriert den FHIR-Server mit `Strong`-Konsistenz, aber die Konsistenzrichtlinie kann auf Anforderungsbasis unter Verwendung des Anforderungsheaders `x-ms-consistency-level` geändert (allgemein gelockert) werden.

## <a name="role-based-access-control"></a>Rollenbasierte Zugriffssteuerung

Der FHIR-Server verwendet [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Zugriffssteuerung. Insbesondere wird die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) erzwungen, wenn der Konfigurationsparameter `FhirServer:Security:Enabled` auf `true` festgelegt ist, und bei allen Anforderungen (außer `/metadata`) an den FHIR-Server muss der Anforderungsheader `Authorization` auf `Bearer <TOKEN>` festgelegt sein. Das Token muss eine oder mehrere Rollen gemäß der Definition im `roles`-Anspruch enthalten. Eine Anforderung ist zulässig, wenn der Token eine Rolle enthält, die die angegebene Aktion für die angegebene Ressource gestattet.

Derzeit werden die zulässigen Aktionen für eine bestimmte Rolle *global* auf die API angewendet.

## <a name="service-limits"></a>Diensteinschränkungen

* [**Anforderungseinheiten (Request Units, RUs):**](../../cosmos-db/concepts-limits.md) Sie können bis zu 10.000 RUs im Portal für Azure API for FHIR konfigurieren. Sie benötigen mindestens 400 RUs oder 40 RUs/GB, je nachdem, welcher Wert größer ist. Wenn Sie mehr als 10.000 RUs benötigen, können Sie über ein Supportticket eine Erhöhung des Werts anfordern. Maximal sind 1.000.000 RUs verfügbar.

* **Paketgröße:** Jedes Paket ist auf 500 Elemente beschränkt.

* **Datengröße:** Daten/Dokumente müssen jeweils etwas kleiner als 2 MB sein.

* **Abonnementlimit:** Standardmäßig ist jedes Abonnement auf maximal 10 FHIR-Serverinstanzen beschränkt. Wenn Sie mehr Instanzen pro Abonnement benötigen, öffnen Sie ein Supportticket, und geben Sie Details zu Ihren Anforderungen an.

* **Gleichzeitige Verbindungen und Instanzen:** Standardmäßig verfügen Sie über 15 gleichzeitige Verbindungen auf zwei Instanzen im Cluster (für insgesamt 30 gleichzeitige Anforderungen). Wenn Sie weitere gleichzeitige Anforderungen benötigen, öffnen Sie ein Supportticket, und geben Sie Details zu Ihren Anforderungen an.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie Informationen zu den unterstützten FHIR-Features in Azure API for FHIR erhalten. Stellen Sie als Nächstes die Azure API for FHIR bereit.
 
>[!div class="nextstepaction"]
>[Bereitstellen von Azure API for FHIR](fhir-paas-portal-quickstart.md)
