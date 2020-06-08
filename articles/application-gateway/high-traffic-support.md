---
title: Unterstützung von hohem Datenverkehrsaufkommen für Application Gateway
description: Dieser Artikel enthält Anleitungen zum Konfigurieren von Azure Application Gateway zur Unterstützung von Szenarios mit hohem Netzwerkdatenverkehrsaufkommen.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: caya
ms.openlocfilehash: 56622ee2c014bd8dbca7c61ec00b927c56f63a40
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83740398"
---
# <a name="application-gateway-high-traffic-support"></a>Unterstützung von hohem Datenverkehr für Application Gateway

>[!NOTE]
> In diesem Artikel werden einige empfohlene Richtlinien beschrieben, die Ihnen bei der Einrichtung Ihres Application Gateway für den Umgang mit zusätzlichem Datenverkehr helfen, der möglicherweise aufgrund der COVID-19-Krise auftreten könnte.

Sie können Application Gateway mit der Web Application Firewall (WAF) für eine skalierbare und sichere Methode zum Verwalten von Datenverkehr an Ihre Webanwendungen verwenden.

Mithilfe der folgenden Vorschläge können Sie Application Gateway mit WAF einrichten, um zusätzlichen Datenverkehr zu verarbeiten.

## <a name="use-the-v2-sku-over-v1-for-its-autoscaling-capabilities-and-performance-benefits"></a>Verwenden Sie die v2-SKU anstelle von v1 wegen ihrer automatischen Skalierungsfunktionen und Leistungsvorteile.
Die v2-SKU bietet automatische Skalierung, um sicherzustellen, dass Ihr Application Gateway bei steigendem Datenverkehr zentral hochskaliert werden kann. Außerdem bietet sie weitere bedeutende Leistungsvorteile, z. B. eine 5-fach bessere TLS-Abladeleistung, schnellere Bereitstellungs- und Aktualisierungszeiten, Zonenredundanz und mehr im Vergleich zu v1. Weitere Informationen finden Sie in der [v2-Dokumentation](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). 

## <a name="set-maximum-instance-count-to-the-maximum-possible-125"></a>Festlegen der maximalen Anzahl von Instanzen auf den maximal möglichen Wert (125)
 
Angenommen, Sie haben eine Application Gateway v2-SKU, dann ermöglicht das Festlegen der maximalen Anzahl von Instanzen auf den maximal möglichen Wert von 125, dass das Application Gateway nach Bedarf horizontal hochskaliert werden kann. Dadurch können Sie die mögliche Zunahme des Datenverkehrs an Ihre Anwendungen bewältigen. Ihnen werden nur die von Ihnen verwendeten Kapazitätseinheiten (Capacity Units, CUs) in Rechnung gestellt.  

## <a name="set-your-minimum-instance-count-based-on-your-average-cu-usage"></a>Festlegen Ihrer Mindestanzahl von Instanzen auf Grundlage Ihrer durchschnittlichen CU-Nutzung

Angenommen, Sie verfügen über eine Application Gateway v2-SKU, dann benötigt die automatische Skalierung sechs bis sieben Minuten für das horizontale Hochskalieren. Bei einer höheren Mindestanzahl von Instanzen kann das Application Gateway Ihren Datenverkehr besser verarbeiten, wenn die Auslastung zunimmt, da eine Spitze im Datenverkehr keinen automatischen Skalierungsvorgang erfordert.  

