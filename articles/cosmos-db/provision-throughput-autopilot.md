---
title: Erstellen von Azure Cosmos-Containern und -Datenbanken im Autopilot-Modus
description: Erfahren Sie mehr über die Vorteile, die Anwendungsfälle und die Bereitstellung von Azure Cosmos-Datenbanken und -Containern im Autopilot-Modus.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 89af30788fe5129cddc6a3607b8c722549b610d1
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2020
ms.locfileid: "76264049"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Erstellen von Azure Cosmos-Containern und -Datenbanken im Autopilot-Modus (Vorschauversion)

Mit Azure Cosmos DB können Sie Durchsatz für Ihre Container im manuellen oder Autopilot-Modus bereitstellen. In diesem Artikel werden die Vorteile des Autopilot-Modus und Anwendungsfälle beschrieben.

> [!NOTE]
> Der Autopilot-Modus ist derzeit in der öffentlichen Vorschau verfügbar. Sie können den [Autopilot-Modus nur für neue Datenbanken und Container aktivieren](#create-a-database-or-a-container-with-autopilot-mode). Der Modus ist für vorhandene Container und Datenbanken nicht verfügbar.

Zusätzlich zur manuellen Bereitstellung von Durchsatz können Sie jetzt Azure Cosmos-Container im Autopilot-Modus konfigurieren. Bei Containern und Datenbanken, die im Autopilot-Modus konfiguriert wurden, wird **der bereitgestellte Durchsatz automatisch und verzögerungsfrei gemäß Ihren Anwendungsanforderungen skaliert, ohne dass dies Auswirkungen auf Verfügbarkeit, Latenz, Durchsatz oder Leistung der allgemeinen Workload hat**.

Beim Konfigurieren von Containern und Datenbanken im Autopilot-Modus müssen Sie den maximalen Durchsatz `Tmax` angeben, der nicht überschritten werden darf. Container können dann ihren Durchsatz so skalieren, dass `0.1*Tmax < T < Tmax` gilt. Mit anderen Worten: Container und Datenbanken werden basierend auf den Anforderungen der Workload unverzüglich skaliert – von einem von Ihnen konfigurierten geringen Wert (z. B. 10 % des maximalen Durchsatzwerts) bis zum konfigurierten Maximalwert. Sie können die Einstellung für den maximalen Durchsatz (`Tmax`) für eine Autopilot-Datenbank oder einen Autopilot-Container jederzeit ändern. Mit der Autopilot-Option gilt der minimale Durchsatz von 400 RU/s pro Container oder Datenbank nicht mehr.

Während der Vorschauphase von Autopilot ist für den angegebenen maximalen Durchsatz im Container oder in der Datenbank der Betrieb innerhalb der berechneten Speicherbegrenzung zulässig. Wenn die Speicherbegrenzung überschritten wird, wird der maximale Durchsatz automatisch auf einen höheren Wert festgelegt. Bei Verwendung des Durchsatzes auf Datenbankebene im Autopilot-Modus wird die Anzahl von Containern, die in einer Datenbank zulässig ist, wie folgt berechnet: `0.001*TMax`. Wenn Sie beispielsweise 20.000 RU/s im Autopilot-Modus bereitstellen, kann die Datenbank über 20 Container verfügen.

## <a name="benefits-of-autopilot-mode"></a>Vorteile des Autopilot-Modus

Azure Cosmos-Container, die im Autopilot-Modus konfiguriert sind, haben die folgenden Vorteile:

* **Einfach:** Container im Autopilot-Modus machen komplexe Abläufe wie die manuelle Verwaltung des bereitgestellten Durchsatzes (RUs) und der Kapazität für verschiedene Container überflüssig.

* **Skalierbar:** Bei Containern im Autopilot-Modus lässt sich die Kapazität des bereitgestellten Durchsatzes nach Bedarf nahtlos skalieren. Dabei werden weder Clientverbindungen oder Anwendungen unterbrochen, noch vorhandene SLAs beeinträchtigt.

* **Kosteneffizient:** Wenn Sie im Autopilot-Modus konfigurierte Container verwenden, zahlen Sie nur für die Ressourcen, die Ihre Workloads benötigen, auf Stundenbasis.

* **Hochverfügbarkeit:** Container im Autopilot-Modus verwenden dasselbe global verteilte, fehlertolerante, hochverfügbare Back-End, um die Dauerhaftigkeit und Hochverfügbarkeit von Daten sicherzustellen.

## <a name="use-cases-of-autopilot-mode"></a>Anwendungsfälle für den Autopilot-Modus

Die Anwendungsfälle für die im Autopilot-Modus konfigurierten Azure Cosmos-Container umfassen:

* **Variable Workloads:** Sie führen eine gering ausgelastete Anwendung mit einer Spitzenauslastung von einer bis zu mehreren Stunden einige Male täglich oder mehrmals pro Jahr aus. Beispiele hierfür sind Anwendungen für Personalwesen, Budgetierung und Betriebsberichterstattung. Für solche Szenarien können Container verwendet werden, die im Autopilot-Modus konfiguriert sind. Es ist nicht mehr erforderlich, eine manuelle Bereitstellung für maximale oder durchschnittliche Kapazitäten vorzunehmen.

* **Unvorhersehbare Workloads:** Sie führen Workloads aus, bei denen die Datenbanknutzung über den ganzen Tag erfolgt, allerdings auch Aktivitätsspitzen auftreten, die nur schwer vorhersehbar sind. Ein Beispiel hierfür ist eine Verkehrsinfo-Website, bei der die Aktivitäten ansteigen, sobald sich der Wetterbericht ändert. Bei Containern, die im Autopilot-Modus konfiguriert sind, wird die Kapazität angepasst, wenn die Anwendung eine Spitzenlast verzeichnet, und wieder horizontal herunterskaliert, wenn die Aktivitäten wieder auf das normale Maß zurückgehen.

* **Neue Anwendungen:** Sie stellen eine neue Anwendung bereit und sind sich nicht sicher, wie viel bereitgestellten Durchsatz (d. h. wie viele RUs) Sie benötigen. Bei Containern, die im Autopilot-Modus konfiguriert sind, können Sie die Skalierung automatisch an den Kapazitätsbedarf und die Anforderungen Ihrer Anwendung anpassen.

* **Selten genutzte Anwendungen:** Sie verfügen über eine Anwendung, die mehrmals pro Tag, Woche oder Monat für wenige Stunden genutzt wird, beispielsweise eine Anwendung/Website/Blogwebsite mit geringem Volumen.

* **Entwicklungs-und Testdatenbanken:** Ihre Entwickler verwenden Container während der Arbeitszeit, benötigen sie jedoch nicht nachts oder an Wochenenden. Bei Containern, die im Autopilot-Modus konfiguriert sind, wird die Skalierung auf ein Minimum beschränkt, wenn sie nicht verwendet werden.

* **Geplante Produktionsworkloads/-abfragen:** Wenn Sie über eine Reihe geplanter Anforderungen/Vorgänge/Abfragen für einen einzelnen Container verfügen und es Leerlaufphasen gibt, in denen Sie den Durchsatz auf ein absolutes Minimum beschränken möchten, können Sie dies jetzt problemlos tun. Wenn eine geplante Abfrage/Anforderung an einen Container übermittelt wird, der im Autopilot-Modus konfiguriert ist, wird der Durchsatz soweit wie nötig zentral hochskaliert, und der Vorgang wird ausgeführt.

Lösungen für die vorherigen Probleme erfordern nicht nur eine sehr lange Implementierungszeit, sondern erhöhen auch die Komplexität der Konfiguration oder Ihres Codes. Außerdem ist häufig ein manuelles Eingreifen erforderlich, um die Probleme zu beheben. Im Autopilot-Modus werden die oben aufgeführten Szenarien standardmäßig unterstützt, sodass Sie sich nicht mehr um diese Probleme kümmern müssen.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Vergleich – im manuellen Modus und im Autopilot-Modus konfigurierte Container

|  | Im manuellen Modus konfigurierte Container  | Im Autopilot-Modus konfigurierte Container |
|---------|---------|---------|
| **Bereitgestellter Durchsatz** | Manuell bereitgestellt. | Automatische und sofortige Skalierung basierend auf den Workload-Verwendungsmustern. |
| **Ratenbegrenzung von Anforderungen/Vorgängen (429)**  | Kann passieren, wenn die Nutzung die bereitgestellte Kapazität überschreitet. | Wird nicht durchgeführt, wenn der verbrauchte Durchsatz unter dem maximalen Durchsatz liegt, den Sie für den Autopilot-Modus ausgewählt haben.   |
| **Kapazitätsplanung** |  Sie müssen eine anfängliche Kapazitätsplanung durchführen und den benötigten Durchsatz bereitstellen. |    Sie müssen sich nicht um die Kapazitätsplanung kümmern. Das System übernimmt automatisch die Kapazitätsplanung und -verwaltung. |
| **Preise** | Manuell bereitgestellte RU/s pro Stunde | Bei Konten mit einer Schreibregion zahlen Sie für den genutzten Durchsatz auf Stundenbasis nach dem Tarif für Autopilot-RU/s pro Stunde. <br/><br/>Bei Konten mit mehreren Schreibregionen fallen keine zusätzlichen Kosten für Autopilot an. Sie zahlen für den genutzten Durchsatz auf Stundenbasis nach demselben Tarif für Multimaster-RU/s pro Stunde. |
| **Für folgende Workloadtypen am besten geeignet** |  Vorhersehbare und stabile Workloads|   Nicht vorhersehbare und variable Workloads  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Erstellen einer Datenbank oder eines Containers mit dem Autopilot-Modus

Sie können den Autopilot-Modus für neue Datenbanken oder Container konfigurieren, während Sie diese über das Azure-Portal erstellen. Führen Sie die folgenden Schritte aus, um eine neue Datenbank oder einen neuen Container zu erstellen, Autopilot zu aktivieren und den maximalen Durchsatz (RU/s) anzugeben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder beim [Azure Cosmos DB-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos DB-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie **Neuer Container** aus. Geben Sie einen Namen für die Datenbank und den Container sowie einen Partitionsschlüssel an. Wählen Sie unter **Durchsatz** die Option **Autopilot** aus. Wählen Sie dann den maximalen Durchsatz (RU/s) aus, der von der Datenbank oder vom Container bei der Verwendung der Autopilot-Option nicht überschritten werden darf.

   ![Erstellen eines Containers und Konfigurieren von Autopilot-Durchsatz](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. Klicken Sie auf **OK**.

Sie können eine Datenbank mit gemeinsam genutztem Durchsatz mit Autopilot-Modus erstellen, indem Sie die Option **Datenbankdurchsatz** auswählen.

## <a id="autopilot-limits"></a> Durchsatz- und Speicherlimits für Autopilot

In der nachstehenden Tabelle werden die maximalen Durchsatz- und Speicherlimits für verschiedene Optionen im Autopilot-Modus gezeigt:

|Maximales Durchsatzlimit  |Maximales Speicherlimit  |
|---------|---------|
|4\.000 RU/s  |   50 GB    |
|20.000 RU/s  |  200 GB  |
|100.000 RU/s    |  1 TB   |
|500.000 RU/s    |  5 TB  |

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [häufig gestellten Fragen zu Autopilot](autopilot-faq.md).
* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
