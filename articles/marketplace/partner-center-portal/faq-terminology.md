---
title: Häufig gestellte Fragen und Terminologie zu Analysen für den kommerziellen Marketplace – Partner Center
description: Hier finden Sie Antworten auf häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace in Partner Center. Dieser Artikel enthält ein Datenwörterbuch für Analyseterminologie.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/17/2020
author: shganesh-dev
ms.author: shganesh
ms.openlocfilehash: aff26d3b491896aa9a1b1fee93d39a5ec93d730c
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91665140"
---
# <a name="commercial-marketplace-analytics-terminology-and-common-questions"></a>Terminologie und häufig gestellte Fragen zu Analysen für den kommerziellen Marketplace

In diesem Artikel werden häufig gestellte Fragen zu Analysenachrichten in Partner Center behandelt. Außerdem wird ein Wörterbuch für Analyseterminologie bereitgestellt.

## <a name="common-questions"></a>Häufig gestellte Fragen

**Ich kann meine Analysedaten in Partner Center nicht anzeigen. Wenn ich auf diese Seiten zugreife, wird die folgende Meldung angezeigt. Warum?**

![Noch keine Daten für Ihre Angebote](./media/analytics-faq-no-data.png)

Diese Meldung kann folgende Ursachen haben:

- Für Ihre im Marketplace veröffentlichten Angebote sind aktuell keine Käufe vorhanden. Dies kann der Fall sein, wenn Ihre Angebote zwar im Marketplace aktiv sind und von Kunden auf den Produktanzeigeseiten betrachtet werden, aber noch nicht gekauft und bereitgestellt wurden.
- Die Veröffentlichung Ihres Angebots ist möglicherweise noch nicht abgeschlossen, und Ihr Angebot wurde noch nicht aktiviert. Nur aktive Angebote können von Kunden erworben werden. Den Status Ihrer Angebote können Sie in der [Übersicht des Dashboards „Analyse“](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary) überprüfen. Weitere Informationen finden Sie unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).
- Ihre Angebote werden möglicherweise als Angebote vom Typ **Kontakt mit mir aufnehmen** gelistet. Hierbei handelt es sich um reine Listenangebote, die von Kunden nicht über den Marketplace erworben werden können. Diese Angebote generieren zwar Leads und werden für Sie freigegeben, da die Angebote aber nicht gekauft werden können, werden auch keine Bestellungen für sie erstellt. Navigieren Sie zur Einrichtungsseite, um die Art Ihrer Angebotslistung zu überprüfen.

**Ich weiß, dass Analysedaten vorhanden sind, aber es wird die folgende Meldung angezeigt:**

![Keine Daten für den angegebenen Datumsbereich](./media/analytics-faq-data-range.png)

Wenn Sie diese Meldung erhalten, bedeutet das, dass Analysedaten vorliegen, aber nicht für den ausgewählten Datumsbereich. Wählen Sie einen anderen oder einen benutzerdefinierten Datumsbereich aus, um beliebige Daten seit 2010 anzuzeigen. Weitere Informationen finden Sie im Abschnitt zu Datumsbereichen unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).

## <a name="dictionary-of-data-terms"></a>Wörterbuch mit Datenbegriffen

