---
title: Automatische Skalierung und zonenredundantes Application Gateway v2
description: In diesem Artikel werden die Standard_v2- und WAF_v2-SKU der Azure-Anwendung vorgestellt, die Autoskalierung und zonenredundante Features umfassen.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: victorh
ms.custom: fasttrack-edit
ms.openlocfilehash: 18bcd57c804746da5cff2efe8713616174fc794d
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83739480"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-v2"></a>Automatische Skalierung und zonenredundantes Application Gateway v2 

Application Gateway und Web Application Firewall (WAF) sind auch in einer Standard_v2- und WAF_v2-SKU verfügbar. Die v2-SKU bietet Leistungsverbesserungen und zusätzliche Unterstützung für wichtige neue Features wie automatische Skalierung und Zonenredundanz sowie Unterstützung für statische VIPs. Vorhandene Features in der Standard- and WAF-SKU werden in der neuen v2-SKU weiterhin unterstützt. Es gelten aber einige wenige Ausnahmen, die im Abschnitt mit dem [Vergleich](#differences-from-v1-sku) aufgeführt sind.

Die neue v2-SKU enthält die folgenden Verbesserungen:

- **Automatische Skalierung**: Für Application Gateway- oder WAF-Bereitstellungen unter der SKU mit automatischer Skalierung ist nun das Hochskalieren und Herunterskalieren je nach Veränderung der Netzwerkdatenverkehr-Auslastungsmuster möglich. Durch die automatische Skalierung entfällt auch die Notwendigkeit, während des Bereitstellens eine Bereitstellungsgröße oder eine Anzahl von Instanzen auszuwählen. Diese SKU bietet wahre Elastizität. In der Standard_v2- und WAF_v2-SKU kann Application Gateway sowohl mit fester Kapazität (automatische Skalierung deaktiviert) als auch mit aktivierter automatischer Skalierung betrieben werden. Der Modus mit fester Kapazität empfiehlt sich für Szenarien mit einheitlichen und vorhersagbaren Workloads. Der Modus mit automatischer Skalierung empfiehlt sich für Anwendungen, bei denen der Anwendungsdatenverkehr Schwankungen unterworfen ist.
- **Zonenredundanz**: Eine Application Gateway- oder WAF-Bereitstellung kann sich über mehrere Verfügbarkeitszonen erstrecken, sodass nicht mehr in jeder Zone mit einem Traffic Manager separate Application Gateway-Instanzen bereitgestellt werden müssen. Sie können eine einzelne Zone oder mehrere Zonen mit bereitgestellten Application Gateway-Instanzen auswählen, um für größere Stabilität gegenüber Zonenausfällen zu sorgen. Der Back-End-Pool für Anwendungen kann auf ähnliche Weise auf Verfügbarkeitszonen verteilt werden.

  Zonenredundanz ist nur dort verfügbar, wo auch Azure-Zonen verfügbar sind. In anderen Regionen werden alle anderen Features unterstützt. Weitere Informationen finden Sie unter [Was sind Verfügbarkeitszonen in Azure?](../availability-zones/az-overview.md).
- **Statische VIP**: Die Application Gateway v2-SKU unterstützt exklusiv den statischen VIP-Typ. Dadurch wird sichergestellt, dass die dem Application Gateway zugeordnete VIP während des Lebenszyklus der Bereitstellung unverändert bleibt, selbst nach einem Neustart.  Es gibt keine statische VIP in v1, daher müssen Sie für das Routing des Domänennamens an App Services über das Application Gateway die URL des Application Gateways anstelle der IP-Adresse verwenden.
- **Erneutes Generieren von Headern**: Application Gateway ermöglicht Ihnen das Hinzufügen, Entfernen oder Aktualisieren von HTTP-Anforderungs- und Antwortheadern mit dem v2-SKU. Weitere Informationen finden Sie unter [Erneutes Generieren von HTTP-Headern mit Application Gateway](rewrite-http-headers.md).
- **Key Vault-Integration**: Application Gateway v2 unterstützt die Integration in Key Vault für Serverzertifikate, die HTTPS-fähigen Listenern zugeordnet sind. Weitere Informationen finden Sie unter [TLS-Terminierung mit Key Vault-Zertifikaten](key-vault-certs.md).
- **Azure Kubernetes Service-Eingangscontroller**: Der Application Gateway v2-Eingangscontroller ermöglicht die Verwendung von Azure Application Gateway als Eingang für einen Azure Kubernetes Service (AKS), der als AKS-Cluster bezeichnet wird. Weitere Informationen finden Sie unter [Was ist der Application Gateway-Eingangscontroller?](ingress-controller-overview.md).
- **Leistungsverbesserungen**: Die v2-SKU bietet eine bis zu fünfmal bessere TLS-Auslagerungsleistung verglichen mit der Standard-/WAF-SKU.
- **Schnellere Bereitstellung und Aktualisierung**: Die v2-SKU ermöglicht verglichen mit der Standard/WAF-SKU eine schnellere Bereitstellung und Aktualisierung. Dies umfasst auch Änderungen an der WAF-Konfiguration.

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Unterstützte Regionen

Die Standard_v2- und WAF_v2-SKU ist in den folgenden Regionen verfügbar: USA, Norden-Mitte, USA, Süden-Mitte, USA, Westen, USA, Westen 2, USA, Osten, USA, Osten 2, USA, Mitte, Europa, Norden, Europa, Westen, Asien, Südosten, Frankreich, Mitte, Vereinigtes Königreich, Westen, Japan, Osten, Japan, Westen, Australien, Osten, Australien, Südosten, Brasilien, Süden, Kanada, Mitte, Kanada, Osten, Asien, Osten, Südkorea, Mitte, Südkorea, Süden, Vereinigtes Königreich, Süden, Indien, Mitte, Indien, Westen, Indien, Süden.

## <a name="pricing"></a>Preise

Bei der v2-SKU basiert das Preismodell auf der Nutzung und ist nicht mehr an die Anzahl oder Größe von Instanzen gebunden. Der Preis der v2-SKU setzt sich aus zwei Komponenten zusammen:

- **Festpreis**: Dies ist der Preis pro Stunde (oder Teil einer Stunde) für die Bereitstellung eines Standard_v2- oder WAF_v2-Gateways. Beachten Sie, dass auch mit 0 zusätzlichen Mindestinstanzen weiterhin die Hochverfügbarkeit des Diensts gewährleistet wird, die grundsätzlich im Festpreis inbegriffen ist.
- **Preis nach Kapazitätseinheit**: Dies sind die nutzungsbasierten Kosten, die zusätzlich zu den Fixkosten berechnet werden. Die Gebühr für Kapazitätseinheiten wird ebenfalls für volle oder teilweise Stunden berechnet. Die Kapazitätseinheit setzt sich aus drei Größen zusammen: Compute-Einheit, permanente Verbindungen und Durchsatz. Die Compute-Einheit ein Maß für die genutzte Prozessorkapazität. Faktoren, die sich auf die Compute-Einheit auswirken, sind TLS-Verbindungen/Sekunde, URL-Rewrite-Berechnungen und Verarbeitung von WAF-Regeln. Permanente Verbindung ist ein Maß für eingerichtete TCP-Verbindungen mit Application Gateway in einem bestimmten Abrechnungsintervall. Der Durchsatz ist die durchschnittliche Anzahl von Megabits pro Sekunde, die vom System in einem bestimmten Abrechnungsintervall verarbeitet werden.  Die Abrechnung erfolgt für alles, was die reservierte Instanzanzahl übersteigt, auf Kapazitätseinheitsebene.

Jede Kapazitätseinheit setzt sich maximal zusammen aus: 1 Compute-Einheit oder 2500 permanente Verbindungen oder 2,22 MBit/s Durchsatz.

Leitfaden für Compute-Einheit:

- **Standard_v2**: Jede Compute-Einheit kann ungefähr 50 Verbindungen pro Sekunde mit TLS-Zertifikat mit RSA-Schlüssel (2048 Bit) unterhalten.
- **WAF_v2**: Jede Compute-Einheit kann etwa 10 gleichzeitige Anforderungen pro Sekunde für eine 70/30%-Kombination des Datenverkehrs mit 70 % Anforderungen, weniger als 2 KB GET/POST und höher unterstützen. Die WAF-Leistung ist derzeit nicht von der Antwortgröße betroffen.

> [!NOTE]
> Jede Instanz kann derzeit etwa 10 Kapazitätseinheiten unterstützen.
> Die Anzahl der Anforderungen, die eine Compute-Einheit verarbeiten kann, hängt von verschiedenen Kriterien wie Größe des TLS-Zertifikatschlüssels, Schlüsselaustauschalgorithmus, erneutes Generieren von Headern und – im Fall von WAF – Größe der eingehenden Anforderung ab. Es wird empfohlen, Anwendungstests auszuführen, um die Anforderungsrate pro Compute-Einheit zu ermitteln. Sowohl Kapazitätseinheit als auch Compute-Einheit werden vor Beginn der Abrechnung als Metrik zur Verfügung gestellt.

Die folgende Tabelle enthält die Beispielpreise und dient lediglich zur Veranschaulichung.

**Preise in der Region „USA, Osten“** :

|              SKU-Name                             | Festpreis ($/Std)  | Preis nach Kapazitätseinheit ($/KE-Std)   |
| ------------------------------------------------- | ------------------- | ------------------------------- |
| Standard_v2                                       |    0,20             | 0,0080                          |
| WAF_v2                                            |    0,36             | 0,0144                          |

Weitere Preisinformationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/application-gateway/). 

