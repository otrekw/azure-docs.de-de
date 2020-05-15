---
title: Transaktionsfunktionen im kommerziellen Marketplace
description: In diesem Artikel werden Überlegungen zur Preisgestaltung, Abrechnung, Rechnungsstellung und Auszahlung für die Transaktionsoption im kommerziellen Marketplace beschrieben.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: dsindona
ms.openlocfilehash: 4aeae69dd50e8c233a1903f6f2c7bd7795b8d7b9
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2020
ms.locfileid: "82857216"
---
# <a name="commercial-marketplace-transact-capabilities"></a>Transaktionsfunktionen im kommerziellen Marketplace

## <a name="transactions-by-publishing-option"></a>Transaktionen nach Veröffentlichungsoption

Für die Verwaltung von Softwarelizenztransaktionen für Angebote im kommerziellen Marketplace ist entweder der Herausgeber oder Microsoft verantwortlich. Wer die Transaktion verwaltet, wird von der Veröffentlichungsoption bestimmt, die Sie für Ihr Angebot auswählen. Informationen zur Verfügbarkeit und Erläuterungen der einzelnen Veröffentlichungsoptionen finden Sie unter [Bestimmen Ihrer Veröffentlichungsoption](./determine-your-listing-type.md#choose-a-publishing-option).

### <a name="list-trial-and-byol-publishing-options"></a>Veröffentlichungsoptionen „Listung“, „Testversion“ und „BYOL“

Herausgeber mit bestehenden E-Commerce-Funktionen können die Veröffentlichungsoptionen „Listung“, „Testversion“ und „BYOL“ (Bring Your Own License) für Werbezwecke und zur Neukundengewinnung auswählen. Bei diesen Optionen ist Microsoft nicht direkt an den Softwarelizenztransaktionen des Herausgebers beteiligt, und es fallen keine damit verbundenen Transaktionsgebühren an. Herausgeber sind für die Unterstützung aller Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich, aber nicht beschränkt auf Auftrag, Auftragsabwicklung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Inkasso. Bei den Veröffentlichungsoptionen „Listung“ und „Testversion“ behalten Herausgeber 100 % der vom Kunden eingezogenen Lizenzgebühren für Software ein.

### <a name="transact-publishing-option"></a>Veröffentlichungsoption „Transaktion“

Die Veröffentlichungsoption „Transaktion“ nutzt die E-Commerce-Funktionen von Microsoft und bietet eine End-to-End-Umgebung von der Ermittlung und Auswertung bis zum Erwerb und zur Implementierung. Transaktionsangebote werden für ein vorhandenes Microsoft-Abonnement oder per Kreditkarte abgerechnet, sodass Microsoft Marketplace-Transaktionen in der Cloud im Auftrag des Herausgebers hosten kann.

Sie wählen die Option „Transaktion“ aus, wenn Sie in Partner Center ein neues Angebot erstellen. Wählen Sie auf der Seite **Angebotseinrichtung** unter **Einrichtungsdetails** die Option „Ja, ich möchte über Microsoft verkaufen, und Microsoft soll Transaktionen in meinem Namen hosten“ aus. Diese Option wird nur angezeigt, wenn die Option „Transaktion“ für Ihren Angebotstyp verfügbar ist.

## <a name="transact-overview"></a>Übersicht über „Transaktion“

Bei Verwendung der Veröffentlichungsoption „Transaktion“ ermöglicht Microsoft den Verkauf von Software von Drittanbietern und die Bereitstellung einiger Angebotstypen für das Azure-Abonnement des Kunden. Sie als Herausgeber müssen bei der Auswahl eines Abrechnungsmodells und Angebotstyps die Abrechnung der Infrastrukturgebühren und Ihrer eigenen Softwarelizenzgebühren berücksichtigen.

Die Veröffentlichungsoption „Transaktion“ wird derzeit für die folgenden Angebotstypen unterstützt:

- Virtuelle Computer
- Azure-Anwendungen
- SaaS-Anwendungen

### <a name="billing-infrastructure-costs"></a>Abrechnung von Infrastrukturkosten

Für **virtuelle Computer und Azure-Anwendungen** werden die Nutzungsgebühren für Azure-Infrastruktur im Azure-Abonnement des Kunden in Rechnung gestellt. Auf der Rechnung des Kunden werden die Infrastrukturnutzungsgebühren separat von den Lizenzgebühren des Softwareanbieters ausgewiesen.

Bei **SaaS-Apps** müssen Sie als Herausgeber die Nutzungsgebühren für Azure-Infrastruktur und Softwarelizenzgebühren als einen Kostenposten verrechnen.  Diese werden dem Kunden als Pauschalgebühr angegeben. Die Nutzung der Azure-Infrastruktur wird verwaltet und dem Partner direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber entscheiden sich in der Regel dafür, die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen. Softwarelizenzgebühren werden nicht gemessen oder nutzungsabhängig berechnet.

## <a name="transact-billing-models"></a>Abrechnungsmodelle für die Veröffentlichungsoption „Transaktion“

Je nach verwendeter Transaktionsoption fallen folgende Softwarelizenzgebühren an:

- **Kostenlos**: Keine Kosten für Softwarelizenzen.
- **Bring-Your-Own-License (BYOL, Verwendung Ihrer eigenen Lizenz)** : Alle anfallenden Gebühren für Softwarelizenzen werden direkt zwischen Herausgeber und Kunde geregelt. Microsoft wickelt nur die Nutzungsgebühren für Azure-Infrastruktur ab. Dies gilt nur für Virtual Machines und Azure-Anwendungen.
- **Nutzungsbasierte Bezahlung**: Softwarelizenzgebühren werden basierend auf der verwendeten Azure-Infrastruktur als Preis pro Stunde und pro Kern (vCPU) ausgewiesen. Dies gilt nur für Virtual Machines und Azure-Anwendungen.
- **Abonnementpreise**: Softwarelizenzgebühren fallen als monatliche oder jährliche wiederkehrende Gebühren an, die als Pauschalgebühr oder pro Arbeitsplatz berechnet werden. Dies gilt nur für SaaS-Apps und verwaltete Azure-Apps.
- **Kostenlose Testversion der Software**: Keine Kosten für Softwarelizenzen für 30 bzw. 90 Tage.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Preise für die Modelle „Kostenlos“ und „BYOL“ (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)

Bei der Veröffentlichung eines kostenlosen oder BYOL- Transaktionsangebots spielt Microsoft keine Rolle bei der Abwicklung des Verkaufsvorgangs für Ihre Softwarelizenzgebühren. Wie bei den Veröffentlichungsoptionen „Listung“ und „Testversion“ behält der Herausgeber 100 % der Softwarelizenzgebühren ein.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Nutzungsbasierte Bezahlung und Abonnementpreis (standortbasiert)

Bei der Veröffentlichung eine Transaktionsangebots mit dem Modell „Nutzungsbasierte Bezahlung“ oder „Abonnement“ stellt Microsoft die Technologie und Dienste zur Verfügung, um Käufe, Rückgaben und Rückbuchungen von Softwarelizenzen zu verarbeiten. In diesem Szenario autorisiert der Herausgeber Microsoft, als Vermittler für diese Zwecke zu fungieren. Der Herausgeber erlaubt Microsoft die Abwicklung der Softwarelizenzierungstransaktion, wobei er seine Kennzeichnung als Verkäufer, Anbieter, Distributor und Lizenzgeber beibehält.

Microsoft ermöglicht es Kunden, Ihre Software zu bestellen, zu lizenzieren und zu nutzen, wobei die Bedingungen des kommerziellen Marketplace von Microsoft und Ihres Endbenutzerlizenzvertrags gelten. Sie müssen Ihren eigenen Endbenutzerlizenzvertrag bereitstellen oder beim Erstellen des Angebots den [Standardvertrag](./standard-contract.md) auswählen.

### <a name="free-software-trials"></a>Kostenlose Testversionen

Bei Veröffentlichungsszenarien des Typs „Transaktion“ können Sie eine Softwarelizenz 30 oder 90 Tage kostenlos zur Verfügung stellen. Diese Rabattierungsmöglichkeit schließt nicht die Kosten für die Nutzung der Azure-Infrastruktur ein, die durch die Nutzung der Partnerlösung verursacht wird.

### <a name="private-offers"></a>Private Angebote

Zusätzlich zur Verwendung von Angebotstypen und Abrechnungsmodellen zur Verrechnung eines Angebots können Sie ein vollständiges privates Angebot mit ausgehandelten, transaktionsspezifischen Preisen oder kundenspezifischen Konfigurationen anbieten. Private Angebote werden von allen drei Veröffentlichungsoptionen des Typs „Transaktion“ unterstützt.

Diese Option ermöglicht höhere oder niedrigere Preise als für das öffentlich verfügbare Angebot. Private Angebote können genutzt werden, um Rabatte zu gewähren oder eine Prämie zu einem Angebot hinzuzufügen. Private Angebote können einem oder mehreren Kunden bereitgestellt werden, indem ihr Azure-Abonnement auf Angebotsebene auf die Zulassungsliste gesetzt wird.

### <a name="examples"></a>Beispiele

**Nutzungsbasierte Bezahlung** 

Die nutzungsbasierte Bezahlung weist die folgende Kostenstruktur auf:

|Ihre Lizenzkosten  | 1,00 USD pro Stunde   |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    |   0,14 USD pro Stunde     |
|*Microsoft führt die Abrechnung mit dem Kunden durch*    |  *1,14 USD pro Stunde*       |
||

In diesem Szenario berechnet Microsoft 1,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

|Berechnung durch Microsoft  | 1,14 USD pro Stunde  |
|---------|---------|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten|   0,80 USD pro Stunde     |
|Microsoft behält 20% Ihrer Lizenzkosten ein  |  0,20 USD pro Stunde       |
|Microsoft behält die Kosten der Azure-Nutzung zu 100 % ein | 0,14 USD pro Stunde |
||

**BYOL (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)**

BYOL weist die folgende Kostenstruktur auf:

|Ihre Lizenzkosten  | Ausgehandelte und von Ihnen in Rechnung gestellte Lizenzgebühr  |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    |   0,14 USD pro Stunde     |
|*Microsoft führt die Abrechnung mit dem Kunden durch*    |  *0,14 USD pro Stunde*       |
||

In diesem Szenario berechnet Microsoft 0,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

|Berechnung durch Microsoft  | 0,14 USD pro Stunde  |
|---------|---------|
|Microsoft behält die Kosten der Azure-Nutzung ein    |   0,14 USD pro Stunde     |
|Microsoft behält 0% Ihrer Lizenzkosten ein   |  0,00 USD pro Stunde       |
||

**SaaS-App-Abonnement**

Diese Option muss für den Verkauf über Microsoft konfiguriert sein und kann per Pauschalpreis oder pro Benutzer monatlich oder jährlich berechnet werden. Wenn Sie für ein SaaS-Angebot die Option **Über Microsoft verkaufen** aktivieren, verfügen Sie über die folgende Kostenstruktur:

| Ihre Lizenzkosten       | 100,00 USD pro Monat  |
|--------------|---------|
| Azure-Nutzungskosten (D1/1-Kern)    | Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden |
| *Microsoft führt die Abrechnung mit dem Kunden durch*    |  *100,00 USD pro Monat (der Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)*  |
||

In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 80,00 USD an den Herausgeber aus.

Für Partner, die für die reduzierte Marketplace-Dienstgebühr qualifiziert sind, wird von Mai 2019 bis Juni 2020 eine reduzierte Transaktionsgebühr für die SaaS-Angebote angezeigt.

In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 90,00 USD an den Herausgeber aus:

|Berechnung durch Microsoft  | 100,00 USD pro Monat  |
|---------|---------|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br> \* Für jede qualifizierte SaaS-App zahlt Microsoft 90 % Ihrer Lizenzkosten.   |   80,00 USD pro Monat <br> \* 90,00 USD pro Monat    |
|Microsoft behält 20% Ihrer Lizenzkosten ein <br> \* Für jede qualifizierte SaaS-App behält Microsoft 10 % Ihrer Lizenzkosten.  |  20,00 USD pro Monat <br> \* 10,00 USD     |

Für bestimmte SaaS-Produkte, die Sie in unserem kommerziellen Marketplace veröffentlichen, reduziert Microsoft die **Marketplace-Dienstgebühr** von 20 % (wie in der Microsoft-Herausgebervereinbarung angegeben) auf 10 %. Damit Ihr Angebot qualifiziert ist, muss mindestens eines Ihrer Angebote von Microsoft als bereit für IP-Co-Selling oder als priorisiert für IP-Co-Selling gekennzeichnet sein. Die Berechtigung muss mindestens fünf Arbeitstage vor dem Ende des vorherigen Kalendermonats bestehen, damit die reduzierte Marketplace-Dienstgebühr für den Monat in Anspruch genommen werden kann. Die reduzierte Marketplace-Dienstgebühr gilt nicht für virtuelle Computer, verwaltete Apps oder andere Produkte, die über den kommerziellen Marketplace zur Verfügung gestellt werden. Diese reduzierte Gebühr ist für qualifizierte Angebote verfügbar, wobei die Lizenzgebühren von Microsoft zwischen dem 1. Mai 2019 und dem 30. Juni 2020 vereinnahmt werden. Nach diesem Zeitpunkt gilt für die Gebühr wieder der normale Betrag.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Rechnungsstellung für den Kunden, Zahlung, Abrechnung und Inkasso

**Rechnungsstellung und Zahlung**: Sie können die vom Kunden bevorzugte Rechnungsstellungsmethode verwenden, um Abonnement- oder nutzungsbasierte Softwarelizenzgebühren zu entrichten.

**Enterprise Agreement**: Wenn die vom Kunden bevorzugte Rechnungsstellungsmethode „Microsoft Enterprise Agreement“ ist, werden Ihre Softwarelizenzgebühren mit dieser Rechnungsstellungsmethode als Postenkosten abgerechnet, und zwar getrennt von allen Azure-spezifischen Nutzungskosten.

**Kreditkarten und monatliche Rechnung**: Kunden können auch per Kreditkarte und monatlicher Rechnung bezahlen. In diesem Fall werden Ihre Softwarelizenzgebühren genau wie beim Enterprise Agreement-Szenario als Postenkosten abgerechnet, und zwar getrennt von allen Azure-spezifischen Nutzungskosten.

**Kostenlose Guthaben und finanzielle Verpflichtung**: Einige Kunden entscheiden sich dafür, Azure mit einer finanziellen Verpflichtung im Enterprise Agreement-Rahmen im Voraus zu bezahlen oder erhalten kostenlose Guthaben für die Nutzung mit Azure. Obwohl diese Guthaben zur Bezahlung der Azure-Nutzung verwendet werden können, dürfen sie nicht zur Zahlung von Softwarelizenzgebühren für Herausgeber verwendet werden.

**Abrechnung und Inkasso**: Die Abrechnung der Softwarelizenz des Herausgebers erfolgt nach der vom Kunden gewählten Methode der Rechnungsstellung und folgt dem Zeitrahmen der Rechnungsstellung. Kunden ohne Enterprise Agreement werden Marketplace-Softwarelizenzen monatlich in Rechnung gestellt. Kunden mit Enterprise Agreement wird monatlich eine Rechnung gestellt, die vierteljährlich ausgefertigt wird.

Bei Auswahl der Preismodelle „Abonnement“ oder „Nutzungsbasierte Bezahlung“ fungiert Microsoft als Vertreter des Herausgebers und ist für alle Aspekte der Abrechnung, Zahlung und des Inkassos verantwortlich.

### <a name="publisher-payout-and-reporting"></a>Auszahlung an den Herausgeber und Berichterstellung

Alle Softwarelizenzgebühren, die von Microsoft als Vertreter erhoben werden, unterliegen einer Transaktionsgebühr von 20 %, sofern nicht anders angegeben, und werden zum Zeitpunkt der Auszahlung an den Herausgeber einbehalten.

Kunden kaufen in der Regel im Rahmen eines Enterprise Agreement oder eines Vertrags mit nutzungsbasierter Bezahlung per Kreditkarte. Der Vertragstyp bestimmt den Zeitpunkt für Abrechnung, Rechnungsstellung, Inkasso und Auszahlung.

>[!NOTE]
>Alle Berichte und Ergebnisse für die Veröffentlichungsoption „Transaktion“ sind in Partner Center im Abschnitt „Analyse“ verfügbar.

#### <a name="billing-questions-and-support"></a>Fragen zu Abrechnung und Support

Weitere Informationen und gesetzliche Richtlinien finden Sie in der [Herausgebervereinbarung](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt) (in Partner Center verfügbar).

Hilfe zu Abrechnungsfragen erhalten Sie beim [Support für Herausgeber im kommerziellen Marketplace](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="transact-requirements"></a>Anforderungen für die Veröffentlichungsoption „Transaktion“

In diesem Abschnitt werden die Transaktionsanforderungen für verschiedene Angebotstypen behandelt.

### <a name="requirements-for-all-offer-types"></a>Anforderungen für alle Angebotstypen

- Für die Veröffentlichungsoption „Transaktion“ sind unabhängig vom Preismodell des Angebots ein Microsoft-Konto und Finanzinformationen erforderlich.
- Obligatorische Finanzinformationen sind beispielsweise ein Auszahlungskonto und ein Steuerprofil.

Weitere Informationen zum Einrichten dieser Konten finden Sie unter [Verwalten Ihres Kontos im kommerziellen Marketplace in Partner Center](https://docs.microsoft.com/azure/marketplace/partner-center-portal/manage-account).

### <a name="requirements-for-specific-offer-types"></a>Anforderungen für bestimmte Angebotstypen

Die Veröffentlichungsoption „Transaktion“ ist nur für die folgenden Typen von Marketplace-Angeboten verfügbar:

- **Virtueller Computer**: Wählen Sie aus den Preismodellen „Kostenlos“, „Bring-Your-Own-License“ oder „Nutzungsbasierte Bezahlung“ aus, und weisen Sie sie als auf Angebotsebene definierte SKUs aus. Auf der Azure-Rechnung des Kunden weist Microsoft die Softwarelizenzgebühren des Herausgebers getrennt von den zugrunde liegenden Azure-Infrastrukturgebühren aus. Azure-Infrastrukturgebühren hängen von der Verwendung der Software des Herausgebers ab.
- **Azure-Anwendungen: Lösungsvorlage oder verwaltete App**: Muss einen oder mehrere virtuelle Computer bereitstellen und zieht sich durch die Summe der Preise für virtuelle Computer. Für verwaltete Apps mit einem Einzelplan kann anstelle der Preise für virtuelle Computer ein pauschales Monatsabonnement als Preismodell gewählt werden. In einigen Fällen werden dem Kunden die Nutzungsgebühren der Azure-Infrastruktur getrennt von den Softwarelizenzgebühren in Rechnung gestellt – aber auf derselben Abrechnung. Wenn Sie allerdings ein Angebot für eine verwaltete App für ISV-Infrastrukturgebühren konfigurieren, werden die Azure-Ressourcen dem Herausgeber berechnet. Für den Kunden wird eine Pauschalgebühr verwendet, die die Kosten für die Infrastruktur, Softwarelizenzen und Verwaltungsdienste abdeckt.

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die erforderlichen Berechtigungen in den Veröffentlichungsoptionen des entsprechenden Angebotstypabschnitts, um Auswahl und Konfiguration Ihres Angebots abzuschließen.
- Entnehmen Sie den Veröffentlichungsmustern der Storefronts Beispiele für die Zuordnung ihrer Lösung zu einem Angebotstyp und einer Konfiguration.
