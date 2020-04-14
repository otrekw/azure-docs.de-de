---
title: Steuern des Datenverkehrs mit Traffic Manager
description: Hier finden Sie bewährte Methoden für die Konfiguration von Azure Traffic Manager bei der Integration in Azure App Service.
ms.assetid: dabda633-e72f-4dd4-bf1c-6e945da456fd
ms.topic: article
ms.date: 02/25/2016
ms.custom: seodec18
ms.openlocfilehash: 040f84288c66f4506919e775b9ea41324b617cfa
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437897"
---
# <a name="controlling-azure-app-service-traffic-with-azure-traffic-manager"></a>Steuern des Azure App Service-Datenverkehrs mit Azure Traffic Manager
> [!NOTE]
> Dieser Artikel bietet zusammenfassende Informationen zu Microsoft Azure Traffic Manager im Hinblick auf Azure App Service. Weitere Informationen über Azure Traffic Manager selbst finden Sie unter den Links am Ende dieses Artikels.
> 
> 

## <a name="introduction"></a>Einführung
Mit Azure Traffic Manager können Sie steuern, wie Anforderungen von Webclients auf Apps in Azure App Service verteilt werden. Wenn einem Azure Traffic Manager-Profil App Service-Endpunkte hinzugefügt werden, verfolgt Azure Traffic Manager den Status Ihrer App Service-Apps (aktiv, angehalten oder gelöscht), sodass der gewünschte Endpunkt als Empfänger des Datenverkehrs ausgewählt werden kann.

## <a name="routing-methods"></a>Routingmethoden
Azure Traffic Manager verwendet vier verschiedene Routingmethoden. Diese Methoden werden in der folgenden Liste beschrieben, soweit sie Azure App Service betreffen.

* **[Priorität](../traffic-manager/traffic-manager-routing-methods.md#priority-traffic-routing-method):** Verwenden einer primären App für den gesamten Datenverkehr und Bereitstellen von Sicherungen für den Fall, dass die primäre App oder die Sicherungs-Apps nicht verfügbar sind
* **[Gewichtet](../traffic-manager/traffic-manager-routing-methods.md#weighted):** Verteilen des Datenverkehrs auf eine Gruppe von Apps, entweder gleichmäßig oder gewichtet, gemäß Ihrer Definition
* **[Leistung](../traffic-manager/traffic-manager-routing-methods.md#performance):** Wenn Apps an unterschiedlichen geografischen Standorten genutzt werden, verwenden der „nächstgelegenen“ App im Hinblick auf die niedrigste Netzwerklatenz
* **[Geografisch](../traffic-manager/traffic-manager-routing-methods.md#geographic):** Weiterleiten von Benutzern basierend auf dem geografischen Standort, von dem ihre DNS-Abfrage stammt, zu bestimmten Apps 

Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](../traffic-manager/traffic-manager-routing-methods.md).

## <a name="app-service-and-traffic-manager-profiles"></a>App Service und Traffic Manager-Profile
Für die Konfiguration zur Steuerung des App Service-App-Datenverkehrs erstellen Sie ein Profil in Azure Traffic Manager, das eine der vier zuvor beschriebenen Lastenausgleichsmethoden verwendet. Anschließend fügen Sie dem Profil die Endpunkte (in diesem Fall App Service) hinzu, für die Sie den Datenverkehr steuern möchten. Der App-Status (aktiv, angehalten oder gelöscht) wird regelmäßig an das Profil übermittelt, sodass Azure Traffic Manager den Datenverkehr entsprechend leiten kann.

Beachten Sie die folgenden Aspekte, wenn Sie Azure Traffic Manager mit Azure verwenden:

* Bei reinen App-Bereitstellungen innerhalb derselben Region bietet App Service bereits eine Failover- und Roundrobin-Funktion, die unabhängig vom App-Modus ist.
* Bei Bereitstellungen in derselben Region, die App Service zusammen mit anderen Azure-Clouddiensten verwenden, können Sie beide Endpunkttypen kombinieren, um Hybridszenarien zu ermöglichen.
* Sie können in einem Profil nur einen App Service-Endpunkt pro Region angeben. Wenn Sie eine App als Endpunkt für eine Region auswählen, stehen die verbleibenden Apps in dieser Region nicht mehr für dieses Profil zur Auswahl.
* Die App Service-Endpunkte, die Sie in einem Azure Traffic Manager-Profil festlegen, werden im Abschnitt **Domänennamen** auf der Konfigurationsseite für die App im Profil angezeigt, können dort jedoch nicht konfiguriert werden.
* Nachdem Sie einem Profil eine App hinzugefügt haben, wird in der **Website-URL** im Dashboard der Portalseite der App die benutzerdefinierte Domänen-URL der App angezeigt, sofern Sie eine eingerichtet haben. Anderenfalls wird die URL des Traffic Manager-Profils angezeigt (z.B. `contoso.trafficmanager.net`). Sowohl der direkte Domänenname der App als auch die Traffic Manager-URL werden auf der Konfigurationsseite der App im Abschnitt **Domänennamen** angezeigt.
* Ihre benutzerdefinierten Domänennamen funktionieren wie erwartet. Sie fügen sie Ihren Apps hinzu, müssen jedoch auch die DNS-Zuordnung konfigurieren, um auf die Traffic Manager-URL zu verweisen. Informationen, wie Sie eine benutzerdefinierte Domäne für eine App Service-App einrichten, finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service mit Traffic Manager-Integration](configure-domain-traffic-manager.md).
* Sie können einem Azure Traffic Manager-Profil nur Apps im Standard- oder Premium-Modus hinzufügen.
* Das Hinzufügen einer App zu einem Traffic Manager-Profil bewirkt, dass die App neu gestartet wird.

## <a name="next-steps"></a>Nächste Schritte
Einen Überblick über die Konzepte und technischen Aspekte von Azure Traffic Manager finden Sie unter [Traffic Manager-Übersicht](../traffic-manager/traffic-manager-overview.md).