**Beispiel 1**

Es wird ein Application Gateway Standard_v2 ohne automatische Skalierung im manuellen Skalierungsmodus mit fester Kapazität von fünf Instanzen bereitgestellt.

Festpreis = 744 (Stunden) * 0,20 $ = 148,8 $ <br>
Kapazitätseinheiten = 744 (Stunden) * 10 Kapazitätseinheiten pro Instanz * 5 Instanzen * 0,008 $ pro Kapazitätseinheitsstunde = 297,6 $

Gesamtpreis = 148,8 $ + 297,6 $ = 446,4 $

**Beispiel 2**

Es wird ein Application Gateway Standard_v2 für einen Monat mit null Mindestinstanzen bereitgestellt, und dieses empfängt während dieser Zeit 25 neue TLS-Verbindungen/Sekunde mit einer Datenübertragung von durchschnittlich 8,88 MBit/s. Bei kurzlebigen Verbindungen ist der Preis wie folgt:

Festpreis = 744 (Stunden) * 0,20 $ = 148,8 $

Preis nach Kapazitätseinheit = 744 (Stunden) * Max. (25/50 Compute-Einheit für Verbindungen/Sekunde, 8,88/2,22 Kapazitätseinheit für Durchsatz) * 0,008 $ = 744 * 4 * 0,008 = 23,81 $