| Attributname | Berichte | Definition|
|---|---|---|
| Azure-Lizenztyp | Kunde, Bestellung | Die Art der Lizenzvereinbarung, über die Kunden Azure erwerben. Wird auch als Kanal bezeichnet. |
| Azure-Lizenztyp: Cloud Solution Provider | Kunde, Bestellung | Der Endkunde erwirbt Azure und Ihr Marketplace-Angebot über seinen Cloud Solution Provider, der als Ihr Handelspartner fungiert.|
| Azure-Lizenztyp: Enterprise | Kunde, Bestellung | Der Endkunde erwirbt Azure und Ihr Marketplace-Angebot über ein Enterprise Agreement, das direkt mit Microsoft abgeschlossen wurde.|
| Azure-Lizenztyp: Enterprise über Handelspartner  | Kunde, Bestellung | Der Endkunde erwirbt Azure und Ihr Marketplace-Angebot über einen Handelspartner, der sein Enterprise Agreement über Microsoft bereitstellt.|
| Azure-Lizenztyp: Nutzungsbasierte Bezahlung| Kunde, Bestellung | Der Endkunde erwirbt Azure und Ihr Marketplace-Angebot über eine Vereinbarung mit nutzungsbasierter Bezahlung (Pay As You Go, PAYG), die direkt mit Microsoft abgeschlossen wird.|
| ID des Abrechnungskontos| Kunde, Bestellung, Verbrauch| Der Bezeichner des Kontos, für das die Abrechnung generiert wird. Ordnen Sie die **ID des Abrechnungskontos** zu **customerID** zu, um Ihren Auszahlungstransaktionsbericht mit den Kunden-, Bestellungs- und Verbrauchsberichten zu verbinden.|
| Cloudinstanzname| Order| Die Microsoft Cloud, in der die Bereitstellung eines virtuellen Computers erfolgt ist.|
| Cloudinstanzname: Azure Global| Order| Die öffentliche globale Microsoft-Cloud.|
| Cloudinstanzname: Azure Government | Order| Behördenspezifische Microsoft-Clouds für eines der folgenden Länder: China, Deutschland und USA.| |
| Kundenort| Kunde| Der Name des vom Kunden angegebenen Orts. Der Ort kann sich von der Angabe im Azure-Abonnement eines Kunden unterscheiden.|
| Kommunikationssprache des Kunden  | Kunde| Die vom Kunden für die Kommunikation bevorzugte Sprache.|
| Firmenname des Kunden | Kunde, Bestellung | Der vom Kunden angegebene Firmenname. Der Name kann sich von der Angabe im Azure-Abonnement eines Kunden unterscheiden.|
| Land/Region des Kunden | Kunde, Bestellung | Der vom Kunden angegebene Name für das Land bzw. die Region. Das Land/die Region kann sich von dem im Azure-Abonnement eines Kunden angegebenen Land bzw. der angegebenen Region unterscheiden.|
| E-Mail-Adresse des Kunden| Kunde| Die vom Endkunden angegebene E-Mail-Adresse. Die E-Mail-Adresse kann sich von der im Azure-Abonnement eines Kunden angegebenen E-Mail-Adresse unterscheiden.|
| Vorname des Kunden| Kunde| Der vom Kunden angegebene Name. Der Name kann sich von dem im Azure-Abonnement eines Kunden angegebenen Namen unterscheiden.|
| Customer ID | Kunde, Bestellung | Der eindeutige, einem Kunden zugewiesene Bezeichner. Ein Kunde kann keine oder mehrere Azure Marketplace-Abonnements haben.|
| Postleitzahl des Kunden  | Kunde| Die vom Kunden angegebene Postleitzahl. Die Postleitzahl kann sich von der im Azure-Abonnement eines Kunden angegebenen Postleitzahl unterscheiden.|
| Bundesland/Kanton des Kunden| Kunde| Das vom Kunden angegebene Bundesland oder der angegebene Kanton. Das Bundesland bzw. der Kanton kann sich von der Angabe im Azure-Abonnement eines Kunden unterscheiden.|
| Erwerbsdatum| Kunde| Das erste Datum, an dem der Kunde ein von Ihnen veröffentlichtes Angebot erworben hat.|
| Verlustdatum| Kunde| Das letzte Datum, an dem der Kunde das letzte aller zuvor erworbenen Angebote storniert hat.|
| Neuer Kunde  | Order| Der Wert identifiziert einen neuen Kunden, der zum ersten Mal eines oder mehrere Ihrer Angebote erwirbt (oder nicht). Der Wert ist „Ja“, wenn das Erwerbsdatum im gleichen Kalendermonat liegt. Der Wert ist „Nein“, wenn der Kunde eines Ihrer Angebote vor dem berichteten Kalendermonat gekauft hat. |
| Vorschau-SKU| Order| Der Wert gibt an, ob Sie die SKU als „Vorschau“ gekennzeichnet haben. Der Wert ist „Ja“, wenn Sie die SKU entsprechend gekennzeichnet haben und nur von Ihnen autorisierte Azure-Abonnements dieses Image bereitstellen und nutzen können. Der Wert ist „Nein“, wenn die SKU nicht als „Vorschau“ definiert wurde. |
| Zustimmung zur Kontaktaufnahme zu Werbezwecken| Kunde| Der Wert gibt an, ob der Kunde proaktiv der Kontaktaufnahme zu Werbezwecken durch die Herausgeber zugestimmt hat. Derzeit wird diese Option für Kunden nicht angezeigt. Daher ist allgemein „Nein“ angegeben. Sobald diese Funktion bereitgestellt wird, wird eine entsprechende Aktualisierung vorgenommen.|
| Marketplace-Lizenztyp| Order| Die Abrechnungsmethode für das Marketplace-Angebot.|
| Marketplace-Lizenztyp: Abrechnung über Azure| Order| Microsoft ist Ihr Vertreter für dieses Marketplace-Angebot und erstellt Abrechnungen für Kunden in Ihrem Auftrag. (Entweder nutzungsbasierte Bezahlung per Kreditkarte oder Firmenrechnung)|
| Marketplace-Lizenztyp: BYOL (Bring Your Own License) | Order| Für die Bereitstellung des virtuellen Computers ist ein vom Kunden bereitgestellter Lizenzschlüssel erforderlich. Microsoft stellt Kunden diese Art von Angebotslistings über den Marketplace nicht in Rechnung.|
| Marketplace-Lizenztyp: Kostenlos| Order| Das Angebot ist für alle Benutzer als kostenlos konfiguriert. Microsoft stellt Kunden die Nutzung dieses Angebots nicht in Rechnung.|
| Marketplace-Lizenztyp: Microsoft als Handelspartner  | Order| Microsoft ist Ihr Handelspartner für dieses Marketplace-Angebot.|
| Marketplace-Abonnement-ID | Kunde, Bestellung | Der eindeutige Bezeichner (ID), der mit dem Azure-Abonnement verknüpft ist, über das der Kunde Ihr Marketplace-Angebot erworben hat. Früher war ID die Azure-Abonnement-GUID.|
| Angebotsname  | Order| Der Name des Marketplace-Angebots.|
| Angebotstyp  | Order| Der Typ des Microsoft Marketplace-Angebots.|
| Angebotstyp: Verwaltete Anwendung  | Order | Verwenden der Azure-App: Angebotstyp „Verwaltete App“, wenn die folgenden Bedingungen erforderlich sind: Sie stellen entweder eine Abonnement-basierte Lösung für Ihren Kunden mit einer VM oder eine vollständig IaaS-basierte Lösung bereit. Sie oder Ihr Kunde setzen voraus, dass die Lösung von einem Partner verwaltet wird. |
| Angebotstyp: Azure-Anwendung| Order | Verwenden Sie eine Lösungsvorlage für Azure-Anwendungen, wenn für eine Lösung neben der einfachen VM noch eine zusätzliche Bereitstellung und Konfigurationsautomatisierung erforderlich ist.|
| Angebotstyp: Beratungsdienst| Order| Beratungsdienste in Azure Marketplace ermöglichen Verbindungen zwischen Kunden und Diensten herzustellen, um ihre Nutzung von Azure zu unterstützen und auszuweiten.|
| Angebotstyp: Container | Order| Verwenden Sie den Angebotstyp „Container“, wenn Ihre Lösung ein Docker-Container-Image ist, das als Kubernetes-basierter Azure Container Service bereitgestellt wird.|
| Angebotstyp: Dynamics 365 Business Central| Order| Verwenden Sie diesen Angebotstyp, wenn Ihre Lösung in Dynamics 365 for Finance and Operations integriert ist|
| Angebotstyp: Dynamics 365 for Customer Engagement | Order| Verwenden Sie diesen Angebotstyp, wenn Ihre Lösung in Dynamics 365 for Customer Engagement integriert ist.|
| Angebotstyp: IoT Edge-Modul | Order| Azure IoT Edge-Module sind die kleinste von IoT Edge verwaltete Recheneinheit. Sie können Microsoft-Dienste (wie Azure Stream Analytics), Dienste von Drittanbietern oder Ihren eigenen lösungsspezifischen Code enthalten. |
| Angebotstyp: Power BI-Anwendung | Order| Verwenden Sie den Angebotstyp „Power BI-Anwendung“, wenn Sie eine Anwendung bereitstellen, die in Power BI integriert ist.|
| Angebotstyp: SaaS-Anwendung| Order| Verwenden Sie die SaaS-App-Angebotstypen, um Ihren Kunden die Möglichkeit zu geben, Ihre SaaS-basierte, technische Lösung als Abonnement zu kaufen.||
| Angebotstyp: Virtual Machine | Order | Verwenden Sie den VM-Angebotstyp beim Bereitstellen einer virtuellen Appliance für das Abonnement Ihres Kunden.|
| Angebotstyp: Visual Studio Marketplace-Erweiterung  | Order| Angebotstyp, der zuvor für Entwickler von Azure DevOps-Erweiterungen verfügbar war. Entwickler von Azure DevOps-Erweiterungen können ihre Erweiterungen ab jetzt direkt an Kunden verkaufen. Erweiterungsangebote können als kostenpflichtig oder einschließlich einer Testversion konfiguriert werden. |
| Stornierungsdatum der Bestellung| Order| Das Datum, an dem die Marketplace-Bestellung storniert wurde.|
| Bestell-ID| Order| Der eindeutige Bezeichner (ID) der Kundenbestellung für Ihren Marketplace-Dienst. Nutzungsbasierte Angebote für virtuelle Computer werden keiner Bestellung zugeordnet.|
| Kaufdatum der Bestellung| Order| Das Datum, an dem die Marketplace-Bestellung erstellt wurde.|
| Bestellstatus| Order| Der Status einer Marketplace-Bestellung zu dem Zeitpunkt, zu dem die Daten zuletzt aktualisiert wurden.|
| Bestellstatus: Aktiv  | Order| Der Kunde hat einen Auftrag erteilt und die Bestellung nicht storniert.|
| Bestellstatus: storniert | Order| Der Kunde hat zuvor einen Auftrag erteilt und anschließend die Bestellung storniert.|
| Anbieter-E-Mail| Kunde| Die E-Mail-Adresse des Anbieters, der an der Beziehung zwischen Microsoft und dem Endkunden beteiligt ist. Beim Lizenztyp „Enterprise über Handelspartner“ ist dies der Handelspartner. Wenn ein Cloud Solution Provider (CSP) beteiligt ist, wird hier der CSP angegeben.|
| Anbietername| Kunde| Der Name des Anbieters, der an der Beziehung zwischen Microsoft und dem Endkunden beteiligt ist. Beim Lizenztyp „Enterprise über Handelspartner“ ist dies der Handelspartner. Wenn ein Cloud Solution Provider (CSP) beteiligt ist, wird hier der CSP angegeben.|
| SKU| Order| Der bei der Veröffentlichung definierte Name der SKU (jetzt als „Plan“ bezeichnet). Ein Angebot kann mehrere SKUs (Pläne) enthalten, aber jede SKU kann nur einem einzigen Angebot zugeordnet werden.|
| Enddatum der Testversion| Order| Das Datum, an dem der Testzeitraum für die Bestellung endet oder beendet ist.|
|||

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht über die im kommerziellen Marketplace im Partner Center verfügbaren Analyseberichte finden Sie unter [Analysen für den kommerziellen Marketplace im Partner Center](./analytics.md).
- Informationen zu Diagrammen, Trends und Werten von aggregierten Daten, mit deren Hilfe Marketplace-Aktivitäten für Ihr Angebot zusammengefasst werden, finden Sie unter [Dashboard „Zusammenfassung“ in Analysen für den kommerziellen Marketplace](./summary-dashboard.md).
- Informationen zu Ihren Aufträgen in einem grafischen und herunterladbaren Format finden Sie unter [Dashboard „Aufträge“ in Analysen für den kommerziellen Marketplace](./orders-dashboard.md).
- Informationen zu Metriken zur Nutzung und zur getakteten Abrechnung für VM-Angebote finden Sie unter [Dashboard „Nutzung“ in Analysen für den kommerziellen Marketplace](./usage-dashboard.md).
- Ausführliche Informationen zu Ihren Kunden, einschließlich Wachstumstrends, finden Sie unter [Dashboard „Kunde“ in Analysen für den kommerziellen Marketplace](./customer-dashboard.md).
- Eine Liste Ihrer Downloadanforderungen in den letzten 30 Tagen finden Sie unter [Dashboard „Downloads“ in Analysen für den kommerziellen Marketplace](./downloads-dashboard.md).
- Eine konsolidierte Ansicht des Kundenfeedbacks für Angebote in Azure Marketplace und AppSource finden Sie unter [Dashboard „Bewertungen und Prüfungen“ in Analysen für den kommerziellen Marketplace](./ratings-reviews.md).
