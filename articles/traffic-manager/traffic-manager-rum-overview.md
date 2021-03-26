---
title: Benutzer-Realmessungen im Azure Traffic Manager
description: In dieser Einführung erfahren Sie, wie Benutzer-Realmessungen von Azure Traffic Manager funktionieren.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: duau
ms.custom: ''
ms.openlocfilehash: 618f8fff532da0f6ae315ad9e4cda35a289949d1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98183709"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Übersicht über Benutzer-Realmessungen in Traffic Manager

Wenn Sie ein Traffic Manager-Profil einrichten, um die Leistungsroutingmethode zu verwenden, prüft der Dienst zuerst, woher die DNS-Abfragen stammen. Anschließend trifft er Routingentscheidungen, um diese Anfragen an die Azure-Region zu leiten, die die kürzeste Wartezeit bietet. Dies geschieht mithilfe der Tabellen mit den Netzwerkwartezeiten, die Traffic Manager für verschiedene Endbenutzernetzwerke verwaltet.

Mithilfe von Benutzer-Realmessungen können Sie die Netzwerkwartezeiten in Azure-Regionen anhand der von Ihren Endbenutzern verwendeten Clientanwendungen messen. Traffic Manager berücksichtigt diese Informationen anschließend zum Treffen von Routingentscheidungen. Wenn Sie Benutzer-Realmessungen einsetzen, können Sie die Genauigkeit des Routings für Anforderungen erhöhen, die von den Netzwerken mit Ihren Endbenutzern stammen. 

## <a name="how-real-user-measurements-work"></a>Funktionsweise von Benutzer-Realmessungen

Für Benutzer-Realmessungen messen Ihre Clientanwendungen die Wartezeit in Azure-Regionen aus der Perspektive der Endbenutzernetzwerke, in denen sie verwendet werden. Wenn Sie beispielsweise eine Webseite haben, auf die Benutzer von verschiedenen Standorten aus zugreifen (etwa in den nordamerikanischen Regionen), können Sie Benutzer-Realmessungen mit der Leistungsroutingmethode verwenden, um Benutzer zur besten Azure-Region zu leiten, in der Ihre Serveranwendung gehostet wird.

Zunächst müssen Sie in Ihre Webseiten ein von Azure bereitgestelltes JavaScript (mit enthaltenem eindeutigen Schlüssel) einbetten. Sobald dies erfolgt ist, fragt das JavaScript, wann immer ein Benutzer diese Webseite besucht, Traffic Manager ab, um Informationen zu den Azure-Regionen zu erhalten, die es messen soll. Der Dienst gibt eine Reihe von Endpunkten an das Skript zurück. Dieses misst dann nacheinander diese Regionen, indem ein in diesen Azure-Regionen gehostetes 1-Pixel-Bild heruntergeladen wird. Anschließend wird die Wartezeit zwischen dem Zeitpunkt des Sendens der Anforderung und dem Empfang des ersten Bytes erfasst. Diese Messungen werden dann dem Traffic Manager-Dienst zurückgemeldet.

Im Laufe der Zeit geschieht dies viele Male und in vielen Netzwerken, was dazu führt, dass Traffic Manager genauere Informationen zu den Wartezeiten der Netzwerke erhält, in denen sich Ihre Endbenutzer befinden. Diese Informationen beginnen, in die von Traffic Manager getroffenen Routingentscheidungen einzufließen. Dies führt zu einer erhöhten Genauigkeit bei den Entscheidungen, die auf den gesendeten Benutzer-Realmessungen basieren.

