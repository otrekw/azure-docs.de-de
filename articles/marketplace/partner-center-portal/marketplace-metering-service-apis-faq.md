---
title: Häufig gestellte Fragen zu Messungsdienst-APIs – Kommerzieller Microsoft-Marketplace
description: Hier finden Sie häufig gestellte Fragen zu den Messungsdienst-APIs für SaaS-Angebote in Microsoft AppSource und im Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/01/2020
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: ddec23f695396b8322d51da62158a97456096ae8
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87319999"
---
# <a name="marketplace-metered-billing-apis---faq"></a>APIs für getaktete Abrechnung im Marketplace – Häufig gestellte Fragen

Nachdem ein Kunde einen SaaS-Dienst oder eine Azure-Anwendung mit einem Plan für verwaltete Apps mit getakteter Abrechnung abonniert hat, wird der Verbrauch für jede verwendete Abrechnungsdimension nachverfolgt.  Wenn der Verbrauch die eingeschlossenen Mengen für den vom Benutzer ausgewählten Abrechnungszeitraum überschreitet, gibt der Dienst Nutzungsereignisse aus.

## <a name="for-both-saas-offers-and-managed-apps"></a>Für SaaS-Angebote und verwaltete Apps

### <a name="how-often-is-it-expected-to-emit-usage"></a>Wie oft wird die Ausgabe der Nutzung erwartet?

Im Idealfall wird davon ausgegangen, dass die Nutzung für die letzte Stunde stündlich ausgegeben wird, jedoch nur, wenn in der letzten Stunde Nutzung stattgefunden hat.

### <a name="is-there-a-maximal-period-between-one-emission-and-the-next-one"></a>Gibt es einen maximalen Zeitraum zwischen einer Ausgabe und der nächsten?

Es gibt keine solche Beschränkung. Eine Nutzung wird lediglich bei Auftreten ausgegeben. Wenn Sie beispielsweise nur eine Nutzungseinheit pro Abonnementlebensdauer übermitteln müssen, ist auch dies möglich.

### <a name="what-is-the-maximum-delay-between-the-time-an-event-occurs-and-the-time-a-usage-event-is-emitted-to-microsoft"></a>Wie groß ist die maximale Verzögerung zwischen dem Zeitpunkt, zu dem ein Ereignis eingetreten ist, und dem Zeitpunkt der Ausgabe des Nutzungsereignisses an Microsoft?

Idealerweise werden Nutzungsereignisse stündlich für Ereignisse ausgegeben, die in der letzten Stunde eingetreten sind. Es werden jedoch Verzögerungen erwartet. Die maximal zulässige Verzögerung beträgt 24 Stunden. Danach werden Nutzungsereignisse nicht mehr akzeptiert. Die bewährte Methode ist, die stündliche Nutzung zu erfassen und als ein Ereignis am Ende der Stunde auszugeben.

Wenn ein Nutzungsereignis beispielsweise um 1 Uhr auftritt, kann ein entsprechendes Nutzungsereignis bis 1 Uhr am Folgetag ausgegeben werden.  Wenn das System, von dem die Nutzung ausgegeben wird, ausfällt, kann es nach der Wiederherstellung das Nutzungsereignis für das Stundenintervall, in dem die Nutzung erfolgt ist, ohne Genauigkeitseinbußen senden.

Wenn 24 Stunden nach der tatsächlichen Nutzung vergangen sind, können die verbrauchten Einheiten immer noch mit späteren Nutzungsereignissen ausgegeben werden.  Diese Vorgehensweise kann jedoch die Glaubwürdigkeit der Abrechnungsereignisberichte für den Endkunden beeinträchtigen.  Es wird empfohlen, ein Senden der Ausgabe von Verbrauchseinheiten einmal pro Tag/Woche/Monat zu vermeiden.  Dadurch wird es schwieriger, die tatsächliche Nutzung durch einen Kunden zu verstehen und Probleme oder Fragen zu lösen, die sich möglicherweise in Bezug auf Nutzungsereignisse ergeben.

Durch ein stündliches Senden der Nutzung werden außerdem Situationen vermieden, in denen der Benutzer das Abonnement kündigt, bevor der Herausgeber das tägliche/wöchentliche/monatliche Ausgabeereignis sendet.

### <a name="what-happens-when-you-send-more-than-one-usage-event-in-the-same-hour"></a>Was geschieht, wenn in derselben Stunde mehrere Nutzungsereignisse gesendet werden?

Für das einstündige Intervall wird nur ein Nutzungsereignis akzeptiert. Das Stundenintervall beginnt in Minute 0 und endet mit Minute 59.  Wenn für die gleiche Stunde mehrere Nutzungsereignisse ausgegeben werden, werden alle Nutzungsereignisse nach dem ersten Ereignis als Duplikate gelöscht.

### <a name="what-happens-when-the-customer-cancels-the-purchase-within-the-time-allowed-by-the-cancellation-policy"></a>Was geschieht, wenn der Kunde den Kauf innerhalb der für die Kündigung zulässigen Zeit kündigt?

Der Pauschalbetrag wird nicht in Rechnung gestellt, jedoch die Überschreitungsnutzung.

### <a name="can-custom-meter-plans-be-used-for-one-time-payments"></a>Können Pläne für benutzerdefinierte Verbrauchseinheiten für einmalige Zahlungen verwendet werden?

Ja, Sie können eine benutzerdefinierte Dimension als eine Einheit für die einmalige Zahlung definieren und für jeden Kunden nur einmal ausgeben.

### <a name="can-custom-meter-plans-be-used-to-tiered-pricing-model"></a>Können Pläne für benutzerdefinierte Verbrauchseinheiten für ein Modell mit gestaffelten Preisen verwendet werden?

