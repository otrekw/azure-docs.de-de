---
title: Transaktionsfunktionen im kommerziellen Microsoft Marketplace
description: In diesem Artikel werden Überlegungen zur Preisgestaltung, Abrechnung, Rechnungsstellung und Auszahlung für die Transaktionsoption im kommerziellen Marketplace beschrieben.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2021
ms.author: mingshen
author: mingshen-ms
ms.openlocfilehash: b17041f25fdd26bd0f8328a206267ef1c4984672
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108144743"
---
# <a name="commercial-marketplace-transact-capabilities"></a>Transaktionsfunktionen im kommerziellen Marketplace

In diesem Artikel werden Überlegungen zur Preisgestaltung, Abrechnung, Rechnungsstellung und Auszahlung im kommerziellen Microsoft-Marketplace beschrieben.

## <a name="transactions-by-listing-option"></a>Transaktionen nach Auflistungsoption

Für die Verwaltung von Softwarelizenztransaktionen für Angebote im kommerziellen Marketplace ist entweder der Herausgeber oder Microsoft verantwortlich. Wer die Transaktion verwaltet, wird durch die Auflistungsoption bestimmt, die Sie für Ihr Angebot auswählen. Informationen zur Verfügbarkeit und Erläuterungen der einzelnen Veröffentlichungsoptionen finden Sie unter [Einführung in Auflistungsoptionen](determine-your-listing-type.md).

### <a name="contact-me-free-trial-and-byol-options"></a>Optionen „Kontaktaufnahme“, „Kostenlose Testversion“ und „BYOL“

Herausgeber können die Optionen _Kontaktaufnahme_ und _Kostenlose Testversion_ für Werbezwecke und zur Neukundengewinnung auswählen. Bei manchen Angebotstypen können Sie als Herausgeber die Option „BYOL“ (_Bring Your Own License_) auswählen, damit Kunden mit einer Lizenz, die sie direkt bei Ihnen gekauft haben, ein Abonnement für Ihr Angebot erwerben können. Bei diesen Optionen ist Microsoft nicht direkt an den Softwarelizenztransaktionen des Herausgebers beteiligt, und es fallen keine damit verbundenen Transaktionsgebühren an. Der gesamte Umsatz geht somit an den Herausgeber.

Bei diesen Optionen sind die Herausgeber für sämtliche Aspekte der Softwarelizenztransaktion verantwortlich. Dazu zählen u. a. Auftrag, Erfüllung, Messung, Abrechnung, Rechnungstellung, Zahlung und Erfassung. Bei der Auflistungsoption „Kontaktaufnahme“ behalten Herausgeber 100 % der vom Kunden eingezogenen Softwarelizenzgebühren ein.

### <a name="transact-publishing-option"></a>Veröffentlichungsoption „Transaktion“

Wenn Sie sich dafür entscheiden, über Microsoft zu verkaufen, profitieren Sie von den E-Commerce-Funktionen von Microsoft und nutzen die End-to-End-Umgebung für Ermittlung und Auswertung bis hin zu Kauf und Implementierung. Bei einem _transaktionsfähigen_ Angebot handelt es sich um ein Angebot, für das Microsoft Geldtransaktionen für eine Softwarelizenz im Namen des Herausgebers durchführt. Transaktionsangebote werden über ein vorhandenes Microsoft-Abonnement oder über eine Kreditkarte abgerechnet, sodass Microsoft Marketplace-Transaktionen in der Cloud im Auftrag des Herausgebers hosten kann.

Sie wählen die Option „Transaktion“ aus, wenn Sie in Partner Center ein neues Angebot erstellen. Diese Option wird nur angezeigt, wenn die Option „Transaktion“ für Ihren Angebotstyp verfügbar ist.

## <a name="transact-overview"></a>Übersicht über „Transaktion“

