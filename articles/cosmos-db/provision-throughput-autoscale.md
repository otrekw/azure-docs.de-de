---
title: Erstellen von Azure Cosmos-Containern und -Datenbanken mit per Autoskalierung bereitgestelltem Durchsatz
description: Informieren Sie sich über die Vorteile, die Anwendungsfälle und die Bereitstellung von Azure Cosmos-Datenbanken und -Containern mit per Autoskalierung bereitgestelltem Durchsatz.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: 81a13dcb7955a7d46f485416bf9b7e4e7be4d9ac
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791713"
---
# <a name="create-azure-cosmos-containers-and-databases-with-autoscale-provisioned-throughput"></a>Erstellen von Azure Cosmos-Containern und -Datenbanken mit per Autoskalierung bereitgestelltem Durchsatz

Mit Azure Cosmos DB können Sie Ihre Container mit dem (manuell bereitgestellten) Standarddurchsatz oder dem per Autoskalierung bereitgestellten Durchsatz konfigurieren. In diesem Artikel werden die Vorteile und Anwendungsfälle der Autoskalierung beschrieben.

> [!NOTE]
> Sie können die [Autoskalierung nur für neue Datenbanken und Container aktivieren](#create-db-container-autoscale). Der Modus ist für vorhandene Container und Datenbanken nicht verfügbar.

Zusätzlich zur Standardbereitstellung von Durchsatz können Sie jetzt Azure Cosmos-Container mit per Autoskalierung bereitgestelltem Durchsatz konfigurieren. Bei Containern und Datenbanken, die mit per Autoskalierung bereitgestelltem Durchsatz konfiguriert wurden, wird **der bereitgestellte Durchsatz automatisch und verzögerungsfrei gemäß Ihren Anwendungsanforderungen skaliert, ohne dass dies Auswirkungen auf Verfügbarkeit, Latenz, Durchsatz oder Leistung der allgemeinen Workload hat**.

Beim Konfigurieren von Containern und Datenbanken mit Autoskalierung müssen Sie den maximalen Durchsatz `Tmax` angeben, der nicht überschritten werden darf. Container können dann ihren Durchsatz so skalieren, dass `0.1*Tmax < T < Tmax` gilt. Mit anderen Worten: Container und Datenbanken werden basierend auf den Anforderungen der Workload unverzüglich skaliert – von einem von Ihnen konfigurierten geringen Wert (z. B. 10 % des maximalen Durchsatzwerts) bis zum konfigurierten Maximalwert. Nach dem Konfigurieren der Autoskalierung können Sie die Einstellung für den maximalen Durchsatz (`Tmax`) für eine Datenbank oder einen Container jederzeit ändern. Mit der Autoskalierungsoption gilt der minimale Durchsatz von 400 RU/s pro Container oder Datenbank nicht mehr.

Für den angegebenen maximalen Durchsatz im Container oder in der Datenbank ist der Betrieb innerhalb der berechneten Speicherbegrenzung zulässig. Wenn die Speicherbegrenzung überschritten wird, wird der maximale Durchsatz automatisch auf einen höheren Wert festgelegt. Bei Verwendung des Durchsatzes auf Datenbankebene mit Autoskalierung wird die Anzahl von Containern, die in einer Datenbank zulässig ist, wie folgt berechnet: `0.001*TMax`. Wenn Sie beispielsweise 20.000 RU/s im Autoskalierungsmodus bereitstellen, kann die Datenbank über 20 Container verfügen.

## <a name="benefits-of-autoscale-provisioned-throughput"></a><a id="autoscale-benefits">Vorteile des per Autoskalierung bereitgestellten Durchsatzes</a>

Azure Cosmos-Container, die mit Autoskalierung konfiguriert sind, bieten die folgenden Vorteile:

* **Einfach:** Container mit Autoskalierung machen komplexe Abläufe wie die manuelle Verwaltung des bereitgestellten Durchsatzes (RUs) und der Kapazität für verschiedene Container überflüssig.

* **Skalierbar:** Bei Containern mit Autoskalierung lässt sich die Kapazität des bereitgestellten Durchsatzes nach Bedarf nahtlos skalieren. Dabei werden weder Clientverbindungen oder Anwendungen unterbrochen, noch vorhandene SLAs beeinträchtigt.

* **Kosteneffizient:** Wenn Sie mit Autoskalierung konfigurierte Container verwenden, zahlen Sie auf Stundenbasis nur für die Ressourcen, die Ihre Workloads benötigen.

* **Hochverfügbarkeit:** Container mit Autoskalierung verwenden dasselbe global verteilte, fehlertolerante, hochverfügbare Back-End, um die Dauerhaftigkeit und Hochverfügbarkeit von Daten sicherzustellen.

## <a name="use-cases-of-autoscale-provisioned-throughput"></a><a id="autoscale-usecases"></a> Anwendungsfälle des per Autoskalierung bereitgestellten Durchsatzes

Folgende Anwendungsfälle bieten sich für die mit Autoskalierung konfigurierten Azure Cosmos-Container an:

* **Variable Workloads:** Sie führen eine gering ausgelastete Anwendung mit einer Spitzenauslastung von einer bis zu mehreren Stunden einige Male täglich oder mehrmals pro Jahr aus. Beispiele hierfür sind Anwendungen für Personalwesen, Budgetierung und Betriebsberichterstattung. Für solche Szenarien können Container verwendet werden, die mit Autoskalierung konfiguriert sind. Es ist nicht mehr erforderlich, eine manuelle Bereitstellung für maximale oder durchschnittliche Kapazitäten vorzunehmen.

* **Unvorhersehbare Workloads:** Sie führen Workloads aus, bei denen die Datenbanknutzung über den ganzen Tag erfolgt, allerdings auch Aktivitätsspitzen auftreten, die nur schwer vorhersehbar sind. Ein Beispiel hierfür ist eine Verkehrsinfo-Website, bei der die Aktivitäten ansteigen, sobald sich der Wetterbericht ändert. Bei Containern, die mit Autoskalierung konfiguriert sind, wird die Kapazität angepasst, wenn die Anwendung eine Spitzenlast verzeichnet, und wieder herunterskaliert, wenn die Aktivitäten wieder auf das normale Maß zurückgehen.

* **Neue Anwendungen:** Sie stellen eine neue Anwendung bereit und sind sich nicht sicher, wie viel bereitgestellten Durchsatz (d. h. wie viele RUs) Sie benötigen. Bei Containern, die mit Autoskalierung konfiguriert sind, können Sie die Skalierung automatisch an den Kapazitätsbedarf und die Anforderungen Ihrer Anwendung anpassen.

* **Selten genutzte Anwendungen:** Sie verfügen über eine Anwendung, die mehrmals pro Tag, Woche oder Monat für wenige Stunden genutzt wird, beispielsweise eine Anwendung/Website/Blogwebsite mit geringem Volumen.

* **Entwicklungs-und Testdatenbanken:** Ihre Entwickler verwenden Container während der Arbeitszeit, benötigen sie jedoch nicht nachts oder an Wochenenden. Bei Containern, die mit Autoskalierung konfiguriert sind, wird auf ein Minimum herunterskaliert, wenn sie nicht verwendet werden.

* **Geplante Produktionsworkloads/-abfragen:** Wenn Sie über eine Reihe geplanter Anforderungen/Vorgänge/Abfragen für einen einzelnen Container verfügen und es Leerlaufphasen gibt, in denen Sie den Durchsatz auf ein absolutes Minimum beschränken möchten, können Sie dies jetzt problemlos tun. Wenn eine geplante Abfrage/Anforderung an einen Container übermittelt wird, der mit Autoskalierung konfiguriert ist, wird der Durchsatz soweit wie nötig hochskaliert, und der Vorgang wird ausgeführt.

Lösungen für die vorherigen Probleme erfordern nicht nur eine sehr lange Implementierungszeit, sondern erhöhen auch die Komplexität der Konfiguration oder Ihres Codes. Außerdem ist häufig ein manuelles Eingreifen erforderlich, um die Probleme zu beheben. Bei der Autoskalierung werden die oben aufgeführten Szenarien standardmäßig unterstützt, sodass Sie sich nicht mehr um diese Probleme kümmern müssen.

## <a name="comparison--standard-manual-vs-autoscale-provisioned-throughput"></a>Vergleich von (manuell bereitgestelltem) Standarddurchsatz und per Autoskalierung bereitgestelltem Durchsatz

|  | Mit Standarddurchsatz konfigurierte Container  | Mit per Autoskalierung bereitgestelltem Durchsatz konfigurierte Container |
|---------|---------|---------|
| **Bereitgestellter Durchsatz** | Manuell bereitgestellt. | Automatische und sofortige Skalierung basierend auf den Workload-Verwendungsmustern. |
| **Ratenbegrenzung von Anforderungen/Vorgängen (429)**  | Kann passieren, wenn die Nutzung die bereitgestellte Kapazität überschreitet. | Wird nicht durchgeführt, wenn der genutzte Durchsatz unter dem maximalen Durchsatz liegt, den Sie für die Autoskalierung ausgewählt haben.   |
| **Kapazitätsplanung** |  Sie müssen eine anfängliche Kapazitätsplanung durchführen und den benötigten Durchsatz bereitstellen. |    Sie müssen sich nicht um die Kapazitätsplanung kümmern. Das System übernimmt automatisch die Kapazitätsplanung und -verwaltung. |
| **Preise** | Manuell bereitgestellte RU/s pro Stunde | Bei Konten mit einer Schreibregion zahlen Sie für den genutzten Durchsatz auf Stundenbasis nach dem Tarif für Autoskalierungs-RUs/s pro Stundensatz. <br/><br/>Bei Konten mit mehreren Schreibregionen fallen keine zusätzlichen Kosten für die Autoskalierung an. Sie zahlen für den genutzten Durchsatz auf Stundenbasis nach demselben Tarif für Multimaster-RU/s pro Stunde. |
| **Für folgende Workloadtypen am besten geeignet** |  Vorhersehbare und stabile Workloads|   Nicht vorhersehbare und variable Workloads  |

## <a name="create-a-database-or-a-container-with-autoscale"></a><a id="create-db-container-autoscale"></a> Erstellen einer Datenbank oder eines Containers mit Autoskalierung

Sie können den Autoskalierungsmodus für neue Datenbanken oder Container konfigurieren, während Sie diese über das Azure-Portal erstellen. Führen Sie die folgenden Schritte aus, um eine neue Datenbank oder einen neuen Container zu erstellen, die Autoskalierung zu aktivieren und den maximalen Durchsatz (RU/s) anzugeben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder beim [Azure Cosmos DB-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos DB-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie **Neuer Container** aus. Geben Sie einen Namen für die Datenbank und den Container sowie einen Partitionsschlüssel an. Wählen Sie unter **Durchsatz** die Option **Autoskalierung** aus. Wählen Sie dann den maximalen Durchsatz (RU/s) aus, der von der Datenbank oder vom Container bei der Verwendung der Autoskalierungsoption nicht überschritten werden darf.

   ![Erstellen eines Containers und Konfigurieren des Durchsatzes mit Autoskalierung](./media/provision-throughput-autoscale/create-container-autoscale-mode.png)

1. Klicken Sie auf **OK**.

Sie können eine Datenbank mit gemeinsam genutztem Durchsatz mit Autoskalierung erstellen, indem Sie die Option **Datenbankdurchsatz bereitstellen** auswählen.

## <a name="throughput-and-storage-limits-for-autoscale"></a><a id="autoscale-limits"></a> Durchsatz- und Speicherlimits für die Autoskalierung

In der nachstehenden Tabelle werden die maximalen Durchsatz- und Speicherlimits für verschiedene Optionen mit Autoskalierung gezeigt:

|Maximales Durchsatzlimit  |Maximales Speicherlimit  |
|---------|---------|
|4\.000 RU/s  |   50 GB    |
|20.000 RU/s  |  200 GB  |
|100.000 RU/s    |  1 TB   |
|500.000 RU/s    |  5 TB  |

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr in den [häufig gestellten Fragen zur Autoskalierung](autoscale-faq.md).
* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
