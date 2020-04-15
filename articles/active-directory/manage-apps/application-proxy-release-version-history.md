---
title: 'Azure AD-Anwendungsproxy: Versionsverlauf | Microsoft-Dokumentation'
description: In diesem Artikel werden alle Versionen des Azure AD-Anwendungsproxys aufgeführt sowie neue Features und behobene Probleme beschrieben.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2020
ms.subservice: app-mgmt
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: f027fbce66a73306165a0ad35d1ba3faa7a5c0bc
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80983890"
---
# <a name="azure-ad-application-proxy-version-release-history"></a>Azure AD-Anwendungsproxy: Verlauf der Versionsveröffentlichungen
In diesem Artikel werden die veröffentlichten Versionen und Features des Anwendungsproxys von Azure Active Directory (Azure AD) aufgeführt. Das Azure AD-Team aktualisiert den Anwendungsproxy regelmäßig mit neuen Features und Funktionen. Anwendungsproxyconnectors werden automatisch aktualisiert, wenn eine neue Version veröffentlicht wird. 

Wir empfehlen die Aktivierung von automatischen Updates für Ihre Connectors, um sicherzustellen, dass Sie über die neuesten Features und Fehlerbehebungen verfügen. Microsoft stellt direkte Unterstützung für die neueste Connectorversion und die unmittelbare Vorgängerversion bereit.

Hier finden Sie eine Liste der zugehörigen Ressourcen:

Resource |  Details
--------- | --------- |
Aktivieren des Anwendungsproxys | In diesem [Tutorial](application-proxy-add-on-premises-application.md) werden die Voraussetzungen zum Aktivieren des Anwendungsproxys und zum Installieren und Registrieren eines Connectors beschrieben.
Grundlegendes zu Azure AD-Anwendungsproxyconnectors | Erfahren Sie mehr über die [Connectorverwaltung](application-proxy-connectors.md) und die [automatische Aktualisierung](application-proxy-connectors.md#automatic-updates) von Connectors.
Herunterladen des Azure AD-Anwendungsproxyconnectors |  [Laden Sie den aktuellen Connector herunter](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download).

## <a name="1515260"></a>1.5.1526.0

### <a name="release-status"></a>Releasestatus

7\. April 2020: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen
-   Connectors verwenden für alle Verbindungen ausschließlich TLS 1.2. Weitere Informationen finden Sie unter [Voraussetzungen für Connectors](application-proxy-add-on-premises-application.md#before-you-begin).
- Verbesserte Signalisierung zwischen Connector und Azure-Diensten. Dazu zählen die Unterstützung zuverlässiger Sitzungen für die WCF-Kommunikation zwischen Connector und Azure-Diensten sowie Verbesserungen des DNS-Caches für die WebSocket-Kommunikation.
- Unterstützung für die Konfiguration eines Proxys zwischen dem Connector und der Back-End-Anwendung. Weitere Informationen finden Sie unter [Verwenden von vorhandenen lokalen Proxyservern](application-proxy-configure-connectors-with-proxy-servers.md).

### <a name="fixed-issues"></a>Behobene Probleme
- Das Fallback auf Port 8080 für die Kommunikation zwischen dem Connector und Azure-Diensten wurde entfernt.
- Debugablaufverfolgungen für die WebSocket-Kommunikation wurden hinzugefügt. 
- Das Beibehalten des SameSite-Attributs wurde behoben, wenn es in Back-End-Anwendungscookies festgelegt war.

## <a name="156120"></a>1.5.612.0

### <a name="release-status"></a>Releasestatus

20. September 2018: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

- Es wurde WebSocket-Unterstützung für die Anwendung QlikSense hinzugefügt. Weitere Informationen zum Integrieren von QlikSense mit dem Anwendungsproxy finden Sie in dieser [exemplarischen Vorgehensweise](application-proxy-qlik.md). 
- Der Installations-Assistent wurde verbessert, um das Konfigurieren eines ausgehenden Proxys zu vereinfachen. 
- TLS 1.2 wurde als Standardprotokoll für Connectors festgelegt. 
- Es wurden neue Microsoft-Software-Lizenzbedingungen hinzugefügt.  

### <a name="fixed-issues"></a>Behobene Probleme

- Es wurde ein Fehler behoben, der zu Arbeitsspeicherverlusten im Connector führte.
- Die Azure Service Bus-Version wurde aktualisiert, wobei auch Connectortimeoutfehler behoben wurden.

## <a name="154020"></a>1.5.402.0

### <a name="release-status"></a>Releasestatus

19. Januar 2018: Für den Download veröffentlicht

### <a name="fixed-issues"></a>Behobene Probleme

- Es wurde Unterstützung für benutzerdefinierte Domänen hinzugefügt, die eine Domänenübersetzung im Cookie erfordern.

## <a name="151320"></a>1.5.132.0

### <a name="release-status"></a>Releasestatus 

25. Mai 2017: Für den Download veröffentlicht 

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen 

Verbesserte Kontrolle über die Beschränkungen für ausgehende Verbindungen von Connectors. 

## <a name="15360"></a>1.5.36.0

### <a name="release-status"></a>Releasestatus

15. April 2017: Für den Download veröffentlicht

### <a name="new-features-and-improvements"></a>Neue Features und Verbesserungen

- Das Onboarding und die Verwaltung wurden vereinfacht, wobei weniger Ports erforderlich sind. Für den Anwendungsproxy müssen jetzt nur zwei ausgehende Standardports geöffnet werden: 443 und 80. Der Anwendungsproxy verwendet weiterhin nur ausgehende Verbindungen, sodass Sie in einer DMZ keine Komponenten benötigen. Weitere Informationen finden Sie in unserer  [Konfigurationsdokumentation](application-proxy-add-on-premises-application.md).  
- Sie können Ihr Netzwerk jetzt über DNS statt über den IP-Adressbereich öffnen, wenn dies von Ihrem externen Proxy oder Ihrer Firewall unterstützt wird. Anwendungsproxydienste erfordern nur Verbindungen mit „*.msappproxy.net“ und „*.servicebus.windows.net“.


## <a name="earlier-versions"></a>Frühere Versionen

Wenn Sie eine Anwendungsproxyconnector-Version vor 1.5.36.0 verwenden, aktualisieren Sie auf die neueste Version, um sicherzustellen, dass Sie über die neuesten vollständig unterstützten Features verfügen.

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über den [Remotezugriff auf lokale Anwendungen über den Azure Active Directory-Anwendungsproxy](application-proxy.md).
- Erste Schritte mit dem Anwendungsproxy finden Sie in [Tutorial: Hinzufügen einer lokalen Anwendung für den Remotezugriff über den Anwendungsproxy](application-proxy-add-on-premises-application.md).