Bei Verwendung der Option „Transaktion“ ermöglicht Microsoft den Verkauf von Drittanbietersoftware und die Bereitstellung einiger Angebotstypen für das Azure-Abonnement des Kunden. Der Herausgeber muss bei der Wahl eines Abrechnungsmodells für ein Angebot die Abrechnung der Infrastrukturgebühren und Ihrer eigenen Softwarelizenzgebühren berücksichtigen.

Die Veröffentlichungsoption „Transaktion“ wird derzeit für die folgenden Angebotstypen unterstützt:

| Angebotstyp | Abrechnungsintervall | Getaktete Abrechnung | Preismodell |
| ------------ | ------------- | ------------- | ------------- |
| Azure-Anwendung<br>(Verwaltete Anwendung) | Monatlich | Ja | Nutzungsbasiert |
| Virtueller Azure-Computer | Monatlich* | Nein | Nutzungsbasiert, BYOL |
| Software-as-a-Service (SaaS) | Monatlich und jährlich | Ja | Pauschalpreis pro Benutzer, nutzungsbasiert |
|||||

`*` Bei Angeboten für virtuelle Azure-Computer werden nutzungsbasierte Abrechnungspläne unterstützt. Diese Pläne werden monatlich stundengenau auf der Grundlage der Nutzung des Abonnements abgerechnet (pro Kern, pro Kerngröße oder pro Markt und Kerngröße).

### <a name="metered-billing"></a>Getaktete Abrechnung

Dank des _Marketplace-Messungsdiensts_ können Sie zusätzlich zu den im Vertrag (Berechtigung) enthaltenen Monats- oder Jahresgebühren auch Gebühren für die nutzungsbasierte Bezahlung (verbrauchsbasiert) angeben. Nutzungskosten können für von Ihnen angegebene Dimensionen des Marketplace-Messungsdiensts berechnet werden. Beispiele wären etwa Bandbreite, Tickets oder verarbeitete E-Mails. Weitere Informationen zur getakteten Abrechnung für SaaS-Angebote finden Sie unter [Volumenabrechnung für SaaS mit dem Messungsdienst für den kommerziellen Marketplace](./partner-center-portal/saas-metered-billing.md). Weitere Informationen zur getakteten Abrechnung für Azure-Anwendungsangebote finden Sie unter [Getaktete Abrechnung für verwaltete Anwendungen](./partner-center-portal/azure-app-metered-billing.md).

### <a name="billing-infrastructure-costs"></a>Abrechnung von Infrastrukturkosten

Für **virtuelle Computer** und **Azure-Anwendungen** werden die Nutzungsgebühren für Azure-Infrastruktur im Azure-Abonnement des Kunden in Rechnung gestellt. Auf der Rechnung des Kunden werden die Infrastrukturnutzungsgebühren separat von den Lizenzgebühren des Softwareanbieters ausgewiesen.

Bei **SaaS-Apps** muss der Herausgeber die Nutzungsgebühren für die Azure-Infrastruktur sowie Softwarelizenzgebühren als einzelnen Posten abrechnen. Diese werden dem Kunden als Pauschalgebühr angegeben. Die Nutzung der Azure-Infrastruktur wird verwaltet und dem Herausgeber direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber entscheiden sich in der Regel dafür, die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen. Softwarelizenzgebühren werden nicht gemessen oder nutzungsabhängig ermittelt.

## <a name="pricing-models"></a>Preismodelle

Je nach verwendeter Transaktionsoption fallen folgende Abonnementgebühren an:

