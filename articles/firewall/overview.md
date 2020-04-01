---
title: Was ist Azure Firewall?
description: Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 03/17/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: ed27097d29f3a10e708044ad7e2e30736e2c60e6
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "79471845"
---
# <a name="what-is-azure-firewall"></a>Was ist Azure Firewall?

![ICSA-Zertifizierung](media/overview/icsa-cert-firewall-small.png)

Azure Firewall ist ein verwalteter, cloudbasierter Netzwerksicherheitsdienst, der Ihre Azure Virtual Network-Ressourcen schützt. Es ist eine vollständig zustandsbehaftete Firewall-as-a-Service mit integrierter Hochverfügbarkeit und uneingeschränkter Cloudskalierbarkeit.

![Firewallübersicht](media/overview/firewall-threat.png)

Sie können Richtlinien zur Anwendungs- und Netzwerkkonnektivität übergreifend für Abonnements und virtuelle Netzwerke zentral erstellen, erzwingen und protokollieren. Azure Firewall verwendet eine statische öffentliche IP-Adresse für Ihre virtuellen Netzwerkressourcen, die es außenstehenden Firewalls ermöglicht, Datenverkehr aus Ihrem virtuellen Netzwerk zu identifizieren.  Der Dienst ist für Protokollierung und Analyse vollständig in Azure Monitor integriert.

Azure Firewall bietet die folgenden Features:

## <a name="built-in-high-availability"></a>Integrierte Hochverfügbarkeit

Hochverfügbarkeit ist integriert, sodass keine zusätzlichen Lastenausgleichsmodule erforderlich sind und Sie nichts konfigurieren müssen.

## <a name="availability-zones"></a>Verfügbarkeitszonen

Zur Erhöhung der Verfügbarkeit kann Azure Firewall während der Bereitstellung so konfiguriert werden, dass mehrere Verfügbarkeitszonen abgedeckt werden. Mit Verfügbarkeitszonen erhöht sich die Verfügbarkeit auf eine Betriebszeit von 99,99 %. Weitere Informationen finden Sie in der [Vereinbarung zum Servicelevel (SLA) für Azure Firewall](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). Die SLA für 99,99 % Betriebszeit wird angeboten, wenn zwei oder mehr Verfügbarkeitszonen ausgewählt werden.

Unter Verwendung der Standard-SLA des Diensts von 99,95 % können Sie Azure Firewall außerdem aus Gründen der Nähe einer bestimmten Zone zuordnen.

Für eine Firewall, die in einer Verfügbarkeitszone bereitgestellt wird, fallen keine zusätzlichen Kosten an. Es fallen jedoch zusätzliche Kosten an für eingehende und ausgehende Datenübertragungen, die mit Verfügbarkeitszonen verbunden sind. Weitere Informationen finden Sie unter [Preisübersicht Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/).

Verfügbarkeitszonen für Azure Firewall sind in Regionen verfügbar, die Verfügbarkeitszonen unterstützen. Weitere Informationen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](../availability-zones/az-overview.md#services-support-by-region)

> [!NOTE]
> Verfügbarkeitszonen können nur während der Bereitstellung konfiguriert werden. Für eine vorhandene Firewall können keine Verfügbarkeitszonen konfiguriert werden.

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](../availability-zones/az-overview.md).

## <a name="unrestricted-cloud-scalability"></a>Uneingeschränkte Cloudskalierbarkeit

Azure Firewall kann entsprechend Ihren Anforderungen hochskaliert werden, um einem sich ändernden Netzwerkdatenverkehr zu entsprechen, sodass Sie nicht für Ihre Spitzenlasten budgetieren müssen.

## <a name="application-fqdn-filtering-rules"></a>FQDN-Anwendungsfilterregeln

Sie können den ausgehenden HTTP/S-Datenverkehr oder Azure SQL-Datenverkehr (Vorschau) auf eine angegebene Liste vollqualifizierter Domänennamen (FQDNs) einschließlich Platzhalter beschränken. Dieses Feature erfordert keine SSL-Terminierung.

## <a name="network-traffic-filtering-rules"></a>Filterregeln für den Netzwerkdatenverkehr

Sie können Netzwerkfilterregeln zum *Zulassen* oder *Verweigern* nach Quell- und Ziel-IP-Adresse, Port und Protokoll zentral erstellen. Azure Firewall ist vollständig zustandsbehaftet, sodass zwischen legitimen Paketen für verschiedene Arten von Verbindungen unterschieden werden kann. Regeln werden übergreifend für mehrere Abonnements und virtuelle Netzwerke erzwungen und protokolliert.