Gesamtpreis = 148,8 $ + 23,81 $ = 172,61 $

Wie Sie sehen können, werden Ihnen lediglich vier Kapazitätseinheiten berechnet, nicht die gesamte Instanz. 

> [!NOTE]
> Die Max-Funktion gibt den größten Wert in einem Wertepaar zurück.


**Beispiel 3**

Es wird ein Application Gateway Standard_v2 für einen Monat mit mindestens fünf Instanzen bereitgestellt. Unter der Voraussetzung, dass kein Datenverkehr besteht und dass die Verbindungen kurzlebig sind, ist der Preis wie folgt:

Festpreis = 744 (Stunden) * 0,20 $ = 148,8 $

Preis nach Kapazitätseinheit = 744 (Stunden) * Max. (0/50 Compute-Einheit für Verbindungen/Sekunde, 0/2,22 Kapazitätseinheit für Durchsatz) * 0,008 $ = 744 * 50 * 0,008 = 297,60 $

Gesamtpreis = 148,80 $ + 297,60 $ = 446,40 $

In diesem Fall werden Ihnen die gesamten fünf Instanzen in Rechnung gestellt, auch wenn kein Datenverkehr vorhanden ist.

**Beispiel 4**

Es wird ein Application Gateway Standard_v2 für einen Monat mit mindestens fünf Instanzen bereitgestellt, aber diesmal gibt es eine durchschnittliche Datenübertragung von 125 MBit/s und 25 neue TLS-Verbindungen pro Sekunde. Unter der Voraussetzung, dass kein Datenverkehr besteht und dass die Verbindungen kurzlebig sind, ist der Preis wie folgt:

Festpreis = 744 (Stunden) * 0,20 $ = 148,8 $

Preis nach Kapazitätseinheit = 744 (Stunden) * Max. (25/50 Compute-Einheit für Verbindungen/Sekunde, 125/2,22 Kapazitätseinheit für Durchsatz) * 0,008 $ = 744 * 57 * 0,008 = 339,26 $

Gesamtpreis = 148,80 $ + 339,26 $ = 488,06 $

In diesem Fall werden Ihnen die ganzen fünf Instanzen zuzüglich sieben Kapazitätseinheiten (das sind 7/10 einer Instanz) in Rechnung gestellt.  

**Beispiel 5**

Es wird ein Application Gateway WAF_v2 für einen Monat bereitgestellt. Während dieses Zeitraums empfängt es 25 neue TLS-Verbindungen/Sekunde mit einer Datenübertragung von durchschnittlich 8,88 MBit/s und gibt 80 Anforderungen pro Sekunde aus. Bei kurzlebigen Verbindungen und unter der Voraussetzung, dass die Berechnung der Compute-Einheiten für die Anwendung 10 RPS pro Compute-Einheit unterstützt, ist der Preis wie folgt:

Festpreis = 744 (Stunden) * 0,36 $ = 267,84 $

Preis nach Kapazitätseinheit = 744 (Stunden) * Max. (Compute-Einheit Max. (25/50 für Verbindungen/Sekunde, 80/10 WAF-RPS), 8,88/2,22 Kapazitätseinheit für Durchsatz) * 0,0144 $ = 744 * 8 * 0,0144 = 85,71 $

Gesamtpreis = 267,84 $ + 85,71 $ = 353,55 $

> [!NOTE]
> Die Max-Funktion gibt den größten Wert in einem Wertepaar zurück.

## <a name="scaling-application-gateway-and-waf-v2"></a>Skalierung von Application Gateway und WAF v2

Application Gateway und WAF können für die Skalierung in zwei Modi konfiguriert werden:

- **Automatische Skalierung**: Bei aktivierter automatischer Skalierung können Application Gateway und WAF v2-SKUs basierend auf den Datenverkehrsanforderungen der Anwendung hoch- oder herunterskalieren. Dieser Modus bietet bessere Elastizität für Ihre Anwendung und Größe oder Anzahl der Instanzen von Application Gateway müssen nicht geschätzt werden. Außerdem ermöglicht dieser Modus Kosteneinsparungen, weil das Gateway nicht mit bereitgestellter Spitzenkapazität für die erwartete maximale Auslastung ausgeführt werden muss. Sie müssen eine Mindestanzahl und optional eine Höchstanzahl von Instanzen angeben. Durch eine Mindestkapazität wird sichergestellt, dass Application Gateway und WAF v2 nicht unter die angegebene Mindestanzahl von Instanzen abfallen, selbst wenn kein Datenverkehr vorhanden ist. Jede Instanz entspricht in etwa zehn zusätzlichen reservierten Kapazitätseinheiten. Bei 0 wird keine Kapazität reserviert. Dieser Wert dient ausschließlich zur automatischen Skalierung. Sie können optional auch eine maximale Anzahl von Instanzen angeben, durch die sichergestellt wird, dass Application Gateway nicht über die angegebene Anzahl von Instanzen hinaus skaliert. Ihnen wird nur die vom Gateway bereitgestellte Menge an Datenverkehr in Rechnung gestellt. Die Anzahl der Instanzen kann zwischen 0 und 125 liegen. Wenn kein Wert angegeben ist, liegt der Standardwert für die maximale Anzahl von Instanzen bei 20.
- **Manuell**: Sie können alternativ auch den manuellen Modus auswählen, in dem das Gateway nicht automatisch skaliert wird. Wenn in diesem Modus mehr Datenverkehr vorliegt, als von Application Gateway oder WAF verarbeitet werden kann, kann dies zu einem Datenverlust führen. Beim manuellen Modus ist die Angabe der Anzahl von Instanzen obligatorisch. Die Anzahl der Instanzen kann zwischen 1 und 125 variieren.

