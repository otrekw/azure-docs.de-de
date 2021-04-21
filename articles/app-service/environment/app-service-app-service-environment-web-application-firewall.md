---
title: Konfigurieren einer WAF
description: Hier erfahren Sie, wie Sie entweder mit Azure Application Gateway oder mit der WAF eines Drittanbieters eine Web Application Firewall (WAF) vor Ihrer App Service-Umgebung konfigurieren.
author: ccompy
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.topic: tutorial
ms.date: 03/03/2018
ms.author: stefsch
ms.custom: mvc, seodec18
ms.openlocfilehash: 56d931f2346e5a0b615d3f11dce3b06396e586b4
ms.sourcegitcommit: 272351402a140422205ff50b59f80d3c6758f6f6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2021
ms.locfileid: "107588716"
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurieren einer Web Application Firewall (WAF) für eine App Service-Umgebung
## <a name="overview"></a>Übersicht

Web Application Firewalls (WAFs) tragen zum Schutz Ihrer Webanwendungen bei, indem sie eingehenden Webdatenverkehr untersuchen und die Einschleusung von SQL-Befehlen, websiteübergreifendes Scripting, das Hochladen von Schadsoftware sowie DDoS-Angriffe und andere Angriffe unterbinden. Darüber hinaus überprüfen sie die Antworten der Back-End-Webserver, um Datenverluste zu verhindern (Data Loss Prevention, DLP). Zusammen mit der von App Service-Umgebungen bereitgestellten Isolierung und zusätzlichen Skalierung ergibt sich dadurch eine ideale Umgebung für das Hosten geschäftswichtiger Webanwendungen, die böswilligen Anforderungen abwehren und hohe Datenverkehrsvolumen bewältigen müssen. Azure bietet eine WAF-Funktion mit [Application Gateway](../../application-gateway/overview.md).  Im Dokument [Integrieren Ihrer ILB-App Service-Umgebung in ein Application Gateway](./integrate-with-application-gateway.md) erfahren Sie, wie Sie Ihre App Service-Umgebung in eine Application Gateway-Instanz integrieren.

