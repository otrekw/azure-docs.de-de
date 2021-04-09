---
title: Datenverkehrsansicht im Azure Traffic Manager
description: In dieser Einführung erfahren Sie, wie die Datenverkehrsanzeige in Traffic Manager funktioniert.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 01/22/2021
ms.author: duau
ms.custom: ''
ms.openlocfilehash: 66376655c61903761d93ea228c6d72fa05734353
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98743048"
---
# <a name="traffic-manager-traffic-view"></a>Traffic Manager-Datenverkehrsansicht

Traffic Manager stellt Ihnen Routing auf DNS-Ebene (Domain Name System) bereit. Dieser Dienst ermöglicht es Ihren Endbenutzern, auf Grundlage der Routingmethode Ihrer Wahl an fehlerfreie Endpunkte weitergeleitet zu werden. Die Datenverkehrsansicht bietet Traffic Manager einen Überblick über Ihre Benutzergruppen (auf Granularitätsebene des DNS-Resolvers) und deren Datenverkehrsmuster. Wenn Sie die Datenverkehrsansicht aktivieren, werden diese Informationen verarbeitet, um Ihnen verwertbare Erkenntnisse zu liefern. 

Die Datenverkehrsansicht ermöglicht Folgendes:
- Verstehen, wo sich Ihre Benutzergruppen befinden (bis zur Granularitätsebene eines lokalen DNS-Resolvers)
- Anzeigen des Umfangs des Datenverkehrs (als DNS-Abfragen beobachtet, die von Azure Traffic Manager bearbeitet wurden), der aus diesen Regionen stammt.
- Erhalten von Einblicken hinsichtlich der repräsentativen Wartezeit dieser Benutzer.
- Detailliertes Untersuchen der spezifischen Datenverkehrsmuster jeder Benutzergruppen in Azure-Regionen, in denen Sie Endpunkte haben 

So können Sie z. B. in der Datenverkehrsansicht nachvollziehen, welche Regionen zwar ein großes Volumen an Datenverkehr haben, aber unter längeren Wartezeiten leiden. Diese Informationen können Sie dann nutzen, um die Erweiterung Ihres Speicherbedarfs in neue Azure-Regionen zu planen. Auf diese Weise kommt es bei der Erfahrung Ihrer Benutzer zu geringeren Wartezeiten.

## <a name="how-traffic-view-works"></a>Funktionsweise der Datenverkehrsansicht

Die Datenverkehrsansicht funktioniert so, dass sie die eingehenden Abfragen berücksichtigt, die in den letzten sieben Tagen für ein Profil empfangen wurden. Aus den Informationen der eingegangenen Abfragen extrahiert die Datenverkehrsansicht die Quell-IP-Adresse des verwendeten DNS-Resolvers, um den Standort der Benutzer darzustellen. Diese Informationen werden auf der Ebene des DNS-Resolvers gruppiert, um Benutzerbasisregionen zu erstellen. Traffic Manager verwaltet die geografischen Informationen von IP-Adressen. Traffic Manager untersucht dann die Azure-Regionen, an die die Abfrage geleitet wird, und erstellt eine Karte des Datenverkehrsflusses der Benutzer aus diesen Regionen.
 
Im nächsten Schritt korreliert Traffic Manager die Zuordnung zwischen Benutzerbasisregion und Azure-Region mit den Wartezeittabellen der Network Intelligence. Diese Tabelle wird für verschiedene Endbenutzernetzwerke verwaltet, um die durchschnittliche Wartezeit für Benutzer aus diesen Regionen zu verstehen, wenn sie eine Verbindung mit Azure-Regionen herstellen. Alle diese Berechnungen werden dann kombiniert, und zwar pro lokaler IP-Adressebene des DNS-Resolvers, bevor sie Ihnen angezeigt werden. Sie können die Informationen auf verschiedene Weise nutzen.

Die Häufigkeit der Datenaktualisierung in der Datenverkehrsanzeige hängt von mehreren internen Dienstvariablen ab. Allerdings werden die Daten einmal alle 24 Stunden aktualisiert.

>[!NOTE]
>Die in der Datenverkehrsansicht beschriebene Wartezeit ist eine repräsentative Wartezeit zwischen dem Endbenutzer und den Azure-Regionen, mit denen eine Verbindung hergestellt wurde, und nicht die Wartezeit von DNS-Lookup. Die Datenverkehrsansicht liefert eine bestmögliche Schätzung der Wartezeit zwischen der lokalen DNS-Auflösung und der Azure-Region, an die die Abfrage weitergeleitet wurde. Sollten nicht genügend Daten zur Verfügung stehen, wird für die Wartezeit ein Nullwert zurückgegeben. 

## <a name="visual-overview"></a>Visuelle Übersicht

Wenn Sie zum Abschnitt **Datenverkehrsansicht** auf Ihrer Traffic Manager-Seite navigieren, sehen Sie eine geografische Karte, die mit Erkenntnissen der Datenverkehrsansicht überlagert ist. Die Karte bietet Informationen zur Benutzergruppe und den Endpunkten für Ihr Traffic Manager-Profil.

![Traffic Manager-Datenverkehrsansicht, Geografische Ansicht][1]

