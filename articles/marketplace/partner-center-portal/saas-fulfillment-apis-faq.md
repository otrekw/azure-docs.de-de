---
title: Häufig gestellte Fragen zur SaaS-Fulfillment-API – kommerzieller Microsoft-Marketplace
description: Erfahren Sie mehr über einige der Integrationsanforderungen für den kommerziellen Microsoft-Marketplace, damit Azure-Kunden SaaS-Angebote abonnieren können.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 4c5d8b438764fa9aa3838b2225c63d412afc519b
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606803"
---
# <a name="common-questions-about-saas-fulfillment-apis"></a>Häufig gestellte Fragen zu SaaS-Fulfillment-APIs

Dieser Artikel beschreibt einige der Integrationsanforderungen für den kommerziellen Microsoft-Marketplace, damit Azure-Kunden SaaS-Angebote abonnieren können.

## <a name="discovery-experience"></a>Auffindbarkeit

Nach der Veröffentlichung eines SaaS-Angebots steht dieses für Azure-Benutzer im Azure Marketplace zur Verfügung. Ihre Kunden können Angebote nach Produkttyp (SaaS) filtern und nach für sie interessanten SaaS-Diensten suchen.

## <a name="purchase-experience"></a>Kauferlebnis

Wenn sich ein Benutzer für ein bestimmtes SaaS-Angebot interessiert, kann er es über den Azure Marketplace abonnieren.

### <a name="what-does-it-mean-for-an-azure-user-to-subscribe-to-a-saas-offer-in-azure-marketplace"></a>Was bedeutet es für einen Azure-Benutzer, ein SaaS-Angebot im Azure Marketplace zu abonnieren?

Es bedeutet, dass der Benutzer die für das SaaS-Angebot geltenden Nutzungsbedingungen und Datenschutzbestimmungen einsehen und den von Ihnen als Herausgeber des SaaS-Angebots festgelegten Abrechnungskonditionen zustimmen kann, um es über Microsoft zu erwerben. Benutzer können ihr vorhandenes Zahlungsprofil in Azure verwenden, um für die Nutzung des SaaS-Angebots zu zahlen.

Diese Möglichkeit ist in mehrfacher Hinsicht von Vorteil. Kunden können nun mit der Microsoft Cloud Platform als vertrauenswürdige Quelle zentral Dienste entdecken und abonnieren, ohne jede ISV-Software untersuchen zu müssen, die sie verwenden möchten. Außerdem können Kunden ihr vorhandenes Zahlungsprofil nutzen und müssen auf diese Weise nicht jede ISV-Software separat zahlen.

### <a name="is-the-user-charged-automatically-when-the-offer-is-subscribed"></a>Werden dem Benutzer bei der Anmeldung für das Angebot automatisch Gebühren berechnet?

Mit dem Abonnement des SaaS-Angebots hat der Benutzer zugestimmt, die Nutzung des SaaS-Angebots über die Microsoft-Plattform zu bezahlen. Die Gebührenabrechnung beginnt jedoch erst mit der Nutzung des Angebots. Der Benutzer muss Ihr SaaS-Angebot aufrufen und die Kontoerstellung bestätigen, um mit der Nutzung des Angebots zu beginnen. Sie benachrichtigen in diesem Fall Microsoft, dass die Abrechnung für dieses SaaS-Kundenabonnement beginnt.

### <a name="how-are-you-notified-when-a-user-subscribes-to-your-saas-offer"></a>Wie werden Sie benachrichtigt, wenn ein Benutzer Ihr SaaS-Angebot abonniert?

Nachdem ein Benutzer ein Angebot abonniert hat, kann er die erworbenen Angebote in Azure ausfindig machen und verwalten. Standardmäßig wird der Status eines neu abonnierten SaaS-Angebots mit **Bereitstellung, Erfüllung ausstehend** angezeigt. In diesem Status wird dem Azure-Benutzer die Aktion **Konto konfigurieren** angeboten, um zur SaaS-Abonnementverwaltung im Azure-Portal zu wechseln.

Wenn der Benutzer die Option **Konto konfigurieren** auswählt, wird er zur Website des SaaS-Diensts weitergeleitet. Der Herausgeber hat die URL zum Zeitpunkt der Angebotsveröffentlichung konfiguriert. Diese Seite wird als Landing Page des Herausgebers bezeichnet. Azure-Benutzer melden sich basierend auf ihren vorhandenen AAD-Anmeldeinformationen in Azure auf der SaaS-Landing Page an.

