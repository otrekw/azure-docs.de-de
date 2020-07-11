---
title: Azure Marketplace
description: Hier wird beschrieben, wie EA-Kunden den Azure Marketplace verwenden können.
author: bandersmsft
ms.reviewer: baolcsva
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 07/06/2020
ms.author: banders
ms.openlocfilehash: acb818cf2a892a3a71f2f59b0a44041767b846d0
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86044922"
---
# <a name="azure-marketplace"></a>Azure Marketplace

In diesem Artikel wird erläutert, wie EA-Kunden und -Partner die Marketplace-Gebühren anzeigen und Azure Marketplace-Käufe aktivieren können.

## <a name="azure-marketplace-for-ea-customers"></a>Azure Marketplace für EA-Kunden

Direkten EA-Kunden werden die Azure Marketplace-Gebühren im Azure Enterprise Portal angezeigt. Alle Azure Marketplace-Käufe und deren Nutzung werden außerhalb der Azure-Vorauszahlung abgerechnet, wobei die Rechnungsstellung vierteljährlich oder monatlich und nachträglich erfolgt.

Indirekte Kunden finden ihre Azure Marketplace-Abonnements im Azure Enterprise Portal auf der Seite **Abonnements verwalten**, wobei die Preise jedoch ausgeblendet sind. Kunden sollten ihren Lizenzierungsdienstanbieter (Licensing Solutions Provider, LSP) zwecks Informationen zu Azure Marketplace-Gebühren kontaktieren.

Neue monatlich oder jährlich wiederkehrende Azure Marketplace-Käufe werden innerhalb des Zeitraums, in dem Azure Marketplace-Artikel erworben werden, vollständig abgerechnet. Diese Artikel werden im folgenden Zeitraum am selben Tag des ursprünglichen Kaufs automatisch verlängert.

Vorhandene monatlich wiederkehrende Gebühren werden weiterhin am ersten jedes Kalendermonats verlängert. Die jährlichen Gebühren werden am Jahrestag des Kaufdatums verlängert.

Manche auf dem Azure Marketplace verfügbaren Wiederverkaufsdienste von Drittanbietern werden jetzt auf den Azure-Vorauszahlungssaldo Ihres Enterprise Agreement (EA) angerechnet. Vorher wurden diese Dienste außerhalb des Azure EA-Vorauszahlungssaldos und separat abgerechnet. Der Azure EA-Vorauszahlungssaldo für diese Dienste im Azure Marketplace vereinfacht die Verwaltung von kundenseitigen Einkäufen und Zahlungen. Eine umfassende Liste der Dienste, die jetzt auf die Azure-Vorauszahlung angerechnet werden, finden Sie im [Update der Azure-Website vom 6. März 2018](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/).

### <a name="partners"></a>Partner

LSPs können eine Azure Marketplace-Preisliste von der Preisblattseite im Azure Enterprise Portal herunterladen. Wählen Sie den Link zur **Marketplace-Preisliste** in der oberen rechten Ecke aus. Die Azure Marketplace-Preisliste zeigt alle verfügbaren Dienste und ihre Preise an.

So laden Sie die Preisliste herunter:

1. Navigieren Sie im Azure Enterprise Portal zu **Berichte** > **Preisblatt**.
1. Suchen Sie in der oberen rechten Ecke den Link zur Azure Marketplace-Preisliste unter Ihrem Benutzernamen.
1. Klicken Sie mit der rechten Maustaste auf den Link, und wählen Sie **Ziel speichern unter** aus.
1. Ändern Sie im Fenster **Speichern** den Titel des Dokuments in `AzureMarketplacePricelist.zip`, wodurch die Datei von einer XLSX-Datei in eine ZIP-Datei geändert wird.
1. Nach dem Download verfügen Sie über eine ZIP-Datei mit länderspezifischen Preislisten.
1. LSPs sollten für länderspezifische Preise die jeweilige Länderdatei nutzen. LSPs können mithilfe der Registerkarte **Benachrichtigungen** erkennen, welche SKUs neu oder abgelaufen sind.
1. Preisänderungen treten selten auf. LSPs erhalten 30 Tage im voraus E-Mail-Benachrichtigungen über Preissteigerungen und Änderungen bei Devisenumrechnungen (Foreign Exchange, FX).
1. LSPs erhalten pro Registrierung, ISV und Quartal eine Rechnung.

