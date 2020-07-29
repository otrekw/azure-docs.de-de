---
title: Verwalten der Azure-Kosten mithilfe einer Automatisierung
description: In diesem Artikel wird erläutert, wie Sie Azure-Kosten mithilfe einer Automatisierung verwalten können.
author: bandersmsft
ms.author: banders
ms.date: 04/15/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: adwise
ms.openlocfilehash: 0727f98b917944f3721c6c6758fde05c2efd8773
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449830"
---
# <a name="manage-costs-with-automation"></a>Verwalten der Kosten per Automatisierung

Sie können mithilfe der Cost Management-Automatisierung einen benutzerdefinierten Satz von Lösungen erstellen, um Kostendaten abzurufen und zu verwalten. Dieser Artikel behandelt gängige Szenarien für die Cost Management-Automatisierung und die Optionen, die in Ihrer jeweiligen Situation zur Verfügung stehen. Wenn Sie eine Entwicklung mithilfe von APIs anstreben, können die hier bereitgestellten Beispiele gängiger API-Anforderungen Sie dabei unterstützen, Ihren Entwicklungsprozess zu beschleunigen.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Automatisieren des Abrufs von Kostendaten zur Offlineanalyse

Möglicherweise müssen Sie Ihre Azure-Kostendaten herunterladen, um sie mit weiteren Datasets zusammenzuführen. Oder Sie müssen Kostendaten in Ihre eigenen Systeme integrieren. Ja nach Datenmenge stehen verschiedene Optionen zur Verfügung. In jedem Fall müssen Sie über Cost Management-Berechtigungen im geeigneten Umfang verfügen, um APIs und Tools zu nutzen. Weitere Informationen finden Sie unter [Zuweisen des Zugriffs auf Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/assign-access-acm-data).

## <a name="suggestions-for-handling-large-datasets"></a>Vorschläge für die Verarbeitung großer Datasets

Wenn Ihre Organisation Azure in großem Umfang nutzt und über viele Ressourcen oder Abonnements verfügt, fällt eine große Menge an Nutzungsdaten an. Excel kann derartig große Dateien häufig nicht laden. In dieser Situation empfehlen wir die folgenden Optionen:

**Power BI**

Power BI wird dazu genutzt, große Datenmengen zu erfassen und zu verarbeiten. Als Enterprise Agreement-Kunde können Sie die Power BI-Vorlagen-App verwenden, um Kosten für Ihr Abrechnungskonto zu analysieren. Der Bericht enthält Schlüsselansichten, die von Kunden verwendet werden. Weitere Informationen finden Sie unter [Analysieren der Azure-Kosten mit der Power BI-Vorlagen-App](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app).

**Power BI-Datenconnector**