Wenn der Azure-Benutzer auf die Landing Page umgeleitet wird, wird der Abfrage-URL ein Token hinzugefügt. Es handelt sich hierbei um ein kurzlebiges Token mit einer Gültigkeitsdauer von 24 Stunden. Sie können anschließend das Vorhandensein dieses Tokens ermitteln und die Microsoft-API aufrufen, um weitere mit dem Token verknüpfte Kontextinformationen abzurufen.

![Ablauf für Kundenabonnement](media/saas-metering-service-integration-flow-a.png)

Weitere Informationen zur Abwicklung von Transaktionen im Lebenszyklus eines SaaS-Angebots finden Sie unter [SaaS-Fulfillment-API](pc-saas-fulfillment-api-v2.md).

### <a name="how-do-you-know-the-saas-offer-to-which-the-user-subscribes-in-azure"></a>Woher wissen Sie, welches SaaS-Angebot der Benutzer in Azure abonniert?

Die Antwort der `Resolve`-API enthält die Angebots- und Planinformationen, die dem SaaS-Abonnement zugeordnet sind.

### <a name="how-can-the-azure-user-change-the-plan-associated-with-this-azure-subscription"></a>Wie kann der Azure-Benutzer den Plan ändern, der diesem Azure-Abonnement zugeordnet ist?

* Der Azure-Benutzer kann den mit dem SaaS-Abonnement verbundenen Plan direkt in der SaaS-Benutzeroberfläche selbst oder über die Microsoft-Plattform ändern.

* Konvertierungen können zu einem beliebigen Zeitpunkt im Abrechnungszeitraum durchgeführt werden. Sie werden zur Bestätigung einer Konvertierung aufgefordert, damit diese wirksam wird.

* Ein Plan mit Vorauszahlung (**monatlich** oder **jährlich**) wird anteilig berechnet. Alle bis zum Zeitpunkt der Konvertierung ermittelten Überschreitungen werden in der nächsten Rechnung abgerechnet. Neue Überschreitungen werden auf Grundlage des neuen Plans in Rechnung gestellt.

>[!Note]
>Sie können Downgrades blockieren, wenn Sie bestimmte Konvertierungspfade nicht unterstützen möchten.

Die folgende Sequenz zeigt den Ablauf, wenn ein Azure-Kunde den Plan über die SaaS-Benutzeroberfläche ändert:

![Ablauf einer Planänderung durch den Kunden](media/saas-metering-service-integration-flow-b.png)

Die folgende Sequenz zeigt den Ablauf, wenn der Azure-Kunde seinen Plan in einem Onlineshop von Microsoft ändert:

![Ablauf für das Ändern eines Plans durch einen Kunden im Onlineshop](media/saas-metering-service-integration-flow-c.png)

### <a name="how-can-the-azure-user-unsubscribe-from-the-plan-associated-with-azure-subscription"></a>Wie kann der Azure-Benutzer den mit dem Azure-Abonnement verbundenen Plan kündigen?

Ein Azure-Benutzer kann das erworbene SaaS-Angebotsabonnement entweder direkt in der SaaS-Benutzeroberfläche oder über die Microsoft-Plattform kündigen. Sobald der Benutzer das Abonnement kündigt, fallen ab dem nächsten Abrechnungszyklus keine Gebühren mehr an.

Die folgende Sequenz zeigt den Ablauf, wenn ein Azure-Kunde das SaaS-Angebot in der SaaS-Benutzeroberfläche kündigt:

![Kündigung des Abonnements über die SaaS-Benutzeroberfläche](media/saas-metering-service-integration-flow-d.png)

Die folgende Sequenz zeigt den Ablauf, wenn der Azure-Benutzer das Abonnement im Onlineshop von Microsoft kündigt:

![Kündigung des Abonnements durch einen Kunden in einem Onlineshop von Microsoft](media/saas-metering-service-integration-flow-e.png)

## <a name="next-steps"></a>Nächste Schritte

[Marketplace-Messungsdienst-APIs](./marketplace-metering-service-apis.md)