### <a name="enabling-azure-marketplace-purchases"></a>Aktivieren von Azure Marketplace-Einkäufen

Unternehmensadministratoren können Azure Marketplace-Käufe für alle Azure-Abonnements unter dieser Registrierung deaktivieren oder aktivieren. Wenn der Unternehmensadministrator Käufe deaktiviert, und Azure-Abonnements vorhanden sind, die bereits über Azure Marketplace-Abonnements verfügen, werden diese Azure Marketplace-Abonnements nicht abgebrochen oder beeinträchtigt.

Obwohl Kunden ihre direkten Azure-Abonnements in Azure EA konvertieren können, indem sie sie ihrer Registrierung im Azure Enterprise Portal zuordnen, werden die untergeordneten Abonnements von dieser Aktion nicht automatisch konvertiert.

So aktivieren Sie Azure Marketplace-Einkäufe:

1. Melden Sie sich beim Azure Enterprise-Portal als Unternehmensadministrator an.
1. Navigieren Sie zu **Verwalten**.
1. Wählen Sie unter **Registrierungsdetails** das Stiftsymbol neben dem Zeilenelement **Azure Marketplace** aus.
1. Schalten Sie nach Bedarf zwischen **Aktiviert/Deaktiviert** oder **Nur kostenlose/BYOL-SKUs** um.
1. Wählen Sie **Speichern** aus.

> [!NOTE]
> BYOL (Bring-Your-Own-License) und die Option „Nur kostenlos“ beschränken Kaufen und Erwerben von Azure Marketplace-SKUs auf BYOL- und kostenlose SKUs.

### <a name="services-billed-hourly-for-azure-ea"></a>Stündlich für Azure EA abgerechnete Dienste

Die folgenden Dienste werden stundenweise unter einem Enterprise Agreement und nicht in monatlichen Raten in MOSP abgerechnet:

- Application Delivery Network
- Web Application Firewall

### <a name="azure-remoteapp"></a>Azure RemoteApp

Wenn Sie über ein Enterprise Agreement verfügen, zahlen Sie für Azure RemoteApp auf Basis Ihrer Enterprise Agreement-Preisstufe. Es fallen keine zusätzlichen Gebühren an. Der Standardpreis umfasst die ersten 40 Stunden. Der unbegrenzte Preis deckt die ersten 80 Stunden ab. RemoteApp verhindert die 80 Stunden überschreitende Nutzung.

## <a name="azure-marketplace-faq"></a>Azure Marketplace – häufig gestellte Fragen

In diesem Abschnitt wird erläutert, wie Ihre Azure-Vorauszahlung für einige Resellerdienste von Drittanbietern in Azure Marketplace gelten kann.

### <a name="what-changed-with-azure-marketplace-services-and-azure-ea-prepayment"></a>Was hat sich mit Azure Marketplace-Diensten und Azure EA-Vorauszahlung geändert?

Ab dem 1. März 2018 nutzen einige Resellerdienste von Drittanbietern die Azure EA-Vorauszahlung. Mit Ausnahme der reservierten Azure-VM-Instanzen (RIs) wurden Dienste zuvor außerhalb der Azure EA-Vorauszahlung und separat abgerechnet.

Wir erweiterten die Nutzung der Azure-Vorauszahlung, um nun einige der von Drittanbietern im Azure Marketplace veröffentlichten Dienste einzubeziehen, die am häufigsten gekauft werden. Die Azure EA-Vorauszahlung für diese Dienste im Azure Marketplace vereinfacht die Verwaltung Ihrer Einkäufe und Zahlungen.

