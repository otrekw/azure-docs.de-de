---
title: 'Konfigurieren von Proxyeinstellungen für den Mobilitätsdienst: Azure-zu-Azure-Notfallwiederherstellung | Microsoft-Dokumentation'
description: Enthält Informationen zur Konfiguration des Mobilitätsdiensts für den Fall, dass Kunden in ihrer Quellumgebung einen Proxy nutzen.
services: site-recovery
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/18/2020
ms.author: sideeksh
ms.openlocfilehash: 3d33b5a89a718a41e5c547551f6e7eb4f7033a63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79504122"
---
# <a name="configure-mobility-service-proxy-settings-for-azure-to-azure-disaster-recovery"></a>Konfigurieren von Proxyeinstellungen für den Mobilitätsdienst: Azure-zu-Azure-Notfallwiederherstellung

Dieser Artikel bietet eine Anleitung zum Anpassen der Netzwerkkonfigurationen auf Azure-Ziel-VMs für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen mithilfe von [Azure Site Recovery](site-recovery-overview.md).

In diesem Dokument werden die Schritte zum Konfigurieren der Proxyeinstellungen für den Azure Site Recovery-Mobilitätsdienst bei einer Azure-zu-Azure-Notfallwiederherstellung beschrieben. 

Proxys sind Netzwerkgateways, die Netzwerkverbindungen mit Endpunkten zulassen bzw. ablehnen. Bei einem Proxy handelt es sich in der Regel um einen Computer außerhalb des Clientcomputers, der versucht, auf Netzwerkendpunkte zuzugreifen. Über eine Umgehungsliste kann der Client Verbindungen direkt mit den Endpunkten herstellen, ohne den Proxy einzubinden. Netzwerkadministratoren können für einen Proxy optional einen Benutzernamen und ein Kennwort festlegen, damit er nur von authentifizierten Clients genutzt werden kann. 

## <a name="before-you-start"></a>Vorbereitung

Erfahren Sie, wie Site Recovery die Notfallwiederherstellung für [dieses Szenario](azure-to-azure-architecture.md) bereitstellt.
Machen Sie sich mit dem [Netzwerkleitfaden](azure-to-azure-about-networking.md) für die Replikation und Wiederherstellung von Azure-VMs zwischen verschiedenen Regionen per [Azure Site Recovery](site-recovery-overview.md) vertraut.
Stellen Sie sicher, dass Ihr Proxy gemäß den Anforderungen Ihrer Organisation richtig eingerichtet wurde.

## <a name="configure-the-mobility-service"></a>Konfigurieren des Mobilitätsdiensts

Vom Mobilitätsdienst werden nur nicht authentifizierte Proxys unterstützt. Es gibt hierbei zwei Möglichkeiten, wie Sie Proxydetails für die Kommunikation mit Site Recovery-Endpunkten eingeben können. 

### <a name="method-1-auto-detection"></a>Methode 1: Automatische Erkennung

Der Mobilitätsdienst erkennt beim Aktivieren der Replikation die Proxyeinstellungen anhand der Umgebungseinstellungen bzw. IE-Einstellungen (nur Windows) automatisch. 

- Windows-Betriebssystem: Beim Aktivieren der Replikation erkennt der Mobilitätsdienst die Proxyeinstellungen, die in Internet Explorer für den lokalen Systembenutzer konfiguriert sind. Zum Einrichten des Proxys für das lokale Systemkonto kann ein Administrator „psexec“ verwenden, um eine Eingabeaufforderung und dann Internet Explorer zu starten. 
- Windows-Betriebssystem: Die Proxyeinstellungen werden als Umgebungsvariablen mit dem Namen „http_proxy“ und „no_proxy“ konfiguriert. 
- Linux-Betriebssystem: Die Proxyeinstellungen werden unter „/etc/profile“ oder „/etc/environment“ als Umgebungsvariablen mit dem Namen „http_proxy“ bzw. „no_proxy“ konfiguriert. 
- Die automatisch erkannten Proxyeinstellungen werden in der Proxykonfigurationsdatei „ProxyInfo.conf“ des Mobilitätsdiensts gespeichert. 
- Standardspeicherort von „ProxyInfo.conf“ 
    - Windows: C:\ProgramData\Microsoft Azure Site Recovery\Config\ProxyInfo.conf 
    - Linux: /usr/local/InMage/config/ProxyInfo.conf


### <a name="method-2-provide-custom-application-proxy-settings"></a>Methode 2: Bereitstellen von benutzerdefinierten Anwendungsproxyeinstellungen

In diesem Fall stellt der Kunde die benutzerdefinierten Anwendungsproxyeinstellungen in der Konfigurationsdatei „ProxyInfo.conf“ des Mobilitätsdiensts bereit. Bei dieser Methode können Kunden den Proxy nur für den Mobilitätsdienst oder einen anderen Proxy für den Azure Site Recovery-Mobilitätsdienst bereitstellen, der sich von den restlichen Anwendungen auf dem Computer unterscheidet (bzw. keinen Proxy bereitstellen).

## <a name="proxy-template"></a>Proxyvorlage
„ProxyInfo.conf“ enthält die folgende Vorlage: [proxy] Address=http://1.2.3.4 Port=5678 BypassList=hypervrecoverymanager.windowsazure.com,login.microsoftonline.com,blob.core.windows.net. Die Umgehungsliste (BypassList) unterstützt keine Platzhalter wie „*.windows.net“, aber das Angeben von „windows.net“ reicht für die Umleitung aus. 

## <a name="next-steps"></a>Nächste Schritte:
- Lesen Sie [Netzwerkkonzepte für die Replikation zwischen Azure-Standorten](site-recovery-azure-to-azure-networking-guidance.md) zum Replizieren von virtuellen Azure-Computern.
- Stellen Sie die Notfallwiederherstellung bereit, indem Sie [Azure-VMs replizieren](site-recovery-azure-to-azure.md).