Wenn Sie Benutzer-Realmessungen verwenden, wird Ihnen die Anzahl der Messungen, die an den Traffic Manager gesendet wurden, in Rechnung gestellt. Weitere Informationen zu den Preisen finden Sie auf der [Traffic Manager-Seite mit Preisdetails](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faqs"></a>Häufig gestellte Fragen

* [Welche Vorteile hat die Verwendung von Benutzer-Realmessungen?](./traffic-manager-faqs.md#what-are-the-benefits-of-using-real-user-measurements)

* [Kann ich Benutzer-Realmessungen auch für andere Regionen als Azure-Regionen verwenden?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-non-azure-regions)

* [Welche Routingmethode profitiert von Benutzer-Realmessungen?](./traffic-manager-faqs.md#which-routing-method-benefits-from-real-user-measurements)

* [Muss ich Benutzer-Realmessungen für jedes Profil separat aktivieren?](./traffic-manager-faqs.md#do-i-need-to-enable-real-user-measurements-each-profile-separately)

* [Wie deaktiviere ich Benutzer-Realmessungen für mein Abonnement?](./traffic-manager-faqs.md#how-do-i-turn-off-real-user-measurements-for-my-subscription)

* [Kann ich Benutzer-Realmessungen mit anderen Anwendungen als Webseiten nutzen?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-client-applications-other-than-web-pages)

* [Wie viele Messungen werden jeweils durchgeführt, wenn meine Webseite mit aktivierten Benutzer-Realmessungen gerendert wird?](./traffic-manager-faqs.md#how-many-measurements-are-made-each-time-my-real-user-measurements-enabled-web-page-is-rendered)

* [Kommt es zu einer Verzögerung, bevor das Skript für Benutzer-Realmessungen auf meiner Webseite ausgeführt wird?](./traffic-manager-faqs.md#is-there-a-delay-before-real-user-measurements-script-runs-in-my-webpage)

* [Kann ich Benutzer-Realmessungen auf die Azure-Regionen begrenzen, für die ich Messungen durchführen möchte?](./traffic-manager-faqs.md#can-i-use-real-user-measurements-with-only-the-azure-regions-i-want-to-measure)

* [Kann ich die Anzahl von durchgeführten Messungen auf einen bestimmten Wert beschränken?](./traffic-manager-faqs.md#can-i-limit-the-number-of-measurements-made-to-a-specific-number)

* [Kann ich die Messungen, die von meiner Clientanwendung durchgeführt werden, als Teil der Benutzer-Realmessungen anzeigen?](./traffic-manager-faqs.md#can-i-see-the-measurements-taken-by-my-client-application-as-part-of-real-user-measurements)

* [Kann ich das von Traffic Manager bereitgestellte Skript für die Messungen ändern?](./traffic-manager-faqs.md#can-i-modify-the-measurement-script-provided-by-traffic-manager)

* [Können andere Personen den Schlüssel sehen, den ich für Benutzer-Realmessungen verwende?](./traffic-manager-faqs.md#will-it-be-possible-for-others-to-see-the-key-i-use-with-real-user-measurements)

* [Können andere Personen meinen RUM-Schlüssel für schädliche Zwecke missbrauchen?](./traffic-manager-faqs.md#can-others-abuse-my-rum-key)

* [Muss ich den JavaScript-Code für die Messungen auf allen meinen Webseiten einfügen?](./traffic-manager-faqs.md#do-i-need-to-put-the-measurement-javascript-in-all-my-web-pages)

* [Können Informationen zu meinen Endbenutzern von Traffic Manager identifiziert werden, wenn ich Benutzer-Realmessungen verwende?](./traffic-manager-faqs.md#can-information-about-my-end-users-be-identified-by-traffic-manager-if-i-use-real-user-measurements)

* [Muss für die Webseite, für die Benutzer-Realmessungen durchgeführt werden, Traffic Manager zu Routingzwecken eingesetzt werden?](./traffic-manager-faqs.md#does-the-webpage-measuring-real-user-measurements-need-to-be-using-traffic-manager-for-routing)

* [Muss ich für die Verwendung mit Benutzer-Realmessungen einen Dienst in den Azure-Regionen hosten?](./traffic-manager-faqs.md#do-i-need-to-host-any-service-on-azure-regions-to-use-with-real-user-measurements)

* [Erhöht sich meine Azure-Bandbreitenauslastung, wenn ich Benutzer-Realmessungen verwende?](./traffic-manager-faqs.md#will-my-azure-bandwidth-usage-increase-when-i-use-real-user-measurements)

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie [Benutzer-Realmessungen mit Webseiten](traffic-manager-create-rum-web-pages.md) verwenden
- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Erfahren Sie mehr zu [Mobile Center](/mobile-center/)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](./quickstart-create-traffic-manager-profile.md)