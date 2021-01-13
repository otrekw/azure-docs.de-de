---
title: Azure Application Gateway – Konfigurationsübersicht
description: In diesem Artikel wird das Konfigurieren der Komponenten von Azure Application Gateway beschrieben.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: 0012ac99600c77dce5940387e1da54f29c3f6ab7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89651996"
---
# <a name="application-gateway-configuration-overview"></a>Application Gateway – Konfigurationsübersicht

Azure Application Gateway besteht aus mehreren Komponenten, die Sie auf verschiedene Weise für verschiedene Szenarien konfigurieren können. In diesem Artikel wird das Konfigurieren der einzelnen Komponenten veranschaulicht.

![Flussdiagramm der Application Gateway-Komponenten](./media/configuration-overview/configuration-overview1.png)

Dieses Bild zeigt eine Anwendung mit drei Listenern. Die ersten zwei sind Listener für mehrere Standorte für `http://acme.com/*` bzw. `http://fabrikam.com/*`. Beide lauschen auf Port 80. Bei dem dritten handelt es sich um einen Basislistener, der über End-to-End-TLS-Beendigung (Transport Layer Security) verfügt, die zuvor als SSL-Beendigung (Secure Sockets Layer) bekannt war.

## <a name="infrastructure"></a>Infrastruktur

Die Application Gateway-Infrastruktur umfasst das virtuelle Netzwerk, Subnetze, Netzwerksicherheitsgruppen und benutzerdefinierte Routen.

Weitere Informationen finden Sie unter [Konfiguration der Application Gateway-Infrastruktur](configuration-infrastructure.md).



## <a name="front-end-ip-address"></a>Front-End-IP-Adresse

Sie können das Application Gateway mit einer öffentlichen IP-Adresse, mit einer privaten IP-Adresse oder mit beidem konfigurieren. Eine öffentliche IP-Adresse ist erforderlich, wenn Sie ein Back-End hosten, auf das Clients über eine für den Internetzugriff verfügbare virtuelle IP (VIP) über das Internet zugreifen müssen.

Weitere Informationen finden Sie unter [Konfiguration der IP-Adresse des Application Gateway-Front-Ends](configuration-front-end-ip.md).

## <a name="listeners"></a>Listener

Ein Listener ist eine logische Entität, die mithilfe von Port, Protokoll, Host und IP-Adresse auf eingehende Verbindungsanforderungen prüft. Wenn Sie den Listener konfigurieren, müssen Sie Werte für diese eingeben, die den entsprechenden Werten in der eingehenden Anforderung auf dem Gateway entsprechen.

Weitere Informationen finden Sie unter [Konfiguration des Application Gateway-Listeners](configuration-listeners.md).

## <a name="request-routing-rules"></a>Anforderungsroutingregeln

Wenn Sie ein Application Gateway mithilfe des Azure-Portals erstellen, erstellen Sie eine Standardregel (*rule1*). Mit dieser Regel wird der Standardlistener (*appGatewayHttpListener*) an den Standard-Back-End-Pool (*appGatewayBackendPool*) und die Back-End-HTTP-Standardeinstellungen ( *appGatewayBackendHttpSettings*) gebunden. Nachdem Sie das Gateway erstellt haben, können Sie die Einstellungen der Standardregel bearbeiten oder neue Regeln erstellen.

Weitere Informationen finden Sie unter [Routingregeln für Application Gateway-Anforderungen](configuration-request-routing-rules.md).

## <a name="http-settings"></a>HTTP-Einstellungen

Das Application Gateway leitet Datenverkehr mithilfe der Konfiguration, die Sie hier festlegen, an die Back-End-Server weiter. Nachdem Sie eine HTTP-Einstellung erstellt haben, müssen Sie sie einer oder mehreren Regeln für das Routing von Anforderungen zuordnen.

Weitere Informationen finden Sie unter [Konfiguration der HTTP-Einstellungen für Application Gateway](configuration-http-settings.md).

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
- [Erstellen eines Anwendungsgateways mit PowerShell](quick-create-powershell.md)
- [Erstellen eines Anwendungsgateways mithilfe der Azure CLI](quick-create-cli.md)