## <a name="alert-if-a-certain-metric-surpasses-75-of-average-cu-utilization"></a>Warnung, wenn eine bestimmte Metrik 75 % der durchschnittlichen CU-Auslastung überschreitet 
Ausführliche Erläuterungen zu unseren Metriken und anderen exemplarischen Vorgehensweisen finden Sie in der [Dokumentation der Application Gateway-Metriken](https://docs.microsoft.com/azure/application-gateway/application-gateway-metrics#metrics-visualization). 

### <a name="example-setting-up-an-alert-on-75-of-average-cu-usage"></a>Beispiel: Einrichten einer Warnung bei 75 % der durchschnittlichen CU-Nutzung

Dieses Beispiel zeigt, wie Sie im Azure-Portal eine Warnung einrichten können, wenn 75 % der durchschnittlichen CU-Nutzung erreicht wird. 
1. Navigieren Sie zu Ihrem **Application Gateway**.
2. Wählen Sie im linken Bereich unter **Überwachung** die Option **Metriken** aus. 
3. Fügen Sie eine Metrik für **Durchschnittliche aktuelle Compute-Einheiten** hinzu. 
![Einrichten der WAF-Metrik](./media/application-gateway-covid-guidelines/waf-setup-metrics.png)
4. Wenn Sie Ihre Mindestanzahl von Instanzen auf Ihre durchschnittliche CU-Nutzung festgelegt haben, fahren Sie fort, und legen Sie eine Warnung fest, wenn 75 % ihrer Mindestanzahl von Instanzen verwendet werden. Wenn Ihre durchschnittliche Auslastung z. B. 10 CUs beträgt, legen Sie eine Warnung ab 7,5 CUs fest. Dadurch wird eine Warnung angezeigt, wenn die Nutzung zunimmt, und Sie erhalten Zeit, zu reagieren. Sie können den Mindestwert erhöhen, wenn Sie der Ansicht sind, dass der Datenverkehr länger so bleibt, um Sie darauf aufmerksam zu machen, dass der Datenverkehr eventuell steigen wird. 
![Einrichten einer WAF-Warnung](./media/application-gateway-covid-guidelines/waf-setup-monitoring-alert.png)

> [!NOTE]
> Sie können festlegen, dass die Warnung bei einem niedrigeren oder höheren CU-Auslastungsprozentsatz auftritt, je nachdem, wie empfindlich Sie auf potenzielle Datenverkehrsspitzen reagieren möchten.

## <a name="set-up-waf-with-geofiltering-and-bot-protection-to-stop-attacks"></a>Einrichten von WAF mit Geofilterung und Bot-Schutz, um Angriffe zu verhindern
Wenn Sie vor Ihrer Anwendung noch eine zusätzliche Sicherheitsschicht benötigen, verwenden Sie die Application Gateway WAF_v2-SKU für WAF-Funktionen. Sie können die v2-SKU so konfigurieren, dass nur der Zugriff auf Ihre Anwendungen aus einem oder mehreren bestimmten Ländern bzw. aus einer oder mehreren Regionen zugelassen wird. Sie richten eine benutzerdefinierte WAF-Regel ein, um Datenverkehr basierend auf dem Geostandort explizit zuzulassen oder zu blockieren. Weitere Informationen finden Sie unter [Geofilterung benutzerdefinierter Regeln](https://docs.microsoft.com/azure/web-application-firewall/ag/geomatch-custom-rules) und [Konfigurieren von benutzerdefinierten Regeln in Application Gateway WAF_v2-SKU mit PowerShell](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

Aktivieren Sie den Bot-Schutz, um bekannte bösartige Bots zu blockieren. Dadurch sollte die Menge des Datenverkehrs, der zu Ihrer Anwendung gelangt, reduziert werden. Weitere Informationen finden Sie unter [Bot-Schutz mit Einrichtungsanweisungen](https://docs.microsoft.com/azure/web-application-firewall/ag/configure-waf-custom-rules).

## <a name="turn-on-diagnostics-on-application-gateway-and-waf"></a>Aktivieren der Diagnose für Application Gateway und WAF

Diagnoseprotokolle ermöglichen es Ihnen, Firewallprotokolle, Leistungsprotokolle und Zugriffsprotokolle anzuzeigen. Sie können diese Protokolle in Azure verwenden, um Application Gateways zu verwalten und Probleme zu behandeln. Weitere Informationen finden Sie in unserer [Diagnosedokumentation](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#diagnostic-logging). 

## <a name="set-up-an-tls-policy-for-extra-security"></a>Einrichten einer TLS-Richtlinie für zusätzliche Sicherheit
Stellen Sie sicher, dass Sie die neueste Version der TLS-Richtlinie verwenden ([AppGwSslPolicy20170401S](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview#appgwsslpolicy20170401s)). Diese erzwingt TLS 1.2 und stärkere Verschlüsselungen. Weitere Informationen finden Sie unter [Konfigurieren von TLS-Richtlinienversionen und Verschlüsselungssammlungen über PowerShell](https://docs.microsoft.com/azure/application-gateway/application-gateway-configure-ssl-policy-powershell).