### <a name="why-did-we-make-this-change"></a>Warum haben wir diese Änderung vorgenommen?

Kunden suchen ständig nach zusätzlichen Möglichkeiten, um die Azure-Vorauszahlung zu nutzen. Diese Änderung wurde häufig von Kunden angefordert, und dies betraf einen großen Teil der Azure Marketplace-Kunden.

### <a name="how-do-you-benefit"></a>Wie profitieren Sie davon?

Sie erhalten eine einfachere Abrechnung und sind eher in der Lage, ihre Azure EA-Vorauszahlung zu nutzen. Da diese Dienste in Ihrer Azure-Vorauszahlung enthalten sind, gewinnt Ihre Azure EA-Vorauszahlung an Wert.

### <a name="what-azure-marketplace-services-use-azure-ea-prepayment-and-how-do-i-know"></a>Welche Azure Marketplace-Dienste verwenden die Azure EA-Vorauszahlung, und wie stelle ich das fest?

Wenn Sie einen Dienst erwerben, der die Azure-Vorauszahlung verwendet, zeigt Azure Marketplace einen Haftungsausschluss dar. Unterstützt werden einige Dienste, die von Red Hat, SUSE, Autodesk und Oracle veröffentlicht werden. Zurzeit werden ähnlich benannte Dienste, die von anderen Parteien veröffentlicht werden, nicht von der Azure-Vorauszahlung abgezogen. Eine vollständige Liste ist am Ende dieser FAQ verfügbar.

### <a name="what-if-my-azure-ea-prepayment-runs-out"></a>Was geschieht, wenn meine Azure EA-Vorauszahlung abläuft?

Wenn Sie Ihre gesamte Azure-Vorauszahlung genutzt haben und sich nun in der Überschreitung befinden, werden die Gebühren für diese Dienste in der nächsten Überschreitungsrechnung zusammen mit anderen Verbrauchsdiensten angezeigt. Vor der Änderung vom 1. März 2018 wurden diese Gebühren mit anderen Azure Marketplace-Diensten abgerechnet.

### <a name="why-dont-all-azure-marketplaces-consume-azure-ea-prepayment"></a>Warum nutzen nicht alle Azure Marketplace-Orte die Azure EA-Vorauszahlung?

Wir arbeiten regelmäßig daran, für die beste Kundenerfahrung im Zusammenhang mit der Azure EA-Vorauszahlung zu sorgen. Diese Änderung betrifft eine große Anzahl von Kunden und einen bedeutenden Teil der Gesamtausgaben im Azure Marketplace. Weitere Dienste könnten künftig hinzugefügt werden.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>Wie wirkt sich dies auf indirekte Registrierung und Partner aus?

Unsere Kunden oder Partner mit indirekter Registrierung sind nicht betroffen. Diese Dienste unterliegen denselben Partnermarkupfunktionen wie andere Verbrauchsdienste. Die einzige Änderung besteht darin, dass die Gebühren in einer anderen Rechnung angezeigt werden und die Zahlung der Gebühren aus der Azure EA-Vorauszahlung des Kunden erfolgt.

### <a name="is-there-a-list-of-azure-marketplace-services-that-consume-azure-ea-prepayment"></a>Gibt es eine Liste mit Azure Marketplace-Diensten, die eine Azure EA-Vorauszahlung nutzen?

Bestimmte Azure Marketplace-Angebote können per Azure-Vorauszahlungsguthaben bezahlt werden. Eine vollständige Liste der Produkte in diesem Programm finden Sie unter [Wiederverkaufsdienste von Drittanbietern im Azure Marketplace verwenden ab sofort die Azure-Vorauszahlung](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment).


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Preisen](ea-pricing-overview.md).