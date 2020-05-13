---
title: 'Übersicht über die Auszahlungszusammenfassung: Azure Marketplace'
description: Die Zahlungsübersicht enthält Einzelheiten zu dem Betrag, den Sie mit Ihrem Angebot eingenommen haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.
author: mingshen
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 48644f2f8148a7aa1974be0d7f761e9b3a55612d
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/04/2020
ms.locfileid: "82782547"
---
# <a name="payout-summary-overview"></a>Übersicht über die Auszahlungszusammenfassung

Die [Auszahlungszusammenfassung](./payout-summary.md) enthält Einzelheiten zu den Beträgen, die Sie mit Microsoft eingenommen haben. Aus ihr geht auch hervor, wann und in welcher Höhe Sie Zahlungen erhalten.

Wenn Sie Angebote über den Azure Marketplace verkaufen, werden Informationen zu erfolgreichen Auszahlungen auch in der Auszahlungszusammenfassung angezeigt. Weitere Informationen zu Azure Marketplace-Zahlungen finden Sie in den [Azure Marketplace-Teilnahmerichtlinien](https://docs.microsoft.com/legal/marketplace/participation-policy) sowie in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

> [!NOTE]
> Qualifiziert für eine Auszahlung sind Beträge, die den [Zahlungsschwellenwert](./payment-thresholds-methods-timeframes.md) von 50 US-Dollar erreichen. Ausführliche Informationen zum Zahlungsschwellenwert finden Sie in der [Microsoft Azure Marketplace-Herausgebervereinbarung](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt).

Alle Berichte und Erkenntnisse im Zusammenhang mit der Veröffentlichungsoption „Transaktion“ sind im Analytics-Abschnitt von Partner Center verfügbar, auf den Sie rechts oben im Portal über das folgende Symbol zugreifen können:

![Auszahlungssymbol rechts oben im Partner Center-Portal](./media/payout-overview.png)

## <a name="roles-and-permissions"></a>Rollen und Berechtigungen

Im Anschluss finden Sie Rollen und Berechtigungen für den Zugriff auf den Auszahlungsbericht:

| Berichte/Seiten | Kontobesitzer | Manager | Entwickler | Mitwirkender im Geschäftsbereich | Mitwirkender im Finanzbereich | Vermarkter |
| --- | --- | --- | --- | --- | --- | --- |
| Kaufbericht (einschließlich Daten in Quasi-Echtzeit) | Kann anzeigen | Kann anzeigen | Kein Zugriff | Kein Zugriff | Kann anzeigen | Kein Zugriff |
| Feedback – Berichte/Antworten | Kann Feedback anzeigen und senden | Kann Feedback anzeigen und senden | Kann Feedback anzeigen und senden | Kein Zugriff | Kein Zugriff | Kann Feedback anzeigen und senden |
| --- | --- | --- | --- | --- | --- | --- |
| Integritätsbericht (einschließlich Daten in Quasi-Echtzeit) | Kann anzeigen | Kann anzeigen | Kann anzeigen | Kann anzeigen | Kein Zugriff | Kein Zugriff |
| Nutzungsbericht | Kann anzeigen | Kann anzeigen | Kann anzeigen | Kann anzeigen | Kein Zugriff | Kein Zugriff |
| Auszahlungskonto | Kann aktualisieren | Kein Zugriff | Kein Zugriff | Kein Zugriff | Kann aktualisieren | Kein Zugriff |
| Steuerprofil | Kann aktualisieren | Kein Zugriff | Kein Zugriff | Kein Zugriff | Kann aktualisieren | Kein Zugriff |
| Auszahlungszusammenfassung | Kann anzeigen | Kein Zugriff | Kein Zugriff | Kein Zugriff | Kann anzeigen | Kein Zugriff  |
| | | | | | | |

## <a name="payout-report-differences"></a>Unterschiede zwischen Auszahlungsberichten

Beim Zahlungsbericht gibt es folgende Unterschiede zwischen dem Cloud-Partnerportal (alt) und Partner Center (neu):

| Cloudpartnerportal | Partner Center |
| --- | --- |
| **Link**: https://cloudpartner.azure.com/ | **Link**: https://partner.microsoft.com/dashboard/payouts/reports/transactionhistory und https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| **Navigation**: In Insights Payout bereitgestellte Auszahlungsberichte | **Navigation**: In Partner Center bereitgestellte Auszahlungsberichte – Auszahlungssymbol |
| **Bereich:**<ul><li>Angezeigt wird die Transaktion pro Position, und zwar für „Inkasso in Bearbeitung“, „Eingezogen“ und „Bezahlt“.</li><li>Bericht: Enthält nach dem Erstellen der Bestellung alle Positionen (einschließlich „Inkasso in Bearbeitung“ und „Abrechnung in Bearbeitung“) sowie den Inkassostatus und die Positionen, die noch nicht für eine Zahlung qualifiziert sind.</li></ul> | **Bereich:**<ul><li>Zeigt die Positionen, sobald sie als qualifizierte Einnahmen betrachtet werden.</li><li>Die Kundenzahlung geht zuerst an Microsoft. Anschließend können die ISVs dann sehen, wie der Auszahlungsbericht gestartet wird.</li><li>Im Auszahlungsbericht werden „Inkasso in Bearbeitung“ und „Abrechnung in Bearbeitung“ nicht angezeigt.</li></ul> |
| **Transaktion nicht auszahlungsbereit**: Abrechnung in Bearbeitung | **Transaktion nicht auszahlungsbereit**: Nächste voraussichtliche Zahlung: Die Auszahlung befindet sich im Status „Nicht verarbeitet“. |
| **Auszahlungsstatus**: – | **Auszahlungsstatus**:<ul><li>Nicht verarbeitet: Die Einnahme ist für eine Auszahlung qualifiziert.</li><li>Anstehend: Die Einnahme wird in die nächste monatliche Auszahlung an den Herausgeber eingeschlossen.</li><li>Gesendet: Die Zahlung wurde an Ihre Bank gesendet.</li></ul> |
| | |

## <a name="payment-schedules"></a>Zahlungspläne

Informationen zu Zahlungsplänen finden Sie im Abschnitt [Zahlungspläne](./payout-policy-details.md#payment-schedules) des Themas **Auszahlungsrichtliniendetails**. Dort wird unter anderem auf Einbehaltungszeiträume, auf die Sichtbarkeit für Partner und auf die Kreditkarten-/Rechnungsverwendung des Kunden eingegangen.

## <a name="transaction-history-download-export"></a>Download des exportierten Transaktionsverlaufs

Mit dieser Option können Sie jede Einnahmeposition, die auf der Seite „Transaktionsverlauf“ angezeigt wird, herunterladen. Hierzu zählen Einnahmetyp, Datum, zugehöriger Transaktionsbetrag, Kunde, Produkt und andere Transaktionsdetails im Zusammenhang mit dem Incentives-Programm.

| Spaltenname | Beschreibung |
| --- | --- |
| earningId | Eindeutiger Bezeichner für jede Einnahme |
| participantID | Primäre Identität des Partners, der unter dem Programm die Einnahme erzielt |
| participantIdType | Programm-ID für Incentive-Programme und Verkäufer, wenn das Programm für Store-Programme und den Azure Marketplace vorgesehen ist. |
| participantName | Name des Partners, der die Einnahme erzielt |
| partnerCountryCode | Standort/Land des Partners, der die Einnahme erzielt |
| programName | Name des Incentive-/Store-Programms |
| transactionId | Eindeutige ID für die Transaktion |
| transactionCurrency | Währung, in der die ursprüngliche Kundentransaktion ausgeführt wurde (nicht die Währung für den Partnerstandort) |
| transactionDate | Datum der Transaktion. Nützlich für Programme, bei denen viele Transaktionen zu einer Einnahme beitragen |
| transactionExchangeRate | Wechselkurs zum Anzeigen des entsprechenden Betrags für die Transaktion in US-Dollar |
| transactionAmount | Transaktionsbetrag in der ursprünglichen Transaktionswährung, auf deren Basis die Einnahme generiert wird |
| transactionAmountUSD | Transaktionsbetrag in US-Dollar (USD) |
| lever | Geschäftsregel für die Einnahme |
| earningrate | Auf den Transaktionsbetrag angewendete Incentive-Rate zum Generieren einer Einnahme |
| quantity | In Rechnung gestellte Menge für Transaktionsprogramme. Abhängig vom Programm |
| quantityType | Art der Menge, beispielsweise: in Rechnung gestellte Menge, monatlich aktive Benutzer (Monthly Active Users, MAU) |
| earningType | Gibt an, worum es sich handelt (Gebühr, Rabatt, Co-Op, Verkauf usw.) |
| earningamount | Einnahmebetrag in der ursprünglichen Transaktionswährung |
| earningAmountUSD | Einnahmebetrag in US-Dollar |
| earningDate | Datum der Einnahme |
| calculationDate | Datum der Berechnung der Einnahme im System |
| earningExchangeRate | Wechselkurs zum Anzeigen des entsprechenden Betrags in US-Dollar |
| exchangeRateDate | Datum des Wechselkurses, der zum Berechnen des Einnahmebetrags in US-Dollar verwendet wurde |
| paymentAmountWOTax | Einnahmebetrag (ohne Steuern) in der Auszahlungswährung nur für Zahlungen mit dem Status &quot;Gesendet&quot; |
| paymentCurrency | Vom Partner im Zahlungsprofil ausgewählte Auszahlungswährung. Wird nur für gesendete Zahlungen angezeigt |
| paymentExchangeRate | Wechselkurs zur Berechnung von „paymentAmountWOTax“ in der Auszahlungswährung mithilfe von „ExchangeRateDate“ |
| paymentId | Eindeutige ID für die Zahlung. Diese Zahl wird auf dem Kontoauszug der Bank angezeigt |
| paymentStatus | Zahlungsstatus |
| paymentStatusDescription | Beschreibung des Zahlungsstatus |
| customerId | Ist immer leer |
| customerName | Ist immer leer |
| partNumber | Ist immer leer |
| ProductName | Mit der Transaktion verknüpfter Produktname |
| productId | Eindeutige Produkt-ID |
| parentProductId | Eindeutige ID des übergeordneten Produkts. Wenn kein übergeordnetes Produkt für die Transaktion vorhanden ist, entspricht die ID des übergeordneten Produkts der Produkt-ID. |
| parentProductName | Name des übergeordneten Produkts. Wenn kein übergeordnetes Produkt für die Transaktion vorhanden ist, entspricht der Name des übergeordneten Produkts dem Produktnamen. |
| productType | Produkttyp (beispielsweise App, Add-On oder Spiel) |
| InvoiceNumber | Rechnungsnummer (nur für Enterprise Agreements) |
| resellerId | ID des Handelspartners |
| resellerName | Name des Handelspartners |
| transactionType | Transaktionstyp (beispielsweise Kauf, Erstattung, Stornierung oder Rückbuchung) |
| localProviderSeller | Lokaler Anbieter/Seller of Record |
| taxRemitted | Betrag der abgeführten Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer). |
| taxRemitModel | Partei, die für das Abführen von Steuern (Verkaufssteuer, Nutzungssteuer oder Mehrwertsteuer/Waren- und Dienstleistungssteuer) verantwortlich ist. |
| storeFee | Der Betrag, der von Microsoft als Gebühr für die Bereitstellung der App oder des Add-Ons im kommerziellen Marketplace einbehalten wird. |
| transactionPaymentMethod | Vom Kunden für die Transaktion verwendetes Zahlungsmittel (beispielsweise Kreditkarte, Abrechnung des Mobilfunkanbieters oder PayPal) |
| tpan | Drittanbieter-Werbenetzwerk |
| customerCountry | Land des Kunden |
| CustomerCity | Stadt des Kunden |
| customerState | Bundesland/Kanton des Kunden |
| customerzip | Postleitzahl des Kunden |
| Mandanten-ID | ID des Mandanten |
| externalReferenceId | Eindeutige ID für das Programm |
| externalReferenceIdLabel | Beschriftung der eindeutigen ID |
| transactionCountryCode | Code des Landes, in dem die Transaktion erfolgt ist |
| taxCountry | Land des Kunden |
| taxState | Bundesland/Kanton des Kunden |
| taxCity | Ort des Kunden |
| taxzipcode | Postleitzahl des Kunden |
| LicensingProgramName | Name des Lizenzierungsprogramms |
| Programmcode | Dem Programmnamen zuzuordnende Zeichenfolge |
| earningAmountInLastPaymentCurrency | Einnahmebetrag in der Währung der letzten Zahlung (leer, wenn noch keine Zahlungen geleistet wurden) |
| lastPaymentCurrency | Währung der letzten Zahlung (leer, wenn noch keine Zahlungen geleistet wurden) |
| AssetId | Eindeutige ID für die Kundenbestellungen für Ihren Marketplace-Dienst. Stellt die Kaufpositionen dar. Es kann mehrere Assets geben. |
| OrderId | Bezieht sich auf die Rechnung eines Kunden |
| LineItemId | Einzelne Position in der Rechnung eines Kunden |
| Land des Kunden | Der vom Kunden angegebene Ländername. Kann sich von dem im Azure-Abonnement eines Kunden angegebenen Land unterscheiden. |
| Kunden EmailAddress | Die vom Kunden angegebene E-Mail-Adresse. Kann sich von der im Azure-Abonnement eines Kunden angegebenen E-Mail-Adresse unterscheiden. |
| SkuId | Die bei Veröffentlichung definierte SKU-ID. Ein Angebot kann mehrere SKUs enthalten, aber eine SKU kann nur einem einzigen Angebot zugeordnet werden. |

> [!NOTE]
> Alle Berichte und Erkenntnisse im Zusammenhang mit der Veröffentlichungsoption „Transaktion“ sind im Analytics-Abschnitt von Partner Center verfügbar.

## <a name="billing-questions-and-support"></a>Fragen zu Abrechnung und Support
Abrechnungsunterstützung erhalten Sie vom [Support für Herausgeber](https://partner.microsoft.com/support/v2/?stage=1).

## <a name="next-step"></a>Nächster Schritt

- [Auszahlungszusammenfassungen](./payout-summary.md)
