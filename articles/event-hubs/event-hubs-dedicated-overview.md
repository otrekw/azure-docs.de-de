---
title: Übersicht über dedizierte Event Hubs – Azure Event Hubs | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über dedizierte Azure Event Hubs, die Bereitstellungen von Event Hubs mit einzelnen Mandanten bieten.
ms.topic: article
ms.date: 10/23/2020
ms.openlocfilehash: 20b153c9093c96e7357a8e439b6655f1db80bd46
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92516993"
---
# <a name="overview-of-event-hubs-dedicated"></a>Übersicht über Event Hubs Dedicated

*Event Hubs-Cluster* bieten Bereitstellungen mit einem einzelnen Mandanten für Kunden mit äußerst anspruchsvollen Streaminganforderungen. Dieses Einzelmandantangebot verfügt über ein garantiertes 99,99 %-SLA und ist nur in unserem Dedicated-Tarif verfügbar. Ein Event Hubs-Cluster kann Millionen Ereignisse pro Sekunde mit garantierter Kapazität und Latenz im Sekundenbruchteilbereich erfassen. Namespaces und Event Hubs, die innerhalb des dedizierten Clusters erstellt werden, bieten alle Funktionen des Standardangebots und mehr, aber ohne jegliche Einschränkung bei eingehenden Daten. Enthalten ist ferner die beliebte [Event Hubs Capture](event-hubs-capture-overview.md)-Funktion ohne zusätzliche Kosten, die es Ihnen ermöglicht, Datenströme zu Batches zusammenzufassen und in Azure Storage oder Azure Data Lake zu protokollieren. 

Cluster werden in Form von **Kapazitätseinheiten (Capacity Units, CUs)** bereitgestellt und abgerechnet, einem vorab zugewiesenen Maß an CPU- und Arbeitsspeicherressourcen. Sie können für jeden Cluster 1, 2, 4, 8, 12, 16 oder 20 CUs erwerben. Wie viele eingehende Daten Sie pro CU erfassen und streamen können, hängt von einer Vielzahl verschiedener Faktoren ab, wie z. B. der Anzahl von Producern und Consumern, Form der Nutzlast, Ausgangsrate (Weitere Informationen finden Sie weiter unten in den Benchmarkergebnissen). 

> [!NOTE]
> Alle Event Hubs-Cluster sind standardmäßig Kafka-aktiviert und unterstützen Kafka-Endpunkte, die von Ihren vorhandenen Kafka-basierten Anwendungen verwendet werden können. Wenn Kafka auf Ihrem Cluster aktiviert ist, hat dies keine Auswirkungen auf Ihre nicht Kafka-basierten Anwendungsfälle. Es gibt weder eine Option noch die Notwendigkeit, Kafka in einem Cluster zu deaktivieren.

## <a name="why-dedicated"></a>Warum dediziert?

Dedizierte Event Hubs bieten drei bestechende Vorteile für Kunden, die Kapazitäten auf Unternehmensniveau benötigen:

#### <a name="single-tenancy-guarantees-capacity-for-better-performance"></a>Ein Einzelmandant garantiert Kapazität für bessere Leistung

Ein Dedicated-Cluster garantiert Kapazität in vollem Umfang und kann mehrere Gigabytes an eingehenden Streamingdaten mit vollständig dauerhaftem Speicher und Latenzen von unter einer Sekunde verarbeiten, sodass Bursts kein Problem mehr darstellen. 

#### <a name="inclusive-and-exclusive-access-to-features"></a>Inklusiver und exklusiver Zugriff auf Funktionen 
Das Dedicated-Angebot umfasst Funktionen wie Capture ohne zusätzliche Kosten sowie exklusiven Zugriff auf neue Features wie BYOK (Bring Your Own Key). Der Dienst verwaltet auch Lastenausgleich, Betriebssystemupdates, Sicherheitspatches und Partitionierung für den Kunden, damit Sie weniger Zeit auf die Wartung der Infrastruktur verwenden müssen und mehr Zeit zum Erstellen von clientseitigen Features haben.  

#### <a name="cost-savings"></a>Kostenersparnis
Bei hohen Eingangsvolumen (> 100 Durchsatzeinheiten) kostet ein Cluster bedeutend weniger pro Stunde als der Kauf einer vergleichbaren Menge an Durchsatzeinheiten im Standard-Angebot.


## <a name="event-hubs-dedicated-quotas-and-limits"></a>Kontingente und Limits von Event Hubs Dedicated

Das Event Hubs Dedicated-Angebot wird zu einem festen Monatspreis mit einem Minimum von vier Stunden Nutzung in Rechnung gestellt. Der Dedicated-Tarif umfasst die Funktionen des Standard-Tarifs, jedoch mit Kapazitäten und Limits auf Unternehmensniveau für Kunden mit anspruchsvollen Workloads. 

| Funktion | Standard | Dediziert |
| --- |:---:|:---:|
| Bandbreite | 20 TUs (bis zu 40 TUs) | 20 CUs |
| Namespaces |  1 | 50 pro CU |
| Event Hubs |  10 pro Namespace | 1000 pro Namespace |
| Eingangsereignisse | Bezahlung pro Million Ereignisse | Enthalten |
| Nachrichtengröße | 1 Million Bytes | 1 Million Bytes |
| Partitionen | 32 pro Event Hub | 1\.024 pro Event Hub |
| Verbrauchergruppen | 20 pro Event Hub | Kein Limit pro CU, 1000 pro Event Hub |
| Brokerverbindungen | 1\.000 enthalten, max. 5.000 | 100.000 enthalten, gleichzeitig Max. |
| Nachrichtenaufbewahrung | 7 Tage, 84 GB enthalten pro TU | 90 Tage, 10 TB enthalten pro CU |
| Erfassung | Bezahlung pro Stunde | Enthalten |

## <a name="how-to-onboard"></a>Onboardingmethoden

Die Self-Service-Funktion zum [Erstellen eines Event Hubs-Clusters](event-hubs-dedicated-cluster-create-portal.md) über das [Azure-Portal](https://aka.ms/eventhubsclusterquickstart) befindet sich jetzt in der Vorschauphase. Wenn Sie Fragen haben oder Hilfe beim Onboarding von Event Hubs Dedicated benötigen, wenden Sie sich an das [Event Hubs-Team](mailto:askeventhubs@microsoft.com).

## <a name="faqs"></a>Häufig gestellte Fragen

[!INCLUDE [event-hubs-dedicated-clusters-faq](../../includes/event-hubs-dedicated-clusters-faq.md)]

## <a name="next-steps"></a>Nächste Schritte

Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter oder den Microsoft-Support, um weitere Details zu Dedicated Event Hubs zu erhalten. Sie können auch einen Cluster erstellen oder weitere Informationen zu Event Hubs-Tarifen erhalten, indem Sie die folgenden Links besuchen:

- [Erstellen eines Event Hubs-Clusters über das Azure-Portal](https://aka.ms/eventhubsclusterquickstart) 
- [Event Hubs Preise](https://azure.microsoft.com/pricing/details/event-hubs/) Wenden Sie sich an Ihren Microsoft-Vertriebsmitarbeiter oder den Microsoft-Support, um weitere Details zur Dedicated Event Hubs-Kapazität zu erhalten.
- Unter [Event Hubs – häufig gestellte Fragen](event-hubs-faq.md) finden Sie Preisinformationen und Antworten auf einige häufig gestellte Fragen zu Event Hubs.
