---
title: Kommerzieller Marketplace – Zahlungsübersicht | Azure Marketplace
description: Die Zahlungsübersicht enthält Einzelheiten zu dem Betrag, den Sie mit Ihrem Angebot eingenommen haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: guide
ms.date: 12/10/2019
ms.openlocfilehash: 0d05802c9d5d80f91913291d710b674369f0ff17
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74979987"
---
# <a name="payout-reporting"></a>Zahlungsberichte

Die **Zahlungsübersicht** enthält Einzelheiten zu dem Betrag, den Sie mit Ihrem Angebot eingenommen haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.

Wenn Sie Angebote über Azure Marketplace verkaufen, werden in der **Zahlungsübersicht** auch Informationen zu erfolgreichen Auszahlungen angezeigt. Weitere Informationen zu Azure Marketplace-Zahlungen finden Sie in den [Teilnahmerichtlinien für Microsoft Azure Marketplace](https://go.microsoft.com/fwlink/p/?LinkId=722436) (Microsoft Azure Marketplace Participation Policies) und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://go.microsoft.com/fwlink/p/?LinkID=699560) (Microsoft Azure Marketplace Publisher Agreement).

> [!NOTE]
> Qualifiziert für eine Auszahlung sind Beträge, die den [Zahlungsschwellenwert](payment-thresholds-methods-timeframes.md) von 50 US-Dollar erreichen. Weitere Informationen zum Zahlungsschwellenwert finden Sie auf dieser Seite und in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://go.microsoft.com/fwlink/p/?LinkID=699560).

- [Zahlungsbericht: Unterschied zwischen Cloud-Partnerportal und Partner Center](#payout-report-difference-between-cloud-partner-portal-and-partner-center)
- [Kundentypen](#customer-types)
- [Korrelation zwischen Zahlung und Nutzung](#corelation-between-payout-and-usage)
- [Download von Transaktionsverläufen](#transaction-history-download-export)
- [Fragen zu Abrechnung und Support](#billing-questions-and-support)

## <a name="payout-report-difference-between-cloud-partner-portal-and-partner-center"></a>Zahlungsbericht: Unterschied zwischen Cloud-Partnerportal und Partner Center

| | Cloudpartnerportal | Partner Center |
|---------|---------|---------|
| Links | https://cloudpartner.azure.com/ | https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory und https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| Navigation | In Insights Payout bereitgestellte Auszahlungsberichte | In Partner Center bereitgestellte Auszahlungsberichte – Auszahlungssymbol |
| `Scope` | <ul> <li>Angezeigt wird die Transaktion pro Position, und zwar für „Inkasso in Bearbeitung“, „Eingezogen“ und „Bezahlt“. </li> <li>Bericht: Enthält nach dem Erstellen der Bestellung alle Positionen (einschließlich „Inkasso in Bearbeitung“ und „Abrechnung in Bearbeitung“) sowie den Inkassostatus und die Positionen, die sich noch nicht für eine Bezahlung qualifiziert haben. </li> </ul> | <ul> <li>Zeigt die Positionen an, sobald sie als qualifizierte Einnahmen betrachtet werden.</li> <li>Die Kundenzahlung geht zuerst an Microsoft. Anschließend können die ISVs dann sehen, wie der Auszahlungsbericht gestartet wird.</li> <li>Im Auszahlungsbericht werden „Inkasso in Bearbeitung“ und „Abrechnung in Bearbeitung“ nicht angezeigt.  </li> </ul>  |
| Transaktion nicht bereit für Auszahlung | Abrechnung in Bearbeitung | Nächste voraussichtliche Zahlung: Die Auszahlung befindet sich im Status „Nicht verarbeitet“.  |
| Auszahlungsstatus |  | Nicht verarbeitet: <br> Die Einnahme ist für eine Auszahlung qualifiziert. Sie behält diesen Status für die Dauer der sogenannten „Cooling Period“, wie sie im Programmhandbuch für das Incentive-Programm definiert ist. <br> <br> Anstehend: <br> Anstehende interne Überprüfung für generierten Zahlungsauftrag, bevor die Zahlung verarbeitet wird. <br> <br> Gesendet: <br> Die Zahlung wurde an Ihre Bank gesendet. |

## <a name="customer-types"></a>Kundentypen 

### <a name="enterprise-agreement"></a>Enterprise Agreement

Kunden ohne Enterprise Agreement werden Marketplace-Softwarelizenzen monatlich in Rechnung gestellt. Kunden mit Enterprise Agreement wird monatlich eine Rechnung gestellt, die vierteljährlich ausgefertigt wird.

### <a name="credit-cards-and-monthly-invoice"></a>Kreditkarten und monatliche Rechnung

Kunden können auch per Kreditkarte und monatlicher Rechnung bezahlen. In diesem Fall werden die Gebühren für Ihre Softwarelizenzen monatlich abgerechnet.

### <a name="csp-and-direct-pay-users"></a>CSP- und Direct Pay-Benutzer

Der Kunde bezahlt beispielsweise mit einer Kreditkarte.

## <a name="corelation-between-payout-and-usage"></a>Korrelation zwischen Auszahlung und Nutzung 

|BESCHREIBUNG    |    Date  | Aufträge/Nutzung  | Auszahlung |
|----------|----------|-----------|-------------|
|Auftragszeitraum   | 15. August 2019 bis 30. August 2019 | **Korrelationsattribute der Aufträge** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Verwendung** <br> <ul> <li>CustomerId </li> <li>Customer Name</li> <li>(UsageReference)PurchaseRecordId/LineItemId</li> <li> Geschätzte erweiterte Gebühren <br> Geschätzte Auszahlung (PC) </li> </ul> |  |
|Laufzeitende (Monat)   | 30. August 2019 | | |
|Abrechnungsdatum | 1\. September 2019 | | |
|Zahlungsdatum des Kunden | 1\. September 2019 | | |
|Treuhandperiode (nur Kreditkarten, 30 Tage) | 1\. September 2019 bis 30. September 2019 | | **Korrelationsattribute der Aufträge:** <br> <ul><li>AssetId</li> <li>Kunden-ID</li> <li> Customer Name</li> </ul> <br> **Verwendung** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Customer Name</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Auszahlungsstatus:** Nicht verarbeitet |
|Start der Forderungslaufzeit | 1\. September 2019 | | |
|Ende der Forderungslaufzeit (max. 30 Tage) | 30. September 2019 | | |
|Datum der Berechnung der Auszahlung (monatlich am 15.) | 1\. Oktober 2019 | | **Korrelationsattribute** <br> <ul><li>AssetId</li> <li>Kunden-ID</li> <li>Customer Name</li> </ul> <br> **Verwendung** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Customer Name</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Auszahlungsstatus:** UpComing |
|Auszahlungsdatum | 15. Oktober 2019 | | **Korrelationsattribute** <br> <ul><li>AssetId</li> <li>Kunden-ID</li> <li> Customer Name</li> </ul> <br> **Verwendung** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Customer Name</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Auszahlungsstatus:** Zahlung gesendet |

### <a name="enterprise-agreement-quarterlymonthly-customers"></a>Enterprise Agreement (vierteljährliche/monatliche Kunden)

| BESCHREIBUNG |    Date  | Verwendung | Auszahlung |
|----------|----------|---------|-----------|
|Auftragszeitraum | 15. August 2019 bis 30. August 2019 | **Korrelationsattribute der Aufträge** <br> <ul> <li>OrderId</li> <li>CustomerId</li> </ul> <br> **Nutzungsbericht** <br> <ul> <li>CustomerId </li> <li>Customer Name</li> <li>(UsageReference)PurchaseRecordId/LineItemId</li> <li> Geschätzte erweiterte Gebühren <br> Geschätzte Auszahlung (PC) </li> </ul> | |
|Laufzeitende (Quartal) | 30. September 2019 | | |
|Abrechnungsdatum | 15. Oktober 2019 | | |
|Treuhandperiode (nur Kreditkarten, 30 Tage) | – | | |
|Start der Forderungslaufzeit | 15. Oktober 2019 | | |
|Nur Kreditkarten, 30 Tage | 1\. November 2019 bis 30. November 2019 | | |
|Ende der Forderungslaufzeit (max. 90 Tage) | 15. Januar 2020 | | |
|Zahlungsdatum des Kunden | 30. Dezember 2019 | | |
|Berechnung der Auszahlung | 15. Januar 2020 | | |
|Auszahlungsdatum | 15. Februar 2020 | | **Bei vierteljährlichen Kunden** <br> <br> **Auftragsbericht** <br> <ul><li>AssetId</li> <li>Kunden-ID</li> <li> Customer Name</li> </ul> <br> **Verwendung** <br> <ul> <li>AssetId</li> <li>CustomerId</li> <li>Customer Name</li> <li>OrderId</li> <li>LineItemId</li> <li>transactionAmount</li> <li>EarningAmountInLastPaymentCurrency</li> </ul> <br> **Auszahlungsstatus:** Gesendet |

<!---
## Billing

Depending on the transaction option used, the publisher’s software license fees can be presented as follows:

* Free: No charge for software licenses.
* **Bring the own license (BYOL)**: Any applicable charges for software licenses are managed directly between the publisher and customer. Microsoft only passes through Azure infrastructure usage fees. (Virtual Machines and Azure Applications only).
* **Pay-as-you-go**: Software license fees are presented as a per-hour, per-core (VCPU) pricing rate based on the Azure infrastructure used. This only applies to Virtual Machines and Azure Applications. 
* **Subscription pricing**: Software license fees are presented as a monthly or annual recurring fee billed as a flat rate or per-seat. This only applies to SaaS Apps and Azure Applications - Managed Apps.

### Pay as you go

If you enable the Pay-As-You-Go option, then you have the following cost structure. 

* If you enable the Pay-As-You-Go option, then you have the following cost structure.

|Your license cost  | $1.00 per hour  |
|---------|---------|
|Azure usage cost (D1/1-Core)    |   $0.14 per hour     |
|*Customer is billed by Microsoft*    |  *$1.14 per hour*       |

* In this scenario, Microsoft bills $1.14 per hour for use of your published VM image.

|Microsoft bills  | $1.14 per hour  |
|---------|---------|
|Microsoft pays you 80% of your license cost|   $0.80 per hour     |
|Microsoft keeps 20% of your license cost  |  $0.20 per hour       |
|Microsoft keeps 100% of the Azure usage cost | $0.14 per hour |

### Bring Your Own License (BYOL)

* If you enable the BYOL option, then you have the following cost structure.

|Your license cost  | License fee negotiated and billed by you  |
|---------|---------|
|Azure usage cost (D1/1-Core)    |   $0.14 per hour     |
|*Customer is billed by Microsoft*    |  *$0.14 per hour*       |

* In this scenario, Microsoft bills $0.14 per hour for use of your published VM image.

|Microsoft bills  | $0.14 per hour  |
|---------|---------|
|Microsoft keeps the Azure usage cost    |   $0.14 per hour     |
|Microsoft keeps 0% of your license cost   |  $0.00 per hour       |

### SaaS App subscription

This option must be configured to sell through Microsoft and can be priced at a flat rate or per user on a monthly or annual basis.
•   If you enable the Sell through Microsoft option for a SaaS offer, then you have the following cost structure.

|Your license cost       | $100.00 per month  |
|--------------|---------|
|Azure usage cost (D1/1-Core)    | Billed directly to the publisher, not the customer |
|*Customer is billed by Microsoft*    |  *$100.00 per month (note: publisher must account for any incurred or pass-through infrastructure costs in the license fee)*  |

* In this scenario, Microsoft bills $100.00 for your software license and pays out $80.00 to the publisher.
* Partners who have qualified for the Reduced Marketplace Service Fee will see a reduced transaction fee on the SaaS offers from May 2019 until June 2020. In this scenario, Microsoft bills $100.00 for your software license and pays out $90.00 to the publisher.

|Microsoft bills  | $100.00 per month  |
|---------|---------|
|Microsoft pays you 80% of your license cost <br> \* Microsoft pays you 90% of your license cost for any qualified SaaS apps   |   $80.00 per month <br> \* $90.00 per month    |
|Microsoft keeps 20% of your license cost <br> \* Microsoft keeps 10% of your license cost for any qualified SaaS apps.  |  $20.00 per month <br> \* $10.00     |

**Reduced Marketplace Service Fee:** For certain SaaS Products that you publish on our Commercial Marketplace, Microsoft will reduce its Marketplace Service Fee from 20% (as described in the Microsoft Publisher Agreement) to 10%.  In order for your Product to qualify, at least one of your products must be designated by Microsoft as either IP co-sell ready or IP co-sell prioritized. To receive this reduced Marketplace Service Fee for the month, eligibility must be met at least five (5) business days before the end of the previous calendar month. Reduced Marketplace Service fee will not apply to VMs, Managed Apps or any other products made available through our Commercial Marketplace.  This Reduced Marketplace Service Fee will be available to qualified offers, with license charges collected by Microsoft between May 1, 2019 and June 30, 2020.  After that time, the Marketplace Service Fee will return to its normal amount.

### Customer invoicing, payment, billing, and collections

**Invoicing and payment**

Publisher can use the customer's preferred invoicing method to deliver subscription or PAYGO software license fees.

**Enterprise agreement** 

If the customer's preferred invoicing method is the Microsoft Enterprise Agreement, your software license fees will be billed using this invoicing method as an itemized cost, separate from any Azure-specific usage costs.

**Credit cards and monthly invoice** 

Customers can also pay using a credit card and a monthly invoice. In this case, your software license fees will be billed just like the Enterprise Agreement scenario, as an itemized cost, separate from any Azure-specific usage costs.



**Billing and collections** 

Publisher software license billing is presented using the customer selected method of invoicing and follows the invoicing timeline. Customers without an Enterprise Agreement in place are billed monthly for marketplace software licenses. Customers with an Enterprise Agreement are billed monthly via an invoice that is presented quarterly.

When subscription or Pay-as-You-Go pricing models are selected, Microsoft acts as the agent of the publisher and is responsible for all aspects of billing, payment, and collection.

### Publisher payout and reporting

* Any software licensing fees collected by Microsoft as an agent are subject to a 20% transaction fee unless otherwise specified and are deducted at the time of publisher payout.

* Customers typically purchase using the Enterprise Agreement or a credit-card enabled pay-as-you-go agreement. The agreement type determines billing, invoicing, collection, and payout timing.
--->

## <a name="transaction-history-download-export"></a>Download des exportierten Transaktionsverlaufs

Mit dieser Option können Sie jede Einnahmeposition, die auf der Seite „Transaktionsverlauf“ angezeigt wird, wie beispielsweise Einnahmetyp, Datum, zugehöriger Transaktionsbetrag, Kunde, Produkt und andere Transaktionsdetails, die für das Incentive-Programm gelten, herunterladen. 

| Spaltenname     | BESCHREIBUNG    | 
|-------------|-------------------------------|
| earningId                      | Eindeutiger Bezeichner für jede Einnahme                                                                                                       |
| participantID                  | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt                                                                            | 
| participantIdType              | Meistens eine Programm-ID für Incentive-Programme und Verkäufer, WENN es sich um Store-Programme und Azure Marketplace handelt                                          | 
| participantName                | Name des Partners, der die Einnahme erzielt                                                                                                              | 
| partnerCountryCode             | Standort/Land des Partners, der die Einnahme erzielt                                                                                                  |
| programName                    | Name des Incentive-/Store-Programms                                                                                                             | 
| transactionId                  | Eindeutige ID für die Transaktion                                                                                                    | 
| transactionCurrency            | Währung, in der die ursprüngliche Kundentransaktion ausgeführt wurde (nicht die Währung für den Partnerstandort)                                     | 
| transactionDate                | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einer Einnahme beitragen                                           | 
| transactionExchangeRate        | Wechselkurs zum Anzeigen des entsprechenden Betrags für die Transaktion in US-Dollar                                                                 | 
| transactionAmount              | Transaktionsbetrag in der ursprünglichen Transaktionswährung, auf deren Basis die Einnahme generiert wird                                              | 
| transactionAmountUSD           | Transaktionsbetrag in US-Dollar                                                                                                                | 
| lever                          | Geschäftsregel für die Einnahme                                                                                                  | 
| earningrate                    | Auf den Transaktionsbetrag angewendete Incentive-Rate zum Generieren einer Einnahme                                                                      | 
| quantity                       | Abhängig vom Programm. Gibt die in Rechnung gestellte Menge für Transaktionsprogramme an                                                            | 
| quantityType                   | Gibt den Typ der Menge an, z. B.: In Rechnung gestellte Menge, MAU                                                                                     |
| earningType                    | Gibt an, ob es sich um eine Gebühr, einen Rabatt, Coop, Verkauf usw. handelt.                                                                                          | 
| earningamount                  | Einnahmebetrag in der ursprünglichen Transaktionswährung                                                                                      |
| earningAmountUSD               | Einnahmebetrag in US-Dollar                                                                                                                    |
| earningDate                    | Datum der Einnahme                                                                                                                      |
| calculationDate                | Datum der Berechnung der Einnahme im System                                                                                            |
| earningExchangeRate            | Wechselkurs zum Anzeigen des entsprechenden Betrags in US-Dollar                                                                                  |
| exchangeRateDate               | Datum des Wechselkurses, der zum Berechnen des Einnahmebetrags in US-Dollar verwendet wurde                                                                                   | 
| paymentAmountWOTax             | Einnahmebetrag (ohne Steuern) in der Auszahlungswährung nur für Zahlungen mit dem Status „Gesendet“                                                                 |
| paymentCurrency                | Vom Partner im Zahlungsprofil ausgewählte Auszahlungswährung. Wird nur für gesendete Zahlungen angezeigt                                                   |
| paymentExchangeRate            | Wechselkurs zur Berechnung von „paymentAmountWOTax“ in der Auszahlungswährung mithilfe von „ExchangeRateDate“                                            |
| paymentId            | Eindeutige ID für die Zahlung. Diese Zahl wird auf dem Kontoauszug der Bank angezeigt                                            |
| paymentStatus            | Zahlungsstatus                                            |
| paymentStatusDescription            | Benutzerfreundliche Beschreibung des Zahlungsstatus                                            |
| customerId                     | Ist immer leer                                                                                                                     |
| customerName                   | Ist immer leer                                                                                                                     |
| partNumber                     | Ist immer leer                                                                                                                     |
| ProductName                    | Mit der Transaktion verknüpfter Produktname                                                                                                       |
| productId                      | Eindeutige Produkt-ID                                                                                                                |
| parentProductId                | Eindeutige ID des übergeordneten Produkts. Hinweis: Wenn kein übergeordnetes Produkt für die Transaktion vorhanden ist, entspricht die ID des übergeordneten Produkts der Produkt-ID. |
| parentProductName              | Name des übergeordneten Produkts. Hinweis: Wenn kein übergeordnetes Produkt für die Transaktion vorhanden ist, entspricht der Name des übergeordneten Produkts dem Produktnamen.   |
| productType                    | Produkttyp (z. B. App, Add-On, Spiel usw.)                                                                                        |
| InvoiceNumber                  | Rechnungsnummer (gilt nur für EA)                                                                                                  |
| resellerId                     | ID des Handelspartners                                                                                                                      |
| resellerName                   | Name des Handelspartners                                                                                                                            |
| transactionType                | Transaktionstyp (z. B. Kauf, Erstattung, Stornierung, Rückbelastung usw.)                                                               |
| localProviderSeller            | Lokaler Anbieter/Seller of Record                                                                                                          |
| taxRemitted                    | Betrag der abgeführten Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer).                                                                                   |
| taxRemitModel                  | Partei, die für das Abführen von Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer) verantwortlich ist.                                                                    |
| storeFee                       | Der von Microsoft als Gebühr für die Bereitstellung der App oder des Add-Ins im Store einbehaltene Betrag.                                            |
| transactionPaymentMethod       | Vom Kunden für die Transaktion verwendetes Zahlungsmittel (z.B. Scheckkarte, Mobile Carrier Billing, PayPal usw.)                                |
| tpan                           | Gibt das Ad Network (Werbenetzwerk) des Drittanbieters an                                                                                                     |
| customerCountry                | Land des Kunden                                                                                                                         |
| CustomerCity                   | Stadt des Kunden                                                                                                                            |
| customerState                  | Bundesland/Kanton des Kunden                                                                                                                           |
| customerzip                    | Postleitzahl des Kunden                                                                                                                 |
| Mandanten-ID                       |                                                                                                                                          |
| externalReferenceId            | Eindeutige ID für das Programm                                                                                                        |
| externalReferenceIdLabel       | Beschriftung der eindeutigen ID                                                                                                                  |
| transactionCountryCode       | Code des Lands, in dem die Transaktion erfolgt ist                                                                                                                  |
| taxCountry       | Land des Käufers                                                                                                                  |
| taxState       | Bundesland/Kanton des Käufers                                                                                                                  |
| taxCity       | Stadt des Käufers                                                                                                                  |
| taxzipcode       | Postleitzahl des Käufers                                                                                                                  |
| LicensingProgramName       |                                                                                                                   |
| Programmcode       | Dem Programmnamen zuzuordnende Zeichenfolge                                                                                                                   |
| earningAmountInLastPaymentCurrency       | Einnahmebetrag in der Währung der letzten Zahlung (das Feld ist leer, wenn keine vorherigen Zahlungen geleistet wurden)                                                                                                                   |
| lastPaymentCurrency       | Währung der letzten Zahlung (das Feld ist leer, wenn keine vorherigen Zahlungen geleistet wurden)                                                                                                                   |
| AssetId       | Eindeutige ID für die Kundenbestellungen für Ihren Marketplace-Dienst.  Stellt die abgewickelten Kaufpositionen dar. Es kann mehrere Assets geben.                                                                                                                   |
| OrderId       | Bezieht sich auf die Rechnung eines Kunden                                                                                                                   |
| LineItemId       | Einzelne Position in der Rechnung eines Kunden                                                                                                                   |
| Land des Kunden       | Der vom Kunden angegebene Ländername.  Dieser kann sich von dem im Azure-Abonnement eines Kunden angegebenen Land unterscheiden.                                                                                                                   |
| Kunden EmailAddress       | Die vom Endkunden angegebene E-Mail-Adresse.  Diese kann sich von der im Azure-Abonnement eines Kunden angegebenen E-Mail-Adresse unterscheiden.                                                                                                                   |
| SkuId       | Die bei Veröffentlichung definierte SKU-ID. Ein Angebot kann mehrere SKUs enthalten, aber eine SKU kann nur einem einzigen Angebot zugeordnet werden.                                                                                                                   |

>[!Note]
>Alle Berichte und Ergebnisse für die Veröffentlichungsoption „Transaktion“ sind im Abschnitt „Erkenntnisse“ im Cloud-Partnerportal oder im Abschnitt „Analyse“ in Partner Center verfügbar.

## <a name="billing-questions-and-support"></a>Fragen zu Abrechnung und Support

Hilfe zu Abrechnungsfragen erhalten Sie beim [Support für Herausgeber auf dem kommerziellen Marketplace](https://aka.ms/marketplacepublishersupport).