## <a name="autoscaling-and-high-availability"></a>Automatische Skalierung und Hochverfügbarkeit

Azure Application Gateway-Instanzen werden immer hochverfügbar bereitgestellt. Der Dienst setzt sich aus mehreren Instanzen zusammen, die entweder gemäß der Konfiguration (bei deaktivierter automatischer Skalierung) oder unter Berücksichtigung der Anwendungslast (bei aktivierter automatischer Skalierung) erstellt werden. Beachten Sie, dass aus Benutzersicht nicht unbedingt ein Einblick in die einzelnen Instanzen, sondern lediglich in den Application Gateway-Dienst als Ganzes möglich ist. Sollte in einer bestimmten Instanz ein Problem auftreten und die Instanz nicht mehr funktionieren, wird von Azure Application Gateway auf transparente Weise eine neue Instanz erstellt.

Beachten Sie, dass der Dienst auch dann noch hochverfügbar ist, wenn Sie die automatische Skalierung mit null Mindestinstanzen konfigurieren. Dies ist immer im Festpreis enthalten.

Die Erstellung einer neuen Instanz kann jedoch etwas dauern (etwa sechs oder sieben Minuten). Zur Vermeidung dieser Downtime können Sie daher die Mindestanzahl von Instanzen auf „2“ festlegen (idealerweise mit Verfügbarkeitszonenunterstützung). Dadurch sind im Normalfall mindestens zwei Instanzen in Azure Application Gateway vorhanden. Sollte nun in einer dieser Instanzen ein Problem auftreten, wird während der Erstellung einer neuen Instanz von der anderen Instanz versucht, den Datenverkehr zu bewältigen. Beachten Sie, dass von einer Azure Application Gateway-Instanz etwa zehn Kapazitätseinheiten unterstützt werden können. Daher empfiehlt es sich gegebenenfalls, die Einstellung für die Mindestanzahl von Instanzen für die automatische Skalierung abhängig von Ihrer üblichen Datenverkehrsmenge auf einen höheren Wert als „2“ festzulegen.

## <a name="feature-comparison-between-v1-sku-and-v2-sku"></a>Funktionsvergleich zwischen v1 SKU und v2 SKU

In der folgenden Tabelle werden die Features der einzelnen SKUs gegenübergestellt.

|                                                   | v1 SKU   | v2 SKU   |
| ------------------------------------------------- | -------- | -------- |
| Automatische Skalierung                                       |          | &#x2713; |
| Zonenredundanz                                   |          | &#x2713; |
| Statische VIP                                        |          | &#x2713; |
| AKS-Eingangscontroller (Azure Kubernetes Service) |          | &#x2713; |
| Azure-Schlüsseltresor-Integration                       |          | &#x2713; |
| Erneutes Generieren von HTTP(S)-Headern                           |          | &#x2713; |
| URL-basiertes Routing                                 | &#x2713; | &#x2713; |
| Hosten mehrerer Websites                             | &#x2713; | &#x2713; |
| Umleitung des Datenverkehrs                               | &#x2713; | &#x2713; |
| Web Application Firewall (WAF)                    | &#x2713; | &#x2713; |
| Benutzerdefinierte WAF-Regeln                                  |          | &#x2713; |
| Beendigung mit TLS (Transport Layer Security)/SSL (Secure Sockets Layer)            | &#x2713; | &#x2713; |
| End-to-End-TLS-Verschlüsselung                         | &#x2713; | &#x2713; |
| Sitzungsaffinität                                  | &#x2713; | &#x2713; |
| Benutzerdefinierte Fehlerseiten                                | &#x2713; | &#x2713; |
| WebSocket-Unterstützung                                 | &#x2713; | &#x2713; |
| HTTP/2-Unterstützung                                    | &#x2713; | &#x2713; |
| Verbindungsausgleich                               | &#x2713; | &#x2713; |

