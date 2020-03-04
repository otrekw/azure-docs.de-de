---
title: Überwachen von Azure Cosmos DB mit Azure Monitor für Cosmos DB (Vorschauversion)| Microsoft-Dokumentation
description: In diesem Artikel wird das Azure Monitor für Cosmos DB-Feature beschrieben, das Cosmos DB-Besitzern ein schnelles Verständnis der Leistungs- und Nutzungsprobleme bei ihren CosmosDB-Konten ermöglicht.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 10/27/2019
ms.openlocfilehash: dece5b0bb0508e2d83ee184e71ef0b4364d25ac8
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77622967"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db-preview"></a>Informationen zu Azure Monitor für Azure Cosmos DB (Vorschau)

Azure Monitor für Azure Cosmos DB (Vorschau) bietet eine Übersicht über Gesamtleistung, Fehler, Kapazität und Betriebsintegrität aller Ihrer Azure Cosmos DB-Ressourcen in einer vereinheitlichten interaktiven Oberfläche. Anhand dieses Artikels können Sie sich mit den Vorteilen dieser neuen Überwachungsoberfläche vertraut machen, und Sie erfahren, wie Sie diese entsprechend den konkreten Anforderungen Ihrer Organisation ändern und anpassen können.   

## <a name="introduction"></a>Einführung

Bevor Sie sich mit der Oberfläche genauer befassen, sollten Sie nachvollziehen können, wie sie Informationen darstellt und visualisiert. 

Vorteile:

* **Sicht im gewünschten Umfang** auf Ihre Azure Cosmos DB-Ressourcen in all Ihren Abonnements an einem zentralen Ort, wobei Sie den Bereich selektiv auf die Abonnements und Ressourcen festlegen können, die Sie bewerten möchten.

* **Drilldownanalyse** einer bestimmten Azure CosmosDB-Ressource, um Probleme zu diagnostizieren oder eine detaillierte Analyse nach Kategorien durchzuführen – Nutzung, Fehler, Kapazität und Vorgänge. Wenn Sie eine dieser Optionen auswählen, erhalten Sie eine detaillierte Übersicht über die relevanten Azure Cosmos DB-Metriken.  

* **Anpassbar**: Diese Oberfläche baut auf Azure Monitor-Arbeitsmappenvorlagen auf, sodass Sie ändern können, welche Metriken angezeigt werden, Schwellenwerte entsprechend Ihren Grenzwerten ändern oder festlegen können und die Änderungen in einer benutzerdefinierten Arbeitsmappe speichern können. Diagramme in der Arbeitsmappe können an Azure-Dashboards angeheftet werden.  

Bei diesem Feature müssen Sie nichts aktivieren oder konfigurieren, da diese Azure Cosmos DB-Metriken standardmäßig erfasst werden.