- **Jetzt herunterladen (kostenlos)** : Keine Softwarelizenzgebühren. Kostenlose Angebote können nicht in ein kostenpflichtiges Angebot umgewandelt werden. Der Kunde muss ein kostenpflichtiges Angebot bestellen.
- **Bring Your Own License** (BYOL, Verwendung Ihrer eigenen Lizenz): Wenn ein Angebot im kommerziellen Marketplace gelistet ist, werden alle anfallenden Gebühren für Softwarelizenzen direkt zwischen Herausgeber und Kunde geregelt. Von Microsoft werden nur die für die Azure-Infrastruktur anfallenden Nutzungsgebühren über das Azure-Abonnementkonto des Kunden abgerechnet.
- **Abonnementpreise**: Softwarelizenzgebühren werden als laufende Abonnementgebühr ausgewiesen, die monatlich oder jährlich fällig ist und als Pauschale oder pro Arbeitsplatz berechnet wird. Bei Kündigungen durch den Kunden in der Mitte der Laufzeit bzw. bei nicht genutzten Diensten erfolgt keine anteilsmäßige Erstattung von laufenden Abonnementgebühren. Laufende Abonnementgebühren können angerechnet werden, wenn der Kunde das Abonnement in der Mitte der Abonnementlaufzeit aktualisiert oder herabstuft.
- **Nutzungsbasierte Preisgestaltung**: Bei Angeboten für virtuelle Azure-Computer werden dem Kunden nutzungsabhängige Gebühren für das Angebot berechnet. Bei VM-Images wird dem Kunden ein Azure Marketplace-Stundensatz in Rechnung gestellt, der vom Herausgeber festgelegt und für die Nutzung der virtuellen Computer erhoben wird, die auf der Grundlage der VM-Images bereitgestellt wurden. Die Stundengebühr kann einheitlich sein oder je nach VM-Größe variieren. Angefangene Stunden werden pro Minute abgerechnet. Pläne werden monatlich abgerechnet.
- **Getaktetes Preismodell**: Bei Azure-Anwendungsangeboten und SaaS-Angeboten können Herausgeber den [Marketplace-Messungsdienst](./partner-center-portal/marketplace-metering-service-apis.md) verwenden, um die Nutzung auf der Grundlage der von ihnen konfigurierten benutzerdefinierten Verbrauchseinheitsdimensionen zu berechnen. Diese Gebühren fallen zusätzlich zu den im Vertrag (Berechtigung) enthaltenen Monats- oder Jahresgebühren an. Beispiele für benutzerdefinierte Verbrauchseinheitsdimensionen wären etwa Bandbreite, Tickets oder verarbeitete E-Mails. Herausgeber können für jeden Plan eine oder mehrere getaktete Dimensionen definieren. Pro Angebot sind jedoch maximal 30 Dimensionen möglich. Herausgeber sind dafür verantwortlich, die Nutzung der einzelnen Kunden mit jeder im Angebot definierten Verbrauchseinheit nachzuverfolgen. Ereignisse sollten innerhalb einer Stunde an Microsoft gemeldet werden. Microsoft erstellt die Kundenrechnungen auf Basis der von den Herausgebern für den jeweiligen Abrechnungszeitraum gemeldeten Nutzungsinformationen.
- **Kostenlose Testversion**: Für Softwarelizenzen, die zwischen 30 Tagen und sechs Monaten gültig sind, werden je nach Angebotstyp keine Kosten berechnet. Wenn Herausgeber eine kostenlose Testversion für mehrere Pläne innerhalb des gleichen Angebots bereitstellen, kann der Kunde zwar zu einer kostenlosen Testversion eines anderen Plans wechseln, der Testzeitraum wird jedoch nicht neu gestartet. Bei Angeboten für virtuelle Computer werden dem Kunden die Azure-Infrastrukturkosten für die Nutzung des Angebots während eines Testzeitraums in Rechnung gestellt. Nach Ablauf des Testzeitraums wird dem Kunden automatisch der Standardtarif für den Plan in Rechnung gestellt, für den er zuletzt die Testversion verwendet hat, sofern er nicht vor dem Ende des Testzeitraums gekündigt hat.

> [!NOTE]
> Angebote, die nach Verbrauch abgerechnet werden, sind nach Verwendung der Lösung nicht erstattungsfähig.

