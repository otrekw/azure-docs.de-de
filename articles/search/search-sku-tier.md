---
title: Auswählen eines Tarifs
titleSuffix: Azure Cognitive Search
description: 'Azure Cognitive Search kann in folgenden Dienstebenen bereitgestellt werden: „Free“, „Basic“ und „Standard“, wobei „Standard“ mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar ist.'
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 062bd41b0803cbb08f74fbcbcebb89bbddeb0d45
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559802"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Auswählen eines Tarifs für die kognitive Azure-Suche

Beim [Erstellen eines Suchdiensts](search-create-service-portal.md) wählen Sie einen Tarif, der für die Lebensdauer des Diensts festgelegt ist. Der ausgewählte Tarif bestimmt:

+ Die Anzahl von Indizes und anderen Objekten, die Sie erstellen können (maximale Grenzwerte)
+ Größe und Geschwindigkeit von Partitionen (physischer Speicher)
+ Die abrechenbare Rate, eine festgelegte monatliche Gebühr, aber auch inkrementelle Kosten, wenn Sie Partitionen oder Replikate hinzufügen

Außerdem gelten für einige [Premium-Features](#premium-features) Tarifanforderungen.

## <a name="tier-descriptions"></a>Tarifbeschreibungen

Folgende Tarife sind verfügbar: **Free**, **Basic**, **Standard** und **Speicheroptimiert**. „Standard“ und „Speicheroptimiert“ werden mit verschiedenen Konfigurationen und Kapazitäten bereitgestellt.

Im folgenden Screenshot aus dem Azure-Portal sind die verfügbaren Tarife ohne Preise aufgeführt (die Preise finden Sie im Portal und auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/search/)). 

![Tarife der kognitiven Azure-Suche](media/search-sku-tier/tiers.png "Tarife der kognitiven Azure-Suche")

Mit **Free** wird ein eingeschränkter Suchdienst für kleinere Projekte wie das Ausführen von Tutorials und Codebeispielen erstellt. Intern werden Replikate und Partitionen von mehreren Abonnenten gemeinsam genutzt. Es ist nicht möglich, einen kostenlosen Dienst zu skalieren oder signifikante Workloads auszuführen.

**Basic** und **Standard** sind die am häufigsten verwendeten abrechenbaren Tarife, wobei **Standard** der vorgegebene Tarif ist. Wenn Sie dedizierte Ressourcen nutzen, können Sie größere Projekte bereitstellen, die Leistung optimieren und die Kapazität steigern.

Einige Tarife sind für bestimmte Verwendungszwecke optimiert. **Standard 3 High Density (S3 HD)** ist beispielsweise ein *Hostingmodus* für S3. Die zugrunde liegende Hardware ist für eine große Anzahl kleinerer Indizes optimiert und für Szenarien mit Mehrinstanzenfähigkeit konzipiert. Die Gebühr pro Einheit unterscheidet sich zwischen „S3 HD“ und „S3“ nicht. Die Hardware ist bei „S3 HD“ jedoch für schnelle Dateilesevorgänge bei einer großen Anzahl kleinerer Indizes optimiert.

**Datenspeicheroptimierte** Tarife bieten eine höhere Speicherkapazität zu einem niedrigeren Preis pro TB als die Standard-Tarife. Der größte Nachteil ist die höhere Wartezeit. Diese müssen Sie bei Ihren spezifischen Anwendungsanforderungen berücksichtigen. Weitere Informationen zu den Leistungsaspekten dieses Tarifs finden Sie unter [Überlegungen zur Leistung und Optimierung von Azure Search](search-performance-optimization.md).

Weitere Informationen zu den verschiedenen Tarifen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/search/), im Artikel [Grenzwerte für den Dienst für die kognitive Azure-Suche](search-limits-quotas-capacity.md) und auf der Portalseite beim Bereitstellen eines Diensts.

<a name="premium-features"></a>

