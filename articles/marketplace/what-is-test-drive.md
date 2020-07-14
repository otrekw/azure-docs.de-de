---
title: Worum handelt es sich bei einer Testversion? Kommerzieller Marketplace von Microsoft
description: Erläuterung des Features der Marketplace-Testversion
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: keferna
ms.author: keferna
ms.date: 06/19/2020
ms.openlocfilehash: 95ca527d8f7912e2c455f94036fe61828b220257
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86121468"
---
# <a name="what-is-a-test-drive"></a>Worum handelt es sich bei einer Testversion?

Eine Testversion ist eine hervorragende Möglichkeit, Ihr Angebot potenziellen Kunden zu präsentieren, indem Sie ihnen die Möglichkeit geben, *das Produkt vor dem Kauf zu testen*. Dies führt zu einer höheren Konversionsrate und zur Generierung aussichtsreicher Leads. Eine Testversion erweckt Ihr Produkt in einem realen Implementierungsszenario zum Leben und generiert gleichzeitig aussichtsreiche Leads.

Testversionen sind verwaltete Instanzen, die Ihre Lösung oder Anwendung interessierten Kunden zur Bereitstellung auf Anforderung zur Verfügung stellen. Sobald eine Testversionsinstanz zugewiesen wurde, steht sie für einen festgelegte Zeitraum zur Verfügung und wird dann gelöscht, um für einen anderen Kunden Platz zu schaffen.