Wenn Sie Ihre Daten täglich analysieren möchten, empfehlen wird die Verwendung des [Power BI-Datenconnectors](https://docs.microsoft.com/power-bi/connect-data/desktop-connect-azure-cost-management) zum Abrufen von Daten für eine detaillierte Analyse. Alle erstellten Berichte werden durch den Connector auf den neuesten Stand gebracht, wenn weitere Kosten anfallen.

**Cost Management-Exporte**

Vielleicht ist es nicht erforderlich, die Daten täglich zu analysieren. In diesem Fall können Sie das Cost Management-Feature [Exporte](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) einsetzen, um Datenexporte in ein Azure Storage-Konto zu planen. Anschließend laden Sie die Daten nach Bedarf in Power BI oder analysieren die Daten in Excel, falls die Datei klein genug ist. Exporte stehen im Azure-Portal zur Verfügung oder können mit der [Export-API](https://docs.microsoft.com/rest/api/cost-management/exports) konfiguriert werden.

**Nutzungsdetails-API**

Erwägen Sie die Verwendung der [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usageDetails), wenn Sie über ein kleines Dataset mit Kostendaten verfügen. Bei einer großen Menge an Kostendaten sollten Sie für einen festgelegten Zeitraum möglichst wenig Nutzungsdaten abfragen. Hierzu legen Sie entweder ein kleines Zeitfenster fest oder verwenden in Ihrer Anforderung einen Filter. Beispielsweise zeigt die API in einem Szenario, in dem Sie Kostendaten für drei Jahre benötigen, eine bessere Leistung, wenn Sie anstelle eines einzigen Aufrufs mehrere Aufrufe für verschiedene Zeitbereiche ausführen. Anschließend können Sie die Daten zur weiteren Analyse in Excel laden.

## <a name="automate-retrieval-with-usage-details-api"></a>Automatischer Abruf mit der Nutzungsdetails-API

Die [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usageDetails) bietet eine einfache Möglichkeit, unformatierte, nicht aggregierte Kostendaten abzurufen, die Ihrer Azure-Rechnung entsprechen. Die API ist nützlich, wenn Ihre Organisation eine Lösung für den programmgesteuerten Datenabruf benötigt. Erwägen Sie den Einsatz der API, wenn Sie kleinere Datasets mit Kostendaten analysieren möchten. Wenn Sie hingegen über große Datasets verfügen, sollten Sie sich für eine der anderen Lösungen entscheiden, die zuvor genannt wurden. Die Daten in der Nutzungsdetails-API werden basierend auf Verbrauchseinheiten pro Tag bereitgestellt. Sie werden bei der Berechnung Ihrer monatlichen Rechnung verwendet. Die allgemein verfügbare Version dieser API ist `2019-10-01`. Verwenden Sie `2019-04-01-preview`, um per API auf die Vorschauversion für Reservierungen und Azure Marketplace-Käufe zuzugreifen.

### <a name="usage-details-api-suggestions"></a>Vorschläge zur Nutzungsdetails-API

**Anforderungszeitplan**

Es wird empfohlen, dass Sie pro Tag _nicht mehr als eine Anforderung_ an die Nutzungsdetails-API übermitteln. Weitere Informationen dazu, wie häufig Kostendaten aktualisiert und wie Beträge gerundet werden, finden Sie unter [Grundlegendes zu Cost Management-Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data#rated-usage-data-refresh-schedule).

**Verwendung von Bereichen oberster Ebene ohne Filterung**

Verwenden Sie die API zum Abrufen aller benötigten Daten auf der höchstmöglichen Ebene. Führen Sie erst dann eine Filterung, Gruppierung oder aggregierte Analyse durch, wenn alle erforderlichen Daten erfasst wurden. Die API ist dafür optimiert, große Mengen an nicht aggregierten, unformatierten Kostendaten bereitzustellen. Weitere Informationen zu den verfügbaren Bereichen in Cost Management finden Sie unter [Verstehen von und Arbeiten mit Bereichen](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-work-scopes). Nachdem Sie die erforderlichen Daten für einen Bereich heruntergeladen haben, verwenden Sie Excel, um die Daten mithilfe von Filtern und Pivottabellen zu analysieren.

## <a name="example-usage-details-api-requests"></a>Nutzungsdetails-API – Beispielanforderungen

Die folgenden Beispielanforderungen werden von Microsoft-Kunden verwendet, um gängige Szenarien abzudecken.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Abrufen von Nutzungsdetails für einen Bereich in einem bestimmten Datumsbereich

Die über die Anforderung zurückgegebenen Daten entsprechen dem Datum, an dem die Nutzung vom Abrechnungssystem erfasst wurde. Die Daten können Kosten mehrerer Rechnungen umfassen.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01

```

### <a name="get-amortized-cost-details"></a>Abrufen amortisierter Kostendaten

Falls Sie für die Ist-Kosten Einkäufe anzeigen möchten, sobald diese anfallen, können Sie den Wert für *metric* in der folgenden Anforderung in `ActualCost` ändern. Für amortisierte Kosten und Ist-Kosten müssen Sie die Version `2019-04-01-preview` verwenden. Die aktuelle API-Version funktioniert – abgesehen vom neuen type/metric-Attribut und geänderten Eigenschaftennamen – genauso wie Version `2019-10-01`. Wenn Sie über eine Microsoft-Kundenvereinbarung verfügen, lauten Ihre Filter im folgenden Beispiel `startDate` und `endDate`.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="retrieve-large-cost-datasets-recurringly-with-exports"></a>Regelmäßiges Abrufen großer Datasets mit Kostendaten mithilfe von Exporten

Das Exportfeature ist eine Lösung, mit der Sie einen regelmäßigen Abruf von Kostendaten planen können. Es handelt sich um die empfohlene Methode zum Abruf nicht aggregierter Kostendaten für Organisationen, deren Nutzungsdateien möglicherweise zu groß sind, um Daten mithilfe der Nutzungsdetails-API zuverlässig abzurufen und herunterzuladen. Die Daten werden in einem Azure Storage-Konto Ihrer Wahl platziert. Von dort können Sie die Daten in Ihre eigenen Systeme laden und nach Bedarf von Ihren Teams analysieren lassen. Informationen zum Konfigurieren von Exporten im Azure-Portal finden Sie unter [Exportieren von Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data).

## <a name="data-latency-and-rate-limits"></a>Datenlatenz und Ratenlimits

Es wird empfohlen, die APIs nicht mehr als einmal pro Tag aufzurufen. Cost Management-Daten werden alle vier Stunden aktualisiert, wenn neue Nutzungsdaten von Azure-Ressourcenanbietern eingehen. Häufigere Aufrufe liefern keine zusätzlichen Daten. Stattdessen wird die Datenlast erhöht. Weitere Informationen dazu, wie oft sich Daten ändern und wie Datenlatenz gehandhabt wird, finden Sie unter [Grundlegendes zu Cost Management-Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/understand-cost-mgt-data).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Fehlercode 429: Aufrufanzahl hat Ratenlimits überschritten

Um allen Cost Management-Abonnenten ein einheitliches Benutzererlebnis zu ermöglichen, gelten für die Cost Management-APIs Ratenlimits. Wenn Sie den Grenzwert erreichen, erhalten Sie den HTTP-Statuscode `429: Too many requests`. Aktuell gelten für unsere APIs die folgenden Durchsatzlimits:

- 30 Aufrufe pro Minute – pro Bereich, pro Benutzer oder Anwendung.
- 200 Aufrufe pro Minute – pro Mandant, pro Benutzer oder Anwendung.

## <a name="next-steps"></a>Nächste Schritte

- [Analysieren der Azure-Kosten mit der Power BI-Vorlagen-App](https://docs.microsoft.com/azure/cost-management-billing/costs/analyze-cost-data-azure-cost-management-power-bi-template-app)
- [Erstellen und Verwalten von exportierten Daten](https://docs.microsoft.com/azure/cost-management-billing/costs/tutorial-export-acm-data) mithilfe von Exporten
- Weitere Informationen zur [Nutzungsdetails-API](https://docs.microsoft.com/rest/api/consumption/usageDetails)