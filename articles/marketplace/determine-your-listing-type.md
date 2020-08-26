---
title: Bestimmen Ihrer Veröffentlichungsoption im kommerziellen Microsoft-Marketplace
description: In diesem Artikel werden die Eignungskriterien und Bedingungen für die Veröffentlichung von Angeboten in Microsoft AppSource und im Azure Marketplace beschrieben.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 07/30/2020
ms.openlocfilehash: 1b0846c68f6860b5c7dac9e93808088dac4f6a05
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607437"
---
# <a name="determine-your-publishing-option"></a>Bestimmung Ihrer Veröffentlichungsoption

Die Veröffentlichungsoption, die Sie für Ihr Angebot auswählen, bezieht sich direkt sowohl auf die erforderlichen Berechtigungen als auch die kommerziellen Marketplace-GTM-Vorteile. Wichtiger ist, dass durch die Auswahl der Veröffentlichungsoption und des Angebotstyps definiert wird, wie Benutzer mit Ihrem kommerziellen Marketplace-Angebot interagieren.

Sie müssen zum Konfigurieren Ihres Angebots die folgenden wesentlichen Konzepte des kommerziellen Marketplace verstehen: Veröffentlichungsoptionen, Angebotstypen und -konfigurationen sowie Handlungsaufforderungen, die regeln, wie und wo Ihr Angebot in den Onlineshops des kommerziellen Marketplace präsentiert wird.

In diesem Artikel lernen Sie Folgendes:

- Bestimmen des geeigneten Onlineshops für Ihre Lösung
- Welche Veröffentlichungsoptionen und Handlungsaufforderungen in den einzelnen Onlineshops zur Verfügung stehen
- Welche Angebotstypen für die einzelnen Veröffentlichungsoptionen verfügbar sind

## <a name="commercial-marketplace-publishing-options"></a>Veröffentlichungsoptionen im kommerziellen Marketplace

In der folgenden Tabelle sind die Veröffentlichungsoptionen für Angebotstypen in Microsoft AppSource und Azure Marketplace aufgeführt.

|   | **Listung (Kontakt)**  | **Listung (Test)**  | **Free** | **BYOL** | **Transaktion**|
| :--------- | :----------- | :------------ | :----------- | :---------- |:---------- |
| **Virtueller Computer** |  |  |  | Azure Marketplace |  Azure Marketplace |
| **Azure-Apps (mehrere VMs)** |  |  | Azure Marketplace | Azure Marketplace | Azure Marketplace  |
| **Containerimage** |  |  | Azure Marketplace | Azure Marketplace |   |
| **IoT Edge-Modul** |  |  | Azure Marketplace | Azure Marketplace |   |
| **Verwaltete Dienste** |  |  |  | Azure Marketplace |   |
| **Beratungsdienste** | Beide Onlineshops |  |  |  |   |
| **SaaS-App** | Beide Onlineshops | Beide Onlineshops | Beide Onlineshops |  | Beide Onlineshops* |
| **Microsoft 365-App** | AppSource | AppSource |  |  | AppSource**  |
| **Dynamics 365-Add-In** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

&#42; SaaS-App-Transaktionsangebote in Microsoft AppSource können aktuell nur per Kreditkarte bezahlt werden.

&#42;&#42; Microsoft 365-Angebote können kostenlos installiert und über das SaaS-Angebot als Lizenzierungsdienst monetarisiert werden. Weitere Informationen finden Sie unter [Monetarisieren Ihres Office 365-Add-Ins über den kommerziellen Microsoft-Marketplace](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace).

## <a name="choose-a-call-to-action"></a>Auswählen einer Handlungsaufforderung

Die verfügbaren Veröffentlichungsoptionen ermöglichen eine differenzierte Kundenbindung und ebnen Ihnen den Weg zu gemeinsamem Zugriff auf Leads und [Vorteile des kommerziellen Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits). Beachten Sie die Aktionsaufrufe, die mit den Veröffentlichungsoptionen verbunden sind:

| **Veröffentlichungsoption**    | **Beschreibung**  |
| :------------------- | :-------------------|
| **Liste** | Dies ist eine einfache Auflistung Ihrer Anwendung oder Ihres Diensts, sodass kommerzielle Marketplace-Benutzer Sie über die Handlungsaufforderung **Kontakt mit mir aufnehmen** bitten können, sich mit dem Kunden in Verbindung zu setzen. |
| **Testversion** | Verwenden Sie den kommerziellen Marketplace, um die Auffindbarkeit zu verbessern und die Bereitstellung einer Testversion Ihrer Lösung zu automatisieren, sodass potenzielle Benutzer vor dem Kauf für einen begrenzten Zeitraum kostenlos einen Eindruck Ihrer SaaS-, IaaS- oder Microsoft-In-App-Benutzeroberfläche gewinnen können. Für die Veröffentlichungsoption „Testen“ werden folgende Aktionsaufrufe verwendet: **Kostenlose Testversion** oder **Testversion**. |
| **BYOL** | Verwenden Sie den kommerziellen Marketplace, um die Ermittelbarkeit zu verbessern und die Bereitstellung Ihrer Lösung zu automatisieren sowie die Finanztransaktion separat durchzuführen. BYOL-Angebotstypen sind ideal für die Migration aus der lokalen Umgebung in die Cloud geeignet. Die Handlungsaufforderung lautet **Jetzt anfordern**.
| **Transaktion** | Transaktionsangebote werden über den kommerziellen Marketplace verkauft. Microsoft ist für die Abrechnung und für Sammlungen verantwortlich. Die Handlungsaufforderung lautet **Jetzt anfordern**.|