Wenn ein Herausgeber die einem Angebot zugeordneten Nutzungsgebühren ändern möchte, sollte er zunächst das Angebot (oder den jeweiligen Plan innerhalb des Angebots) aus dem kommerziellen Marketplace entfernen. Das Entfernen sollte in Übereinstimmung mit den Anforderungen der [Microsoft-Herausgebervereinbarung](/legal/marketplace/msft-publisher-agreement) erfolgen. Anschließend kann der Herausgeber ein neues Angebot (oder einen Plan innerhalb des Angebots) mit den neuen Nutzungsgebühren veröffentlichen. Weitere Informationen zum Entfernen eines Angebots oder Plans finden Sie unter [Einstellen des Verkaufs eines Angebots oder Plans](./partner-center-portal/update-existing-offer.md#stop-selling-an-offer-or-plan).

### <a name="free-contact-me-and-bring-your-own-license-byol-pricing"></a>Preisgestaltung für die Optionen „Kostenlos“, „Kontaktaufnahme“ und „BYOL“ (Bring-Your-Own-License)

Bei der Veröffentlichung eines Angebots mit der Option „Jetzt herunterladen (kostenlos)“, „Kontaktaufnahme“ oder „BYOL“ spielt Microsoft keine Rolle bei der Abwicklung der Verkaufstransaktion für Ihre Softwarelizenzgebühren. Der Herausgeber behält 100 Prozent der Softwarelizenzgebühren.

### <a name="usage-based-and-subscription-pricing"></a>Nutzungsbasierte Preisgestaltung und Abonnementpreise

Bei Veröffentlichung eines Angebots als nutzungs- oder abonnementbasierte Transaktion stellt Microsoft die Technologie und Dienste zum Verarbeiten von Käufen, Rückgaben und Rückbuchungen von Softwarelizenzen zur Verfügung. In diesem Szenario autorisiert der Herausgeber Microsoft, als Vermittler für diese Zwecke zu fungieren. Der Herausgeber erlaubt Microsoft die Abwicklung der Softwarelizenzierungstransaktion. Dabei behält der Herausgeber die Rolle als Verkäufer, Anbieter, Distributor und Lizenzgeber.

Microsoft ermöglicht es Kunden, Ihre Software zu bestellen, zu lizenzieren und zu nutzen, wobei die Bedingungen des kommerziellen Marketplace von Microsoft und Ihres Endbenutzerlizenzvertrags gelten. Sie müssen entweder Ihren eigenen Endbenutzerlizenzvertrag bereitstellen oder beim Erstellen des Angebots den [Standardvertrag](./standard-contract.md) auswählen.

### <a name="free-software-trials"></a>Kostenlose Testversionen

Bei Veröffentlichungsszenarien des Typs „Transaktion“ können Sie je nach Abonnement eine Softwarelizenz zwischen 30 und 120 Tagen kostenlos zur Verfügung stellen. Kunden werden anfallende Nutzungsgebühren für die Azure-Infrastruktur in Rechnung gestellt.

### <a name="examples-of-pricing-and-store-fees"></a>Beispiele für Preise und Store-Gebühren

**Nutzungsbasiert**

Die nutzungsbasierte Preisgestaltung weist folgende Kostenstruktur auf:

| **Ihre Lizenzkosten** | **1,00 USD pro Stunde** |
|---------|---------|
| Azure-Nutzungskosten (D1/1-Kern) | 0,14 USD pro Stunde |
| _Microsoft führt die Abrechnung mit dem Kunden durch_ | _1,14 USD pro Stunde_ |
||

In diesem Szenario berechnet Microsoft 1,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

| **Microsoft berechnet** | **1,14 USD pro Stunde**  |
|---------|---------|
| Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten | 0,80 USD pro Stunde |
| Microsoft behält 20% Ihrer Lizenzkosten ein  |  0,20 USD pro Stunde |
| Microsoft behält die Kosten der Azure-Nutzung zu 100 % ein | 0,14 USD pro Stunde |
||

**BYOL (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)**

BYOL weist die folgende Kostenstruktur auf:

| **Ihre Lizenzkosten** | **Ausgehandelte und von Ihnen in Rechnung gestellte Lizenzgebühr** |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    |   0,14 USD pro Stunde     |
| _Microsoft führt die Abrechnung mit dem Kunden durch_ | _0,14 USD pro Stunde_ |
||

In diesem Szenario berechnet Microsoft 0,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

| **Microsoft berechnet** | **0,14 USD pro Stunde** |
|---------|---------|
| Microsoft behält die Kosten der Azure-Nutzung ein | 0,14 USD pro Stunde |
| Microsoft behält 0% Ihrer Lizenzkosten ein | 0,00 USD pro Stunde |
||

**SaaS-App-Abonnement**

Für SaaS-Abonnements kann eine Pauschalgebühr oder eine Monats- oder Jahresgebühr pro Benutzer festgelegt werden. Wenn Sie für ein SaaS-Angebot die Option **Über Microsoft verkaufen** aktivieren, ergibt sich folgende Kostenstruktur:

| **Ihre Lizenzkosten** | **100,00 USD pro Monat** |
|--------------|---------|
| Azure-Nutzungskosten (D1/1-Kern) | Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden |
| _Microsoft führt die Abrechnung mit dem Kunden durch_ | _100,00 USD pro Monat (der Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)_ |
||

In diesem Szenario stellt Microsoft 100.00 USD für Ihre Softwarelizenz in Rechnung und zahlt 80.00 USD oder 90.00 USD an Sie aus (je nachdem, ob das Angebot für eine reduzierte Store-Dienstgebühr qualifiziert ist).

| **Microsoft berechnet** | **100,00 USD pro Monat** |
|---------|---------|
| Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br> \* Für jede qualifizierte SaaS-App zahlt Microsoft 90 % Ihrer Lizenzkosten. | 80,00 USD pro Monat <br> \* 90,00 USD pro Monat |
| Microsoft behält 20% Ihrer Lizenzkosten ein <br> \* Für jede qualifizierte SaaS-App behält Microsoft 10 % Ihrer Lizenzkosten. | 20,00 USD pro Monat <br> \* 10,00 USD |

### <a name="commercial-marketplace-service-fees"></a>Dienstgebühren im kommerziellen Marketplace

Wir berechnen eine Store-Standarddienstgebühr von 20 %, wenn Kunden Ihr Transaktionsangebot über den kommerziellen Marketplace erwerben. Ausführliche Informationen zu dieser Gebühr finden Sie in Abschnitt 5c der [Microsoft-Herausgebervereinbarung](/legal/marketplace/msft-publisher-agreement).

Bestimmte Transaktionsangebote, die Sie im kommerziellen Marketplace veröffentlichen, sind ggf. für eine reduzierte Store-Dienstgebühr von zehn Prozent qualifiziert. Ein entsprechend qualifiziertes Angebot muss von Microsoft als _Azure IP Co-Sell Incentivized_ gekennzeichnet worden sein. Die Anforderungen für die Berechtigung müssen mindestens fünf Arbeitstage vor dem Ende eines jeden Kalendermonats erfüllt sein, damit die reduzierte Marketplace-Dienstgebühr in Anspruch genommen werden kann. Sobald die Anforderungen für die Berechtigung erfüllt sind, wird die reduzierte Dienstgebühr für alle Transaktionen ab dem ersten Tag des Folgemonats gewährt, bis der Status _Azure IP Co-Sell Incentivized_ nicht mehr gegeben ist. Einzelheiten zur Berechtigung für das IP-Co-Selling finden Sie unter [Requirements for Co-sell Status](/legal/marketplace/certification-policies#3000-requirements-for-co-sell-status) (Anforderungen für den Co-Selling-Status).

Die reduzierte Marketplace-Dienstgebühr gilt für SaaS, VMs, verwaltete Apps und andere berechtigte transaktionsfähige IaaS-Lösungen im Rahmen der Azure IP-Co-Selling-Incentives, die über den kommerziellen Marketplace bereitgestellt werden. Kostenpflichtige SaaS-Angebote, die einer Microsoft Teams-App oder mindestens zwei Microsoft 365-Add-Ins (wie Excel, PowerPoint, Word, Outlook und SharePoint) zugeordnet sind und in Microsoft AppSource veröffentlicht werden, können ebenfalls für diesen Rabatt qualifiziert sein.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Rechnungsstellung für den Kunden, Zahlung, Abrechnung und Inkasso

**Rechnungsstellung und Zahlung**: Sie können die vom Kunden bevorzugte Rechnungsstellungsmethode verwenden, um abonnement- oder [nutzungsbasierte](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) Softwarelizenzgebühren zu entrichten.

**Enterprise Agreement**: Wenn die vom Kunden bevorzugte Rechnungsstellungsmethode „Microsoft Enterprise Agreement“ ist, werden Ihre Softwarelizenzgebühren mit dieser Rechnungsstellungsmethode als aufgeschlüsselte Kosten abgerechnet, und zwar getrennt von allen Azure-spezifischen Nutzungskosten.

**Kreditkarten und monatliche Rechnung**: Kunden können per Kreditkarte und monatlicher Rechnung bezahlen. In diesem Fall werden Ihre Softwarelizenzgebühren genau wie beim Enterprise Agreement-Szenario als Postenkosten abgerechnet, und zwar getrennt von allen Azure-spezifischen Nutzungskosten.

**Kostenlose Guthaben und finanzielle Verpflichtung**: Manche Kunden entscheiden sich dafür, Azure im Enterprise Agreement-Rahmen mit einer finanziellen Verpflichtung im Voraus zu bezahlen oder erhalten kostenloses Guthaben für die Azure-Nutzung. Obwohl diese Guthaben zur Bezahlung der Azure-Nutzung verwendet werden können, dürfen sie nicht zur Zahlung von Softwarelizenzgebühren für Herausgeber verwendet werden.

**Abrechnung und Inkasso**: Die Abrechnung der Softwarelizenz des Herausgebers erfolgt nach der vom Kunden gewählten Methode der Rechnungsstellung und folgt dem Zeitrahmen der Rechnungsstellung. Kunden ohne Enterprise Agreement werden Marketplace-Softwarelizenzen monatlich in Rechnung gestellt. Kunden mit Enterprise Agreement wird monatlich eine Rechnung gestellt, die vierteljährlich ausgefertigt wird.

Bei Auswahl der Preismodelle „Abonnement“ oder „Nutzungsbasierte Bezahlung“ fungiert Microsoft als Vertreter des Herausgebers und ist für alle Aspekte der Abrechnung, Zahlung und des Inkassos verantwortlich.

### <a name="publisher-payout-and-reporting"></a>Auszahlung an den Herausgeber und Berichterstellung

Alle Softwarelizenzgebühren, die von Microsoft als Vertreter erhoben werden, unterliegen einer Transaktionsgebühr von 20 %, sofern nicht anders angegeben, und werden zum Zeitpunkt der Auszahlung an den Herausgeber einbehalten.

Kunden kaufen in der Regel im Rahmen eines Enterprise Agreement oder eines Vertrags mit nutzungsbasierter Bezahlung per Kreditkarte. Der Vertragstyp bestimmt den Zeitpunkt für Abrechnung, Rechnungsstellung, Inkasso und Auszahlung.

>[!NOTE]
>Alle Berichte und Ergebnisse für die Veröffentlichungsoption „Transaktion“ sind in Partner Center im Abschnitt „Analyse“ verfügbar.

#### <a name="billing-questions-and-support"></a>Fragen zu Abrechnung und Support

Weitere Informationen und gesetzliche Richtlinien finden Sie in der [Microsoft-Herausgebervereinbarung](/legal/marketplace/msft-publisher-agreement). Hilfe bei Abrechnungsfragen erhalten Sie vom [Support für Herausgeber im kommerziellen Marketplace](https://aka.ms/marketplacepublishersupport).

## <a name="transact-requirements"></a>Anforderungen für die Veröffentlichungsoption „Transaktion“

In diesem Abschnitt werden die Transaktionsanforderungen für verschiedene Angebotstypen behandelt.

### <a name="requirements-for-all-offer-types"></a>Anforderungen für alle Angebotstypen

- Für die Veröffentlichungsoption „Transaktion“ sind unabhängig vom Preismodell des Angebots ein Microsoft-Konto und Finanzinformationen erforderlich.
- Obligatorische Finanzinformationen sind beispielsweise ein Auszahlungskonto und ein Steuerprofil.

Weitere Informationen zum Einrichten dieser Konten finden Sie unter [Verwalten Ihres Kontos im kommerziellen Marketplace in Partner Center](manage-account.md).

### <a name="requirements-for-specific-offer-types"></a>Anforderungen für bestimmte Angebotstypen

Die Möglichkeit, Transaktionen über Microsoft abzuwickeln, steht nur für folgende Angebotstypen des kommerziellen Marketplace zur Verfügung. Diese Liste gibt Aufschluss über die Anforderungen, die erfüllt werden müssen, um die Angebotstypen im kommerziellen Marketplace transaktionsfähig zu machen.

- **Azure-Anwendung (Lösungsvorlagenpläne oder Pläne für verwaltete Anwendungen)** : In einigen Fällen werden dem Kunden die Nutzungsgebühren der Azure-Infrastruktur getrennt von den Softwarelizenzgebühren, aber auf der gleichen Abrechnung in Rechnung gestellt. Wenn Sie allerdings einen Plan für eine verwaltete App für ISV-Infrastrukturgebühren konfigurieren, werden die Azure-Ressourcen dem Herausgeber in Rechnung gestellt. Für den Kunden wird eine Pauschalgebühr erhoben, die die Kosten für die Infrastruktur, Softwarelizenzen und Verwaltungsdienste abdeckt.

- **Virtueller Azure-Computer**: Wählen Sie ein kostenloses, BYOL-basiertes oder nutzungsbasierten Preismodell aus. Auf der Azure-Rechnung des Kunden weist Microsoft die Softwarelizenzgebühren des Herausgebers getrennt von den zugrunde liegenden Azure-Infrastrukturgebühren aus. Azure-Infrastrukturgebühren hängen von der Verwendung der Software des Herausgebers ab.

- **SaaS-Anwendung**: Muss eine mehrinstanzenfähige Lösung sein, [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) für die Authentifizierung verwenden und mit den [SaaS-Fulfillment-APIs](partner-center-portal/pc-saas-fulfillment-api-v2.md) integriert sein. Die Nutzung der Azure-Infrastruktur wird verwaltet und direkt mit Ihnen (dem Herausgeber) abgerechnet. Nutzungsgebühren für die Azure-Infrastruktur sowie Softwarelizenzgebühren müssen daher von Ihnen als einzelner Posten abgerechnet werden. Ausführliche Informationen finden Sie unter [Pläne](plan-saas-offer.md#plans).

## <a name="private-plans"></a>Private Pläne

Sie können einen privaten Plan für ein Angebot mit ausgehandelten, transaktionsspezifischen Preisen oder kundenspezifischen Konfigurationen anbieten.

Private Pläne ermöglichen die Verwendung höherer oder niedrigerer Preise für bestimmte Kunden (im Vergleich zum öffentlich verfügbaren Angebot). Mithilfe von privaten Plänen können Sie Rabatte gewähren oder ein Angebot mit einem Aufschlag versehen. Private Pläne können für einzelne oder mehrere Kunden verfügbar gemacht werden, indem deren Azure-Abonnement auf Planebene aufgelistet wird.

## <a name="next-steps"></a>Nächste Schritte

- Entnehmen Sie den Veröffentlichungsmustern der Onlineshops Beispiele für die Zuordnung Ihrer Lösung zu einem Angebotstyp und einer Konfiguration.
- [Leitfaden zur Veröffentlichung nach Angebotstyp](publisher-guide-by-offer-type.md)