Neben Azure Application Gateway stehen verschiedene Marketplace-Optionen wie [Barracuda WAF for Azure](https://www.barracuda.com/programs/azure) zur Verfügung, die über den [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/barracudanetworks.waf?tab=PlansAndPrice) bezogen werden können. Im weiteren Verlauf dieses Dokuments erfahren Sie, wie Sie Ihre App Service-Umgebung in ein Barracuda WAF-Gerät integrieren.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Einrichten
Für dieses Dokument konfigurieren wir die App Service-Umgebung hinter mehreren Barracuda WAF-Instanzen mit Lastenausgleich, sodass nur Datenverkehr von der WAF die App Service-Umgebung erreichen kann und kein Zugriff aus der DMZ möglich ist. Außerdem haben wir Azure Traffic Manager die Barracuda WAF-Instanzen für den Lastenausgleich von Azure-Rechenzentren und Regionen vorgelagert. Ein allgemeines Diagramm der Einrichtung sieht aus wie in der folgenden Abbildung dargestellt:

![Diagram: optionale Azure Traffic Manager-Instanz, die eine Verbindung mit Web Application Firewall-Instanzen herstellt, die wiederum eine Verbindung mit der Netzwerk-ACL herstellen, um nur Datenverkehr von der Firewall in einer App Service-Umgebung zuzulassen, die Web-, API- und Mobil-Apps für zwei Umgebungen enthält][Architecture] 

> [!NOTE]
> Mit der Einführung der [ILB-Unterstützung für die App Service-Umgebung](app-service-environment-with-internal-load-balancer.md) können Sie die ASE so konfigurieren, dass darauf von der DMZ nicht zugegriffen werden kann und sie nur im privaten Netzwerk zur Verfügung steht. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurieren der App Service-Umgebung
Informationen zum Konfigurieren einer App Service-Umgebung finden Sie in [unserer Dokumentation](app-service-web-how-to-create-an-app-service-environment.md) zu diesem Thema. Sobald Sie eine App Service-Umgebung eingerichtet haben, können Sie in dieser Umgebung Web-Apps, API-Apps und [mobile Apps](/previous-versions/azure/app-service-mobile/app-service-mobile-value-prop) erstellen, die alle hinter der WAF geschützt werden, die wir im nächsten Abschnitt konfigurieren.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfiguration des Barracuda WAF-Clouddiensts
Barracuda hat einen [ausführlichen Artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) zur Bereitstellung seiner WAF auf einem virtuellen Computer in Azure verfasst. Wir befolgen diese Anweisungen, wollen aber zugleich Redundanz und keine einzelne Fehlerquelle hinzufügen, weshalb wir mindestens zwei VMs mit WAF-Instanzen im selben Clouddienst bereitstellen.

### <a name="adding-endpoints-to-cloud-service"></a>Hinzufügen von Endpunkten zum Clouddienst
Sobald Ihr Clouddienst zwei oder mehr WAF VM-Instanzen aufweist, können Sie im [Azure-Portal](https://portal.azure.com/) HTTP- und HTTPS-Endpunkte hinzufügen, die von Ihrer Anwendung verwendet werden, wie in der folgenden Abbildung dargestellt:

![Endpunkt konfigurieren][ConfigureEndpoint]

Wenn Ihre Anwendungen andere Endpunkte verwenden, sollten Sie diese auch unbedingt dieser Liste hinzufügen. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurieren von Barracuda WAF über das zugehörige Verwaltungsportal
Barracuda WAF verwendet für die Konfiguration über sein Verwaltungsportal den TCP-Port 8000. Wenn Sie mehrere Instanzen von WAF-VMs verwenden, müssen Sie die Schritte hier für jede VM-Instanz wiederholen. 

> [!NOTE]
> Sobald Sie die WAF-Konfiguration abgeschlossen haben, entfernen Sie den TCP/8000-Endpunkt aus allen WAF-VMs, um Ihre WAF zu schützen.
> 
> 

Fügen Sie, wie in der folgenden Abbildung gezeigt, den Verwaltungsendpunkt hinzu, damit Sie Ihre Barracuda WAF konfigurieren können.

![Verwaltungsendpunkt hinzufügen][AddManagementEndpoint]

Navigieren Sie in einem Browser zum Verwaltungsendpunkt für Ihren Clouddienst. Wenn der Clouddienst „test.cloudapp.net“ heißt, greifen Sie auf diesen Endpunkt zu, indem Sie zu `http://test.cloudapp.net:8000` navigieren. Es sollte, wie in der folgenden Abbildung gezeigt, eine Anmeldeseite angezeigt werden, auf der Sie sich mit den Anmeldeinformationen anmelden können, die Sie beim Einrichten der WAF-VM angegeben haben.

![Anmeldeseite für die Verwaltung][ManagementLoginPage]

Nach der Anmeldung sollte ein Dashboard wie in der folgenden Abbildung angezeigt werden, das grundlegende Statistiken zum WAF-Schutz enthält.

![Dashboard für die Verwaltung][ManagementDashboard]

Auf der Registerkarte **Dienste** können Sie Ihre WAF für die Dienste konfigurieren, die von ihr geschützt werden sollen. Weitere Informationen zum Konfigurieren Ihrer Barracuda WAF finden Sie in [der dazugehörigen Dokumentation](https://campus.barracuda.com/product/webapplicationfirewall/doc/4259884/configure-the-barracuda-web-application-firewall-from-the-web-interface/). Im nachstehenden Beispiel wurde eine App Service-App zum Verarbeiten von Datenverkehr über HTTP und HTTPS konfiguriert.

![Verwaltungsdienste hinzufügen][ManagementAddServices]

> [!NOTE]
> Je nachdem, wie Ihre Anwendungen konfiguriert sind und welche Features in Ihrer App Service-Umgebung verwendet werden, müssen Sie Datenverkehr für alle TCP-Ports außer 80 und 443 weiterleiten, beispielsweise wenn Sie IP-TLS für eine App Service-App eingerichtet haben. Eine Liste der Netzwerkports, die in App Service-Umgebungen verwendet werden, finden Sie in der [Dokumentation zum Steuern des eingehenden Datenverkehrs](app-service-app-service-environment-control-inbound-traffic.md) im Abschnitt „Netzwerkports“.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurieren von Microsoft Azure Traffic Manager (optional)
Wenn Ihre Anwendung in mehreren Regionen verfügbar ist, sollten Sie mithilfe von [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md)für einen Lastenausgleich sorgen. Fügen Sie hierzu im [klassischen Azure-Portal](https://portal.azure.com) einen Endpunkt unter Angabe des Clouddienstnamens für Ihre WAF dem Traffic Manager-Profil hinzu, wie in der folgenden Abbildung dargestellt. 

![Traffic Manager-Endpunkt][TrafficManagerEndpoint]

Wenn Ihre Anwendung eine Authentifizierung erfordert, stellen Sie sicher, dass Sie über eine Ressource verfügen, die keine Authentifizierung bei Traffic Manager benötigt, um ein Pingsignal zum Prüfen der Verfügbarkeit der Anwendung senden zu können. Sie können die URL auf der Seite **Konfiguration** im [Azure-Portal](https://portal.azure.com) konfigurieren, wie in der folgenden Abbildung zu sehen:

![Traffic Manager konfigurieren][ConfigureTrafficManager]

Zum Weiterleiten der Traffic Manager-Pingsignale von Ihrer WAF zu Ihrer Anwendung müssen Sie für Ihre Barracuda WAF die Funktion "Websiteübersetzungen" so einrichten, dass Datenverkehr zu Ihrer Anwendung weitergeleitet wird, wie im folgenden Beispiel dargestellt:

![Websiteübersetzungen][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Schützen des Datenverkehrs zu einer App Service-Umgebung mithilfe von Netzwerksicherheitsgruppen (NSGs)
In der [Dokumentation zum Steuern des eingehenden Datenverkehrs](app-service-app-service-environment-control-inbound-traffic.md) finden Sie ausführliche Informationen zum Einschränken des Datenverkehrs zu Ihrer App Service-Umgebung von der WAF nur durch Verwenden der VIP-Adresse Ihres Clouddiensts. Hier ist ein PowerShell-Beispielbefehl zur Durchführung dieser Aufgabe für TCP-Port 80.

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
```

Ersetzen Sie "SourceAddressPrefix" durch die virtuelle IP-Adresse (VIP) des Clouddiensts Ihrer WAF.

> [!NOTE]
> Die VIP Ihres Clouddiensts ändert sich, wenn Sie diesen löschen und neu erstellen. Stellen Sie sicher, dass Sie nach diesem Schritt die IP-Adresse in der Netzwerkressourcengruppe ändern. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