## <a name="fqdn-tags"></a>FQDN-Tags

FQDN-Tags erleichtern es Ihnen, Netzwerkdatenverkehr bekannter Azure-Dienste durch die Firewall zuzulassen. Angenommen, Sie möchten Netzwerkdatenverkehr von Windows Update durch die Firewall zulassen. Sie erstellen eine entsprechende Anwendungsregel, und schließen das Windows Update-Tag ein. Jetzt kann der Netzwerkdatenverkehr von Windows Update durch Ihre Firewall fließen.

## <a name="service-tags"></a>Diensttags

Ein Diensttag steht für eine Gruppe von IP-Adressen und hat die Aufgabe, bei der Erstellung von Sicherheitsregeln die Komplexität zu verringern. Sie können weder ein eigenes Diensttag erstellen noch angeben, welche IP-Adressen in einem Tag enthalten sind. Microsoft verwaltet die Adresspräfixe, die mit dem Diensttag abgedeckt werden, und aktualisiert das Diensttag automatisch, wenn sich die Adressen ändern.

## <a name="threat-intelligence"></a>Bedrohungsanalyse

Das Filtern auf Basis von Threat Intelligence kann für Ihre Firewall aktiviert werden, damit diese Sie bei Datenverkehr von und zu bekannten schädlichen IP-Adressen oder Domänen warnt und diesen verweigert. Die IP-Adressen und Domänen stammen aus dem Microsoft Threat Intelligence-Feed.

## <a name="outbound-snat-support"></a>SNAT-Unterstützung für ausgehenden Datenverkehr

