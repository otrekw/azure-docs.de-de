---
title: Abrechnung des kommerziellen Marketplace | Azure Marketplace
description: In diesem Artikel werden Themen zum Handel auf dem kommerziellen Marketplace behandelt.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.topic: guide
ms.date: 12/12/2019
ms.openlocfilehash: a0912e1dca63fabfe6bc546305824a1c582b9a2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279741"
---
# <a name="commercial-marketplace-billing"></a>Abrechnung des kommerziellen Marketplace

In diesem Artikel werden folgende Themen zum Handel auf dem kommerziellen Marketplace behandelt:

- [Veröffentlichungsoptionen in Marketplace](#marketplace-publishing-options)
- [Allgemeine Übersicht über die Veröffentlichungsoption „Transaktion“](#transact-general-overview)
- [Abrechnungsmodelle für die Veröffentlichungsoption „Transaktion“](#transact-billing-models)

## <a name="marketplace-publishing-options"></a>Veröffentlichungsoptionen in Marketplace

Der kommerzielle Marketplace bietet Herausgebern verschiedene Veröffentlichungsoptionen.

### <a name="list-and-trial-publishing-options"></a>Veröffentlichungsoptionen „Listung“ und „Testversion“

Herausgeber können die Veröffentlichungsoptionen „Listung“, „Testversion“ und „BYOL“ (Bring Your Own License) für Werbezwecke und zur Neukundengewinnung nutzen. Bei diesen Optionen ist Microsoft nicht direkt an den Softwarelizenztransaktionen des Herausgebers beteiligt, und es fallen keine damit verbundenen Transaktionsgebühren an. Herausgeber sind für die Unterstützung aller Aspekte der Softwarelizenztransaktion verantwortlich, einschließlich, aber nicht beschränkt auf: Auftrag, Auftragsabwicklung, Messung, Abrechnung, Rechnungsstellung, Zahlung und Inkasso. Bei den Veröffentlichungsoptionen „Listung“ und „Testversion“ behalten Herausgeber 100 % der vom Kunden eingezogenen Lizenzgebühren für Software ein.

### <a name="transact-publishing-option"></a>Veröffentlichungsoption „Transaktion“

Zusätzlich zu den Veröffentlichungsoptionen „Listung“ und „Testversion“ steht Herausgebern die Veröffentlichungsoption „Transaktion“ zur Verfügung. Bei dieser Option werden die global verfügbaren E-Commerce-Funktionen von Microsoft genutzt, und Microsoft kann Marketplace-Cloudtransaktionen im Namen des Herausgebers hosten.

## <a name="transact-general-overview"></a>Allgemeine Übersicht über die Veröffentlichungsoption „Transaktion“

Bei Verwendung der Veröffentlichungsoption „Transaktion“ ermöglicht Microsoft den Verkauf von Software von Drittanbietern und die Bereitstellung einiger Angebotstypen für das Azure-Abonnement des Kunden. Der Herausgeber muss bei der Auswahl eines Abrechnungsmodells und Angebotstyps die Abrechnung der Infrastrukturgebühren und der eigenen Softwarelizenzgebühren des Herausgebers berücksichtigen.

Die Veröffentlichungsoption „Transaktion“ wird derzeit für die folgenden Angebotstypen unterstützt: Virtuelle Computer, Azure-Anwendungen und SaaS-Anwendungen.

![Transaktionen im Azure Marketplace](./media/transact-amp.png)

### <a name="billing-infrastructure-costs"></a>Abrechnung von Infrastrukturkosten

#### <a name="for-virtual-machines-and-azure-applications"></a>Für virtuelle Computer und Azure-Anwendungen

Für virtuelle Computer und Azure-Anwendungen werden die Nutzungsgebühren für Azure-Infrastruktur im Azure-Abonnement des Kunden in Rechnung gestellt. Auf der Rechnung des Kunden werden die Infrastrukturnutzungsgebühren separat von den Lizenzgebühren des Softwareanbieters ausgewiesen.

#### <a name="for-saas-apps"></a>Für SaaS-Apps

Bei SaaS-Apps muss der Herausgeber die Nutzungsgebühren für Azure-Infrastruktur und Softwarelizenzgebühren als einen Kostenposten verrechnen. Diese werden dem Kunden als Pauschalgebühr angegeben. Die Nutzung der Azure-Infrastruktur wird verwaltet und dem Partner direkt in Rechnung gestellt. Tatsächliche Nutzungsgebühren für die Infrastruktur werden dem Kunden nicht angezeigt. Herausgeber entscheiden sich in der Regel dafür, die Nutzungsgebühren für Azure-Infrastruktur in ihren Softwarelizenzpreis einfließen zu lassen. Softwarelizenzgebühren werden nicht gemessen oder nutzungsabhängig berechnet.

## <a name="transact-billing-models"></a>Abrechnungsmodelle für die Veröffentlichungsoption „Transaktion“

Abhängig von der verwendeten Transaktionsoption können die Softwarelizenzgebühren des Herausgebers wie folgt ausgewiesen werden:

- *Free:* Keine Kosten für Softwarelizenzen.
- *BYOL (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz):* Alle anfallenden Gebühren für Softwarelizenzen werden direkt zwischen Herausgeber und Kunde geregelt. Microsoft wickelt nur die Nutzungsgebühren für Azure-Infrastruktur ab. (Nur für virtuelle Computer und Azure-Anwendungen.)
- *Nutzungsbasierte Bezahlung:* Softwarelizenzgebühren werden basierend auf der verwendeten Azure-Infrastruktur als Preis pro Stunde und pro Kern (vCPU) ausgewiesen. Dieses Modell gilt nur für virtuelle Computer und Azure-Anwendungen.
- *Abonnementpreise:* Softwarelizenzgebühren fallen als monatliche oder jährliche wiederkehrende Gebühren an, die als Pauschalgebühr oder pro Arbeitsplatz berechnet werden. Dieses Modell gilt nur für SaaS-Apps und verwaltete Azure-Anwendungen.
- *Kostenlose Softwaretestversion:* Keine Kosten für Softwarelizenzen für 30 bzw. 90 Tage.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Preise für die Modelle „Kostenlos“ und „BYOL“ (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)

Bei der Veröffentlichung eines kostenlosen oder BYOL- Transaktionsangebots spielt Microsoft keine Rolle bei der Abwicklung des Verkaufsvorgangs für Ihre Softwarelizenzgebühren. Wie bei den Veröffentlichungsoptionen „Listung“ und „Testversion“ behält der Herausgeber 100 % der Softwarelizenzgebühren ein.

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Nutzungsbasierte Bezahlung und Abonnementpreis (standortbasiert)

Bei der Veröffentlichung eine Transaktionsangebots mit dem Modell „Nutzungsbasierte Bezahlung“ oder „Abonnement“ stellt Microsoft die Technologie und Dienste zur Verfügung, um Käufe, Rückgaben und Rückbuchungen von Softwarelizenzen zu verarbeiten. In diesem Szenario autorisiert der Herausgeber Microsoft, als Vermittler für diese Zwecke zu fungieren. Der Herausgeber erlaubt Microsoft die Abwicklung der Softwarelizenzierungstransaktion, wobei er seine Kennzeichnung als Verkäufer, Anbieter, Distributor und Lizenzgeber beibehält.

Microsoft ermöglicht es Kunden, Software des Herausgebers zu bestellen, zu lizenzieren und zu nutzen, wobei die Bedingungen des kommerziellen Marketplace von Microsoft und des Endbenutzerlizenzvertrags des Herausgebers gelten. Herausgeber müssen ihren Endbenutzerlizenzvertrag bereitstellen oder beim Erstellen des Angebots den [Standardvertrag](https://docs.microsoft.com/azure/marketplace/standard-contract) auswählen.

### <a name="free-software-trials"></a>Kostenlose Testversionen

Bei Veröffentlichungsszenarien des Typs „Transaktion“ kann der Herausgeber eine Softwarelizenz 30 oder 90 Tage kostenlos zur Verfügung stellen. Diese Rabattierungsmöglichkeit schließt nicht die Kosten für die Nutzung der Azure-Infrastruktur ein, die durch die Nutzung der Partnerlösung verursacht wird.

### <a name="private-offers"></a>Private Angebote

Zusätzlich zur Verwendung von Angebotstypen und Abrechnungsmodellen zur Verrechnung eines Angebots können Herausgeber ein vollständiges privates Angebot mit ausgehandelten, transaktionsspezifischen Preisen oder kundenspezifischen Konfigurationen anbieten. Private Angebote werden von allen drei Veröffentlichungsoptionen des Typs „Transaktion“ unterstützt.

Diese Option ermöglicht höhere oder niedrigere Preise als für das öffentlich verfügbare Angebot. Private Angebote können genutzt werden, um Rabatte zu gewähren oder eine Prämie zu einem Angebot hinzuzufügen. Private Angebote können einem oder mehreren Kunden bereitgestellt werden, indem sie ihr Azure-Abonnement auf Angebotsebene auf die Zulassungsliste setzen.

### <a name="examples"></a>Beispiele

#### <a name="pay-as-you-go"></a>Nutzungsbasierte Bezahlung

* Wenn Sie die Option mit nutzungsbasierter Bezahlung aktivieren, sieht die Kostenstruktur folgendermaßen aus.

|Ihre Lizenzkosten  | 1,00 USD pro Stunde  |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    |   0,14 USD pro Stunde     |
|*Microsoft führt die Abrechnung mit dem Kunden durch*    |  *1,14 USD pro Stunde*       |

* In diesem Szenario berechnet Microsoft 1,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

|Berechnung durch Microsoft  | 1,14 USD pro Stunde  |
|---------|---------|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten|   0,80 USD pro Stunde     |
|Microsoft behält 20% Ihrer Lizenzkosten ein  |  0,20 USD pro Stunde       |
|Microsoft behält die Kosten der Azure-Nutzung zu 100 % ein | 0,14 USD pro Stunde |

### <a name="bring-your-own-license-byol"></a>BYOL (Bring-Your-Own-License, Verwendung Ihrer eigenen Lizenz)

* Wenn Sie die BYOL-Option aktivieren, sieht die Kostenstruktur folgendermaßen aus.

|Ihre Lizenzkosten  | Ausgehandelte und von Ihnen in Rechnung gestellte Lizenzgebühr  |
|---------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    |   0,14 USD pro Stunde     |
|*Microsoft führt die Abrechnung mit dem Kunden durch*    |  *0,14 USD pro Stunde*       |

* In diesem Szenario berechnet Microsoft 0,14 USD pro Stunde für die Verwendung Ihres veröffentlichten VM-Images.

|Berechnung durch Microsoft  | 0,14 USD pro Stunde  |
|---------|---------|
|Microsoft behält die Kosten der Azure-Nutzung ein    |   0,14 USD pro Stunde     |
|Microsoft behält 0% Ihrer Lizenzkosten ein   |  0,00 USD pro Stunde       |

### <a name="saas-app-subscription"></a>SaaS-App-Abonnement

Diese Option muss für den Verkauf über Microsoft konfiguriert sein und kann per Pauschalpreis oder pro Benutzer monatlich oder jährlich berechnet werden.

•   Wenn Sie für ein SaaS-Angebot die Option „Über Microsoft verkaufen“ aktivieren, verfügen Sie über die folgende Kostenstruktur.

|Ihre Lizenzkosten       | 100,00 USD pro Monat  |
|--------------|---------|
|Azure-Nutzungskosten (D1/1-Kern)    | Abrechnung direkt mit dem Herausgeber, nicht mit dem Kunden |
|*Microsoft führt die Abrechnung mit dem Kunden durch*    |  *100,00 USD pro Monat (Hinweis: Herausgeber muss alle anfallenden oder weiterzugebenden Infrastrukturkosten in der Lizenzgebühr berücksichtigen)*  |

* In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 80,00 USD an den Herausgeber aus.
* Für Partner, die für die reduzierte Marketplace-Dienstgebühr qualifiziert sind, wird von Mai 2019 bis Juni 2020 eine reduzierte Transaktionsgebühr für die SaaS-Angebote angezeigt. In diesem Szenario stellt Microsoft 100,00 USD für Ihre Softwarelizenz in Rechnung und zahlt 90,00 USD an den Herausgeber aus.

|Berechnung durch Microsoft  | 100,00 USD pro Monat  |
|---------|---------|
|Microsoft zahlt Ihnen 80% Ihrer Lizenzkosten <br> \* Für jede qualifizierte SaaS-App zahlt Microsoft 90 % Ihrer Lizenzkosten.   |   80,00 USD pro Monat <br> \* 90,00 USD pro Monat    |
|Microsoft behält 20% Ihrer Lizenzkosten ein <br> \* Für jede qualifizierte SaaS-App behält Microsoft 10 % Ihrer Lizenzkosten.  |  20,00 USD pro Monat <br> \* 10,00 USD     |

**Reduzierte Marketplace-Dienstgebühr:** Für SaaS-Produkte, die Sie in unserem kommerziellen Marketplace veröffentlichen, reduziert Microsoft die Marketplace-Dienstgebühr von 20 % (wie in der Microsoft-Herausgebervereinbarung angegeben) auf 10 %.  Damit Ihr Produkt qualifiziert ist, muss mindestens eines Ihrer Produkte von Microsoft als bereit für IP-Co-Selling oder als priorisiert für IP-Co-Selling gekennzeichnet sein. Die Berechtigung muss mindestens fünf (5) Arbeitstage vor dem Ende des vorherigen Kalendermonats bestehen, damit die reduzierte Marketplace-Dienstgebühr für den Monat in Anspruch genommen werden kann. Die reduzierte Marketplace-Dienstgebühr gilt nicht für virtuelle Computer, verwaltete Apps oder andere Produkte, die über unseren kommerziellen Marketplace zur Verfügung gestellt werden.  Die reduzierte Marketplace-Dienstgebühr ist für qualifizierte Angebote verfügbar, wobei die Lizenzgebühren von Microsoft zwischen dem 1. Mai 2019 und dem 30. Juni 2020 vereinnahmt werden.  Nach diesem Zeitpunkt gilt für die Marketplace-Dienstgebühr wieder der normale Betrag.