Ja, es kann mit jeder benutzerdefinierten Dimension implementiert werden, die eine einzelne Preisstufe darstellt.

Beispielsweise möchte Contoso 0,5 US-Dollar pro E-Mail für die ersten 1000 E-Mails, 0,4 US-Dollar pro E-Mail zwischen 1000 und 5000 E-Mails und 0,2 US-Dollar pro E-Mail bei mehr als 5000 E-Mails berechnen. Es können hierfür drei benutzerdefinierte Dimensionen definiert werden, die den drei Tarifen für E-Mails entsprechen. In diesem Fall werden Einheiten der ersten Dimension ausgegeben, solange die Anzahl der E-Mails unter 1000 bleibt. Einheiten der zweiten Dimension werden ausgegeben, wenn die Anzahl der E-Mails zwischen 1000 und 5000 liegt. Bei mehr als 5000 E-Mails werden schließlich Einheiten der dritten Dimension ausgegeben.

### <a name="what-happens-if-the-marketplace-metering-service-has-an-outage"></a>Was geschieht, wenn der Marketplace-Messungsdienst ausfällt?

Wenn der ISV eine benutzerdefinierte Verbrauchseinheit sendet und einen Fehler empfängt, der möglicherweise durch ein Problem auf Microsoft-Seite verursacht wird (in der Regel, wenn ähnliche Ereignisse zuvor ohne Fehler akzeptiert wurden), sollte der ISV warten und die Ausgabe wiederholen.

Wenn der Fehler weiterhin auftritt, sollte diese benutzerdefinierte Verbrauchseinheit in der nächsten Stunde erneut übermittelt werden (Menge kumulieren). Dieser Vorgang sollte dann fortgesetzt werden, bis eine fehlerfreie Antwort empfangen wird.

## <a name="for-saas-offers-only"></a>Nur für SaaS-Angebote

### <a name="what-happens-when-you-emit-usage-for-a-saas-subscription-that-has-been-unsubscribed-already"></a>Was geschieht, wenn die Nutzung für ein SaaS-Abonnement ausgegeben wird, das bereits gekündigt wurde?

An die Marketplace-Plattform ausgegebene Nutzungsereignisse werden nicht akzeptiert, nachdem ein SaaS-Abonnement gelöscht wurde.

Die Nutzung kann nur für Abonnements im Status „Subscribed“(Abonniert) ausgegeben werden (nicht für Abonnements im Status `PendingFulfillmentStart`, `Suspended` oder `Unsubscribed`).

Die einzige Ausnahme ist die Meldung der Nutzung für den Zeitraum vor Kündigung des SaaS-Abonnements.

Beispielsweise hat der Kunde das SaaS-Abonnement heute um 15 Uhr gekündigt. Es ist jetzt 17 Uhr, und der Herausgeber kann immer noch die Nutzung für den Zeitraum zwischen gestern 18 Uhr und heute 15 Uhr für dieses SaaS-Abonnement ausgeben.

### <a name="can-you-get-a-list-of-all-saas-subscriptions-including-active-and-unsubscribed-subscriptions"></a>Kann ich eine Liste aller SaaS-Abonnements, einschließlich aktiver und gekündigter Abonnements, abrufen?

Ja, durch Aufruf der [API zum Abrufen der Abonnementliste](pc-saas-fulfillment-api-v2.md#subscription-apis), da sie eine Liste aller SaaS-Abonnements enthält. Im Statusfeld in der Antwort für das jeweilige SaaS-Abonnement wird angegeben, ob das Abonnement aktiv oder gekündigt ist.

### <a name="are-the-start-and-end-dates-of-saas-subscription-term-and-overage-usage-emission-connected"></a>Sind das Start- und Enddatum der SaaS-Abonnementlaufzeit und die Ausgabe der Überschreitungsnutzung miteinander verbunden?

Überschreitungsereignisse können jederzeit für ein vorhandenes SaaS-Abonnement im Status *Subscribed* (Abonniert) ausgegeben werden. Es ist Aufgabe des Herausgebers, Nutzungsereignisse auf Grundlage der im Abrechnungsplan definierten Richtlinie auszugeben. Die Überschreitung muss basierend auf den Datumsangaben berechnet werden, die für den Abrechnungszeitraum des SaaS-Abonnements definiert sind. 

Wenn der Herausgeber z. B. einen SaaS-Plan definiert, bei dem 1000 E-Mails für 100 US-Dollar im monatlichen Pauschalpreis enthalten sind, wird für jede E-Mail über der Anzahl von 1000 eine benutzerdefinierte Dimension von 1 US-Dollar berechnet.

Wenn der Kunde das Abonnement am 6. Januar kauft und aktiviert, werden die im Pauschalpreis enthaltenen 1000 E-Mails ab diesem Tag gezählt. Wenn also bis zum 5. Februar (Ende des ersten Abonnementmonats) nur 900 E-Mails gesendet werden, zahlt der Kunde nur den Pauschalbetrag für den ersten Monat dieses Abonnements, und es werden zwischen dem 6. Januar und dem 5. Februar keine Überschreitungsereignisse vom Herausgeber ausgegeben. Am 6. Februar wird das Abonnement automatisch verlängert, und die Zählung wird erneut gestartet. Wenn der Kunde am 15. Februar bereits 1000 E-Mails gesendet hat, werden die restlichen E-Mails, die bis zum 5. März gesendet werden, als Überschreitung (1 US-Dollar pro E-Mail) basierend auf den vom Herausgeber ausgegebenen Überschreitungsereignissen in Rechnung gestellt.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen finden Sie unter [Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md).