## <a name="feature-availability-by-tier"></a>Funktionsverfügbarkeit pro Tarif

Die meisten Funktionen sind in allen Tarifen (einschließlich des Free-Tarifs) verfügbar. In einigen Fällen wirkt sich der von Ihnen ausgewählte Tarif auf die Fähigkeit aus, eine Funktion zu implementieren. In der folgenden Tabelle werden die Funktionseinschränkungen im Zusammenhang mit der Dienstebene (dem Tarif) beschrieben.

| Funktion | Einschränkungen |
|---------|-------------|
| [Indexer](search-indexer-overview.md) | Indexer sind auf S3 HD nicht verfügbar.  |
| [KI-Anreicherung](search-security-manage-encryption-keys.md) | Wird im Free-Tarif ausgeführt, aber nicht empfohlen. |
| [Von Kunden verwaltete Verschlüsselungsschlüssel](search-security-manage-encryption-keys.md) | Im Free-Tarif nicht verfügbar. |
| [IP-Firewallzugriff](service-configure-firewall.md) | Im Free-Tarif nicht verfügbar. |
| [Privater Endpunkt (Integration mit Azure Private Link)](service-create-private-endpoint.md) | Für eingehende Verbindungen bei einem Suchdienst, nicht im Free-Tarif verfügbar. Für ausgehende Verbindungen von Indexern zu anderen Azure-Ressourcen, nicht verfügbar im Tarif „Free“ oder „S3 HD“. Für Indexer, die Skillsets verwenden, nicht verfügbar im Tarif „Free“, „Basic“, „S1“ oder „S3 HD“|

Ressourcenintensive Funktionen funktionieren möglicherweise nicht gut, wenn Sie ihnen nicht genügend Kapazität zuweisen. Beispielsweise umfasst die [KI-Anreicherung](cognitive-search-concept-intro.md) Skills mit langer Laufzeit, die bei einem kostenlosen Dienst zu einem Timeout führen, sofern es sich nicht um ein kleines Dataset handelt.

## <a name="billable-events"></a>Abrechenbare Ereignisse

Für eine auf der kognitiven Azure-Suche basierende Lösung können folgende Kosten anfallen:

