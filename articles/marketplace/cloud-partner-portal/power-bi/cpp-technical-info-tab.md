---
title: Technische Informationen für ein Power BI-App-Angebot | Azure Marketplace
description: Informationen zum Konfigurieren von Feldern des Typs „Technische Informationen“ für ein Power BI-App-Angebot für den Marketplace von Microsoft AppSource.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: dsindona
ms.openlocfilehash: b012ac012c7de3c6428ecbe2d2b922a2a9aa0523
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "82141775"
---
# <a name="power-bi-apps-technical-info-tab"></a>Registerkarte „Technische Informationen“ für Power BI-Apps

>[!Important]
>Ab dem 13. April 2020 beginnen wir mit der Umstellung der Verwaltung Ihrer Power BI-App-Angebote auf Partner Center. Nach der Migration erstellen und verwalten Sie Ihre Angebote im Partner Center. Befolgen Sie zum Verwalten Ihrer migrierten Angebote die Anweisungen unter [Übersicht über die App-Erstellung mit Power BI](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-power-bi-app-offer).

Verwenden Sie auf der Seite **Neues Angebot** die Registerkarte **Technische Informationen**, um die URL des Power BI-Installationspakets und andere Informationen bereitzustellen, die Sie zum Überprüfen des neuen Angebots benötigen.  Bei ihrer Erstveröffentlichung sind alle Power BI-Apps kostenlos und stehen in AppSource zum Download bereit. Aus diesem Grund können Sie keine Stock Keeping Einheiten (SKUs) für diesen Angebotstyp definieren.

![Registerkarte „Technische Informationen“](./media/technical-info-tab.png)


## <a name="technical-info-fields"></a>Felder auf der Registerkarte „Technische Informationen“ 

Füllen Sie auf der Registerkarte **Technische Informationen** die in der folgenden Tabelle beschriebenen Felder aus. Ein Sternchen (*) am Ende einer Feldbezeichnung bedeutet, dass das Feld erforderlich ist.

|        Feld          |  BESCHREIBUNG                                                                 |
|    ---------------    |  ----------------------------------------------------------------------------|
| **Installations-URL\***     | Power BI generiert diese URL, wenn Sie die App veröffentlichen und in die Produktion höherstufen.  Weitere Informationen finden Sie unter [Veröffentlichen von Apps mit Dashboards und Berichten in Power BI](https://docs.microsoft.com/power-bi/service-create-distribute-apps).  |
|  **Überprüfungsanweisungen**  |  Fügen Sie wahlweise Anweisungen (bis zu 3.000 Zeichen) hinzu, um das Microsoft-Überprüfungsteam beim Konfigurieren und Testen Ihrer App sowie beim Herstellen einer Verbindung zu Ihrer App zu unterstützen. Fügen Sie typische Konfigurationseinstellungen, Konten, Parameter oder andere Informationen ein, die zum Testen der Option „Daten verbinden“ verwendet werden können. Diese Informationen werden nur dem Überprüfungsteam angezeigt und dienen nur zu Überprüfungszwecken.  |
| **Wird diese App als Power BI-Inhaltspaket erstellt?** | Dieses Feld wird derzeit nur intern verwendet. Übernehmen Sie die Standardeinstellung **Nein**. Wenn Sie die Einstellung in **Ja** ändern, beenden Sie möglicherweise den Veröffentlichungsprozess.  |  
|  |  |


## <a name="next-steps"></a>Nächste Schritte

Stellen Sie auf der nächsten Registerkarte [Storefrontdetails](./cpp-storefront-details-tab.md) Marketinginformationen und rechtliche Hinweise zu Ihrer App bereit.

