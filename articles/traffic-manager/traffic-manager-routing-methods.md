---
title: 'Azure Traffic Manager: Routingmethoden für Datenverkehr'
description: Dieser Artikel erläutert die verschiedenen Methoden für das Datenverkehrsrouting, die von Traffic Manager verwendet werden.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: eeebded128f636ecba2e4e0dab1bc2f0632aaa6a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98730971"
---
# <a name="traffic-manager-routing-methods"></a>Traffic Manager-Routingmethoden

Azure Traffic Manager unterstützt sechs Methoden für das Datenverkehrsrouting, um zu bestimmen, wie Netzwerkverkehr an die verschiedenen Dienstendpunkte weitergeleitet wird. Der Traffic Manager wendet für jedes Profil die Datenverkehrsrouting-Methode an, die ihm in der jeweils empfangenen DNS-Abfrage zugeordnet ist. Mit der Methode für das Datenverkehrsrouting wird festgelegt, welcher Endpunkt in der DNS-Antwort zurückgegeben wird.

Die folgenden vier Methoden für das Datenverkehrsrouting sind in Traffic Manager verfügbar:

* **[Priorität:](#priority-traffic-routing-method)** Wählen Sie **Priorität** srouting aus, wenn Sie einen primären Dienstendpunkt für den gesamten Datenverkehr haben möchten. Für den Fall, dass der primäre oder einer der Sicherungsendpunkte nicht verfügbar ist, können Sie mehrere Sicherungsendpunkte bereitstellen.
* **[Gewichtet:](#weighted)** Wählen Sie **gewichtetes** Routing, wenn Sie Datenverkehr anhand der definierten Gewichtung auf eine Gruppe von Endpunkten verteilen möchten. Legen Sie die Gewichtung identisch fest, um eine gleichmäßige Verteilung auf alle Endpunkte zu erzielen.
* **[Leistung:](#performance)** Wählen Sie **Leistung** srouting aus, wenn sich Ihre Endpunkte an unterschiedlichen geografischen Standorten befinden und Endbenutzer an den „nächstgelegenen“ Endpunkt für die geringste Netzwerklatenz weitergeleitet werden sollen.
* **[Geografisch:](#geographic)** Wählen Sie **geografisch** es Routing aus, um Benutzer an bestimmte Endpunkte (Azure, Extern oder Geschachtelt) weiterzuleiten, je nach dem geografischen Ursprungsort ihrer DNS-Abfragen. Mit dieser Routingmethode können Sie Compliance in Szenarien wie Datenhoheitsmandate, Verortung von Inhalten und Benutzererfahrungen und das Messen von Datenverkehr aus verschiedenen Regionen einhalten.
* **[MultiValue](#multivalue):** Wählen Sie **MultiValue** für Traffic Manager-Profile aus, die nur IPv4/IPv6-Adressen als Endpunkte enthalten können. Wenn eine Abfrage für dieses Profil empfangen wird, werden alle fehlerfreien Endpunkte zurückgegeben.
* **[Subnetz:](#subnet)** Wählen Sie die Datenverkehrsrouting-Methode **Subnetz** aus, um Gruppen von Endbenutzer-IP-Adressbereichen einem bestimmten Endpunkt zuzuordnen. Wenn eine Anforderung empfangen wird, wird der Endpunkt zurückgegeben, der für die Quell-IP-Adresse dieser Anforderung zugeordnet ist. 


Alle Traffic Manager-Profile verfügen über Integritätsüberwachung und automatisches Failover für Endpunkte. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md). Innerhalb eines einzelnen Traffic Manager-Profils können Sie immer nur eine Methode für das Datenverkehrsrouting gleichzeitig konfigurieren. Sie können jederzeit eine andere Methode für das Datenverkehrsrouting für Ihr Profil auswählen. Ihre Änderungen werden innerhalb einer Minute ohne Ausfallzeiten angewendet. Sie können Methoden für das Datenverkehrsrouting kombinieren, indem Sie geschachtelte Traffic Manager-Profile verwenden. Das Schachteln von Profilen gestattet ausgefeilte Konfigurationen beim Datenverkehrsrouting, die die Anforderungen größerer und komplexer Anwendungen erfüllen. Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Prioritätsmethode für das Datenverkehrsrouting

Organisationen möchten häufig sicherstellen, dass ihre Dienste zuverlässig funktionieren. Dazu stellen sie einen oder mehrere Sicherungsdienste für den Fall bereit, dass der primäre Dienst ausfällt. Mithilfe der prioritätsbasierten Methode für das Datenverkehrsrouting können Azure-Kunden dieses Failovermuster problemlos implementieren.

![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/priority.png)

Das Traffic Manager-Profil enthält eine Liste mit Dienstendpunkten, denen Prioritäten zugeordnet sind. Standardmäßig sendet Traffic Manager den gesamten Datenverkehr an den primären Endpunkt (mit der höchsten Priorität). Wenn der primäre Endpunkt nicht verfügbar ist, leitet Traffic Manager den Datenverkehr an den sekundären Endpunkt weiter. In einer Situation, in der weder der primäre noch der sekundäre Endpunkt verfügbar ist, wird der Datenverkehr an den dritten Endpunkt gesendet usw. Ob ein Endpunkt verfügbar ist, hängt vom konfigurierten Status (aktiviert oder deaktiviert) sowie von der fortlaufenden Endpunktüberwachung ab.

### <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Mit Azure Resource Manager wird die Endpunktpriorität mithilfe der für jeden Endpunkt definierten Prioritätseigenschaft explizit konfiguriert. Für diese Eigenschaft wird ein Wert zwischen 1 und 1000 festgelegt. Ein niedrigerer Wert stellt eine höhere Priorität dar. Der gleiche Prioritätswert kann nicht für mehrere Endpunkte konfiguriert werden. Diese Eigenschaft kann optional festgelegt werden. Wird sie nicht festgelegt, wird eine Standardpriorität basierend auf der Endpunktreihenfolge verwendet.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Gewichtete Methode für das Datenverkehrsrouting
Bei der gewichteten Methode für das Datenverkehrsrouting wird der Datenverkehr gleichmäßig verteilt oder eine vordefinierte Gewichtung verwendet.

![Gewichtete Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/weighted.png)

Bei der gewichteten Methode für das Datenverkehrsrouting wird jedem Endpunkt im Rahmen der Traffic Manager-Profilkonfiguration eine Gewichtung zugewiesen. Die Gewichtung ist eine Ganzzahl zwischen 1 und 1000. Dieser Parameter ist optional. Wenn er nicht angegeben wird, wird die Standardgewichtung 1 verwendet. Je höher die Gewichtung, desto höher die Priorität.

Für jede empfangene DNS-Abfrage wählt Traffic Manager einen verfügbaren Endpunkt nach dem Zufallsprinzip aus. Die Wahrscheinlichkeit für die Auswahl eines Endpunkts basiert hierbei auf der Gewichtung, die allen verfügbaren Endpunkten zugewiesen wurde. Die Verwendung der gleichen Gewichtung für alle Endpunkte führt zu einer gleichmäßigen Verteilung des Datenverkehrs. Wenn für bestimmte Endpunkte eine höhere oder niedrigere Gewichtung verwendet wird, werden diese Endpunkte häufiger oder seltener in den DNS-Antworten zurückgegeben.

Durch die gewichtete Methode werden einige nützliche Szenarios ermöglicht:

* Allmähliches Anwendungsupgrade: Weisen Sie einen Prozentwert des Datenverkehrs zu, der an einen neuen Endpunkt weitergeleitet wird, und erhöhen Sie den Datenverkehr dann im Lauf der Zeit allmählich auf 100 Prozent.
* Anwendungsmigration zu Azure: Erstellen Sie ein Profil mit Azure- und externen Endpunkten. Passen Sie die Gewichtung der Endpunkte so an, dass die neuen Endpunkte bevorzugt werden.
* Cloudbursting für mehr Kapazität: Erweitern Sie eine lokale Bereitstellung schnell in die Cloud, indem Sie sie hinter einem Traffic Manager-Profil zur Verfügung stellen. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können Sie weitere Endpunkte hinzufügen oder aktivieren. Geben Sie dann an, welcher Teil des Datenverkehrs an jeden Endpunkt gesendet wird.

Sie haben die Möglichkeit, Gewichtungen mithilfe des Azure-Portals, mit Azure PowerShell, über die Befehlszeilenschnittstelle oder mithilfe von REST-APIs zu konfigurieren.

Denken Sie daran, dass DNS-Antworten von Clients zwischengespeichert werden. Sie werden auch von den rekursiven DNS-Servern zwischengespeichert, die von den Clients um Auflösen von DNS-Namen verwendet werden. Dieses Zwischenspeichern hat möglicherweise Auswirkungen auf die gewichtete Datenverkehrsverteilungen. Wenn die Anzahl von Clients und rekursiven DNS-Servern hoch ist, funktioniert die Verteilung des Datenverkehrs wie erwartet. Wenn die Anzahl von Clients oder rekursiven DNS-Servern jedoch gering ist, kann diese Zwischenspeicherung die Verteilung des Datenverkehrs massiv verzerren.

Gängige Anwendungsfälle:

* Entwicklungs- und Testumgebungen
* Kommunikation zwischen Anwendungen
* Anwendungen mit einer kleinen Benutzerbasis, die eine gemeinsame rekursive DNS-Infrastruktur nutzen (beispielsweise die Mitarbeiter eines Unternehmens, in dem Verbindungen über einen Proxy hergestellt werden)

Diese Auswirkungen der DNS-Zwischenspeicherung gelten für alle DNS-basierten Systeme für das Datenverkehrsrouting, nicht nur für Azure Traffic Manager. In einigen Fällen lässt sich das Problem möglicherweise durch eine explizite Bereinigung des DNS-Cache umgehen. Wenn dies nicht funktioniert, eignet sich eine alternative Methode für das Datenverkehrsrouting eventuell besser.

## <a name="performance-traffic-routing-method"></a><a name = "performance"></a>Leistungsorientierte Methode für das Datenverkehrsrouting

Die Reaktionsfähigkeit Ihrer Anwendungen kann verbessert werden, indem Sie Endpunkte an mindestens zwei Standorten auf der ganzen Welt bereitstellen. Mit der leistungsorientierten Methode für das Datenverkehrsrouting können Sie Datenverkehr an den Standort weiterleiten, der Ihnen am nächsten liegt.

![Leistungsorientierte Methode für das Datenverkehrsrouting in Azure Traffic Manager](media/traffic-manager-routing-methods/performance.png)

Der nächstgelegene Endpunkt ist nicht unbedingt derjenige, der geografisch am nächsten liegt. Vielmehr wird mit dieser Methode für das Datenverkehrsrouting der nächstgelegene Endpunkt durch die Messung der Netzwerklatenz bestimmt. Traffic Manager überwacht anhand einer Internetlatenztabelle die Roundtripzeit zwischen IP-Adressbereichen und den einzelnen Azure-Rechenzentren.

In dieser Internetlatenztabelle sucht der Dienst nach der IP-Quelladresse der eingehenden DNS-Abfrage. Anschließend wählt der Traffic Manager einen im Azure-Rechenzentrum verfügbaren Endpunkt mit der niedrigsten Wartezeit für diesen IP-Adressbereich aus. Dann gibt Traffic Manager diesen Endpunkt in der DNS-Antwort zurück.

Wie unter [Funktionsweise von Traffic Manager](traffic-manager-how-it-works.md) beschrieben, empfängt Traffic Manager DNS-Abfragen nicht direkt von Clients. Stattdessen erhält er DNS-Abfragen vom rekursiven DNS-Dienst, für dessen Verwendung die Clients konfiguriert sind. Somit handelt es sich bei der IP-Adresse, die zur Ermittlung des „nächstgelegenen“ Endpunkts verwendet wird, nicht um die IP-Adresse des Clients, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. Diese IP-Adresse ist ein guter Proxy für den Client.


Um Änderungen im globalen Internet und neue Azure-Regionen zu berücksichtigen, aktualisiert Traffic Manager die verwendete Internetlatenztabelle regelmäßig. Die Anwendungsleistung ist jedoch je nach Echtzeitschwankung der Auslastung im Internet unterschiedlich. Die leistungsorientierte Methode für das Datenverkehrsrouting überwacht nicht die Auslastung eines bestimmten Dienstendpunkts. Wenn ein Endpunkt nicht mehr verfügbar ist, wird er von Traffic Manager nicht in die DNS-Abfrageantworten aufgenommen.


Beachten Sie Folgendes:

* Wenn Ihr Profil mehrere Endpunkte in der gleichen Azure-Region enthält, wird der an diese Region geleitete Datenverkehr gleichmäßig auf die verfügbaren Endpunkte verteilt. Wenn Sie eine andere Verteilung des Datenverkehrs innerhalb einer Region bevorzugen, können Sie dies mithilfe von [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) erreichen.
* Wenn alle verfügbaren Endpunkte in der nächsten Azure-Region heruntergestuft sind, verschiebt der Traffic Manager den Datenverkehr zu den Endpunkten in der nächstgelegenen Azure-Region. Wenn Sie eine bevorzugte Failoversequenz definieren möchten, verwenden Sie [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).
* Wenn die leistungsorientierte Methode für das Datenverkehrsrouting mit externen oder geschachtelten Endpunkten verwendet wird, müssen Sie den Standort dieser Endpunkte angeben. Wählen Sie die Azure-Region aus, die Ihrer Bereitstellung am nächsten ist. Bei diesen Standorten handelt es sich um die Werte, die von der Internetlatenztabelle unterstützt werden.
* Der Algorithmus, mit dem der Endpunkt ausgewählt wird, ist deterministisch. Wiederholte DNS-Abfragen des gleichen Clients werden an den gleichen Endpunkt geleitet. Clients verwenden unterwegs üblicherweise verschiedene rekursive DNS-Server. Der Client kann daher an einen anderen Endpunkt weitergeleitet werden. Routing kann sich ebenfalls auf die Internetlatenztabelle auswirken. Dies ist der Grund, warum die leistungsorientierte Methode für das Datenverkehrsrouting nicht gewährleistet, dass ein Client immer an denselben Endpunkt weitergeleitet wird.
* Wenn die Internetlatenztabelle geändert wird, stellen Sie möglicherweise fest, dass einige Clients an einen anderen Endpunkt weitergeleitet werden. Diese Änderung reflektiert ein genaueres Routing basierend auf aktuellen Latenzdaten. Diese Updates sind wichtig, um angesichts der kontinuierlichen Entwicklung des Internets die Genauigkeit der leistungsorientierten Methode für das Datenverkehrsrouting sicherzustellen.

## <a name="geographic-traffic-routing-method"></a><a name = "geographic"></a>Geografische Routingmethode für Datenverkehr

Traffic Manager-Profile können für die Verwendung der geografischen Routingmethode konfiguriert werden, damit Benutzer je nach dem geografischen Ursprungsort ihrer DNS-Abfrage an bestimmte Endpunkte (Azure, Extern oder Geschachtelt) weitergeleitet werden. Mit dieser Routingmethode können Sie bei Datenhoheitsmandaten, der Verortung von Inhalten und Benutzererfahrungen und dem Messen von Datenverkehr aus verschiedenen Regionen die Compliance einhalten.
Wenn ein Profil für geografisches Routing konfiguriert ist, muss jedem Endpunkt, der dem betreffenden Profil zugeordnet ist, eine Reihe geografischer Regionen zugeordnet sein. Die Granularität einer geografischen Region kann die folgenden Abstufungen aufweisen 
- Welt – jede Region
- Regionale Gruppierung - zum Beispiel Afrika, Naher Osten, Australien/Pazifik usw. 
- Land/Region - zum Beispiel Irland, Peru, Hongkong (SAR) usw. 
- Bundesland/Provinz: zum Beispiel USA-Kalifornien, Australien-Queensland, Kanada-Alberta usw. (Hinweis: Diese Granularitätsstufe wird nur für Bundesstaaten/Provinzen in Australien, Kanada und den USA unterstützt.)

Wenn einem Endpunkt eine Region oder eine Reihe von Regionen zugewiesen ist, werden alle Anforderungen aus diesen Regionen nur an den betreffenden Endpunkt geroutet. Traffic Manager verwendet die IP-Quelladresse der DNS-Abfrage, um die Region zu bestimmen, aus der eine Benutzerabfrage stammt. Häufig handelt es sich um die IP-Adresse des lokalen DNS-Resolvers, der die Abfrage für den Benutzer ausführt.  

![Geografische Methode für das Datenverkehrsrouting in Azure Traffic Manager](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager liest die IP-Quelladresse der DNS-Abfrage und entscheidet, aus welcher geografischen Region sie stammt. Anschließend überprüft er, ob ein Endpunkt vorhanden ist, dem die betreffende geografische Region zugeordnet ist. Diese Suche beginnt auf der niedrigsten Granularitätsstufe (Bundesland/Provinz für die unterstützten Regionen, andernfalls auf der Ebene Land/Region) und schreitet bis zu höchsten Ebene **Welt** fort. Die erste Übereinstimmung, die bei diesem Durchlauf gefunden wird, wird als der in der Antwort auf die Abfrage zurückzugebende Endpunkt ausgewählt. Falls eine Übereinstimmung mit einem geschachtelten Endpunkttyp festgestellt wird, wird auf der Grundlage seiner Routingmethode ein Endpunkt mit dem entsprechenden untergeordneten Profil zurückgegeben. Die folgenden Punkte treffen auf dieses Verhalten zu:

- Wenn der Routingtyp geografisches Routing ist, kann ein Endpunkt in einem Traffic Manager-Profil nur einer geografischen Region zugeordnet werden. Diese Einschränkung stellt sicher, dass das Routing von Benutzern deterministisch erfolgt, sodass Kunden Szenarios aktivieren können, die unzweideutige geografische Grenzen erfordern.
- Wenn die Region eines Benutzers unter der geografischen Zuordnung zweier Endpunkte aufgeführt wird, wählt Traffic Manager den Endpunkt mit der niedrigsten Granularität aus. Traffic Manager verwirft Routinganforderungen aus der betreffenden Region an den anderen Endpunkt. Betrachten wir zum Beispiel ein Profil vom geografischen Routingtyp mit zwei Endpunkten: Endpunkt 1 und Endpunkt 2. Endpunkt 1 ist für den Empfang von Datenverkehr aus Irland und Endpunkt 2 für den Empfang von Datenverkehr aus Europa konfiguriert. Wenn eine Anforderung aus Irland stammt, wird sie immer an Endpunkt 1 geroutet.
- Da eine Region nur einem Endpunkt zugeordnet sein kann, gibt Traffic Manager eine Antwort zurück, unabhängig vom Integritätsstatus des Endpunkts.

    >[!IMPORTANT]
    >Es wird Kunden dringend empfohlen, die geografische Routingmethode für Endpunkte vom Typ „Geschachtelt“ zu verwenden, die über untergeordnete Profile mit wenigstens zwei erreichbaren Endpunkten verfügen.
- Wenn ein übereinstimmender Endpunkt gefunden wird und dieser den Status **Stopped** (Beendet) aufweist, gibt Traffic Manager die Antwort „NODATA“ zurück. In diesem Fall erfolgen keine weiteren Suchläufe weiter oben in der Hierarchie der geografischen Regionen. Dieses Verhalten trifft auch auf geschachtelte Endpunkttypen zu, wenn sich das untergeordnete Profil im Status **Beendet** oder **Deaktiviert** befindet.
- Wenn sich ein Endpunkt im Status **Disabled** (Deaktiviert) befindet, wird er in den Prozess der Regionszuordnung nicht einbezogen. Dieses Verhalten trifft auch auf geschachtelte Endpunkttypen zu, wenn der Endpunkt sich im Status **Deaktiviert** befindet.
- Wenn eine Abfrage aus einer geografischen Region stammt, für die es im betreffenden Profil keine Zuordnung gibt, gibt Traffic Manager die Antwort „NODATA“ zurück. Daher wird dringend empfohlen, geografisches Routing mit einem Endpunkt zu verwenden, idealerweise vom Typ „Geschachtelt“ mit mindestens zwei Endpunkten im untergeordneten Profil, dem die Region **Welt** zugeordnet ist. Diese Konfiguration stellt außerdem sicher, dass alle IP-Adressen, die keiner Region zugeordnet sind, verarbeitet werden.

Wie unter [Funktionsweise von Traffic Manager](traffic-manager-how-it-works.md) beschrieben, empfängt Traffic Manager DNS-Abfragen nicht direkt von Clients. Er erhält DNS-Abfragen vom rekursiven DNS-Dienst, für dessen Verwendung die Clients konfiguriert sind. Dies ist der Grund, warum es sich bei der IP-Adresse, die zur Ermittlung der Region verwendet wird, nicht um die IP-Adresse eines Clients handelt, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. Diese IP-Adresse ist ein guter Proxy für den Client.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Was sind einige Anwendungsfälle, in denen geografisches Routing nützlich ist?](./traffic-manager-faqs.md#what-are-some-use-cases-where-geographic-routing-is-useful)

* [Wie entscheide ich, ob ich die leistungsbezogene oder geografische Routingmethode verwenden sollte?](./traffic-manager-faqs.md#how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method)

* [Welche Bereiche werden von Traffic Manager für das geografische Routing unterstützt?](./traffic-manager-faqs.md#what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing)

* [Wie bestimmt Traffic Manager, woher die Abfrage eines Benutzers stammt?](./traffic-manager-faqs.md#how-does-traffic-manager-determine-where-a-user-is-querying-from)

* [Ist garantiert, dass Traffic Manager immer den genauen geografischen Standort des Benutzers ermitteln kann?](./traffic-manager-faqs.md#is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case)

* [Muss sich ein Endpunkt physisch in der Region befinden, für die er für geografisches Routen konfiguriert ist?](./traffic-manager-faqs.md#does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing)

* [Kann ich geografische Regionen Endpunkten in einem Profil zuordnen, das nicht für geografisches Routing konfiguriert ist?](./traffic-manager-faqs.md#can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing)

* [Warum tritt ein Fehler auf, wenn ich versuche, die Routingmethode eines vorhandenen Profils in geografisch zu ändern?](./traffic-manager-faqs.md#why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic)

* [Warum wird es Kunden dringend empfohlen, in einem Profil mit aktiviertem geografischem Routing geschachtelte Profile anstelle von Endpunkten zu erstellen?](./traffic-manager-faqs.md#why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled)

* [Gibt es Einschränkungen hinsichtlich der API-Version, die diesen Routingtyp unterstützt?](./traffic-manager-faqs.md#are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type)

## <a name="multivalue-traffic-routing-method"></a><a name = "multivalue"></a>Routingmethode „MultiValue“ für Datenverkehr
Mit der Routingmethode **MultiValue** für Datenverkehr können Sie mehrere fehlerfreie Endpunkte in einer einzelnen DNS-Abfrageantwort abrufen. Diese Konfiguration ermöglicht es dem Aufrufer, clientseitige Wiederholungsversuche mit anderen Endpunkten auszuführen, falls ein zurückgegebener Endpunkt nicht reagiert. Dieses Muster kann zur Erhöhung der Verfügbarkeit eines Diensts beitragen und bei einer neuen DNS-Abfrage zum Abrufen eines fehlerfreien Endpunkts die Latenz verringern. Die Routingmethode „MultiValue“ funktioniert nur, wenn alle Endpunkte vom Typ „Extern“ und als IPv4- oder IPv6-Adressen angegeben sind. Wenn eine Abfrage für dieses Profil empfangen wird, werden alle fehlerfreien Endpunkte unter Berücksichtigung einer konfigurierbaren maximalen Rückgabeanzahl zurückgegeben.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Was sind einige Anwendungsfälle, in denen MultiValue-Routing nützlich ist?](./traffic-manager-faqs.md#what-are-some-use-cases-where-multivalue-routing-is-useful)

* [Wie viele Endpunkte werden zurückgegeben, wenn das MultiValue-Routing verwendet wird?](./traffic-manager-faqs.md#how-many-endpoints-are-returned-when-multivalue-routing-is-used)

* [Erhalte ich den gleichen Satz von Endpunkten, wenn das MultiValue-Routing verwendet wird?](./traffic-manager-faqs.md#will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used)

## <a name="subnet-traffic-routing-method"></a><a name = "subnet"></a>Routingmethode „Subnetz“ für Datenverkehr
Mit der Routingmethode **Subnetz** für Datenverkehr können Sie eine Gruppe von Endbenutzer-IP-Adressbereichen bestimmten Endpunkten in einem Profil zuordnen. Wenn Traffic Manager eine DNS-Abfrage für dieses Profil empfängt, wird die IP-Quelladresse dieser Anforderung überprüft. Anschließend bestimmt er, welchem Endpunkt sie zugeordnet ist, und gibt diesen Endpunkt in der Abfrageantwort zurück. In den meisten Fällen ist die IP-Quelladresse der DNS-Resolver, der vom Aufrufer verwendet wird.

Die IP-Adresse, die einem Endpunkt zugeordnet werden soll, kann in Form von CIDR-Bereichen (etwa 1.2.3.0/24) oder als Adressbereich (etwa 1.2.3.4–5.6.7.8) angegeben werden. Die einem Endpunkt zugeordneten IP-Adressbereiche müssen innerhalb dieses Profils eindeutig sein. Der Adressbereich darf sich nicht mit dem Satz von IP-Adressen eines anderen Endpunkts im selben Profil überlappen.
Wenn Sie einen Endpunkt ohne Adressbereich definieren, funktioniert dieser als Fallback und verarbeitet den Datenverkehr aller verbleibenden Subnetze. Wenn kein Fallbackendpunkt enthalten ist, sendet der Traffic Manager eine NODATA-Antwort für undefinierte Bereiche. Es empfiehlt sich dringend, einen Fallbackendpunkt zu definieren, um sicherzustellen, dass alle möglichen IP-Adressbereiche für Ihre Endpunkte angegeben werden.

Die Routingmethode „Subnetz“ kann verwendet werden, um Benutzern, die eine Verbindung über einen bestimmten IP-Adressraum herstellen, ein anderes Erlebnis zu bieten. Beispielsweise können Sie festlegen, dass alle Anforderungen aus Ihrem Firmenbüro an einen anderen Endpunkt weitergeleitet werden. Diese Routingmethode ist besonders nützlich, wenn Sie versuchen, eine rein interne Version Ihrer App zu testen. Ein weiteres Szenario wäre etwa die Bereitstellung einer anderen Erfahrung für Benutzer, die eine Verbindung über einen bestimmten ISP herstellen, um beispielsweise Benutzer eines bestimmten ISP zu blockieren.

### <a name="faqs"></a>Häufig gestellte Fragen

* [Welche Anwendungsfälle gibt es, in denen Subnetzrouting sinnvoll ist?](./traffic-manager-faqs.md#what-are-some-use-cases-where-subnet-routing-is-useful)

* [Wie erkennt Traffic Manager die IP-Adresse des Endbenutzers?](./traffic-manager-faqs.md#how-does-traffic-manager-know-the-ip-address-of-the-end-user)

* [Wie kann ich bei der Verwendung von Subnetzrouting IP-Adressen angeben?](./traffic-manager-faqs.md#how-can-i-specify-ip-addresses-when-using-subnet-routing)

* [Wie kann ich bei der Verwendung von Subnetzrouting einen Fallbackendpunkt angeben?](./traffic-manager-faqs.md#how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing)

* [Was geschieht, wenn ein Endpunkt in einem Profil vom Typ Subnetzrouting deaktiviert ist?](./traffic-manager-faqs.md#what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile)


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe der [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md)