+ [Kosten für den Dienst](#service-costs) selbst, der rund um die Uhr mit einer minimalen Konfiguration (eine Partition und ein Replikat) im Basistarif ausgeführt wird. Sie können sich dies als die Fixkosten für die Ausführung des Diensts vorstellen.

+ Hinzufügen von Kapazität (Replikate oder Partitionen), wobei die Kosten in Inkrementen der abrechenbaren Rate steigen

+ Bandbreitengebühren (ausgehende Datenübertragung)

+ Für bestimmte Funktionen oder Features erforderliche Add-On-Dienste:

  + KI-Anreicherung (erfordert [Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/))
  + Wissensspeicher (erfordert [Azure Storage](https://azure.microsoft.com/pricing/details/storage/))
  + Inkrementelle Anreicherung (erfordert [Azure Storage](https://azure.microsoft.com/pricing/details/storage/), gilt für KI-Anreicherung)
  + Kundenseitig verwaltete Schlüssel und doppelte Verschlüsselung (erfordert [Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/))
  + Private Endpunkte für ein Zugriffsmodell ohne Internet (erfordert [Azure Private Link](https://azure.microsoft.com/pricing/details/private-link/))

### <a name="service-costs"></a>Dienstkosten

Im Unterschied zu virtuellen Computern oder anderen Ressourcen, die angehalten werden können, um Gebühren zu vermeiden, ist ein Dienst für die kognitive Azure-Suche immer auf der Hardware verfügbar, die für Ihre ausschließliche Nutzung bestimmt ist. Daher ist die Erstellung eines Diensts ein abrechenbares Ereignis, das mit dem Erstellen des Diensts beginnt und mit dem Löschen des Diensts endet. 

Die Mindestgebühr fällt zum abrechenbaren Preis für die erste Sucheinheit (ein Replikat × eine Partition) an. Dieser Betrag gilt für die Lebensdauer des Diensts, da der Dienst nicht mit weniger als dieser Konfiguration ausgeführt werden kann. Über dieses Minimum hinaus können Sie Replikate und Partitionen unabhängig voneinander hinzufügen. Inkrementelle Kapazitätserweiterungen durch Replikate und Partitionen erhöhen die Abrechnung basierend auf der folgenden Formel: [(Replikate × Partitionen × Preis)](#search-units). Der abgerechnete Preis hängt dabei von Ihrem ausgewählten Tarif ab.

Wenn Sie Ihre Kosten für eine Suchlösung abschätzen möchten, bedenken Sie, dass sich Preis und Kapazität nicht linear verhalten. (Bei einer Verdopplung der Kapazität fallen mehr als doppelt so hohe Kosten an.) Ein Beispiel zur Funktionsweise der Formel finden Sie unter [Zuordnen von Replikaten und Partitionen](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

### <a name="bandwidth-charges"></a>Bandbreitengebühren

Die Verwendung von [Indexern](search-indexer-overview.md) kann sich auf die Abrechnung auswirken, wenn sich die Azure-Datenquelle in einer anderen Region als Azure Cognitive Search befindet. In diesem Szenario sind dies die Kosten für das Verschieben von ausgehenden Daten aus der Azure-Datenquelle in Azure Cognitive Search. 

Sie können die Gebühren für ausgehende Daten vollständig vermeiden, wenn Sie den Dienst für die kognitive Azure-Suche in derselben Region wie Ihre Daten erstellen. Die folgenden Informationen stammen von der Seite [Preisübersicht Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/):

+ Microsoft stellt keine Gebühren für in beliebige Azure-Dienste eingehende Daten in Rechnung.
+ Es fallen keine Gebühren für aus Azure Cognitive Search ausgehende Daten an. Beispiel: Wenn sich Ihr Suchdienst in der Region „USA, Westen“ befindet und eine Azure-Web-App in der Region „USA, Osten“ bereitgestellt wird, stellt Microsoft keine Gebühren für die Abfrageantwortnutzlasten aus „USA, Westen“ in Rechnung.
+ Bei Lösungen mit mehreren Diensten fallen keine Gebühren für Datenübertragungen an, wenn sich alle Dienste in derselben Region befinden.

Für ausgehende Daten fallen Gebühren an, wenn sich die Dienste in verschiedenen Regionen befinden. Diese Gebühren sind nicht Teil Ihrer eigentlichen Rechnung für die kognitive Azure-Suche. Sie werden hier jedoch erwähnt, weil die Kosten für eine Datenübertragung per Pull aus unterschiedlichen Regionen mithilfe von Daten oder um KI erweiterte Indexer in Ihrer Gesamtrechnung aufgeführt werden.

### <a name="ai-enrichment-with-cognitive-services"></a>KI-Anreicherung mit Cognitive Services

Für die [KI-Anreicherung](cognitive-search-concept-intro.md) sollten Sie eine [abrechenbare Azure Cognitive Services-Ressource](cognitive-search-attach-cognitive-services.md) in derselben Region wie die kognitive Azure-Suche im S0-Tarif für die nutzungsbasierte Bezahlung der Verarbeitung anfügen. Durch das Anfügen von Cognitive Services fallen keine festen Kosten an. Sie bezahlen nur für die benötigte Verarbeitung.

| Vorgang | Auswirkungen auf die Abrechnung |
|-----------|----------------|
| Dokumententschlüsselung, Textextraktion | Kostenlos |
| Dokumententschlüsselung, Bildextraktion | Die Abrechnung erfolgt entsprechend der Anzahl von Bildern, die aus den Dokumenten extrahiert werden. In einer [Indexerkonfiguration](/rest/api/searchservice/create-indexer#indexer-parameters) löst der Parameter **imageAction** die Bildextraktion aus. Ist **imageAction** auf „None“ festgelegt (Standardeinstellung), fallen keine Gebühren für die Bildextraktion an. Die Preise für die Bildextraktion sind auf der Seite mit [Preisdetails](https://azure.microsoft.com/pricing/details/search/) für die kognitive Azure-Suche dokumentiert.|
| [Integrierte kognitive Qualifikationen](cognitive-search-predefined-skills.md) | Wird zum gleichen Preis abgerechnet, als ob Sie die Aufgabe direkt mithilfe von Cognitive Services ausgeführt hätten. |
| Benutzerdefinierte Qualifikationen | Bei einer benutzerdefinierten Qualifikation handelt es sich um die von Ihnen bereitgestellte Funktionalität. Die Kosten für die Verwendung einer benutzerdefinierten Qualifikation hängen ganz davon ab, ob mit dem benutzerdefinierten Code andere gebührenpflichtige Dienste aufgerufen werden. |

Mit dem Feature [Inkrementelle Anreicherung (Vorschau)](cognitive-search-incremental-indexing-conceptual.md) können Sie einen Cache bereitstellen, durch den der Indexer beim Ausführen nur der kognitiven Fähigkeiten, die beim zukünftigen Ändern Ihres Skillsets notwendig sind, effizienter arbeiten kann. Das spart Zeit und Geld.

<a name="search-units"></a>

## <a name="billing-formula-r-x-p--su"></a>Formel für die Abrechnung (R × P = SU)

Bei Vorgängen der kognitiven Azure-Suche ist das wichtigste Abrechnungskonzept die *Sucheinheit* (Search Unit, SU). Da die kognitive Azure-Suche bei der Indizierung und bei Abfragen sowohl von Replikaten als auch von Partitionen abhängig ist, ist es nicht sinnvoll, entweder nur nach dem einen oder nur nach dem anderen abzurechnen. Stattdessen basiert die Abrechnung auf einer Kombination beider.

Die SU ist das Produkt aus den von einem Dienst verwendeten *Replikaten* und *Partitionen*: **(R x P = SU)** .

Jeder Dienst beginnt mit mindestens einer SU (einem Replikat multipliziert mit einer Partition). Die maximale Anzahl von SUs für einen Dienst beträgt 36. Dieser Höchstwert kann auf verschiedene Weise erreicht werden: beispielsweise mit 6 Partitionen x 6 Replikaten oder mit 3 Partitionen x 12 Replikaten. Üblicherweise werden weniger als die Gesamtkapazität an SUs verwendet (3 Replikate x 3 Partitionen werden beispielsweise als 9 SUs abgerechnet). Gültige Kombinationen finden Sie in der Tabelle [Partitions- und Replikatskombinationen](search-capacity-planning.md#chart).

Der Preis wird stündlich pro SU abgerechnet. Dabei steigt der Preis mit jedem höheren Tarif an. Höhere Tarife enthalten größere und schnellere Partitionen, was zu einem insgesamt höheren Stundensatz für den jeweiligen Tarif führt. Die Preise für die einzelnen Tarife finden Sie auf der Seite [Azure Search – Preise](https://azure.microsoft.com/pricing/details/search/).

Die meisten Kunden schalten nur einen Teil der Gesamtkapazität online und halten den Rest in Reserve. Die Anzahl der Partitionen und Replikate, die Sie online schalten, bestimmt (mit der SU-Formel berechnet) den stündlichen Preis.

## <a name="next-steps"></a>Nächste Schritte

Die Kostenverwaltung ist ein wesentlicher Bestandteil der Kapazitätsplanung. Fahren Sie im nächsten Schritt mit dem folgenden Artikel fort. Darin erfahren Sie, wie Sie die Kapazität schätzen und Kosten verwalten können.

> [!div class="nextstepaction"]
> [Schätzen von Kapazität und Kosten in Azure Cognitive Search](search-sku-manage-costs.md)