Alle IP-Adressen für ausgehenden Datenverkehr des virtuellen Netzwerks werden in die öffentliche IP-Adresse der Azure Firewall übersetzt (Source Network Address Translation). Sie können Datenverkehr aus Ihrem virtuellen Netzwerk an Remoteziele im Internet identifizieren und zulassen. Azure Firewall verfügt nicht über SNAT, wenn die Ziel-IP ein privater IP-Bereich gemäß [IANA RFC 1918](https://tools.ietf.org/html/rfc1918) ist. 

Wenn Ihre Organisation einen öffentlichen IP-Adressbereich für private Netzwerke verwendet, leitet Azure Firewall den Datenverkehr per SNAT an eine der privaten IP-Adressen der Firewall in AzureFirewallSubnet weiter. Sie können Azure Firewall so konfigurieren, dass Ihr öffentlicher IP-Adressbereich **nicht** per SNAT weitergeleitet wird. Weitere Informationen finden Sie unter [Azure Firewall SNAT – private Adressbereiche](snat-private-range.md).

## <a name="inbound-dnat-support"></a>DNAT-Unterstützung für eingehenden Datenverkehr

Der eingehende Internet-Netzwerkdatenverkehr zur öffentlichen IP-Adresse Ihrer Firewall wird in die privaten IP-Adressen in Ihren virtuellen Netzwerken übersetzt (Ziel-Netzwerkadressübersetzung, DNAT) und gefiltert.

## <a name="multiple-public-ip-addresses"></a>Mehrere öffentliche IP-Adressen

Sie können der Firewall mehrere öffentliche IP-Adressen zuordnen (bis zu 100).

Dies ermöglicht die folgenden Szenarien:

- **DNAT:** Sie können mehrere Standardportinstanzen auf Ihre Back-End-Server übersetzen. Wenn Sie beispielsweise über zwei öffentliche IP-Adressen verfügen, können Sie den TCP-Port 3389 (RDP) für beide IP-Adressen übersetzen.
- **SNAT:** Für ausgehende SNAT-Verbindungen stehen zusätzliche Ports zur Verfügung, was die Gefahr einer Überlastung des SNAT-Ports verringert. Aktuell wählt Azure Firewall die öffentliche IP-Quelladresse für eine Verbindung nach dem Zufallsprinzip aus. Wenn Sie in Ihrem Netzwerk über eine nachgeschaltete Filterung verfügen, müssen Sie alle öffentlichen IP-Adressen zulassen, die mit Ihrer Firewall verbunden sind.

## <a name="azure-monitor-logging"></a>Azure Monitor-Protokollierung

Alle Ereignisse sind in Azure Monitor integriert, sodass Sie Protokolle in einem Speicherkonto archivieren sowie Ereignisse an Ihren Event Hub streamen oder an Azure Monitor-Protokolle senden können.

## <a name="certifications"></a>Zertifizierungen

Azure Firewall ist mit PCI (Payment Card Industry), SOC (Service Organization Controls), ISO (International Organization for Standardization) und ICSA Labs konform. Weitere Informationen finden Sie unter [Azure Firewall-Konformitätszertifizierungen](compliance-certifications.md).


## <a name="known-issues"></a>Bekannte Probleme

Azure Firewall weist die folgenden bekannten Probleme auf:

|Problem  |BESCHREIBUNG  |Minderung  |
|---------|---------|---------|
Netzwerkfilterregeln für andere Protokolle als TCP/UDP (z.B. ICMP) funktionieren nicht für den Internetdatenverkehr|Netzwerkfilterregeln für andere Protokolle als TCP/UDP funktionieren nicht mit SNAT für Ihre öffentliche IP-Adresse. Nicht-TCP/UDP-Protokolle werden zwischen Spoke-Subnetzen und VNets unterstützt.|Azure Firewall verwendet Standard Load Balancer, [das SNAT für IP-Protokolle derzeit nicht unterstützt](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Wir prüfen Möglichkeiten, um dieses Szenario in einer zukünftigen Version zu unterstützen.|
|Fehlende PowerShell- und CLI-Unterstützung für ICMP|Azure PowerShell und CLI weisen keine Unterstützung von ICMP als gültiges Protokoll in Netzwerkregeln auf.|Es ist weiterhin möglich, ICMP über das Portal und die REST-API als Protokoll zu verwenden. Wir arbeiten daran, ICMP in Kürze in PowerShell und CLI hinzuzufügen.|
|Für FQDN-Tags muss ein Protokoll/Port festgelegt werden|Für Anwendungsregeln mit FQDN-Tags ist eine „Port: Protokoll“-Definition erforderlich.|Sie können **https** als port:-Protokollwert verwenden. Wir arbeiten daran, dieses Feld optional zu machen, wenn FQDN-Tags verwendet werden.|
|Das Verlagern einer Firewall in eine andere Ressourcengruppe oder ein anderes Abonnement wird nicht unterstützt|Das Verlagern einer Firewall in eine andere Ressourcengruppe oder ein anderes Abonnement wird nicht unterstützt.|Die Unterstützung dieser Funktionalität ist Teil unserer Roadmap. Um eine Firewall in eine andere Ressourcengruppe oder ein anderes Abonnement zu verschieben, müssen Sie die aktuelle Instanz löschen und in der neuen Ressourcengruppe bzw. im Abonnement neu erstellen.|
|Threat Intelligence-Warnungen sind möglicherweise maskiert.|Netzwerkregeln mit Ziel 80/443 für ausgehende Filterung maskieren Threat Intelligence-Warnungen, wenn diese für den Modus „Alert only“ (Nur Warnen) konfiguriert sind.|Erstellen Sie die ausgehende Filterung für 80/443 mithilfe von Anwendungsregeln. Oder ändern Sie den Threat Intelligence-Modus zu **Alert and Deny** (Warnen und Verweigern).|
|Azure Firewall verwendet Azure DNS nur für die Namensauflösung.|Azure Firewall löst FQDNs nur mit Azure DNS auf. Ein benutzerdefinierter DNS-Server wird nicht unterstützt. Dies hat keine Auswirkungen auf die DNS-Auflösung in anderen Subnetzen.|Wir arbeiten daran, diese Beschränkung zu lockern.|
|Azure Firewall-SNAT/DNAT funktioniert für private IP-Ziele nicht|Die Azure Firewall-SNAT/DNAT-Unterstützung ist auf eingehenden/ausgehenden Internetdatenverkehr beschränkt. SNAT/DNAT funktioniert derzeit für private IP-Ziele nicht. Beispiel: Spoke zu Spoke.|Dies ist eine aktuelle Beschränkung.|
|Erste Konfiguration der öffentlichen IP-Adresse kann nicht entfernt werden|Jede öffentliche Azure Firewall-IP-Adresse ist einer *IP-Konfiguration* zugewiesen.  Die erste IP-Konfiguration wird während der Bereitstellung der Firewall zugewiesen und enthält in der Regel auch einen Verweis auf das Firewallsubnetz (sofern dies nicht über eine Vorlagenbereitstellung explizit anders konfiguriert wurde). Sie können diese IP-Konfiguration nicht löschen, weil damit die Zuordnung der Firewall aufgehoben würde. Sie können die öffentliche IP-Adresse, die dieser IP-Konfiguration zugeordnet ist, weiterhin ändern oder entfernen, wenn der Firewall mindestens eine andere öffentliche IP-Adresse zur Verwendung zur Verfügung steht.|Dies ist beabsichtigt.|
|Verfügbarkeitszonen können nur während der Bereitstellung konfiguriert werden.|Verfügbarkeitszonen können nur während der Bereitstellung konfiguriert werden. Sie können keine Verfügbarkeitszonen konfigurieren, nachdem eine Firewall bereitgestellt wurde.|Dies ist beabsichtigt.|
|SNAT für eingehende Verbindungen|Zusätzlich zu DNAT werden Verbindungen über die öffentliche IP-Adresse der Firewall (eingehend) per SNAT in eine der privaten öffentlichen IP-Adressen übersetzt. Diese Anforderung gilt derzeit (auch für Aktiv/Aktiv-NVAs), um symmetrisches Routing sicherzustellen.|Um die ursprüngliche Quelle für HTTP/S beizubehalten, können Sie [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For)-Header verwenden. Verwenden Sie beispielsweise einen Dienst wie [Azure Front Door](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) oder [Azure Application Gateway](../application-gateway/rewrite-http-headers.md) vor der Firewall. Sie können auch WAF als Teil von Azure Front Door verwenden und mit der Firewall verketten.
|SQL-FQDN-Filterung wird nur im Proxymodus unterstützt (Port 1433)|Für Azure SQL-Datenbank, Azure SQL Data Warehouse und verwaltete SQL-Datenbank-Instanzen gilt:<br><br>Während der Vorschau wird die SQL-FQDN-Filterung nur im Proxymodus unterstützt (Port 1433).<br><br>Für Azure SQL-IaaS gilt:<br><br>Wenn Sie keine Standardports verwenden, können Sie diese Ports in den Anwendungsregeln angeben.|Für SQL im Umleitungsmodus – die Standardeinstellung beim Herstellen von Verbindungen aus Azure heraus – können Sie den Zugriff stattdessen mit dem SQL-Diensttag in den Azure Firewall-Netzwerkregeln filtern.
|Ausgehender Datenverkehr an TCP-Port 25 ist nicht zulässig.| Ausgehende SMTP-Verbindungen, die den TCP-Port 25 verwenden, werden blockiert. Der Port 25 wird in erster Linie für die E-Mail-Zustellung ohne Authentifizierung verwendet. Dies ist das standardmäßige Plattformverhalten für virtuelle Computer. Weitere Informationen finden Sie unter [Behandeln von Problemen mit ausgehenden SMTP-Verbindungen in Azure](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Anders als bei virtuellen Computern kann diese Funktion für Azure Firewall derzeit nicht aktiviert werden.|Verwenden Sie die empfohlene Methode für den E-Mail-Versand, die im Artikel zur SMTP-Problembehandlung dokumentiert ist. Sie können auch den virtuellen Computer, der ausgehenden SMTP-Zugriff benötigt, von Ihrer Standardroute zur Firewall ausschließen und stattdessen direkten ausgehenden Internetzugriff konfigurieren.
|Aktives FTP wird nicht unterstützt.|Zum Schutz vor FTP-Bounce-Angriffen ist aktives FTP in Azure Firewall deaktiviert.|Sie können stattdessen passives FTP verwenden. Die TCP-Ports 20 und 21 müssen weiterhin explizit in der Firewall geöffnet werden.
|Metrik für SNAT-Portnutzung zeigt 0 % an|Für die Metrik zur SNAT-Portnutzung der Azure Firewall wird ggf. auch dann 0 % angezeigt, wenn SNAT-Ports verwendet werden. In diesem Fall führt die Verwendung der Metrik als Teil der Metriken für die Firewallintegrität zu einem falschen Ergebnis.|Dieses Problem wurde behoben, und der Rollout für die Produktion ist für Mai 2020 geplant. In einigen Fällen wird das Problem durch die erneute Bereitstellung der Firewall behoben, aber dies ist nicht garantiert. Verwenden Sie den Integritätszustand der Firewall als zwischenzeitliche Problemumgehung nur, um nach *status=degraded* zu suchen (nicht nach *status=unhealthy*). Die Portauslastung wird als *degraded* (heruntergestuft) angezeigt. Der Status *not healthy* (fehlerhaft) ist für zukünftige Zwecke reserviert, wenn mehr Metriken zur Firewallintegrität verfügbar sind. 

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal](tutorial-firewall-deploy-portal.md)
- [Bereitstellen von Azure Firewall mit einer Vorlage](deploy-template.md)
- [Erstellen einer Azure Firewall-Testumgebung](scripts/sample-create-firewall-test.md)
