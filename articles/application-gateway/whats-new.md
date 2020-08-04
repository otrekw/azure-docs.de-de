---
title: Neuerungen in Azure Application Gateway
description: Enthält Informationen zu den Neuerungen in Azure Application Gateway, z. B. aktuelle Versionshinweise, bekannte Probleme, Fehlerbehebungen, veraltete Funktionen und anstehende Änderungen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.date: 06/10/2020
ms.author: victorh
ms.openlocfilehash: 6ea0b28f5e0f23e0f8bb30e8fe2b0f792bce0709
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287448"
---
# <a name="whats-new-in-azure-application-gateway"></a>Neuerungen in Azure Application Gateway

Azure Application Gateway wird fortlaufend aktualisiert. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Versionen
- Bekannte Probleme
- Behebung von Programmfehlern
- Veraltete Funktionen

## <a name="new-features"></a>Neue Funktionen

|Funktion  |BESCHREIBUNG  |Hinzufügedatum  |
|---------|---------|---------|
| Platzhalterhostnamen in Listenern (Vorschauversion) | Sie können jetzt Platzhalterhostnamen in einem Listener für mehrere Standorte und bis zu fünf Hostnamen pro Listener definieren. Weitere Informationen finden Sie unter [Platzhalterhostnamen in Listenern (Vorschauversion)](multiple-site-overview.md#wildcard-host-names-in-listener-preview). | Juli 2020 |
| URL-Rewrite (Vorschauversion) | Sie können den URL-Pfad und die Abfragezeichenfolgenparameter nun mithilfe von URL-Rewrite umschreiben Unter [Umschreiben von HTTP-Headern und -URLs mit Application Gateway](rewrite-http-headers-url.md) finden Sie weitere Informationen, und unter [Umschreiben von URLs mit Azure Application Gateway: Azure-Portal (Vorschau)](rewrite-url-portal.md) finden Sie eine Schritt-für-Schritt-Anleitung. | Juli 2020 |
| Benutzerdefinierter Port für Integritätstests | In der Application Gateway v2-SKU kann nun ein benutzerdefinierter Port in der Konfiguration für Integritätstests angegeben werden. Weitere Informationen finden Sie unter [Systemüberwachung des Application Gateways – Übersicht](application-gateway-probe-overview.md). | Juli 2020 |
| AKS-Add-On für den Application Gateway-Eingangscontroller (Application Gateway Ingress Controller, AGIC) (Vorschau) |Der Application Gateway-Eingangscontroller kann nun über die Azure CLI in einer Zeile als natives AKS-Add-On bereitgestellt werden. Da es sich um ein AKS-Add-On handelt, kann AGIC ein vollständig verwalteter Dienst werden und dennoch im AKS-Cluster des Kunden ausgeführt werden. Weitere Informationen finden Sie im Abschnitt mit den [Unterschieden zwischen AGIC-Add-Ons](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on). |Juni 2020 |
| Benutzerdefinierte Routen (User-Defined Routes, UDR) in Version 2 (Vorschau) |Benutzerdefinierte Routen werden nun in einigen Szenarien in Application Gateway v2-SKUs unterstützt. Weitere Informationen finden Sie unter [Application Gateway-Konfiguration: Übersicht](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet). |März 2020 |
|Änderungen für das Affinitätscookie |Wenn die cookiebasierte Affinität aktiviert ist, fügt Application Gateway zusätzlich zum bereits vorhandenen Cookie „ApplicationGatewayAffinity“ ein identisches Cookie namens *ApplicationGatewayAffinityCORS* ein. *ApplicationGatewayAffinityCORS* werden zwei weitere Attribute hinzugefügt ( *"SameSite = None; Secure"* ), damit persistente Sitzungen auch bei ursprungsübergreifenden Anforderungen erhalten bleiben. Weitere Informationen finden Sie unter [Cookiebasierte Affinität](configuration-overview.md#cookie-based-affinity). |Februar 2020 |
|Verbesserungen der Tests |Mit den Verbesserungen benutzerdefinierter Tests in der Application Gateway v2-SKU wurden [Testkonfigurationen](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#create-probe-for-application-gateway-v2-sku) und [bedarfsgesteuerte Back-End-Integritätstests](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-probe-portal#test-backend-health-with-the-probe) vereinfacht und [mehr Diagnoseinformationen](https://docs.microsoft.com/azure/application-gateway/application-gateway-backend-health-troubleshooting#error-messages) hinzugefügt, um Sie bei der Behandlung von Problemen mit der Back-End-Integrität zu unterstützen.  |Oktober 2019 |
|Weitere Metriken |Wir haben die folgenden neuen Metriken hinzugefügt, mit denen Sie Ihre Application Gateway v2-SKU überwachen können: [„Zeitmetriken“](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#timing-metrics), „Back-End-Antwortstatus“, „Empfangene Bytes“, „Gesendete Bytes“, „Client-TLS-Protokoll“ und „Aktuelle Compute-Einheiten“. Informationen finden Sie unter [Von der Application Gateway V2-SKU unterstützte Metriken](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-supported-by-application-gateway-v2-sku). |August 2019 |
|Benutzerdefinierte WAF-Regeln |Application Gateway WAF_v2 unterstützt nun das Erstellen von benutzerdefinierten Regeln. Siehe [Custom rules for Web Application Firewall v2](custom-waf-rules-overview.md) (Benutzerdefinierte Regeln für Web Application Firewall v2) |Juni 2019 |
|Automatische Skalierung, Zonenredundanz, Unterstützung von statischen VIPs (allgemeine Verfügbarkeit) |Allgemeine Verfügbarkeit der v2-SKU, die automatische Skalierung, Zonenredundanz, Verbesserung der Leistung, statische VIPs, Schlüsseltresor und Neugenerierung von Headern unterstützt. Siehe die [Dokumentation zur automatischen Skalierung per Application Gateway](application-gateway-autoscaling-zone-redundant.md). |April 2019 |
|Schlüsseltresor-Integration |Application Gateway unterstützt jetzt die Integration mit Key Vault (in der öffentlichen Vorschau) für Serverzertifikate, die an HTTPS-fähige Listener angefügt sind. Siehe [TLS-Terminierung mit Key Vault-Zertifikaten](key-vault-certs.md). |April 2019 |
|CRUD/Umschreibungen von Headern     |Sie können die HTTP-Header jetzt umschreiben. Siehe [Tutorial: Erstellen eines Application Gateways und erneutes Generieren eines HTTP-Headers](tutorial-http-header-rewrite-powershell.md).|Dezember 2018|
|WAF-Konfiguration und Ausschlussliste     |Wir haben weitere Optionen hinzugefügt, die Sie beim Konfigurieren Ihrer WAF sowie bei der Verringerung falsch positiver Ergebnisse unterstützen. Weitere Informationen finden Sie unter [WAF-Anforderungsgrößenlimits und Ausschlusslisten](application-gateway-waf-configuration.md).|Dezember 2018|
|Automatische Skalierung, Zonenredundanz, Unterstützung von statischen VIPs      |Mit der v2 SKU wurden viele Verbesserungen eingeführt, z. B. automatische Skalierung, verbesserte Leistung und mehr. Weitere Informationen finden Sie unter [Was ist Azure Application Gateway?](overview.md).|September 2018|
|Verbindungsausgleich     |Der Verbindungsausgleich ermöglicht Ihnen das korrekte Entfernen von Mitgliedern aus Ihren Back-End-Pools. Weitere Informationen finden Sie unter [Verbindungsausgleich](features.md#connection-draining).|September 2018|
|Benutzerdefinierte Fehlerseiten     |Mit benutzerdefinierten Fehlerseiten können Sie eine Fehlerseite im Format Ihrer restlichen Websites erstellen. Informationen zur Aktivierung finden Sie unter [Erstellen von benutzerdefinierten Application Gateway-Fehlerseiten](custom-error.md).|September 2018|
|Verbesserungen für Metriken     |Dank verbesserter Metriken erhalten Sie einen besseren Einblick in Ihren Application Gateway-Zustand. Informationen zur Aktivierung von Metriken für Application Gateway finden Sie unter [Back-End-Integrität, Diagnoseprotokolle und Metriken für Application Gateway](application-gateway-diagnostics.md).|Juni 2018|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Application Gateway finden Sie unter [Was ist Azure Application Gateway?](overview.md).