>[!NOTE]
>Für den Zugriff auf dieses Feature fallen keine Gebühren an. Ihnen werden nur die grundlegenden Features von Azure Monitor in Rechnung gestellt, die Sie entsprechend der Beschreibung auf der Seite [Azure Monitor-Preisdetails](https://azure.microsoft.com/pricing/details/monitor/) konfigurieren oder aktivieren.

## <a name="view-operation-level-metrics-for-azure-cosmos-db"></a>Anzeigen von Metriken auf Vorgangsebene für Azure Cosmos DB

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

1. Wählen Sie auf der Navigationsleiste auf der linken Seite die Option **Monitor** und anschließend **Metrik** aus.

   ![Bereich „Metriken“ in Azure Monitor](./media/cosmosdb-insights-overview/monitor-metrics-blade.png)

1. Klicken Sie im Bereich **Metriken** auf **Ressource auswählen**. Wählen Sie dann das erforderliche **Abonnement** und die **Ressourcengruppe** aus. Wählen Sie unter **Ressourcentyp** die Option **Azure Cosmos DB-Konten** aus. Wählen Sie dann eins der vorhandenen Azure Cosmos-Konten und anschließend **Anwenden** aus.

   ![Auswählen eines Cosmos DB-Kontos zum Anzeigen von Metriken](./media/cosmosdb-insights-overview/select-cosmosdb-account.png)

1. Als Nächstes können Sie eine Metrik aus der Liste der verfügbaren Metriken auswählen. Sie können spezifische Metriken für Anforderungseinheiten, Speicher, Wartezeit, Verfügbarkeit, Cassandra usw. auswählen. Ausführliche Informationen zu allen verfügbaren Metriken in dieser Liste finden Sie im Artikel [Metriken nach Kategorie](../../cosmos-db/monitor-cosmos-db-reference.md). In diesem Beispiel wählen Sie **Anforderungseinheiten** und als Aggregationswert **Mittelw.** aus.

   Zusätzlich zu diesen Angaben können Sie auch **Zeitbereich** und **Zeitgranularität** für die Metriken auswählen. Sie können Metriken maximal für die letzten 30 Tage anzeigen.  Nach Anwendung des Filters wird ein darauf basierendes Diagramm angezeigt. Sie sehen die durchschnittliche Anzahl der pro Minute verbrauchten Anforderungseinheiten für den ausgewählten Zeitraum.  

   ![Auswählen einer Metrik über das Azure-Portal](./media/cosmosdb-insights-overview/metric-types.png)

### <a name="add-filters-to-metrics"></a>Hinzufügen von Filtern zu Metriken

Sie können Metriken und das angezeigte Diagramm auch nach bestimmten Werten für **CollectionName**, **DatabaseName**, **OperationType**, **Region** und **StatusCode** filtern. Wählen Sie zum Filtern der Metriken **Filter hinzufügen** und dann die erforderliche Eigenschaft aus, etwa **OperationType**. Wählen Sie anschließend einen Wert aus, beispielsweise **Query**. Im Diagramm werden die Anforderungseinheiten angezeigt, die für den Abfragevorgang im ausgewählten Zeitraum verbraucht wurden. Die über eine gespeicherte Prozedur ausgeführten Vorgänge werden nicht protokolliert und sind daher nicht unter der Metrik „OperationType“ verfügbar.

![Hinzufügen eines Filters zum Auswählen der Metrikgranularität](./media/cosmosdb-insights-overview/add-metrics-filter.png)

Sie können Metriken mit der Option **Apply splitting** (Aufteilung anwenden) gruppieren. Beispielsweise können Sie die Anforderungseinheiten nach Vorgangstyp gruppieren und das Diagramm für alle Vorgänge auf einmal anzeigen, wie in der folgenden Abbildung dargestellt:

![Hinzufügen eines Filters zum Anwenden der Aufteilung](./media/cosmosdb-insights-overview/apply-metrics-splitting.png)

## <a name="view-utilization-and-performance-metrics-for-azure-cosmos-db"></a>Anzeigen von Nutzungs- und Leistungsmetriken für Azure Cosmos DB

Führen Sie die folgenden Schritte aus, um die Nutzung und Leistung Ihrer Speicherkonten in allen ihren Abonnements anzuzeigen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie nach **Monitor**, und wählen Sie **Monitor** aus.

    ![Suchfeld mit dem Wort „Monitor“, und eine Dropdownliste „Dienste“ mit dem Eintrag „Monitor“ mit einem Tachometersymbol](./media/cosmosdb-insights-overview/search-monitor.png)

3. Wählen Sie **Cosmos DB (Vorschau)** aus.

    ![Screenshot der Arbeitsmappe mit Cosmos DB-Übersicht](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Übersicht

In der **Übersicht** werden in der Tabelle interaktive Azure Cosmos DB-Metriken aufgeführt. Sie können die Ergebnisse anhand der Optionen filtern, die Sie in den folgenden Dropdownlisten auswählen:

* **Abonnements**: Es werden nur Abonnements aufgelistet, die über eine Azure Cosmos DB-Ressource verfügen.  

* **Cosmos DB**: Sie können alle oder eine Teilmenge von Azure Cosmos DB-Ressourcen oder eine einzelne Azure Cosmos DB-Ressource auswählen.

* **Zeitbereich** – Standardmäßig werden die Informationen aus den letzten 4 Stunden basierend auf der entsprechenden Auswahl angezeigt.

In der Zähler-Kachel unter den Dropdownlisten wird die Gesamtzahl der Azure Cosmos DB-Ressourcen in den ausgewählten Abonnements angezeigt. Für Spalten in der Arbeitsmappe, die Transaktionsmetriken melden, gibt es eine bedingte Farbcodierung oder Wärmebilder. Die dunkelste Farbe hat den höchsten Wert, und eine hellere Farbe basiert auf den niedrigsten Werten. 

Wenn Sie einen Dropdownpfeil neben einer der Azure Cosmos DB-Ressourcen auswählen, wird eine Aufschlüsselung der Leistungsmetriken auf der jeweiligen Datenbankcontainerebene eingeblendet:

![Erweiterte Dropdownliste, in der einzelne Datenbankcontainer und die zugehörige Leistungsaufschlüsselung angezeigt werden](./media/cosmosdb-insights-overview/container-view.png)

Wenn Sie den blau hervorgehobenen Azure Cosmos DB-Ressourcennamen auswählen, rufen Sie die standardmäßige **Übersicht** für das zugehörige Azure Cosmos DB-Konto auf. 

### <a name="failures"></a>Fehler

Wählen Sie oben auf der Seite **Fehler** aus, und der Bereich **Fehler** der Arbeitsmappenvorlage wird geöffnet. Hier wird die Gesamtzahl der Anforderungen mit der Verteilung der Antworten angezeigt, die für die Anforderungen vorhanden sind:

![Screenshot von Fehlern mit Aufschlüsselung nach HTTP-Anforderungstyp](./media/cosmosdb-insights-overview/failures.png)

| Code      |  BESCHREIBUNG       | 
|-----------|:--------------------|
| `200 OK`  | Einer der folgenden REST-Vorgänge war erfolgreich: </br>- GET für eine Ressource. </br> - PUT für eine Ressource. </br> - POST für eine Ressource. </br> - POST für eine gespeicherte Prozedur zum Ausführen der gespeicherten Prozedur.|
| `201 Created` | Ein POST-Vorgang zum Erstellen einer Ressource war erfolgreich. |
| `404 Not Found` | Es wird versucht, eine nicht mehr vorhandene Ressource zu verarbeiten. Beispiel: Die Ressource wurde möglicherweise bereits gelöscht. |

Eine vollständige Liste der Statuscodes finden Sie im Artikel zu [Azure Cosmos DB HTTP-Statuscodes](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

### <a name="capacity"></a>Capacity

Wählen Sie oben auf der Seite **Kapazität** aus, und der Bereich **Kapazität** der Arbeitsmappenvorlage wird geöffnet. Hier werden die Anzahl der vorhandenen Dokumente, der Dokumentzuwachs über die Zeit, die Datennutzung sowie die Gesamtmenge des verbleibenden verfügbaren Speichers angezeigt.  Anhand dieser Angaben können Sie potenzielle Speicher- und Datennutzungsprobleme ermitteln.

![Arbeitsmappe „Kapazität“](./media/cosmosdb-insights-overview/capacity.png) 

Wie bei der Übersichtsarbeitsmappe wird beim Auswählen der Dropdownliste neben einer Azure Cosmos DB-Ressource in der Spalte **Abonnement** eine Aufschlüsselung nach den einzelnen Containern der Datenbank angezeigt.

### <a name="operations"></a>Operationen (Operations) 

Wählen Sie oben auf der Seite **Vorgänge** aus, und der Bereich **Vorgänge** der Arbeitsmappenvorlage wird geöffnet. Hier können Sie Ihre Anforderungen überprüfen, aufgeschlüsselt nach dem Typ der getätigten Anforderungen. 

Im folgenden Beispiel ist ersichtlich, dass `eastus-billingint` hauptsächlich Leseanforderungen empfängt, jedoch auch eine kleine Anzahl von Upsert- und Erstellungsanforderungen vorhanden ist. `westeurope-billingint` hingegen ist in Bezug auf Anforderungen schreibgeschützt, zumindest über die letzten vier Stunden, auf die der Bereich der Arbeitsmappe derzeit über ihren Zeitbereichsparameter beschränkt ist.

![Bereich „Vorgänge“ der Arbeitsmappe](./media/cosmosdb-insights-overview/operation.png) 

## <a name="pin-export-and-expand"></a>Anheften, Exportieren und Erweitern

Sie können einen beliebigen Metrikabschnitt an ein [Azure-Dashboard](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) anheften, indem Sie oben rechts im Abschnitt das Stecknadelsymbol auswählen.

![Beispiel für das Anheften eines Metrikabschnitts an das Dashboard](./media/cosmosdb-insights-overview/pin.png)

Wenn Sie die Daten im Excel-Format exportieren möchten, wählen Sie das Abwärtspfeil-Symbol links neben dem Stecknadelsymbol aus.

![Symbol „Arbeitsmappe exportieren“](./media/cosmosdb-insights-overview/export.png)

Wenn Sie alle Dropdownansichten in der Arbeitsmappe erweitern oder reduzieren möchten, wählen Sie links vom Exportsymbol das Erweiterungssymbol aus:

![Symbol „Arbeitsmappe erweitern“](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db-preview"></a>Anpassen von Azure Monitor für Azure Cosmos DB (Vorschau)

Da diese Oberfläche auf Azure Monitor-Arbeitsmappenvorlagen aufbaut, stehen Ihnen die Optionen **Anpassen** > **Bearbeiten** zur Verfügung, und mit **Speichern** können Sie eine Kopie der geänderten Version in einer benutzerdefinierten Arbeitsmappe speichern. 

![Symbolleiste zum Anpassen](./media/cosmosdb-insights-overview/customize.png)

Arbeitsmappen werden in einer Ressourcengruppe gespeichert, und zwar entweder im für Sie privaten Abschnitt **Meine Berichte** oder im Abschnitt **Freigegebene Berichte**, der für alle Benutzer mit Zugriff auf die Ressourcengruppe zugänglich ist. Nachdem Sie die benutzerdefinierte Arbeitsmappe gespeichert haben, müssen Sie zum Arbeitsmappenkatalog wechseln, um die Mappe zu starten.

![Starten des Arbeitsmappenkatalogs über die Befehlsleiste](./media/cosmosdb-insights-overview/gallery.png)

## <a name="next-steps"></a>Nächste Schritte

* Konfigurieren Sie [Metrikwarnungen](../platform/alerts-metric.md) und [Dienstintegritätsbenachrichtigungen](../../service-health/alerts-activity-log-service-notifications.md), um automatisierte Warnungen einzurichten, die beim Erkennen von Problemen hilfreich sein können.

* Informieren Sie sich unter [Erstellen interaktiver Berichte mit Azure Monitor-Arbeitsmappen](../app/usage-workbooks.md) über die Szenarien, die Arbeitsmappen unterstützen sollen, wie Sie neue Berichte erstellen und vorhandene Berichte anpassen können, und vieles mehr.
