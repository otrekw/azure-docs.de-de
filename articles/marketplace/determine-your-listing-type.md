---
title: Bestimmen Ihrer Veröffentlichungsoption im kommerziellen Microsoft-Marketplace
description: In diesem Artikel werden die Eignungskriterien und Bedingungen für die Veröffentlichung von Angeboten in Microsoft AppSource und im Azure Marketplace beschrieben.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 06/22/2020
ms.openlocfilehash: 3d8692d3180e4164bff544f71a1216097a390773
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86103652"
---
# <a name="determine-your-publishing-option"></a>Bestimmung Ihrer Veröffentlichungsoption

Die Veröffentlichungsoption, die Sie für Ihr Angebot auswählen, bezieht sich direkt sowohl auf die erforderlichen Berechtigungen als auch die kommerziellen Marketplace-GTM-Vorteile. Wichtiger ist, dass durch die Auswahl der Veröffentlichungsoption und des Angebotstyps definiert wird, wie Benutzer mit Ihrem kommerziellen Marketplace-Angebot interagieren.

Sie müssen zum Konfigurieren Ihres Angebots die folgenden wesentlichen kommerziellen Marketplace-Konzepte verstehen: Veröffentlichungsoptionen, Angebotstypen und -konfigurationen sowie Handlungsaufforderungen, die regeln, wie und wo Ihr Angebot in den kommerziellen Marketplace-Storefronts präsentiert wird.

In diesem Artikel lernen Sie Folgendes:

- Bestimmen der geeigneten Storefront für Ihre Lösung
- Welche Veröffentlichungsoptionen und Aktionsaufrufe in den jeweiligen Storefronts zur Verfügung stehen
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
| **Beratungsdienste** | Beide Storefronts |  |  |  |   |
| **SaaS-App** | Beide Storefronts | Beide Storefronts | Beide Storefronts |  | Beide Storefronts* |
| **Microsoft 365-App** | AppSource | AppSource |  |  | AppSource**  |
| **Dynamics 365-Add-In** |  AppSource | AppSource |  |  |   |
| **PowerApps** | AppSource |AppSource  |  |  |   |

&#42; SaaS-App-Transaktionsangebote in Microsoft AppSource können aktuell nur per Kreditkarte bezahlt werden.

&#42;&#42; Microsoft 365-Angebote können kostenlos installiert und über das SaaS-Angebot als Lizenzierungsdienst monetarisiert werden. Weitere Informationen finden Sie unter [Monetarisieren Ihres Office 365-Add-Ins über den kommerziellen Microsoft-Marketplace](/office/dev/store/monetize-addins-through-microsoft-commercial-marketplace).

## <a name="selecting-a-storefront"></a>Auswählen einer Storefront

Bevor Sie eine Veröffentlichungsoption auswählen, müssen Sie die erforderlichen Storefrontberechtigungen für kommerzielle Marketplace-Lösungen, -Apps und -Dienste kennen. Jede Storefront bedient spezielle Kundenanforderungen und richtet sich an bestimmte Zielgruppen. Ihr Angebotstyp, Ihre Transaktionsfunktionen und Ihre Kategorie oder Branche bestimmen, wo Ihr Angebot veröffentlicht werden soll.

**Microsoft AppSource-Anwendungen** sind Branchenlösungen, die in Azure oder für folgende Produkte erstellt werden können: Dynamics 365, Office 365, Power BI oder Power Apps. AppSource-Beratungsdienste sind professionelle Dienstangebote, die Kunden beim Einstieg in die Nutzung von Dynamics 365 und Power BI oder deren Optimierung unterstützen.

Bei **Azure Marketplace**-Anwendungen handelt es sich um technische „Baustein“-Lösungen, die unter bzw. für Azure erstellt werden und für IT-Experten oder Entwickler ausgelegt sind. Azure Marketplace-Beratungsdienste sind professionelle Dienstangebote, die Kunden beim Einstieg in die Nutzung von Azure oder deren Optimierung unterstützen.

