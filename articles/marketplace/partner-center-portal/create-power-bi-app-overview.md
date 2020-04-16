---
title: Übersicht über die Power BI-App-Erstellung – Azure Marketplace
description: In diesem Artikel werden die allgemeinen Schritte zum Veröffentlichen einer Power BI-App in Microsoft AppSource beschrieben. Die technischen und geschäftlichen Anforderungen, die Ihre Power BI-App erfüllen muss, damit sie auf dem kommerziellen Marketplace veröffentlicht werden kann, werden ebenfalls angegeben.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: c348a172b16e12334d33cf2718609694147fdce3
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674961"
---
# <a name="power-bi-app-creation-overview"></a>Übersicht über die Power BI-App-Erstellung

> [!IMPORTANT]
> Wir verlagern die Verwaltung Ihrer Power BI-App-Angebote aus dem Cloud-Partnerportal in Partner Center. Befolgen Sie für die Verwaltung Ihrer Angebote bis zur erfolgten Migration die Anweisungen unter [Power BI-App-Angebote](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-power-bi-offer) für das Cloud-Partnerportal.

In diesem Artikel wird erläutert, wie Sie eine Power BI-App in Microsoft [AppSource](https://appsource.microsoft.com/) veröffentlichen. In einer Power BI-App sind anpassbare Inhalte wie Datasets, Berichte und Dashboards gepackt. Sie können die App dann mit anderen Power BI-Plattformen bereitstellen, die AppSource verwenden, vom Entwickler zugelassene Änderungen und Anpassungen vornehmen und die App mit Ihren eigenen Daten verbinden.

## <a name="publishing-benefits"></a>Vorteile der Veröffentlichung

Vorteile der Veröffentlichung im kommerziellen Marketplace:

- Bewerben Sie Ihr Unternehmen, indem Sie die Marke Microsoft nutzen.
- In AppSource können Sie mehr als 100 Millionen Office 365- und Dynamics 365-Benutzer erreichen und im Azure Marketplace über 200.000 Organisationen.
- Nutzen Sie in diesen Marketplaces hochwertige Leads.
- Lassen Sie Ihre Dienste durch die Außendienst- und Telefonmarketingteams von Microsoft bewerben.

## <a name="overview"></a>Übersicht

:::image type="content" source="media/power-bi-app-publishing-steps.png" alt-text="Übersicht über die Schritte zum Veröffentlichen einer Power BI-App" border="false":::

Dies sind die wichtigsten Schritte zur Veröffentlichung:

1. Erstellen Sie eine Anwendung in Power BI. Sie erhalten einen Link zur Paketinstallation. Dies ist die wichtigste technische Ressource für das Angebot. Senden Sie das Testpaket an die Vorproduktion, bevor Sie das Angebot in Partner Center erstellen. Weitere Informationen finden Sie unter [Was sind Power BI-Apps?](https://docs.microsoft.com/power-bi/service-template-apps-overview).
2. Fügen Sie das Marketingmaterial hinzu, z. B. einen offiziellen Namen, eine Beschreibung und Logos.
3. Schließen Sie die rechtlichen und unterstützenden Dokumente ein, z. B. Nutzungsbedingungen, Datenschutzrichtlinie, Unterstützungsrichtlinie und Benutzerhilfe.
4. Erstellen Sie das Angebot: Verwenden Sie Partner Center, um die Details des Angebots zu bearbeiten, darunter die Angebotsbeschreibung, Marketingmaterial, rechtliche Hinweise und Supportinformationen sowie Ressourcenspezifikationen.
5. Übermitteln Sie es zur Veröffentlichung.
6. Überwachen Sie den Fortschritt in Partner Center, wo das AppSource-Onboardingteam Ihre App testet, überprüft und zertifiziert.
7. Nach der Zertifizierung sollten Sie die App in der Testumgebung überprüfen und sie dann freigeben. Dadurch wird sie in AppSource gelistet (in den Onlinemodus geschaltet).
8. Überführen Sie das Paket in Power BI in die Produktionsumgebung. Ausführliche Informationen finden Sie unter [Verwalten der Veröffentlichung von Power BI-Apps](https://docs.microsoft.com/power-bi/service-template-apps-create#manage-the-template-app-release).

## <a name="before-you-begin"></a>Voraussetzungen

Überprüfen Sie die nachstehenden Links mit Vorlagen, Tipps und Beispielen.

- [Erstellen einer Power BI-App](https://docs.microsoft.com/power-bi/service-template-apps-create)
- [Tipps zum Erstellen einer Power BI-App](https://docs.microsoft.com/power-bi/service-template-apps-tips)
- [Beispiele](https://docs.microsoft.com/power-bi/service-template-apps-samples)

## <a name="requirements"></a>Requirements (Anforderungen)

Um im kommerziellen Marketplace veröffentlicht zu werden, muss Ihre Power BI-App die folgenden technischen und geschäftlichen Anforderungen erfüllen.

### <a name="technical-requirements"></a>Technische Anforderungen

Die technische Hauptressource, die Sie benötigen, ist eine [Power BI-App](https://go.microsoft.com/fwlink/?linkid=2028636). Dabei handelt es sich um eine Sammlung von primären Datasets, Berichten oder Dashboards. Außerdem sind optionale verbundene Dienste und eingebettete Datasets enthalten. Dies wurde zuvor als [Inhaltspaket](https://docs.microsoft.com/power-bi/service-organizational-content-pack-introduction) bezeichnet. Weitere Informationen zum Entwickeln dieser Art von App finden Sie unter [Was sind Power BI-Apps?](https://go.microsoft.com/fwlink/?linkid=2028636).

#### <a name="get-an-installation-web-address"></a>Erhalten einer Webadresse für die Installation

Eine Power BI-App kann nur innerhalb der [Power BI](https://powerbi.microsoft.com/)-Umgebung erstellt werden.

1. Melden Sie sich mit einer [Power BI Pro-Lizenz](https://docs.microsoft.com/power-bi/service-admin-purchasing-power-bi-pro) an.
2. Erstellen und testen Sie Ihre App in Power BI.
3. Wenn Sie die Webadresse für die App-Installation erhalten, fügen Sie sie auf der Seite **Technische Konfiguration** in Partner Center hinzu.

Nachdem Ihre App in Power BI erstellt und getestet wurde, speichern Sie die Webadresse für die Anwendungsinstallation, da Sie diese benötigen, um [ein Power BI-App-Angebot zu erstellen](https://aka.ms/AzureCreatePBIServiceApp).

### <a name="business-requirements"></a>Geschäftliche Anforderungen

Die geschäftlichen Anforderungen umfassen verfahrenstechnische, vertragliche und rechtliche Verpflichtungen. Die Voraussetzungen lauten wie folgt:

- Sie müssen ein registrierter Herausgeber im kommerziellen Marketplace sein. Wenn Sie nicht registriert sind, führen Sie die Schritte unter [Weg zum Herausgeber im kommerziellen Marketplace](https://docs.microsoft.com/azure/marketplace/become-publisher) aus.
- Stellen Sie Inhalte bereit, die die Kriterien erfüllen, damit Ihr Angebot in AppSource gelistet wird. Weitere Informationen finden Sie im Blogbeitrag [Have an app to list on AppSource? Here’s how](https://appsource.microsoft.com/blogs/have-an-app-to-list-on-appsource-here-s-how) (Informationen zum Listen einer Anwendung in AppSource).
- Akzeptieren Sie die [Datenschutzbestimmungen von Microsoft](https://privacy.microsoft.com/privacystatement), und befolgen Sie sie.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines Power BI-App-Angebots in Partner Center](https://aka.ms/AzureCreatePBIServiceApp)