Als Herausgeber verwalten und konfigurieren Sie die Testversionseinstellungen im Partner Center. Die Einzelheiten der technischen Konfiguration hängen von der Art des Angebots ab, mit dem Sie arbeiten. Eine ausführliche Anleitung finden Sie im Link unter [Nächster Schritt](#next-step) am Ende dieses Themas.

Potenzielle Kunden entdecken Ihre Testversion im kommerziellen Marketplace. Sie stellen ihre Kontaktinformationen zur Verfügung und erklären sich mit den Bedingungen Ihres Angebots und der Datenschutzrichtlinie einverstanden. Anschließend erhalten sie Zugriff auf Ihre vorkonfigurierte Umgebung, um sie für einen bestimmten Zeitraum auszuprobieren. Kunden erhalten einen praktischen, interaktiven Test der wichtigen Features und Leistungen Ihres Produkts, während Sie einen vielversprechenden Vertriebslead erhalten.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Als Herausgeber verwalten und konfigurieren Sie die Testversionseinstellungen im Partner Center. Nach dem Setup wird Ihre Testversion zu einer verwalteten Instanz, die bei Bedarf dem Kunden, der sie anfordert, bereitgestellt wird. Sobald eine Testversionsinstanz zugewiesen wurde, steht sie für einen festgelegte Zeitraum zur Verfügung und wird dann gelöscht, um für einen anderen Kunden Platz zu schaffen.

## <a name="types-of-test-drives"></a>Arten von Testversionen

Im kommerziellen Marketplace stehen verschiedene Testversionen für ausgewählte Angebote zur Verfügung, die vom jeweiligen Produkt, Szenario und Marketplace abhängen, in dem Sie sich befinden:

- Azure Resource Manager
- Gehostete Testversion
    - Dynamics 365 Business Central
    - Dynamics 365 for Customer Engagement
    - Dynamics 365 for Operations
- Logik-App
- Power BI

Einzelheiten zur Konfiguration einer dieser Testversionen finden Sie im Link unter [Nächster Schritt](#next-step) am Ende dieses Themas.

### <a name="azure-resource-manager-test-drive"></a>Azure Resource Manager-Testversion

Diese Bereitstellungsvorlage enthält alle Azure-Ressourcen, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet. Dies ist die einzige Testversionsoption für VM- oder Azure-App-Angebote.

### <a name="hosted-test-drive"></a>Gehostete Testversion

Bei einer gehosteten Testversion entfällt die Komplexität des Setups, da Microsoft den Dienst, der die Benutzereinrichtung, Bereitstellung und Aufhebung der Bereitstellung der Testversion durchführt, hostet und verwaltet. Wenn Sie ein Angebot in Microsoft AppSource haben, bauen Sie Ihre Testversion so auf, dass sie mit einer Dynamics AX/CRM-Instanz oder einer anderen Ressource außerhalb von Azure verbunden werden kann. Wählen Sie diesen Typ für AppSource-Angebote, um eine Verbindung mit diesen Dynamics 365-Angeboten herzustellen:

- Verwenden Sie [Dynamics 365 for Business Central](partner-center-portal/create-new-operations-offer.md) für ein ERP-System (Enterprise Resource Planning) von Business Central für Finanzen, Betrieb, Lieferkette und CRM.
- Verwenden Sie [Dynamics 365 for Customer Engagement](partner-center-portal/create-new-customer-engagement-offer.md) für ein Kundenbindungssystem für Vertrieb, Services, Projektdienst und Außendienst.
- Verwenden Sie [Dynamics 365 for Operations](partner-center-portal/create-new-operations-offer.md) für ein ERP-System (Enterprise Resource Planning) von Business Central für Finanzen, Betrieb, Fertigung und Lieferkette.

### <a name="logic-app-test-drive"></a>Testversion für Logik-Apps

Diese Art von Testversion wird nicht von Microsoft gehostet. Verwenden Sie es zum Herstellen einer Verbindung mit einem Dynamics 365-Angebot oder einer anderen benutzerdefinierten Ressource.

### <a name="power-bi-test-drive"></a>Power BI-Testversion

Hierbei handelt es sich um einen eingebetteten Link zu einem benutzerdefinierten Dashboard. Für jedes Produkt, mit dem nur eine interaktive Power BI-Visualisierung demonstriert werden soll, sollte dieser Testversionstyp verwendet werden.

## <a name="transforming-examples"></a>Transformationsbeispiele

Der Vorgang zum Transformieren einer Ressourcenarchitektur in eine Testversion kann schwierig sein. Sehen Sie sich diese Beispiele an, wie Sie aktuelle Architekturen am besten transformieren.

[Transformieren einer Websitevorlage in eine Testversion](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Website-Deployment-Template-for-Test-Drive)

[Transformieren einer Vorlage für virtuelle Computer in eine Testversion](https://github.com/Azure/AzureTestDrive/wiki/Transforming-Virtual-Machine-Deployment-Template-for-Test-Drive)

[Transformieren vorhandener Resource Manager-Vorlagen in eine Testversion](https://github.com/Azure/AzureTestDrive/wiki/Deploying-Existing-Solutions)

## <a name="generate-leads-from-your-test-drive"></a>Generieren von Leads mithilfe der Testversion

Eine kommerzielle Marketplace-Testversion ist ein hervorragendes Tool für Vermarkter. Wir empfehlen Ihnen, es in Ihre Markteinführungsaktivitäten einzubeziehen, um mehr Leads für Ihr Unternehmen zu generieren. Eine detaillierte Anleitung finden Sie unter [Kundenleads aus Ihrem Angebot im kommerziellen Marketplace](https://github.com/partner-center-portal/commercial-marketplace-get-customer-leads.md).

Wenn es dank eines Testversionsleads zu einem Geschäftsabschluss kommt, denken Sie daran, diesen bei [Microsoft Partner Sales Connect](https://support.microsoft.com/help/3155788/getting-started-with-microsoft-partner-sales-connect) zu registrieren. Darüber hinaus freuen wir uns über Berichte, wie eine Testversion zur Kundengewinnung beigetragen hat.

## <a name="other-resources"></a>Weitere Ressourcen

Zusätzliche Ressourcen zu Testversionen:

- [Test Drive Best Practices](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) (Bewährte Methoden für Testversionen)
- [Übersicht](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF-Datei; stellen Sie sicher, dass Ihr Popupblocker deaktiviert ist.)

## <a name="next-step"></a>Nächster Schritt

- [Technische Konfiguration der Testversion](test-drive-technical-configuration.md)