> [!NOTE]
> Die v2-SKU mit automatischer Skalierung unterstützt nun [Standardintegritätstests](application-gateway-probe-overview.md#default-health-probe), um die Integrität aller Ressourcen im Back-End-Pool automatisch zu überwachen und solche Back-End-Elemente zu markieren, die als fehlerhaft gelten. Der Standardintegritätstest wird automatisch für Back-Ends konfiguriert, die keine benutzerdefinierte Testkonfiguration aufweisen. Weitere Informationen finden Sie unter [Systemüberwachung des Application Gateways](application-gateway-probe-overview.md).

## <a name="differences-from-v1-sku"></a>Unterschiede zur v1-SKU

In diesem Abschnitt werden die Features und Einschränkungen der v2-SKU beschrieben, die sich von denen der v1-SKU unterscheiden.

|Unterschied|Details|
|--|--|
|Authentifizierungszertifikat|Wird nicht unterstützt.<br>Weitere Informationen finden Sie unter [Übersicht über End-to-End-TLS mit Application Gateway](ssl-overview.md#end-to-end-tls-with-the-v2-sku).|
|Mischen von Standard_v2 und Standardanwendungsgateway in demselben Subnetz|Nicht unterstützt|
|Benutzerdefinierte Route im Application Gateway-Subnetz|Unterstützt (spezifische Szenarien). In der Vorschauversion.<br> Weitere Informationen zu unterstützten Szenarien finden Sie unter [Application Gateway-Konfiguration: Übersicht](configuration-overview.md#user-defined-routes-supported-on-the-application-gateway-subnet).|
|NSG für Eingangsportbereich| - 65200 bis 65535 für Standard_v2-SKU<br>- 65503 bis 65534 für Standard-SKU.<br>Weitere Informationen finden Sie in den [häufig gestellten Fragen](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Leistungsprotokolle in Azure-Diagnose|Wird nicht unterstützt.<br>Es sollten Azure-Metriken verwendet werden.|
|Abrechnung|Die Abrechnung soll planmäßig am 1. Juli 2019 beginnen.|
|FIPS-Modus|Diese werden derzeit nicht unterstützt.|
|Reiner ILB-Modus|Dies wird derzeit nicht unterstützt. Öffentlicher und ILB-Modus kombiniert werden unterstützt.|
|Integration mit Net Watcher|Wird nicht unterstützt.|
|Integrieren von Azure Security Center|Noch nicht verfügbar.

## <a name="migrate-from-v1-to-v2"></a>Migrieren von v1 zu v2

Im PowerShell-Katalog ist ein Azure PowerShell-Skript verfügbar, mit dessen Hilfe Sie Ihr v1-Application Gateway / Ihre v1-WAF zur v2-SKU mit automatischer Skalierung migrieren können. Mithilfe dieses Skript können Sie die Konfiguration von Ihrem v1-Gateway kopieren. Die Datenverkehrsmigration liegt weiterhin in Ihrer Verantwortung. Weitere Informationen finden Sie unter [Migrieren von Azure Application Gateway und Web Application Firewall von v1 zu v2](migrate-v1-v2.md).

## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart: Weiterleiten von Webdatenverkehr per Azure Application Gateway – Azure-Portal](quick-create-portal.md)
- [Erstellen Sie mit Azure PowerShell ein zonenredundantes Application Gateway mit automatischer Skalierung und einer reservierten virtuellen IP-Adresse.](tutorial-autoscale-ps.md)
- Erfahren Sie mehr über [Application Gateway](overview.md).
- Erfahren Sie mehr über [Azure Firewall](../firewall/overview.md).
