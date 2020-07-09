---
title: Azure Application Gateway-Funktionen
description: Informieren Sie sich über Funktionen von Azure Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: victorh
ms.openlocfilehash: f021eed959ef88a1ef3671e1d0ace8080710c92a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "80810231"
---
# <a name="azure-application-gateway-features"></a>Azure Application Gateway-Funktionen

[Azure Application Gateway](overview.md) ist ein Lastenausgleich für Webdatenverkehr, mit dem Sie eingehenden Datenverkehr für Ihre Webanwendungen verwalten können.

![Application Gateway-Konzepte](media/overview/figure1-720.png)

Application Gateway umfasst die folgenden Funktionen:

- [SSL/TLS-Beendigung (Secure Sockets Layer)](#secure-sockets-layer-ssltls-termination)
- [Automatische Skalierung](#autoscaling)
- [Zonenredundanz](#zone-redundancy)
- [Statische VIP](#static-vip)
- [Web Application Firewall](#web-application-firewall)
- [Eingangscontroller für AKS](#ingress-controller-for-aks)
- [URL-basiertes Routing](#url-based-routing)
- [Hosten mehrerer Websites](#multiple-site-hosting)
- [Umleitung](#redirection)
- [Sitzungsaffinität](#session-affinity)
- [WebSocket- und HTTP/2-Datenverkehr](#websocket-and-http2-traffic)
- [Verbindungsausgleich](#connection-draining)
- [Benutzerdefinierte Fehlerseiten](#custom-error-pages)
- [Erneutes Generieren von HTTP-Headern](#rewrite-http-headers)
- [Festlegen der Größe](#sizing)

## <a name="secure-sockets-layer-ssltls-termination"></a>SSL/TLS-Beendigung (Secure Sockets Layer)

Application Gateway unterstützt die SSL/TLS-Beendigung am Gateway, wonach der Datenverkehr in der Regel unverschlüsselt zu den Back-End-Servern gelangt. Mit diesem Feature können Webserver vom kostspieligen Verschlüsselungs- und Entschlüsselungsaufwand befreit werden. Manchmal ist die unverschlüsselte Kommunikation mit den Servern allerdings keine akzeptable Option. Der Grund hierfür können Sicherheits- oder Complianceanforderungen sein, oder dass die Anwendung nur eine sichere Verbindung akzeptiert. Für Anwendungen dieser Art unterstützt Application Gateway die End-to-End-Verschlüsselung mit SSL/TLS.

Weitere Informationen finden Sie unter [Übersicht über SSL-Beendigung und End-to-End-SSL mit Application Gateway](ssl-overview.md).

## <a name="autoscaling"></a>Automatische Skalierung

Application Gateway Standard_v2 unterstützt automatische Skalierung und kann zentral hoch- oder herunterskalieren, je nach Veränderung der Datenverkehr-Auslastungsmuster. Durch die automatische Skalierung entfällt auch die Notwendigkeit, während des Bereitstellens eine Bereitstellungsgröße oder eine Anzahl von Instanzen auszuwählen. 

Weitere Informationen zu den Funktionen von Application Gateway Standard_v2 finden Sie unter [Automatische Skalierung der v2-SKU](application-gateway-autoscaling-zone-redundant.md).

## <a name="zone-redundancy"></a>Zonenredundanz

Ein Application Gateway Standard_v2 kann sich über mehrere Verfügbarkeitszonen erstrecken und so höhere Fehlerresilienz bieten sowie die Notwendigkeit beseitigen, in jeder Zone separate Application Gateways bereitstellen zu müssen.

## <a name="static-vip"></a>Statische VIP

Die Application Gateway Standard_v2-SKU unterstützt ausschließlich den statischen VIP-Typ. Dadurch wird sichergestellt, dass die dem Application Gateway zugeordnete VIP auch über die gesamte Lebensdauer des Application Gateways unverändert bleibt.

## <a name="web-application-firewall"></a>Web Application Firewall

Web Application Firewall (WAF) ist ein Dienst, der zentralisierten Schutz Ihrer Webanwendungen vor verbreiteten Exploits und Sicherheitsrisiken bietet. Die WAF basiert auf den Regeln der [OWASP-Kernregelsätze (Open Web Application Security Project)](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) der Version 3.1 (nur WAF_v2), 3.0 und 2.2.9. 

Webanwendungen sind zunehmend Ziele böswilliger Angriffe, die allgemein bekannte Sicherheitslücken ausnutzen. Zu diesen Sicherheitslücken (Exploits) gehören üblicherweise Angriffe durch Einschleusung von SQL-Befehlen oder Angriffe durch websiteübergreifende Skripts, um nur einige zu nennen. Die Verhinderung solcher Angriffe im Anwendungscode ist oft schwierig und erfordert strenge Wartung, Patching und Überwachung auf vielen Ebenen der Anwendungstopologie. Eine zentrale Web Application Firewall vereinfacht die Sicherheitsverwaltung erheblich und bietet Anwendungsadministratoren einen besseren Schutz vor Bedrohungen und Angriffen. Mit einer WAF-Lösung können Sie ebenfalls schneller auf ein Sicherheitsrisiko reagieren, da eine bekannte Schwachstelle an einem zentralen Ort gepatcht wird, statt jede einzelne Webanwendung separat zu sichern. Vorhandene Anwendungsgateways lassen sich problemlos in ein Anwendungsgateway mit Web Application Firewall konvertieren.

Weitere Informationen finden Sie unter [Was ist die Azure Web Application Firewall?](../web-application-firewall/overview.md).

## <a name="ingress-controller-for-aks"></a>Eingangscontroller für AKS
Der Application Gateway-Eingangscontroller (Application Gateway Ingress Controller, AGIC) ermöglicht die Verwendung von Application Gateway als Eingang für einen [AKS-Cluster (Azure Kubernetes Service)](https://azure.microsoft.com/services/kubernetes-service/). 

Der Eingangscontroller wird als Pod im AKS-Cluster ausgeführt, nutzt [Kubernetes-Eingangsressourcen](https://kubernetes.io/docs/concepts/services-networking/ingress/) und konvertiert sie in eine Application Gateway-Konfiguration, die es dem Gateway ermöglicht, einen Lastenausgleich für den an die Kubernetes-Pods gesendeten Datenverkehr durchzuführen. Der Eingangscontroller unterstützt nur die SKUs „Application Gateway Standard_v2“ und „WAF_v2“. 

Weitere Informationen finden Sie unter [Was ist der Application Gateway-Eingangscontroller?](ingress-controller-overview.md).

## <a name="url-based-routing"></a>URL-basiertes Routing

Mit dem Routing auf URL-Pfadbasis kann Datenverkehr basierend auf URL-Pfaden von Anforderungen an Back-End-Serverpools weitergeleitet werden. Ein mögliches Szenario ist die Weiterleitung von Anforderungen für unterschiedliche Inhaltstypen an verschiedene Pools.

Beispielsweise werden Anforderungen von `http://contoso.com/video/*` an VideoServerPool und `http://contoso.com/images/*` an ImageServerPool weitergeleitet. DefaultServerPool wird ausgewählt, wenn keines der Pfadmuster zutrifft.

Weitere Informationen finden Sie unter [Routing auf URL-Pfadbasis – Übersicht](url-route-overview.md).

## <a name="multiple-site-hosting"></a>Hosten mehrerer Websites

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Website auf derselben Anwendungsgatewayinstanz zu konfigurieren. Mit dieser Funktion können Sie eine effizientere Topologie für Ihre Bereitstellungen konfigurieren, indem Sie einer Application Gateway-Instanz bis zu 100 Websites hinzufügen (für optimale Leistung). Jede Website kann zu einem eigenen Pool umgeleitet werden. Das Anwendungsgateway kann Datenverkehr beispielsweise für `contoso.com` und `fabrikam.com` über die beiden Serverpools „ContosoServerPool“ und „FabrikamServerPool“ bereitstellen.

Anforderungen für `http://contoso.com` werden an „ContosoServerPool“ und Anforderungen für `http://fabrikam.com` an „FabrikamServerPool“ weitergeleitet.

Analog dazu können zwei Unterdomänen der gleichen übergeordneten Domäne in der gleichen Anwendungsgatewaybereitstellung gehostet werden. Beispiele für die Verwendung von untergeordneten Domänen sind `http://blog.contoso.com` und `http://app.contoso.com`, die unter nur einer Anwendungsgatewaybereitstellung gehostet werden.

Weitere Informationen finden Sie unter [Application Gateway – Hosten mehrerer Websites](multiple-site-overview.md).

## <a name="redirection"></a>Umleitung

Ein typisches Szenario vieler Webanwendungen ist die Unterstützung der automatischen Umleitung von HTTP zu HTTPS, um sicherzustellen, dass die gesamte Kommunikation zwischen einer Anwendung und ihren Benutzern über einen verschlüsselten Pfad stattfindet.

In der Vergangenheit haben Sie unter Umständen auch Verfahren wie die Erstellung eines dedizierten Pools verwendet, der den alleinigen Zweck hatte, eingehende HTTP-Anforderungen zu HTTPS umzuleiten. Application Gateway unterstützt die Umleitung von Application Gateway-Datenverkehr. Dies vereinfacht die Anwendungskonfiguration, optimiert die Ressourcennutzung und ermöglicht neue Umleitungsszenarien wie etwa die globale und pfadbasierte Umleitung. Die Application Gateway-Umleitung ist nicht auf HTTP zu HTTPS beschränkt. Vielmehr handelt es sich um einen generischen Umleitungsmechanismus, sodass Sie Umleitungen für jeden Port durchführen können, den Sie mithilfe von Regeln definieren. Auch die Umleitung an eine externe Website wird unterstützt.

Die Application Gateway-Umleitung bietet Folgendes:

- Globale Umleitung von einem Port zu einem anderen Port über das Gateway. Dies ermöglicht HTTP-zu-HTTPS-Umleitungen auf einer Website.
- Pfadbasierte Umleitung. Bei dieser Art von Umleitung kann HTTP nur in einem bestimmten Websitebereich zu HTTPS umgeleitet werden – etwa in einem durch `/cart/*` gekennzeichneten Einkaufswagenbereich.
- Umleitung an eine externe Website.

Weitere Informationen finden Sie unter [Übersicht über die Umleitung in Application Gateway](redirect-overview.md).

## <a name="session-affinity"></a>Sitzungsaffinität

Das Feature „Cookiebasierte Sitzungsaffinität“ ist hilfreich, wenn eine Benutzersitzung auf dem gleichen Server bleiben soll. Mithilfe von durch das Gateway verwalteten Cookies kann Application Gateway weiteren Datenverkehr einer Benutzersitzung zur Verarbeitung an den gleichen Server weiterleiten. Dies ist hilfreich, wenn der Sitzungsstatus für eine Benutzersitzung lokal auf dem Server gespeichert wird.

Weitere Informationen finden Sie unter [Funktionsweise von Application Gateway](how-application-gateway-works.md#modifications-to-the-request).

## <a name="websocket-and-http2-traffic"></a>WebSocket- und HTTP/2-Datenverkehr

Application Gateway verfügt über native Unterstützung für das WebSocket- und das HTTP/2-Protokoll. Die WebSocket-Unterstützung kann von Benutzern nicht selektiv aktiviert oder deaktiviert werden.

Das WebSocket- und das HTTP/2-Protokoll ermöglichen die Vollduplexkommunikation zwischen einem Server und einem Client über eine TCP-Verbindung mit langer Laufzeit. Dies ermöglicht wiederum mehr Interaktivität bei der Kommunikation zwischen dem Webserver und dem Client, da die Kommunikation auch ohne die bei HTTP-basierten Implementierungen erforderlichen Abfragen bidirektional sein kann. Diese Protokolle zeichnen sich im Vergleich zu HTTP durch einen geringen Mehraufwand aus. Außerdem können sie die gleiche TCP-Verbindung für mehrere Anforderungen/Antworten verwenden, was eine effizientere Ressourcennutzung zur Folge hat. Diese Protokolle sind für die Nutzung der üblichen HTTP-Ports 80 und 443 konzipiert.

Weitere Informationen finden Sie unter [WebSocket-Unterstützung](application-gateway-websocket.md) und [HTTP/2-Unterstützung](configuration-overview.md#http2-support).

## <a name="connection-draining"></a>Verbindungsausgleich

Mit dem Verbindungsausgleich können Sie eine korrekte Entfernung von Mitgliedern des Back-End-Pools bei geplanten Dienstupdates erzielen. Diese Einstellung wird über die HTTP-Einstellung des Back-Ends aktiviert und kann bei der Erstellung einer Regel auf alle Mitglieder eines Back-End-Pools angewendet werden. Nach der Aktivierung stellt Application Gateway sicher, dass alle Instanzen eines Back-End-Pools, deren Registrierung aufgehoben wird, keine neuen Anforderungen mehr erhalten, während vorhandene Anforderungen innerhalb eines konfigurierten Zeitlimits abgeschlossen werden können. Dies gilt sowohl für Back-End-Instanzen, die durch eine Änderung der Benutzerkonfiguration explizit aus dem Back-End-Pool entfernt werden, als auch für Back-End-Instanzen, die von den Integritätstests als fehlerhaft gemeldet werden. Die einzige Ausnahme hierbei sind Anforderungen zum Aufheben der Registrierung von Instanzen, deren Registrierung aufgrund der durch das Gateway verwalteten Sitzungsaffinität explizit aufgehoben wurden. Diese Anforderungen werden weiterhin per Proxy an die Instanzen weitergeleitet, deren Registrierung aufgehoben wird.

Weitere Informationen finden Sie unter [Application Gateway-Konfiguration: Übersicht](configuration-overview.md#connection-draining).

## <a name="custom-error-pages"></a>Benutzerdefinierte Fehlerseiten

Mit Application Gateway können Sie benutzerdefinierte Fehlerseiten erstellen, anstatt Standardfehlerseiten anzuzeigen. Sie können für eine benutzerdefinierte Fehlerseite Ihr eigenes Branding und Layout verwenden.

Weitere Informationen finden Sie unter [Benutzerdefinierte Fehler](custom-error.md).

## <a name="rewrite-http-headers"></a>Erneutes Generieren von HTTP-Headern

HTTP-Header ermöglichen Client und Server das Übergeben von zusätzlichen Informationen mit der Anforderung oder der Antwort. Das Umschreiben dieser HTTP-Header hilft Ihnen, mehrere wichtige Szenarien zu meistern, wie z.B.:

- Hinzufügen von sicherheitsbezogenen Headerfeldern wie HSTS/X-XSS-Protection.
- Entfernen von Headerfeldern aus Antworten, die vertrauliche Informationen preisgeben können.
- Entfernen von Portinformationen aus X-Forwarded-For-Headern.

Application Gateway unterstützt das Hinzufügen, Entfernen oder Aktualisieren von HTTP-Anforderungs- und -Antwortheadern, während die Anforderungs- und Antwortpakete zwischen dem Client und den Back-End-Pools übertragen werden. Es bietet Ihnen auch die Möglichkeit, Bedingungen hinzuzufügen, um sicherzustellen, dass die angegebenen Header nur dann neu geschrieben werden, wenn bestimmte Bedingungen erfüllt sind.

Weitere Informationen finden Sie unter [Erneutes Generieren von HTTP-Headern](rewrite-http-headers.md).

## <a name="sizing"></a>Festlegen der Größe

Die Standard_v2-SKU von Application Gateway kann für die automatische Skalierung oder für Bereitstellungen fester Größe konfiguriert werden. Diese SKU bietet keine verschiedenen Instanzgrößen. Weitere Informationen zur Leistung und zu den Preisen von v2 finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway v2](application-gateway-autoscaling-zone-redundant.md#pricing).

Application Gateway Standard wird derzeit in drei Größen angeboten: **klein**, **mittel** und **groß**. Kleine Instanzen sind für Entwicklungs- und Testszenarien vorgesehen.

Eine vollständige Liste mit den Einschränkungen von Anwendungsgateways finden Sie unter [Application Gateway service limits (Einschränkungen von Application Gateway)](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

Die folgende Tabelle zeigt den durchschnittlichen Leistungsdurchsatz für jede v1-Application Gateway-Instanz mit aktivierter SSL-Auslagerung:

| Durchschnittliche Größe der Back-End-Seitenantwort | Klein | Medium | Groß |
| --- | --- | --- | --- |
| 6 KB |7,5 MBit/s |13 MBit/s |50 MBit/s |
| 100 KB |35 MBit/s |100 MBit/s |200 MBit/s |

> [!NOTE]
> Hierbei handelt es sich um ungefähre Werte für den Durchsatz des Anwendungsgateways. Der tatsächliche Durchsatz ist abhängig von verschiedenen Umgebungsdetails wie etwa durchschnittliche Seitengröße, Speicherort der Back-End-Instanzen und Verarbeitungszeit für die Seitenbereitstellung. Für genaue Leistungsangaben sollten Sie Ihre eigenen Tests ausführen. Diese Werte dienen nur als Leitfaden für die Kapazitätsplanung.

## <a name="version-feature-comparison"></a>Funktionsvergleich der Versionen

Einen Vergleich der Funktionen von Application Gateway v1 mit v2 finden Sie unter [Automatische Skalierung und zonenredundantes Application Gateway v2](application-gateway-autoscaling-zone-redundant.md#feature-comparison-between-v1-sku-and-v2-sku).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Funktionsweise von Application Gateway finden Sie unter [Funktionsweise von Application Gateway](how-application-gateway-works.md).