>[!Note]
>„Doppeltes Listing“ (nur für SaaS-Apps): Wenn Ihr SaaS-Angebot sowohl für eine technische Zielgruppe (Azure Marketplace) als auch für geschäftliche Benutzer (AppSource) konzipiert ist, können Sie eine Kategorie und/oder eine Unterkategorie auswählen, die für beide Storefronts angewendet werden kann. Beachten Sie, dass ein solches „doppeltes Listing“ Ihres Angebots auf einem Nutzenversprechen basieren muss, das für beide Zielgruppen gilt. Klicken Sie [hier](./gtm-offer-listing-best-practices.md#categories), um die Kategorien für jede Storefront anzuzeigen.

## <a name="choose-a-publishing-option"></a>Auswählen einer Veröffentlichungsoption

Die verfügbaren Veröffentlichungsoptionen ermöglichen eine differenzierte Kundenbindung und ebnen Ihnen den Weg zu gemeinsamem Zugriff auf Leads und [Vorteile des kommerziellen Marketplace](https://docs.microsoft.com/azure/marketplace/gtm-your-marketplace-benefits). Beachten Sie die Aktionsaufrufe, die mit den Veröffentlichungsoptionen verbunden sind:

| **Veröffentlichungsoption**    | **Beschreibung**  |
| :------------------- | :-------------------|
| **Liste** | Dies ist eine einfache Auflistung Ihrer Anwendung oder Ihres Diensts, sodass kommerzielle Marketplace-Benutzer Sie über die Handlungsaufforderung **Kontakt mit mir aufnehmen** bitten können, sich mit dem Kunden in Verbindung zu setzen. |
| **Testversion** | Verwenden Sie den kommerziellen Marketplace, um die Auffindbarkeit zu verbessern und die Bereitstellung einer Testversion Ihrer Lösung zu automatisieren, sodass potenzielle Benutzer vor dem Kauf für einen begrenzten Zeitraum kostenlos einen Eindruck Ihrer SaaS-, IaaS- oder Microsoft-In-App-Benutzeroberfläche gewinnen können. Für die Veröffentlichungsoption „Testen“ werden folgende Aktionsaufrufe verwendet: **Kostenlose Testversion** oder **Testversion**. |
| **BYOL** | Verwenden Sie den kommerziellen Marketplace, um die Ermittelbarkeit zu verbessern und die Bereitstellung Ihrer Lösung zu automatisieren sowie die Finanztransaktion separat durchzuführen. BYOL-Angebotstypen sind ideal für die Migration aus der lokalen Umgebung in die Cloud geeignet. Die Handlungsaufforderung lautet **Jetzt anfordern**.
| **Transaktion** | Transaktionsangebote werden über den kommerziellen Marketplace verkauft. Microsoft ist für die Abrechnung und für Sammlungen verantwortlich. Die Handlungsaufforderung lautet **Jetzt anfordern**.|

> [!Note]
> Bei Verwendung der Veröffentlichungsoption „Transaktion“ sollten Sie sich zunächst mit der Preisgestaltung, Abrechnung, Rechnungsstellung und Auszahlung vertraut machen, bevor Sie einen Angebotstyp auswählen und Ihr Angebot erstellen. Weitere Informationen finden Sie im Artikel [Transaktionsfunktionen im kommerziellen Marketplace](./marketplace-commercial-transaction-capabilities-and-considerations.md).

## <a name="next-steps"></a>Nächste Schritte

- Sobald Sie sich für eine Veröffentlichungsoption entschieden haben, können Sie den [Angebotstyp auswählen](./publisher-guide-by-offer-type.md), der verwendet wird, um Ihr Angebot zu präsentieren.
- Informieren Sie sich über die erforderlichen Berechtigungen in den Veröffentlichungsoptionen des entsprechenden Angebotstypabschnitts, um Auswahl und Konfiguration Ihres Angebots abzuschließen.
- Entnehmen Sie den Veröffentlichungsmustern der Storefronts Beispiele für die Zuordnung ihrer Lösung zu einem Angebotstyp und einer Konfiguration.