> [!Note]
> Bei Verwendung der Veröffentlichungsoption „Transaktion“ sollten Sie sich zunächst mit der Preisgestaltung, Abrechnung, Rechnungsstellung und Auszahlung vertraut machen, bevor Sie einen Angebotstyp auswählen und Ihr Angebot erstellen. Weitere Informationen finden Sie im Artikel [Transaktionsfunktionen im kommerziellen Marketplace](./marketplace-commercial-transaction-capabilities-and-considerations.md).

## <a name="selecting-an-online-store"></a>Auswählen eines Onlineshops

Jeder Onlineshop bedient spezielle Kundenanforderungen und richtet sich an bestimmte Zielgruppen. Ihr Angebotstyp, Ihre Transaktionsfunktionen und Ihre Kategorie bestimmen, wo Ihr Angebot veröffentlicht wird. Kategorien und Unterkategorien werden den einzelnen Onlineshops basierend auf der Zielgruppe zugeordnet:

**Microsoft AppSource** richtet sich an Geschäftskunden, die branchenspezifische Lösungen und Beratungsdienste für Dynamics 365, Microsoft 365 und Power Platform benötigen.

**Azure Marketplace** richtet sich an IT-Experten und Entwickler, die sich für Lösungen interessieren, die für oder in Azure entwickelt wurden, sowie für Beratungsdienste, die Ihre Nutzung von Azure beschleunigen.

Wählen Sie die Kategorie und Unterkategorie aus, die am besten auf Ihre Zielgruppe ausgerichtet ist. Beispielsweise sollte eine Web Application Firewall im Azure Marketplace unter der Kategorie „Sicherheit“ veröffentlicht werden, da es sich bei der Zielgruppe um IT-Experten handelt. Eine Anwendung für das Vertragsmanagement sollte stattdessen in AppSource unter der Kategorie „Vertrieb“ veröffentlicht werden, da es sich bei der Zielgruppe um Geschäftskunden handelt. Die Auswahl der falschen Kategorie oder Unterkategorie kann dazu führen, dass das Angebot im falschen Onlineshop veröffentlicht wird.

### <a name="publishing-to-both-online-stores-saas-offers-only"></a>Veröffentlichen in beiden Onlineshops (nur SaaS-Angebote)

SaaS-Angebote können im Azure Marketplace oder in AppSource veröffentlicht werden. Wenn Ihr SaaS-Angebot für eine technische Zielgruppe (Azure Marketplace) *und* für Geschäftskunden (AppSource) ausgelegt ist, wählen Sie eine Kategorie und/oder Unterkategorie aus, die für jeden Onlineshop gilt. Bei Angeboten, die in beiden Onlineshops veröffentlicht werden, sollte sich das Nutzenversprechen auf IT-Experten *und* auf Geschäftskunden beziehen.

> [!IMPORTANT]
> SaaS-Angebote mit getakteter Abrechnung sind über den Azure Marketplace und das Azure-Portal verfügbar. SaaS-Angebote, die nur private Pläne umfassen, sind über das Azure-Portal verfügbar.

| Getaktete Abrechnung | Öffentlicher Plan | Privater Tarif | Verfügbar in: |
|---|---|---|---|
| Ja             | Ja         | Nein           | Azure Marketplace und Azure-Portal |
| Ja             | Ja         | Ja          | Azure Marketplace und Azure-Portal* |
| Ja             | Nein          | Ja          | Nur Azure-Portal |
| Nein              | Nein          | Ja          | Nur Azure-Portal |

&#42; Der private Plan des Angebots ist nur über das Azure-Portal verfügbar.

Beispielsweise wird ein Angebot mit getakteter Abrechnung, das nur einen privaten Plan (keinen öffentlichen Plan) umfasst, von Kunden im Azure-Portal erworben. Erfahren Sie mehr über [Private Angebote im kommerziellen Microsoft-Marketplace](private-offers.md).

### <a name="categories"></a>Kategorien

Kategorien und Unterkategorien werden den einzelnen Onlineshops basierend auf der Zielgruppe zugeordnet. Wählen Sie die Kategorien und Unterkategorien aus, die am besten zu Ihrem Angebot und Ihrer Zielgruppe passen. Sie können Folgendes auswählen:

- Wählen Sie mindestens eine und maximal zwei Kategorien aus. Sie können eine primäre und eine sekundäre Kategorie auswählen.
- Bis zu zwei Unterkategorien für jede primäre und/oder sekundäre Kategorie. Wenn keine Unterkategorie ausgewählt wird, ist das Angebot weiterhin unter der ausgewählten Kategorie auffindbar.

[!INCLUDE [categories and subcategories](./includes/categories.md)]

## <a name="next-steps"></a>Nächste Schritte

- Sobald Sie sich für eine Veröffentlichungsoption entschieden haben, können Sie den [Angebotstyp auswählen](./publisher-guide-by-offer-type.md), der verwendet wird, um Ihr Angebot zu präsentieren.
- Informieren Sie sich über die erforderlichen Berechtigungen in den Veröffentlichungsoptionen des entsprechenden Angebotstypabschnitts, um Auswahl und Konfiguration Ihres Angebots abzuschließen.
- Entnehmen Sie den Veröffentlichungsmustern der Onlineshops Beispiele für die Zuordnung Ihrer Lösung zu einem Angebotstyp und einer Konfiguration.
