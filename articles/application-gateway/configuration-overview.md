---
title: Azure Application Gateway – Konfigurationsübersicht
description: In diesem Artikel wird das Konfigurieren der Komponenten von Azure Application Gateway beschrieben.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: absha
ms.openlocfilehash: bd6f04ca7e24e380ad657f967284704ad613375a
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2020
ms.locfileid: "83996396"
---
# <a name="application-gateway-configuration-overview"></a>Application Gateway – Konfigurationsübersicht

Azure Application Gateway besteht aus mehreren Komponenten, die Sie auf verschiedene Weise für verschiedene Szenarien konfigurieren können. In diesem Artikel wird das Konfigurieren der einzelnen Komponenten veranschaulicht.

![Flussdiagramm der Application Gateway-Komponenten](./media/configuration-overview/configuration-overview1.png)

Dieses Bild zeigt eine Anwendung mit drei Listenern. Die ersten zwei sind Listener für mehrere Standorte für `http://acme.com/*` bzw. `http://fabrikam.com/*`. Beide lauschen auf Port 80. Bei dem dritten handelt es sich um einen Basislistener, der über End-to-End-TLS-Beendigung (Transport Layer Security) verfügt, die zuvor als SSL-Beendigung (Secure Sockets Layer) bekannt war.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Virtuelles Azure-Netzwerk und dediziertes Subnetz

Ein Application Gateway ist eine dedizierte Bereitstellung in Ihrem virtuellen Netzwerk. Innerhalb Ihres virtuellen Netzwerks ist ein dediziertes Subnetz für das Application Gateway erforderlich. Sie können in einem Subnetz mehrere Instanzen einer bestimmten Application Gateway-Bereitstellung betreiben. Außerdem können Sie weitere Application Gateway-Instanzen im Subnetz bereitstellen. Aber Sie können keine andere Ressource im Application Gateway-Subnetz bereitstellen.

> [!NOTE]
> Sie können ein Standard_v2- und ein standardmäßiges Azure Application Gateway nicht parallel in demselben Subnetz betreiben.

#### <a name="size-of-the-subnet"></a>Subnetzgröße

Application Gateway nutzt eine private IP-Adresse pro Instanz sowie eine weitere private IP-Adresse, wenn eine private Front-End-IP konfiguriert ist.

Azure reserviert außerdem fünf IP-Adressen in jedem Subnetz für die interne Verwendung: die ersten vier und die letzte IP-Adresse. Stellen Sie sich beispielsweise 15 Application Gateway-Instanzen ohne private Front-End-IP vor. Sie benötigen mindestens 20 IP-Adressen für dieses Subnetz: fünf zur internen Verwendung und 15 für die Application Gateway-Instanzen. Sie benötigen also mindestens ein /27-Subnetz.

Stellen Sie sich ein Subnetz mit 27 Application Gateway-Instanzen und einer privaten Front-End-IP vor. In diesem Fall benötigen Sie 33 IP-Adressen: 27 für die Application Gateway-Instanzen, eine für das private Front-End und fünf für die interne Verwendung. Sie benötigen also mindestens ein /26-Subnetz.

Sie sollten mindestens ein /28-Subnetz verwenden. Diese Größe bietet Ihnen 11 nutzbare IP-Adressen. Wenn für Ihre Anwendungslast mehr als zehn Application Gateway-Instanzen erforderlich sind, sollten Sie eine Subnetzgröße von /27 oder /26 in Betracht ziehen.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Netzwerksicherheitsgruppen im Application Gateway-Subnetz

Netzwerksicherheitsgruppen (NSG) werden im Application Gateway-Subnetz unterstützt. Es gelten jedoch einige Einschränkungen:

- Sie müssen eingehenden Internetdatenverkehr über die TCP-Ports 65503–65534 (für die Application Gateway v1-SKU) und über die TCP-Ports 65200–65535 (für die v2-SKU) mit dem Zielsubnetz **Beliebig** und der Quelle **GatewayManager**-Diensttag zulassen. Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Diese Ports werden von Azure-Zertifikaten geschützt (gesperrt). Externe Entitäten, einschließlich der Kunden dieses Gateways, können nicht auf diesen Endpunkten kommunizieren.

- Die ausgehende Internetverbindung kann nicht blockiert sein. Standardausgangsregeln in der NSG ermöglichen Internetkonnektivität. Wir empfehlen Folgendes:

  - Entfernen Sie die Standardausgangsregeln nicht.
  - Erstellen Sie keine anderen Ausgangsregeln, die ausgehende Konnektivität verweigern.

- Datenverkehr vom **AzureLoadBalancer**-Tag muss zulässig sein.

#### <a name="allow-application-gateway-access-to-a-few-source-ips"></a>Zulassen des Application Gateway-Zugriffs auf einige wenige Quell-IPs

Verwenden Sie für dieses Szenario Netzwerksicherheitsgruppen im Application Gateway-Subnetz. Legen Sie die folgenden Einschränkungen für das Subnetz in dieser Priorität fest:

1. Erlauben Sie eingehenden Datenverkehr aus einer Quell-IP oder einem IP-Adressbereich mit dem Ziel des gesamten Application Gateway-Subnetzadressbereichs und dem Zielport als eingehenden Zugriffsport, z. B. Port 80 für den HTTP-Zugriff.
2. Erlauben Sie eingehende Anforderungen von der Quelle **GatewayManager**-Diensttag und dem Ziel **Beliebig** sowie den Zielports 65503-65534 für die Application Gateway v1-SKU und den Ports 65200-65535 für v2-SKU für die [Kommunikation des Back-End-Integritätsstatus](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Dieser Portbereich ist für die Kommunikation mit der Azure-Infrastruktur erforderlich. Diese Ports werden von Azure-Zertifikaten geschützt (gesperrt). Ohne entsprechende Zertifikate können externe Entitäten keine Änderungen an diesen Endpunkten vornehmen.
3. Lassen Sie eingehende Azure Load Balancer-Tests (*AzureLoadBalancer-Tag*) und eingehenden virtuellen Netzwerkdatenverkehr (*VirtualNetwork*-Tag) für die [Netzwerksicherheitsgruppe](https://docs.microsoft.com/azure/virtual-network/security-overview) zu.
4. Blockieren Sie den gesamten übrigen eingehenden Datenverkehr mit einer Alle-verweigern-Regel.
5. Zulassen von ausgehendem Datenverkehr an das Internet für alle Ziele.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Im Application Gateway-Subnetz unterstützte benutzerdefinierte Routen

> [!IMPORTANT]
> Das Verwenden benutzerdefinierter Routen im Application Gateway-Subnetz kann dazu führen, dass der Integritätsstatus in der [Ansicht der Back-End-Integrität](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) als **Unbekannt** angezeigt wird. Außerdem können bei der Generierung von Application Gateway-Protokollen und -Metriken Fehler auftreten. Sie sollten keine benutzerdefinierten Routen im Application Gateway-Subnetz verwenden, damit Sie die Back-End-Integrität, Protokolle und Metriken anzeigen können.

- **v1**

   Für die v1 SKU werden benutzerdefinierte Routen (User-Defined Routes, UDRs) im Application Gateway-Subnetz unterstützt, solange sie die End-to-End-Anforderung/Antwort-Kommunikation nicht ändern. Beispielsweise können Sie eine benutzerdefinierte Route im Application Gateway-Subnetz einrichten, um auf eine Firewallappliance für die Paketüberprüfung zu verweisen. Sie müssen jedoch sicherstellen, dass das Paket nach der Überprüfung das vorgesehene Ziel erreichen kann. Ein Unterlassen kann zu einem falschen Integritätstest oder Datenverkehrsrouting-Verhalten führen. Dies schließt gelernte Routen oder standardmäßige 0.0.0.0/0-Routen ein, die durch Azure ExpressRoute oder VPN-Gateways im virtuellen Netzwerk verteilt werden.

- **v2**

   Für die v2-SKU gibt es unterstützte und nicht unterstützte Szenarios:

   **Unterstützte v2-Szenarios**
   > [!WARNING]
   > Eine fehlerhafte Konfiguration der Routingtabelle kann zu asymmetrischer Routenplanung in Application Gateway v2 führen. Stellen Sie sicher, dass der gesamte Datenverkehr der Verwaltungs-/Steuerungsebene direkt an das Internet und nicht über ein virtuelles Gerät gesendet wird. Dies kann sich auch auf die Protokollierung und Metriken auswirken.


  **Szenario 1**: Benutzerdefinierte Routen zum Deaktivieren der BGP-Routenverteilung (Border Gateway Protocol) an das Application Gateway-Subnetz

   Manchmal wird die Standardgatewayroute (0.0.0.0/0) per ExpressRoute oder VPN-Gateways angekündigt, die dem virtuellen Application Gateway-Netzwerk zugeordnet sind. Dies unterbricht den Datenverkehr der Verwaltungsebene, da er einen direkten Pfad zum Internet erfordert. In solchen Szenarios kann eine benutzerdefinierte Route zum Deaktivieren der BGP-Routenverteilung verwendet werden. 

   Führen Sie die folgenden Schritte aus, um die BGP-Routenverteilung zu deaktivieren:

   1. Erstellen Sie eine Routingtabelleressource in Azure.
   2. Deaktivieren Sie den Parameter **Routenverteilung des Gateways für virtuelle Netzwerke**. 
   3. Ordnen Sie die Routingtabelle dem entsprechenden Subnetz zu. 

   Das Aktivieren der benutzerdefinierten Route für dieses Szenario sollte keine vorhandenen Setups unterbrechen.

  **Szenario 2**: Benutzerdefinierte Route zum Weiterleiten von Datenverkehr an 0.0.0.0/0 an das Internet

   Sie können eine benutzerdefinierte Route erstellen, um Datenverkehr an 0.0.0.0/0 direkt an das Internet zu übermitteln. 

  **Szenario 3:** Benutzerdefinierte Route für Azure Kubernetes Service mit kubenet

  Wenn Sie kubenet mit Azure Kubernetes Service (AKS) und dem Application Gateway-Eingangscontroller (AGIC, Application Gateway Ingress Controller) verwenden, benötigen Sie eine Routingtabelle, die die Weiterleitung des von Application Gateway an die Pods gesendeten Datenverkehrs an den richtigen Knoten erlaubt. Dies ist nicht erforderlich, wenn Sie Azure CNI verwenden. 

  Führen Sie die folgenden Schritte aus, um die Routingtabelle einzurichten, damit kubenet funktioniert:

  1. Wechseln Sie zu der Ressourcengruppe, die von AKS erstellt wurde (der Name der Ressourcengruppe sollte mit „MC_“ beginnen).
  2. Suchen Sie von AKS in dieser Ressourcengruppe erstellte Routentabelle. Die Routingtabelle sollte mit den folgenden Informationen aufgefüllt sein:
     - Das Adresspräfix sollte dem IP-Adressbereich der Pods entsprechen, die Sie in AKS erreichen möchten. 
     - Der Typ des nächsten Hops sollte „Virtuelles Gerät“ sein. 
     - Die Adresse des nächsten Hops sollte der IP-Adresse des Knotens entsprechen, der die Pods hostet.
  3. Ordnen Sie diese Routingtabelle dem Subnetz des Application Gateways zu. 
    
  **Nicht unterstützte v2-Szenarios**

  **Szenario 1**: Benutzerdefinierte Route für virtuelle Geräte

  Szenarios, in denen Datenverkehr an 0.0.0.0/0 über ein virtuelles Gerät, ein virtuelles Hub-/Spoke-Netzwerk oder lokal (Tunnelerzwingung) umgeleitet werden muss, werden in V2 nicht unterstützt.

## <a name="front-end-ip"></a>Front-End-IP

Sie können das Application Gateway mit einer öffentlichen IP-Adresse, mit einer privaten IP-Adresse oder mit beidem konfigurieren. Eine öffentliche IP-Adresse ist erforderlich, wenn Sie ein Back-End hosten, auf das Clients über eine für den Internetzugriff verfügbare virtuelle IP (VIP) über das Internet zugreifen müssen. 

Eine öffentliche IP-Adresse ist nicht für einen internen Endpunkt erforderlich, der nicht für den Internetzugriff verfügbar ist. Dieser wird als *Interner Lastenausgleich*-Endpunkt (Internal Load-Balancer, ILB) oder private Front-End-IP bezeichnet. Ein ILB-Application Gateway ist für interne Branchenanwendungen nützlich, die nicht für das Internet verfügbar gemacht werden. Es ist auch hilfreich für Dienste und Ebenen in einer Anwendung mit mehreren Ebenen innerhalb einer Sicherheitsgrenze, die nicht für das Internet verfügbar gemacht werden, aber eine Roundrobin-Lastverteilung, Sitzungspersistenz oder TLS-Beendigung erfordern.

Es werden nur jeweils eine öffentliche und private IP-Adresse unterstützt. Sie wählen die Front-End-IP beim Erstellen des Application Gateways aus.

- Für eine öffentliche IP-Adresse können Sie eine neue öffentliche IP-Adresse erstellen oder eine vorhandene öffentliche IP-Adresse am gleichen Speicherort wie das Application Gateway verwenden. Weitere Informationen finden Sie unter [Statische und dynamische öffentliche IP-Adresse im Vergleich](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address).

- Für eine private IP-Adresse können Sie eine private IP-Adresse aus dem Subnetz angeben, in dem das Application Gateway erstellt wird. Wenn Sie keine angeben, wird automatisch eine beliebige IP-Adresse aus dem Subnetz ausgewählt. Der von Ihnen ausgewählte Typ der IP-Adresse (statisch oder dynamisch) kann später nicht mehr geändert werden. Weitere Informationen finden Sie unter [Erstellen eines Application Gateways mit einem internen Lastenausgleich (ILB)](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Eine Front-End-IP-Adresse ist einem *Listener* zugeordnet, der auf über die Front-End-IP eingehende Anforderungen prüft.

## <a name="listeners"></a>Listener

Ein Listener ist eine logische Entität, die mithilfe von Port, Protokoll, Host und IP-Adresse auf eingehende Verbindungsanforderungen prüft. Wenn Sie den Listener konfigurieren, müssen Sie Werte für diese eingeben, die den entsprechenden Werten in der eingehenden Anforderung auf dem Gateway entsprechen.

Wenn Sie ein Application Gateway mithilfe des Azure-Portals erstellen, erstellen Sie außerdem einen Standardlistener durch Auswählen von Protokoll und Port für den Listener. Sie können wahlweise HTTP2-Unterstützung auf dem Listener aktivieren. Nach der Erstellung des Application Gateways können Sie die Einstellungen dieses Standardlisteners (*appGatewayHttpListener*) bearbeiten oder neue Listener erstellen.

### <a name="listener-type"></a>Listenertyp

Wenn Sie einen neuen Listener erstellen, wählen Sie zwischen [*grundlegend* und *mehreren Standorten*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners) aus.

- Wenn alle Ihre Anforderungen (für eine beliebige Domäne) akzeptiert und an Back-End-Pools weitergeleitet werden sollen, wählen Sie „Basic“ aus. Erfahren Sie, [wie Sie ein Application Gateway mit einem grundlegenden Listener erstellen](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Wenn Sie Anforderungen an verschiedene Back-End-Pools weiterleiten möchten, abhängig vom *Host*header oder Hostnamen, wählen Sie Listener für mehrere Standorte aus, bei dem Sie auch einen Hostnamen angeben müssen, der mit der eingehenden Anforderung übereinstimmt. Das hat den Grund, dass Application Gateway HTTP 1.1-Hostheader verwendet, um mehrere Websites an der gleichen öffentlichen IP-Adresse und dem gleichen Port zu hosten.

#### <a name="order-of-processing-listeners"></a>Verarbeitungsreihenfolge von Listenern

Bei der v1-SKU werden Anforderungen entsprechend der Reihenfolge der Regeln und des Listenertyps abgeglichen. Wenn eine Regel mit dem Basislistener in der Reihenfolge an erster Stelle steht, wird sie zuerst verarbeitet und akzeptiert alle Anforderungen für diese Port- und IP-Kombination. Um dies zu vermeiden, konfigurieren Sie die Regeln zuerst mit Listenern für mehrere Standorte, und verschieben Sie die Regel mit dem Basislistener an die letzte Stelle in der Liste.

Für v2 SKUs werden Listener für mehrere Standorte vor grundlegenden Listenern verarbeitet.

### <a name="front-end-ip"></a>Front-End-IP

Wählen Sie die Front-End-IP-Adresse aus, die Sie diesem Listener zuordnen möchten. Der Listener lauscht auf eingehende Anforderungen für diese IP-Adresse.

### <a name="front-end-port"></a>Front-End-Port

Wählen Sie den Front-End-Port aus. Wählen Sie einen vorhandenen Port aus, oder erstellen Sie einen neuen. Wählen Sie einen beliebigen Wert aus dem [zulässigen Portbereich](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports) aus. Sie können nicht nur die bekannten Ports wie 80 und 443 verwenden, sondern jeden geeigneten zulässigen benutzerdefinierten Port. Ein Port kann für öffentlich zugängliche Listener oder für nur privat zugängliche Listener verwendet werden.

### <a name="protocol"></a>Protocol

Wählen Sie HTTP oder HTTPS aus:

- Wenn Sie sich für HTTP entscheiden, ist der Datenverkehr zwischen dem Client und dem Application Gateway unverschlüsselt.

- Wählen Sie HTTPS aus, wenn Sie die [TLS-Beendigung](features.md#secure-sockets-layer-ssltls-termination) oder [End-to-End-TLS-Verschlüsselung](https://docs.microsoft.com/azure/application-gateway/ssl-overview) verwenden möchten. Der Datenverkehr zwischen dem Client und dem Application Gateway ist verschlüsselt. Außerdem wird die TLS-Verbindung am Application Gateway getrennt. Wenn Sie die End-to-End-TLS-Verschlüsselung wünschen, müssen Sie HTTPS auswählen und die Einstellung des **Back-End-HTTP** konfigurieren. Dadurch wird sichergestellt, dass der Datenverkehr auf dem Weg vom Application Gateway zum Back-End erneut verschlüsselt wird.


Zum Konfigurieren der TLS-Beendigung und End-to-End-TLS-Verschlüsselung müssen Sie ein Zertifikat zum Listener hinzufügen, um Application Gateway das Ableiten eines symmetrischen Schlüssels zu ermöglichen. Dies wird durch die TLS-Protokollspezifikation vorgegeben. Der symmetrische Schlüssel wird zum Verschlüsseln und Entschlüsseln des an das Gateway gesendeten Datenverkehrs verwendet. Das Gatewayzertifikat muss im PFX-Format (Personal Information Exchange, privater Informationsaustausch) vorliegen. Mit diesem Format können Sie den privaten Schlüssel exportieren, den das Gateway zum Verschlüsseln und Entschlüsseln von Datenverkehr verwendet.

#### <a name="supported-certificates"></a>Unterstützte Zertifikate

Weitere Informationen finden Sie unter [Unterstützte Zertifikate für die TLS-Beendigung](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Unterstützung zusätzlicher Protokolle

#### <a name="http2-support"></a>HTTP2-Unterstützung

Die Unterstützung des HTTP/2-Protokolls ist nur für Clients verfügbar, die mit Application Gateway-Listenern verbunden sind. Die Kommunikation mit Back-End-Serverpools erfolgt über HTTP/1.1. Die HTTP/2-Unterstützung ist standardmäßig deaktiviert. Der folgende Azure PowerShell-Codeausschnitt zeigt, wie Sie sie aktivieren:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket-Unterstützung

Die WebSocket-Unterstützung ist standardmäßig aktiviert. Sie kann von Benutzern nicht aktiviert oder deaktiviert werden. Sie können das WebSocket-Protokoll sowohl mit HTTP- als auch mit HTTPS-Listenern verwenden.

### <a name="custom-error-pages"></a>Benutzerdefinierte Fehlerseiten

Sie können benutzerdefinierte Fehler auf globaler Ebene oder Listenerebene definieren. Doch das Erstellen von benutzerdefinierten Fehlerseiten auf globaler Ebene über das Azure-Portal wird derzeit nicht unterstützt. Sie können auf Listenerebene eine benutzerdefinierte Fehlerseite für einen 403-WAF-Fehler oder eine 502-Wartungsseite konfigurieren. Außerdem müssen Sie eine öffentlich zugängliche Blob-URL für den bestimmten Fehlerstatuscode angeben. Weitere Informationen finden Sie unter [Create Application Gateway custom error pages](https://docs.microsoft.com/azure/application-gateway/custom-error) (Erstellen von benutzerdefinierten Application Gateway-Fehlerseiten).

![Application Gateway-Fehlercodes](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Wie Sie eine globale benutzerdefinierte Fehlerseite konfigurieren, erfahren Sie unter [Azure PowerShell-Konfiguration](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="tls-policy"></a>TLS-Richtlinie

Sie können die TLS/SSL-Zertifikatverwaltung zentralisieren sowie den Ver- und Entschlüsselungsaufwand für eine Back-End-Serverfarm verringern. Diese zentralisierte TLS-Behandlung ermöglicht auch die Angabe einer zentralen TLS-Richtlinie, die auf Ihre Sicherheitsanforderungen abgestimmt ist. Sie können zwischen einer *standardmäßig vorhandenen*, *vordefinierten* oder *benutzerdefinierten* TLS-Richtlinie auswählen.

Sie konfigurieren die TLS-Richtlinie, um die TLS-Protokollversionen zu steuern. Sie können eine Application Gateway-Instanz so konfigurieren, dass es eine Mindestprotokollversion für TLS-Handshakes von TLS 1.0, TLS 1.1 und TLS 1.2 verwendet. Standardmäßig sind SSL 2.0 und 3.0 deaktiviert und nicht konfigurierbar. Weitere Informationen finden Sie unter [TLS-Richtlinienübersicht für Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Nachdem Sie einen Listener erstellt haben, ordnen Sie ihm eine Anforderungsroutingregel zu. Diese Regel bestimmt, wie vom Listener empfangene Anforderungen an das Back-End weitergeleitet werden.

## <a name="request-routing-rules"></a>Anforderungsroutingregeln

Wenn Sie ein Application Gateway mithilfe des Azure-Portals erstellen, erstellen Sie eine Standardregel (*rule1*). Mit dieser Regel wird der Standardlistener (*appGatewayHttpListener*) an den Standard-Back-End-Pool (*appGatewayBackendPool*) und die Back-End-HTTP-Standardeinstellungen ( *appGatewayBackendHttpSettings*) gebunden. Nachdem Sie das Gateway erstellt haben, können Sie die Einstellungen der Standardregel bearbeiten oder neue Regeln erstellen.

### <a name="rule-type"></a>Regeltyp

Wenn Sie eine Regel erstellen, wählen Sie zwischen [*grundlegend* und *pfadbasiert*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules) aus.

- Wählen Sie „grundlegend“ aus, wenn Sie alle an den zugeordneten Listener gerichteten Anforderungen (z.B. *blog<i></i>.contoso.com/\*)* an einen einzelnen Back-End-Pool weiterleiten möchten.
- Wählen Sie „pfadbasiert“ aus, wenn Sie Anforderungen von bestimmten URL-Pfaden an bestimmte Back-End-Pools weiterleiten möchten. Das Pfadmuster wird nur auf den Pfad der URL angewendet, nicht auf ihre Abfrageparameter.

#### <a name="order-of-processing-rules"></a>Verarbeitungsreihenfolge von Regeln

Für die v1- und v2-SKU wird die Musterzuordnung eingehender Anforderungen in der Reihenfolge verarbeitet, in der die Pfade in der URL-Pfadzuordnung der pfadbasierten Regel aufgelistet sind. Wenn eine Anforderung mit dem Muster in mindestens zwei Pfaden in der Pfadzuordnung übereinstimmt, wird der zuerst aufgelistete Pfad abgeglichen. Die Anforderung wird an das Back-End weitergeleitet, das diesem Pfad zugeordnet ist.

### <a name="associated-listener"></a>Zugeordneter Listener

Ordnen Sie der Regel einen Listener zu, damit die *Anforderungsroutingregel*, die dem Listener zugeordnet ist, ausgewertet wird, um den Back-End-Pool zu bestimmen, an den die Anforderung weitergeleitet werden soll.

### <a name="associated-back-end-pool"></a>Zugeordneter Back-End-Pool

Ordnen Sie der Regel den Back-End-Pool zu, der die Back-End-Ziele enthält, die Anforderungen verarbeiten, die der Listener empfängt.

 - Für eine grundlegende Regel ist nur ein einziger Back-End-Pool zulässig. Alle Anforderungen auf dem zugeordneten Listener werden an diesen Back-End-Pool weitergeleitet.

 - Fügen Sie für eine pfadbasierte Regel mehrere Back-End-Pools hinzu, die den einzelnen URL-Pfaden entsprechen. Mit dem eingegebenen URL-Pfad übereinstimmende Anforderungen werden an den entsprechenden Back-End-Pool weitergeleitet. Fügen Sie darüber hinaus einen standardmäßigen Back-End-Pool hinzu. Anforderungen, die keinem URL-Pfad in der Regel entsprechen, werden an diesen Pool weitergeleitet.

### <a name="associated-back-end-http-setting"></a>Zugeordnete Back-End-HTTP-Einstellung

Fügen Sie für jede Regel eine Back-End-HTTP-Einstellung hinzu. Anforderungen werden vom Application Gateway mithilfe der Portnummer, des Protokolls und anderer in dieser Einstellung festgelegten Informationen an die Back-End-Ziele weitergeleitet.

Für eine grundlegende Regel ist nur eine einzige Back-End-HTTP-Einstellung zulässig. Alle Anforderungen auf dem zugeordneten Listener werden mit dieser HTTP-Einstellung an die entsprechenden Back-End-Ziele weitergeleitet.

Fügen Sie für eine pfadbasierte Regel mehrere Back-End-HTTP-Einstellungen hinzu, die den einzelnen URL-Pfaden entsprechen. Anforderungen, die mit dem URL-Pfad in dieser Einstellung übereinstimmen, werden mithilfe der den einzelnen URL-Pfaden entsprechenden HTTP-Einstellungen an die entsprechenden Back-End-Ziele weitergeleitet. Fügen Sie auch eine Standard-HTTP-Einstellung hinzu. Anforderungen, die keinem URL-Pfad in dieser Regel entsprechen, werden mit der Standard-HTTP-Einstellung an den Standard-Back-End-Pool weitergeleitet.

### <a name="redirection-setting"></a>Umleitungseinstellung

Wenn die Umleitung für eine grundlegende Regel konfiguriert ist, werden alle Anforderungen auf dem zugeordneten Listener an das Ziel umgeleitet. Dies ist eine *globale Umleitung*. Wenn die Umleitung für eine pfadbasierte Regel konfiguriert ist, werden nur Anforderungen in einem bestimmten Websitebereich umgeleitet. Ein Beispiel ist ein Warenkorbbereich, der durch */cart/\** gekennzeichnet ist. Dies ist eine *pfadbasierte Umleitung*.

Weitere Informationen zu Umleitungen finden Sie unter [Übersicht über die Umleitung in Application Gateway](redirect-overview.md).

#### <a name="redirection-type"></a>Umleitungstyp

Wählen Sie den erforderlichen Umleitungstyp aus: *Dauerhaft(301)* , *Temporär(307)* , *Gefunden(302)* oder *Verweis(303)* .

#### <a name="redirection-target"></a>Umleitungsziel

Wählen Sie einen anderen Listener oder eine externe Website als Umleitungsziel aus.

##### <a name="listener"></a>Listener

Wählen Sie einen Listener als Umleitungsziel aus, um Datenverkehr von einem Listener zu einem anderen auf dem Gateway umzuleiten. Diese Einstellung ist erforderlich, wenn Sie HTTP-zu-HTTPS-Umleitung aktivieren möchten. Sie leitet Datenverkehr vom Quelllistener, der prüft, ob HTTP-Anforderungen eingehen, an den Ziellistener um, der prüft, ob HTTPS-Anforderungen eingehen. Sie können auch wählen, die Abfragezeichenfolge und den Pfad der ursprünglichen Anforderung in die Anforderung einzubeziehen, die an das Umleitungsziel weitergeleitet wird.

![Dialogfeld mit Application Gateway-Komponenten](./media/configuration-overview/configure-redirection.png)

Weitere Informationen zur HTTP-zu-HTTPS-Umleitung finden Sie unter:
- [Erstellen eines Anwendungsgateways mit Umleitung von HTTP zu HTTPS über das Azure-Portal](redirect-http-to-https-portal.md)
- [Erstellen eines Anwendungsgateways mit Umleitung von HTTP zu HTTPS mithilfe von Azure PowerShell](redirect-http-to-https-powershell.md)
- [Erstellen eines Anwendungsgateways mit Umleitung von HTTP zu HTTPS über die Azure-Befehlszeilenschnittstelle](redirect-http-to-https-cli.md)

##### <a name="external-site"></a>Externe Website

Wählen Sie „Externe Website“ aus, wenn Sie den Datenverkehr am Listener, der dieser Regel zugeordnet ist, auf eine externe Website umleiten möchten. Sie können wählen, die Abfragezeichenfolge der ursprünglichen Anforderung in die Anforderung einzubeziehen, die an das Umleitungsziel weitergeleitet wird. Sie können den Pfad zu der externen Website in der ursprünglichen Anforderung nicht weiterleiten.

Weitere Informationen zur Umleitung finden Sie unter:
- [Erstellen eines Anwendungsgateways mit externer Umleitung durch Azure PowerShell](redirect-external-site-powershell.md)
- [Erstellen eines Anwendungsgateways mit externer Umleitung durch die Azure CLI](redirect-external-site-cli.md)

#### <a name="rewrite-the-http-header-setting"></a>Umschreiben der HTTP-Headereinstellung

Diese Einstellung fügt HTTP-Anforderungs- und -Antwortheader hinzu, entfernt oder aktualisiert sie, während die Anforderungs-/Antwortpakete zwischen dem Client und den Back-End-Pools verschoben werden. Weitere Informationen finden Sie unter

 - [Erneutes Generieren von HTTP-Headern in Application Gateway (Public Preview)](rewrite-http-headers.md)
 - [Angeben Ihrer Regelkonfiguration für das erneute Generieren eines HTTP-Headers](rewrite-http-headers-portal.md)

## <a name="http-settings"></a>HTTP-Einstellungen

Das Application Gateway leitet Datenverkehr mithilfe der Konfiguration, die Sie hier festlegen, an die Back-End-Server weiter. Nachdem Sie eine HTTP-Einstellung erstellt haben, müssen Sie sie einer oder mehreren Regeln für das Routing von Anforderungen zuordnen.

### <a name="cookie-based-affinity"></a>Cookiebasierte Affinität

Azure Application Gateway verwendet vom Gateway verwaltete Cookies zum Beibehalten von Benutzersitzungen. Wenn ein Benutzer die erste Anforderung an Application Gateway sendet, setzt dieses in der Antwort ein Affinitätscookie mit einem Hashwert, der die Sitzungsdetails enthält, sodass die nachfolgenden Anfragen, die das Affinitätscookie enthalten, zum selben Back-End-Server geleitet werden, um die Bindung aufrechtzuerhalten. 

Diese Funktion ist hilfreich, wenn eine Benutzersitzung auf demselben Server bleiben soll und der Sitzungszustand lokal auf dem Server für eine Benutzersitzung gespeichert wird. Wenn die Anwendung cookiebasierte Affinität nicht verarbeiten kann, können Sie diese Funktion nicht verwenden. Um sie zu verwenden, stellen Sie sicher, dass die Clients Cookies unterstützen.

Das [v80-Update](https://chromiumdash.appspot.com/schedule) des [Chromium-Browsers](https://www.chromium.org/Home) enthielt ein Mandat, bei dem HTTP-Cookies ohne [SameSite](https://tools.ietf.org/id/draft-ietf-httpbis-rfc6265bis-03.html#rfc.section.5.3.7)-Attribut als „SameSite=Lax“ behandelt werden müssen. Im Falle von CORS-Anforderungen (Cross-Origin Resource Sharing, Ressourcenfreigabe zwischen verschiedenen Ursprüngen), wenn das Cookie in einem Drittanbieterkontext gesendet werden muss, muss es *SameSite=None; Secure*-Attribute verwenden und sollte nur über HTTPS gesendet werden. Andernfalls sendet der Browser in einem reinen HTTTP-Szenario die Cookies nicht im Drittanbieterkontext. Das Ziel dieses Updates von Chrome besteht darin, die Sicherheit zu erhöhen und CSRF-Angriffe (Cross-Site Request Forgery, siteübergreifende Anforderungsfälschung) zu vermeiden. 

Zur Unterstützung dieser Änderung fügt Application Gateway (alle SKU-Typen) ab dem 17. Februar 2020 ein weiteres Cookie namens *ApplicationGatewayAffinityCORS* zusätzlich zum vorhandenen Cookie *ApplicationGatewayAffinity* hinzu. Dem *ApplicationGatewayAffinityCORS*-Cookie sind zwei weitere Attribute hinzugefügt ( *"SameSite = None; Secure"* ), damit persistente Sitzungen selbst für ursprungsübergreifende Anforderungen erhalten bleiben.

Beachten Sie, dass der Standardname des Affinitätscookies *ApplicationGatewayAffinity* lautet, was Sie aber ändern können. Für den Fall, dass Sie einen benutzerdefinierten Affinitätscookienamen verwenden, wird ein zusätzliches Cookie mit CORS als Suffix hinzugefügt. Beispielsweise *CustomCookieNameCORS*.

> [!NOTE]
> Wenn das Attribut *SameSite=None* festgelegt ist, ist es obligatorisch, dass das Cookie auch das Flag *Secure* enthält und über HTTPS gesendet werden muss.  Wenn Sitzungsaffinität über CORS erforderlich ist, müssen Sie Ihren Workload zu HTTPS migrieren. Weitere Informationen zur TLS-Abladung und End-to-End-TLS-Dokumentation für Application Gateway finden Sie unter: [Übersicht](ssl-overview.md), [Konfigurieren einer Application Gateway-Instanz mit TLS-Beendigung mithilfe des Azure-Portals](create-ssl-portal.md) und [Konfigurieren der End-to-End-TLS-Verschlüsselung mithilfe von Application Gateway und dem Portal](end-to-end-ssl-portal.md).

### <a name="connection-draining"></a>Verbindungsausgleich

Mit dem Verbindungsausgleich können Sie Elemente des Back-End-Pools bei geplanten Dienstupdates korrekt entfernen. Sie können diese Einstellung bei der Erstellung einer Regel auf alle Elemente eines Back-End-Pools anwenden. Dadurch wird sichergestellt, dass alle Instanzen, deren Registrierung aufgehoben wird, bestehende Verbindungen aufrechterhalten, die Verarbeitung laufender Anforderungen für einen konfigurierbaren Zeitraum fortsetzen und keine neuen Anforderungen oder Verbindungen empfangen. Die einzige Ausnahme hierbei sind Anforderungen zur Aufhebung der Registrierung von Instanzen aufgrund der durch das Gateway verwalteten Sitzungsaffinität. Diese Anforderungen werden weiterhin an die Instanzen weitergeleitet, deren Registrierung aufgehoben wird. Verbindungsausgleich gilt für Back-End-Instanzen, die explizit aus dem Back-End-Pool entfernt werden.

### <a name="protocol"></a>Protocol

Application Gateway unterstützt sowohl HTTP als auch HTTPS für das Routing von Anforderungen an die Back-End-Server. Bei Auswahl von HTTP ist Datenverkehr an die Back-End-Server unverschlüsselt. Wenn unverschlüsselte Kommunikation nicht akzeptabel ist, wählen Sie HTTPS.

Diese Einstellung unterstützt zusammen mit HTTPS im Listener [End-to-End-TLS](ssl-overview.md). Damit können Sie vertrauliche Daten sicher verschlüsselt an das Back-End übertragen. Jeder Back-End-Server im Back-End-Pool mit aktiviertem End-to-End-TLS muss mit einem Zertifikat konfiguriert sein, um die sichere Kommunikation zu erlauben.

### <a name="port"></a>Port

Diese Einstellung gibt den Port an, auf dem die Back-End-Server auf Datenverkehr vom Application Gateway lauschen. Sie können Ports im Bereich von 1 bis 65535 konfigurieren.

### <a name="request-timeout"></a>Anforderungszeitlimit

Diese Einstellung ist die Anzahl der Sekunden, die das Application Gateway auf eine Antwort vom Back-End-Server wartet.

### <a name="override-back-end-path"></a>Back-End-Pfad außer Kraft setzen

Mit dieser Einstellung können Sie einen optionalen benutzerdefinierten Weiterleitungspfad konfigurieren, der für die Weiterleitung der Anforderung an das Back-End verwendet werden soll. Ein beliebiger Teil des Eingangspfads, der mit dem benutzerdefinierten Pfad im Feld **Back-End-Pfad außer Kraft setzen** übereinstimmt, wird in den weitergeleiteten Pfad kopiert. Die folgende Tabelle zeigt die Funktionsweise dieses Features:

- Wenn die HTTP-Einstellung einer einfachen Routingregel für Anforderungen zugeordnet ist:

  | Ursprüngliche Anforderung  | Back-End-Pfad außer Kraft setzen | An das Back-End weitergeleitete Anforderung |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /override/            | /override/home/              |
  | /home/secondhome/ | /override/            | /override/home/secondhome/   |

- Wenn die HTTP-Einstellung einer pfadbasierten Routingregel für Anforderungen zugeordnet ist:

  | Ursprüngliche Anforderung           | Pfadregel       | Back-End-Pfad außer Kraft setzen | An das Back-End weitergeleitete Anforderung |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home/            | /pathrule*      | /override/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /override/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /override/            | /override/home/secondhome/   |
  | /pathrule/home/            | /pathrule/home* | /override/            | /override/                   |
  | /pathrule/home/secondhome/ | /pathrule/home* | /override/            | /override/secondhome/        |
  | /pathrule/                 | /pathrule/      | /override/            | /override/                   |

### <a name="use-for-app-service"></a>Für App Service verwenden

Dies ist eine reine Benutzeroberflächenverknüpfung, die die zwei erforderlichen Einstellungen für das Azure App Service-Back-End auswählt. Sie aktiviert **Hostnamen aus Back-End-Adresse auswählen** und erstellt einen neuen benutzerdefinierten Test, wenn Sie nicht bereits einen besitzen. (Weitere Informationen finden Sie im Abschnitt [Hostnamen aus Back-End-Adresse auswählen](#pick) dieses Artikels.) Es wird ein neuer Test erstellt, dessen Header aus der Adresse des Back-End-Elements ausgewählt wird.

### <a name="use-custom-probe"></a>Benutzerdefinierten Test verwenden

Diese Einstellung ordnet einen [benutzerdefinierten Test](application-gateway-probe-overview.md#custom-health-probe) einer HTTP-Einstellung zu. Einer HTTP-Einstellung kann nur ein benutzerdefinierter Test zugeordnet werden. Wenn Sie nicht explizit einen benutzerdefinierten Test zuordnen, wird der [Standardtest](application-gateway-probe-overview.md#default-health-probe-settings) zur Überwachung der Integrität des Back-Ends verwendet. Sie sollten einen benutzerdefinierten Test erstellen, um größere Kontrolle über die Integritätsüberwachung Ihrer Back-Ends zu haben.

> [!NOTE]
> Der benutzerdefinierte Test überwacht die Integrität des Back-End-Pools nicht, sofern die entsprechende HTTP-Einstellung nicht explizit einem Listener zugeordnet ist.

### <a name="pick-host-name-from-back-end-address"></a><a id="pick"/></a>Auswählen eines Hostnamens aus der Back-End-Adresse

Diese Funktion legt den *Host*-Header in der Anforderung dynamisch auf den Hostnamen des Back-End-Pools fest. Sie verwendet dazu eine IP-Adresse oder einen vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN ).

Dieses Feature ist nützlich, wenn der Domänenname des Back-Ends vom DNS-Namen des Application Gateways abweicht und sich das Back-End für die Auflösung zum richtigen Endpunkt auf einen bestimmten Hostheader stützt.

Ein Beispielfall sind mehrinstanzenfähige Dienste auf dem Back-End. Ein App Service ist ein mehrinstanzenfähiger Dienst, der einen gemeinsam genutzten Speicherplatz mit einer einzigen IP-Adresse verwendet. Daher kann der Zugriff auf einen App Service nur über die Hostnamen erfolgen, die in den Einstellungen der benutzerdefinierten Domäne konfiguriert werden.

Standardmäßig ist der benutzerdefinierte Domänenname *example.azurewebsites.net*. Überschreiben Sie den Hostnamen in der ursprünglichen Anforderung an den Hostnamen des App-Diensts, um mithilfe einer Application Gateway-Instanz über einen Hostnamen auf Ihren App-Dienst zuzugreifen, der nicht explizit im App-Dienst oder dem vollqualifizierten Domänennamen der Application Gateway-Instanz registriert ist. Aktivieren Sie zu diesem Zweck die Einstellung **Hostnamen aus Back-End-Adresse auswählen**.

Für eine benutzerdefinierte Domäne, deren bestehender benutzerdefinierter DNS-Name dem App Service zugeordnet ist, müssen Sie diese Einstellung nicht aktivieren.

> [!NOTE]
> Diese Einstellung ist für die App Service-Umgebung nicht erforderlich, da es sich dabei um eine dedizierte Bereitstellung handelt.

### <a name="host-name-override"></a>Hostnamen außer Kraft setzen

Diese Funktion ersetzt den *Host*-Header in der beim Application Gateway eingehenden Anforderung durch den Hostnamen, den Sie angeben.

Wenn beispielsweise *www.contoso.com* in der Einstellung **Hostname** angegeben wird, wird die ursprüngliche Anforderung *`https://appgw.eastus.cloudapp.azure.com/path1` beim Weiterleiten der Anforderung an den Back-End-Server in *`https://www.contoso.com/path1` geändert.

## <a name="back-end-pool"></a>Back-End-Pool

Sie können vier Typen von Back-End-Elementen festlegen, auf die ein Back-End-Pool verweisen kann: einen bestimmten virtuellen Computer, eine VM-Skalierungsgruppe, eine IP-Adresse/einen FQDN oder einen App Service. 

Nachdem Sie einen Back-End-Pool erstellt haben, müssen Sie ihn einer oder mehreren Regeln für das Routing von Anforderungen zuordnen. Ferner müssen Sie Integritätstests für jeden Back-End-Pool in Ihrem Application Gateway konfigurieren. Wenn eine Bedingung einer Anfoderungsroutingregel erfüllt ist, leitet das Application Gateway den Datenverkehr an die fehlerfreien Server (die durch die Integritätstests bestimmt werden) im entsprechenden Back-End-Pool weiter.

## <a name="health-probes"></a>Integritätstests

Ein Application Gateway überwacht standardmäßig die Integrität aller Ressourcen in seinem Back-End. Sie sollten aber unbedingt für jede Back-End-HTTP-Einstellung einen benutzerdefinierten Test erstellen, um größere Kontrolle über die Überwachung der Integrität zu erhalten. Informationen zum Konfigurieren eines benutzerdefinierten Tests finden Sie unter [Einstellungen für die benutzerdefinierte Integritätsüberprüfung](application-gateway-probe-overview.md#custom-health-probe-settings).

> [!NOTE]
> Nachdem Sie einen benutzerdefinierten Integritätstest erstellt haben, müssen Sie ihn einer Back-End-HTTP-Einstellung zuordnen. Ein benutzerdefinierter Test überwacht die Integrität des Back-End-Pools nicht, sofern die entsprechende HTTP-Einstellung nicht explizit mithilfe einer Regel einem Listener zugeordnet ist.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun über Application Gateway-Komponenten informiert haben, können Sie folgende Aktionen ausführen:

- [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal](quick-create-portal.md)
- [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure PowerShell](quick-create-powershell.md)
- [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway: Azure CLI](quick-create-cli.md)
