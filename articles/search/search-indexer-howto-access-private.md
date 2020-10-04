---
title: Über private Endpunkte auf sichere Ressourcen zugreifende Indexer
titleSuffix: Azure Cognitive Search
description: Anleitung zum Einrichten privater Endpunkte für Indexer zur Kommunikation mit sicheren Ressourcen
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: e9944e95dd452cd00e63280ad8002141591f521e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90971432"
---
# <a name="accessing-secure-resources-via-private-endpoints"></a>Zugreifen auf sichere Ressourcen über private Endpunkte

Azure-Ressourcen (z. B. Speicherkonten, die als Datenquellen verwendet werden) können so konfiguriert werden, dass auf sie nur von einer bestimmten Liste virtueller Netzwerke aus zugegriffen werden kann. Sie können auch so konfiguriert werden, dass sie jeglichen Zugriff auf „öffentliche Netzwerke“ verbieten.
Kunden können Azure Cognitive Search auffordern, eine (ausgehende) [private Endpunktverbindung](https://docs.microsoft.com/azure/private-link/private-endpoint-overview) zu erstellen, um über Indexer sicher auf Daten aus solchen Datenquellen zuzugreifen.

## <a name="shared-private-link-resources-management-apis"></a>Freigegebene Verwaltungs-APIs für Private Link-Ressourcen

Private Endpunkte, die von Azure Cognitive Search auf Kundenwunsch erstellt werden, um auf „sichere“ Ressourcen zuzugreifen, werden als *freigegebene Private Link-Ressourcen* bezeichnet. Der Kunde gibt den Zugriff auf eine Ressource (z. B. ein Speicherkonto) frei, für die das Onboarding in den [Azure Private Link-Dienst](https://azure.microsoft.com/services/private-link/) durchgeführt wurde.

Azure Cognitive Search bietet über die Search-Verwaltungs-API die Möglichkeit, [freigegebene Private Link-Ressourcen zu erstellen oder zu aktualisieren](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/createorupdate). Sie werden diese API zusammen mit anderen Verwaltungs-APIs von *freigegebenen Private Link-Ressourcen* verwenden, um den Zugriff auf eine sichere Ressource von einem Azure Cognitive Search-Indexer zu konfigurieren.

Private Endpunktverbindungen mit einigen Ressourcen können nur über die Vorschauversion der Search-Verwaltungs-API (`2020-08-01-Preview`) erstellt werden, die in der nachstehenden Tabelle mit dem Tag „preview“ gekennzeichnet ist. Ressourcen ohne das Tag „preview“ können sowohl über die Vorschau-API als auch über die GA-API erstellt werden (`2020-08-01`).

Im Folgenden finden Sie eine Liste der Azure-Ressourcen, zu denen ausgehende private Endpunkte von Azure Cognitive Search erstellt werden können. Die `groupId`, die in der folgenden Tabelle aufgeführt ist, muss in der API genau verwendet werden (Groß-/Kleinschreibung beachten), um eine freigegebene Private Link-Ressource zu erstellen.

| Azure-Ressource | Gruppen-ID |
| --- | --- |
| Azure Storage: Blob (oder) ADLS Gen2 | `blob`|
| Azure Storage: Tabellen | `table`|
| Azure Cosmos DB: SQL-API | `Sql`|
| Azure SQL-Datenbank | `sqlServer`|
| Azure Database for MySQL (Vorschau) | `mysqlServer`|
| Azure-Schlüsseltresor | `vault` |
| Azure Functions (Vorschau) | `sites` |

Die Liste der Azure-Ressourcen, für die ausgehende private Endpunktverbindungen unterstützt werden, kann auch über die [Liste unterstützter APIs](https://docs.microsoft.com/rest/api/searchmanagement/privatelinkresources/listsupported) abgefragt werden.

Für die Zwecke dieses Leitfadens wird eine Mischung aus [ARMClient](https://github.com/projectkudu/ARMClient) und [Postman](https://www.postman.com/) verwendet, um die REST-API-Aufrufe zu veranschaulichen.

> [!NOTE]
> Im gesamten vorliegenden Leitfaden wird angenommen, dass der Name des Suchdiensts __contoso-search__ lautet, der in der Ressourcengruppe __contoso__ eines Abonnements mit der Abonnement-ID __00000000-0000-0000-0000-000000000000__ vorhanden ist. Die Ressourcen-ID dieses Diensts lautet `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search`.

Im restlichen Leitfaden wird gezeigt, wie der Dienst __contoso-search__ konfiguriert werden kann, sodass seine Indexer auf Daten aus dem sicheren Speicherkonto `/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage` zugreifen können.

## <a name="securing-your-storage-account"></a>Schützen Ihres Speicherkontos

Konfigurieren Sie das Speicherkonto so, dass der [Zugriff nur von bestimmten Subnetzen aus zulässig ist](https://docs.microsoft.com/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network). Wenn Sie diese Option über das Azure-Portal aktivieren und den Satz leer lassen, bedeutet dies, dass kein Datenverkehr aus einem virtuellen Netzwerk zugelassen wird.

   ![Zugriff über virtuelles Netzwerk](media\search-indexer-howto-secure-access\storage-firewall-noaccess.png "Zugriff über virtuelles Netzwerk")

> [!NOTE]
> Der [vertrauenswürdige Microsoft-Dienstansatz](https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) kann mithilfe virtueller Netzwerk- oder IP-Einschränkungen eines solchen Speicherkontos umgangen werden und dem Suchdienst den Zugriff auf Daten im Speicherkonto ermöglichen, wie in der [Anleitung](search-indexer-howto-access-trusted-service-exception.md) beschrieben. Bei Verwendung dieses Ansatzes erfolgt die Kommunikation zwischen Azure Cognitive Search und dem Speicherkonto jedoch über die öffentliche IP-Adresse des Speicherkontos, über das sichere Microsoft-Backbone-Netzwerk.

## <a name="step-1-create-a-shared-private-link-resource-to-the-storage-account"></a>Schritt 1: Erstellen einer freigegebenen Private Link-Ressource zum Speicherkonto

Führen Sie den folgenden API-Aufruf aus, um bei Azure Cognitive Search anzufordern, dass eine ausgehende private Endpunktverbindung mit dem Speicherkonto erstellt wird.

`armclient PUT https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01 create-pe.json`

Der Inhalt der `create-pe.json`-Datei (die den Anforderungstext für die API darstellt) lautet wie folgt:

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve"
      }
}
```

Bei Erfolg wird eine `202 Accepted`-Antwort zurückgegeben. Der Prozess der Erstellung eines ausgehenden privaten Endpunkts ist ein zeitintensiver (asynchroner) Vorgang. Er umfasst die Bereitstellung der folgenden Ressourcen:

1. Ein privater Endpunkt, dem eine private IP-Adresse in einem `"Pending"`-Zustand zugeordnet wurde. Die private IP-Adresse wird aus dem Adressraum abgerufen, der dem virtuellen Netzwerk der suchdienstspezifischen privaten Indexer-Ausführungsumgebung zugeordnet ist. Nach der Genehmigung des privaten Endpunkts stammt jede Kommunikation von Azure Cognitive Search zum Speicherkonto von der privaten IP-Adresse und einem sicheren Private Link-Kanal.
2. Eine private DNS-Zone für den Ressourcentyp, basierend auf der `groupId`. Dadurch wird sichergestellt, dass bei jedem DNS-Lookup zur privaten Ressource die IP-Adresse verwendet wird, die dem privaten Endpunkt zugeordnet ist.

Stellen Sie sicher, dass Sie die richtige `groupId` für den Ressourcentyp angeben, für den Sie den privaten Endpunkt erstellen. Jeder Konflikt führt zu einer nicht erfolgreichen Antwortnachricht.

Wie alle asynchronen Azure-Vorgänge gibt der `PUT`-Aufruf einen `Azure-AsyncOperation`-Headerwert zurück, der wie folgt aussieht:

`"Azure-AsyncOperation": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

Dieser URI kann in regelmäßigen Abständen abgerufen werden, um den Status des Vorgangs zu erhalten. Wir empfehlen zu warten, bis der Vorgangsstatus der freigegebenen Private Link-Ressource einen abschließenden Zustand erreicht hat (d. h. `succeeded`), bevor Sie fortfahren.

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe/operationStatuses/08586060559526078782?api-version=2020-08-01"`

```json
{
    "status": "running" | "succeeded" | "failed"
}
```

## <a name="step-2a-approve-the-private-endpoint-connection-for-the-storage-account"></a>Schritt 2a: Genehmigen der privaten Endpunktverbindung für das Speicherkonto

> [!NOTE]
> In diesem Abschnitt wird mithilfe des Azure-Portals der Genehmigungsflow für einen privaten Endpunkt zur Speicherung durchlaufen. Stattdessen kann auch die [REST-API](https://docs.microsoft.com/rest/api/storagerp/privateendpointconnections) verwendet werden, die über einen Speicherressourcenanbieter verfügbar ist.
>
> Andere Anbieter wie CosmosDB, Azure SQL Server usw. bieten ebenfalls ähnliche APIs von Speicherressourcenanbietern zur Verwaltung privater Endpunktverbindungen.

Navigieren Sie zur Registerkarte „**Private Endpunktverbindungen**“ des Speicherkontos im Azure-Portal. Es sollte eine Anforderung für eine private Endpunktverbindung mit der Anforderungsnachricht des vorherigen API-Aufrufs vorliegen (sobald der asynchrone Vorgang __erfolgreich__ ausgeführt wurde).

   ![Genehmigung privater Endpunkte](media\search-indexer-howto-secure-access\storage-privateendpoint-approval.png "Genehmigung privater Endpunkte")

Wählen Sie den privaten Endpunkt aus, der von Azure Cognitive Search erstellt wurde (mithilfe der Spalte „Privater Endpunkt“ können Sie die private Endpunktverbindung anhand des in der vorherigen API angegebenen Namens identifizieren), und wählen Sie „Genehmigen“ mit einer entsprechenden Nachricht aus (die Nachricht ist nicht signifikant). Stellen Sie sicher, dass die private Endpunktverbindung wie folgt angezeigt wird (die Aktualisierung des Status im Portal kann 1-2 Minuten dauern).

![Privater Endpunkt: „Genehmigt“](media\search-indexer-howto-secure-access\storage-privateendpoint-after-approval.png "Privater Endpunkt: „Genehmigt“")

Nachdem die Anforderung für die Verbindung mit dem privaten Endpunkt genehmigt wurde, bedeutet dies, dass der Datenverkehr *in der Lage ist*, über den privaten Endpunkt zu fließen. Nachdem der private Endpunkt genehmigt ist, erstellt Azure Cognitive Search die erforderlichen DNS-Zonenzuordnungen in der dafür erstellten DNS-Zone.

## <a name="step-2b-query-the-status-of-the-shared-private-link-resource"></a>Schritt 2b: Abfragen des Status der freigegebenen Private Link-Ressource

 Um zu bestätigen, dass die freigegebene Private Link-Ressource nach der Genehmigung aktualisiert wurde, rufen Sie ihren Status über die [GET-API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get) ab.

`armclient GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Search/searchServices/contoso-search/sharedPrivateLinkResources/blob-pe?api-version=2020-08-01`

Wenn der `properties.provisioningState` der Ressource `Succeeded` und `properties.status` entsprechend `Approved` ist, bedeutet dies, dass die freigegebene Private Link-Ressource funktionsfähig ist und Indexer so konfiguriert werden können, dass sie über den privaten Endpunkt kommunizieren.

```json
{
      "name": "blob-pe",
      "properties": {
        "privateLinkResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Storage/storageAccounts/contoso-storage",
        "groupId": "blob",
        "requestMessage": "please approve",
        "status": "Approved",
        "resourceRegion": null,
        "provisioningState": "Succeeded"
      }
}

```

## <a name="step-3-configure-indexer-to-run-in-the-private-environment"></a>Schritt 3: Konfigurieren der Indexerausführung in der privaten Umgebung

> [!NOTE]
> Dieser Schritt kann sogar vor der Genehmigung der privaten Endpunktverbindung durchgeführt werden. Bis zur Genehmigung der privaten Endpunktverbindung endet jeder Indexer, der versucht, mit einer sicheren Ressource (z. B. dem Speicherkonto) zu kommunizieren, mit einem vorübergehenden Fehlerstatus. Neue Indexer werden nicht erstellt. Sobald die private Endpunktverbindung genehmigt ist, können Indexer auf das private Speicherkonto zugreifen.

1. [Erstellen Sie eine Datenquelle](https://docs.microsoft.com/rest/api/searchservice/create-data-source), die auf das Speicherkonto und einen entsprechenden Container innerhalb des Speicherkontos verweist. Die folgende Abbildung zeigt diese Anforderung, die über Postman ausgeführt wurde.
![Erstellen der Datenquelle](media\search-indexer-howto-secure-access\create-ds.png "Datenquellenerstellung")

2. Auf ähnliche Weise [erstellen Sie einen Index](https://docs.microsoft.com/rest/api/searchservice/create-index) und [erstellen optional ein Skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) mithilfe der REST-API.

3. [Erstellen Sie einen Indexer](https://docs.microsoft.com/rest/api/searchservice/create-indexer), der auf die Datenquelle, den Index und das oben erstellte Skillset verweist. Erzwingen Sie außerdem, dass der Indexer in der privaten Ausführungsumgebung ausgeführt wird, indem Sie die Indexerkonfigurationseigenschaft `executionEnvironment` auf `"Private"` festlegen.
![Create Indexer](media\search-indexer-howto-secure-access\create-idr.png "Indexererstellung")

Der Indexer sollte erfolgreich erstellt werden und entsprechende Fortschritte machen: Inhalte aus dem Speicherkonto über die private Endpunktverbindung indizieren. Der Status des Indexers kann über die [Indexerstatus-API](https://docs.microsoft.com/rest/api/searchservice/get-indexer-status) überwacht werden.

> [!NOTE]
> Wenn Sie bereits über vorhandene Indexer verfügen, können Sie diese einfach über die [PUT-API](https://docs.microsoft.com/rest/api/searchservice/create-indexer) aktualisieren, um die `executionEnvironment` auf `"Private"` festzulegen.

## <a name="troubleshooting-issues"></a>Fragen zur Problembehandlung

- Wenn beim Erstellen eines Indexers die Erstellung mit einer Fehlermeldung wie „Die Anmeldeinformationen für die Datenquelle sind ungültig“ fehlschlägt, bedeutet dies, dass entweder die private Endpunktverbindung nicht *genehmigt* wurde oder nicht funktionsfähig ist.
Rufen Sie den Status der freigegebenen Private Link-Ressource mithilfe der [GET-API](https://docs.microsoft.com/rest/api/searchmanagement/sharedprivatelinkresources/get) ab. Wenn sie *genehmigt* worden ist, prüfen Sie den `properties.provisioningState` der Ressource. Wenn es `Incomplete` ist, bedeutet dies, dass einige der zugrunde liegenden Abhängigkeiten für die Ressource nicht bereitgestellt wurden. Stellen Sie die `PUT`-Anforderung erneut aus, die freigegebene Private Link-Ressource, die das Problem beheben sollte, „neu zu erstellen“. Möglicherweise ist eine erneute Genehmigung erforderlich. Überprüfen Sie den Status der Ressource erneut, um sich zu vergewissern.
- Wenn der Indexer erstellt wird, ohne seine `executionEnvironment` festzulegen, könnte die Erstellung des Indexers erfolgreich sein, aber sein Ausführungsverlauf wird zeigen, dass Indexerausführungen nicht erfolgreich sind. Sie sollten [den Indexer](https://docs.microsoft.com/rest/api/searchservice/update-indexer) aktualisieren, um die Ausführungsumgebung anzugeben.
- Wenn der Indexer ohne Festlegen der `executionEnvironment` erstellt und erfolgreich ausgeführt wird, bedeutet dies, dass Azure Cognitive Search entschieden hat, dass seine Ausführungsumgebung die suchdienstspezifische „private“ Umgebung ist. Dies kann sich jedoch aufgrund einer Vielzahl von Faktoren (vom Indexer verbrauchte Ressourcen, Belastung des Suchdiensts usw.) ändern und zu einem späteren Zeitpunkt zu einem Fehler führen, sodass wir Ihnen dringend empfehlen, die `executionEnvironment` auf `"Private"` festzulegen, um sicherzustellen, dass in Zukunft kein Fehler auftritt.
- [Kontingente und Grenzwerte](search-limits-quotas-capacity.md) bestimmen, wie viele freigegebene Private Link-Ressourcen erstellt werden können und hängen von der SKU des Suchdiensts ab.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu privaten Endpunkten:

- [Was sind private Endpunkte?](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)
- [Für private Endpunkte erforderliche DNS-Konfigurationen](https://docs.microsoft.com/azure/private-link/private-endpoint-dns)