### <a name="user-base-information"></a>Benutzergruppeninformationen

Für die lokalen DNS-Resolver, für die Standortinformationen verfügbar sind, werden diese auf der Karte angezeigt. Die Farbe des DNS-Resolvers kennzeichnet die durchschnittliche Wartezeit für Endbenutzer, die diesen DNS-Resolver für ihre Traffic Manager-Abfragen verwendet haben.

Wenn Sie auf den Standort eines DNS-Resolvers auf der Karte zeigen, wird Folgendes eingeblendet:
- die IP-Adresse des DNS-Resolvers
- das Datenverkehrsvolumen für DNS-Abfragen aus der Sicht von Traffic Manager
- die Endpunkte, an die Datenverkehr vom DNS-Resolver weitergeleitet wurde, als eine Linie zwischen dem Endpunkt und dem DNS-Resolver 
- die durchschnittliche Wartezeit von diesem Standort bis zum Endpunkt, dargestellt als Farbe der verbindenden Linie

### <a name="endpoint-information"></a>Endpunktinformationen

Die Azure-Regionen, in denen sich die Endpunkte befinden, werden als blaue Punkte auf der Karte angezeigt. Externe Endpunkte ohne zugeordnete Azure-Region werden am oberen Rand der Karte angezeigt. Wählen Sie einen beliebigen Endpunkt aus, um die verschiedenen Standorte anzuzeigen (basierend auf dem verwendeten DNS-Resolver), von denen Datenverkehr an diesen Endpunkt weitergeleitet wurde. Die Verbindungen werden als Linie zwischen dem Endpunkt und dem Standort des DNS-Resolvers angezeigt. Ihre Farbe entspricht der repräsentativen Wartezeit zwischen dem jeweiligen Paar. Der Name des Endpunkts und die Azure-Region, in der er ausgeführt wird, werden angezeigt. Die Gesamtmenge der Anforderungen, die von diesem Traffic Manager-Profil an ihn umgeleitet werden, wird ebenfalls angezeigt.


## <a name="tabular-listing-and-raw-data-download"></a>Tabellarische Auflistung und Rohdatendownload

Sie können die Daten der Datenverkehrsansicht in einem tabellarischen Format im Azure-Portal anzeigen. Es gibt für jedes IP/Endpunkt-Paar des DNS-Resolvers einen Eintrag, der Folgendes anzeigt:

* Die IP-Adresse des DNS-Resolvers.
* Der Name.
* Den geografischen Standort der Azure-Region, in der sich der Endpunkt befindet (falls verfügbar).
* Das Volumen der Anforderungen an diesen Endpunkt, die diesem DNS-Resolver zugeordnet sind.
* Die repräsentative Wartezeit von Endbenutzern, die dieses DNS verwenden (sofern verfügbar). 

Sie können die Daten der Datenverkehrsansicht auch als CSV-Datei herunterladen, die dann als Teil eines gewünschten Analyseworkflows verwendet werden kann.

## <a name="billing"></a>Abrechnung

Wenn Sie die Datenverkehrsansicht verwenden, erfolgt die Abrechnung anhand der Anzahl der Datenpunkte, die zur Erstellung der gezeigten Erkenntnisse verwendet wurden. Der derzeit einzige verwendete Datenpunkt sind die Abfragen, die vom Traffic Manager-Profil empfangen werden. Weitere Informationen zu den Preisen finden Sie auf der [Traffic Manager-Seite mit Preisdetails](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faqs"></a>Häufig gestellte Fragen

* [Wozu dient die Datenverkehrsansicht?](./traffic-manager-faqs.md#what-does-traffic-view-do)

* [Welche Vorteile ergeben sich für mich durch die Verwendung der Datenverkehrsansicht?](./traffic-manager-faqs.md#how-can-i-benefit-from-using-traffic-view)

* [Inwiefern unterscheidet sich die Datenverkehrsansicht von den Traffic Manager-Metriken, die über Azure Monitor verfügbar sind?](./traffic-manager-faqs.md#how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor)

* [Werden für die Datenverkehrsansicht EDNS-Clientsubnetz-Informationen verwendet?](./traffic-manager-faqs.md#does-traffic-view-use-edns-client-subnet-information)

* [Wie viele Tage mit Daten werden von der Datenverkehrsansicht genutzt?](./traffic-manager-faqs.md#how-many-days-of-data-does-traffic-view-use)

* [Wie werden externe Endpunkte von der Datenverkehrsansicht behandelt?](./traffic-manager-faqs.md#how-does-traffic-view-handle-external-endpoints)

* [Muss ich die Datenverkehrsansicht für jedes Profil meines Abonnements aktivieren?](./traffic-manager-faqs.md#do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription)

* [Wie kann ich die Datenverkehrsansicht deaktivieren?](./traffic-manager-faqs.md#how-can-i-turn-off-traffic-view)

* [Wie funktioniert die Abrechnung für die Datenverkehrsansicht?](./traffic-manager-faqs.md#how-does-traffic-view-billing-work)

## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-overview.md)
- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
- Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](./quickstart-create-traffic-manager-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-traffic-view-overview/trafficview.png