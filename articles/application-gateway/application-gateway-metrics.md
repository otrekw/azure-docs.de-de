---
title: Azure Monitor-Metriken für Application Gateway
description: Erfahren Sie, wie Sie mit Metriken die Leistung von Application Gateway überwachen.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 8/29/2019
ms.author: absha
ms.openlocfilehash: 12ecacf1266c0d8211f5928a933cfd4acf8c49f0
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551385"
---
# <a name="metrics-for-application-gateway"></a>Metriken für Application Gateway

Application Gateway veröffentlicht Datenpunkte, sogenannte Metriken, für die Leistung Ihrer Application Gateway- und Back-End-Instanzen bei [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview). Diese Metriken sind numerische Werte in einer geordneten Menge von Zeitreihendaten, die einen Aspekt Ihrer Application Gateway-Instanz zu einem bestimmten Zeitpunkt beschreiben. Wenn Anforderungen durch Application Gateway geleitet werden, werden die Metriken in 60-Sekunden-Intervallen gemessen und gesendet. Wenn keine Anforderungen durch Application Gateway oder keine Daten für eine Metrik geleitet werden, wird die Metrik nicht gemeldet. Weitere Informationen finden Sie unter [Azure Monitor-Metriken](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

## <a name="metrics-supported-by-application-gateway-v2-sku"></a>Von der Application Gateway V2-SKU unterstützte Metriken

### <a name="timing-metrics"></a>Zeitmetriken

Die folgenden Metriken bezüglich der zeitlichen Steuerung der Anforderung und der Antwort sind verfügbar. Durch Analysieren dieser Metriken für einen bestimmten Listener können Sie feststellen, ob die Verlangsamung der Anwendung auf das WAN, Application Gateway, das Netzwerk zwischen Application Gateway und der Back-End-Anwendung oder der Back-End-Anwendungsleistung zurückzuführen ist.

> [!NOTE]
>
> Wenn Application Gateway mehr als einen Listener enthält, filtern Sie immer nach der Dimension *Listener*, und vergleichen Sie dabei verschiedene Latenzmetriken, um aussagekräftige Rückschlüsse zu erhalten.

- **Client-RTT**

  Durchschnittliche Roundtripzeit zwischen Clients und Application Gateway. Diese Metrik gibt an, wie lange es dauert, Verbindungen herzustellen und Bestätigungen zurückzugeben. 

- **Application Gateway-Gesamtzeit**

  Durchschnittliche Zeit, bis eine Anforderung verarbeitet und die zugehörige Antwort gesendet wurde. Dies wird berechnet als durchschnittliches Intervall zwischen dem Zeitpunkt, zu dem Application Gateway das erste Byte einer HTTP-Anforderung empfängt, bis zu dem Zeitpunkt, zu dem der Vorgang zum Senden der Antwort abgeschlossen ist. Beachten Sie, dass dies in der Regel die Application Gateway-Verarbeitungszeit, die Zeit für das Durchlaufen des Netzwerks durch die Anforderungs- und Antwortpakete und die Zeit bis zur Antwort des Back-End-Servers einschließt.
  
Falls *Client-RTT* erheblich höher als die *Application Gateway-Gesamtzeit* ist, kann hieraus geschlossen werden, dass die vom Client beobachtete Latenz an der Netzwerkkonnektivität zwischen dem Client und Application Gateway liegt. Wenn beide Wartezeiten vergleichbar sind, könnte die hohe Wartezeit auf Folgendes zurückzuführen sein: Application Gateway, das Netzwerk zwischen Application Gateway und der Back-End-Anwendung oder die Back-End-Anwendungsleistung.

- **Antwortzeit für erstes Byte des Back-Ends**

  Das Zeitintervall zwischen dem Herstellen einer Verbindung mit dem Back-End-Server und dem Empfang des ersten Bytes des Antwortheaders und somit eine Abschätzung der Verarbeitungszeit des Back-End-Servers

- **Antwortzeit für letztes Byte des Back-Ends**

  Das Zeitintervall zwischen dem Herstellen einer Verbindung mit dem Back-End-Server und dem Empfang des letzten Bytes des Antworttexts
  
Wenn die *Application Gateway-Gesamtzeit* viel höher als die *Antwortzeit des letzten Back-End-Bytes* für einen bestimmten Listener ist, kann daraus abgeleitet werden, dass die hohe Latenz am Application Gateway liegen könnte. Wenn die beiden Metriken hingegen vergleichbar sind, kann das Problem entweder auf dem Netzwerk zwischen dem Application Gateway und der Back-End-Anwendung oder auf der Leistung der Back-End-Anwendung beruhen.

- **Verbindungszeit für das Back-End**

  Zeitaufwand für das Herstellen einer Verbindung mit einer Back-End-Anwendung. Im Fall von SSL umfasst dies die mit dem Handshake verbrachte Zeit. Beachten Sie, dass sich diese Metrik von den anderen Latenzmetriken unterscheidet, da diese nur die Verbindungszeit misst und deshalb nicht direkt mit den anderen Wartezeiten hinsichtlich des Betrags verglichen werden sollte. Wenn Sie jedoch das Muster der *Back-End-Verbindungszeit* mit dem Muster der anderen Wartezeiten vergleichen, kann dies darauf hindeuten, ob eine Erhöhung anderer Wartezeiten aufgrund einer Variation im Netzwerk zwischen dem Application Gatway und der Back-End-Anwendung abgeleitet werden könnte. 
  

### <a name="application-gateway-metrics"></a>Application Gateway-Metriken

Für Application Gateway werden folgende Metriken unterstützt:

- **Empfangene Bytes**

   Anzahl der von Application Gateway von den Clients empfangenen Bytes

- **Gesendete Bytes**

   Anzahl der von Application Gateway an die Clients gesendeten Bytes

- **Client-TLS-Protokoll**

   Anzahl von TLS- und Nicht-TLS-Anforderungen, die von dem Client initiiert wurden, der eine Verbindung mit Application Gateway hergestellt hat. Um die Verteilung des TLS-Protokolls anzuzeigen, filtern Sie nach dem Dimensions-TLS-Protokoll.

- **Aktuelle Kapazitätseinheiten**

   Anzahl der verbrauchten Kapazitätseinheiten. Kapazitätseinheiten geben die verbrauchsbasierten Kosten an, die zusätzlich zu den Fixkosten berechnet werden. Die Kapazitätseinheit setzt sich aus drei Größen zusammen: Compute-Einheit, permanente Verbindungen und Durchsatz. Jede Kapazitätseinheit setzt sich maximal zusammen aus: 1 Compute-Einheit oder 2500 permanente Verbindungen oder 2,22 MBit/s Durchsatz.

- **Aktuelle Compute-Einheiten**

   Menge der verbrauchten Prozessorkapazität. Faktoren, die sich auf die Compute-Einheit auswirken, sind TLS-Verbindungen/Sekunde, URL-Rewrite-Berechnungen und Verarbeitung von WAF-Regeln. 

- **Aktuelle Verbindungen**

   Anzahl von aktuellen Verbindungen mit Application Gateway

- **Anforderungsfehler**

   Anzahl von fehlerhaften Anforderungen über Application Gateway. Die Anzahl der Anforderungen kann weiter gefiltert werden, um die Anzahl für die einzelnen/spezifischen Kombinationen aus Back-End-Pools und HTTP-Einstellungen anzuzeigen.


- **Antwortstatus**

   Von Application Gateway zurückgegebener HTTP-Antwortstatus. Die Antwortstatuscode-Verteilung kann weiter kategorisiert werden, um Antworten in 2xx-, 3xx-, 4xx- und 5xx-Kategorien anzuzeigen.

- **Durchsatz**

   Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat

- **Anforderungen insgesamt**

   Anzahl von erfolgreichen Anforderungen über Application Gateway. Die Anzahl der Anforderungen kann weiter gefiltert werden, um die Anzahl für die einzelnen/spezifischen Kombinationen aus Back-End-Pools und HTTP-Einstellungen anzuzeigen.

- **Anzahl von übereinstimmenden Web Application Firewall-Regeln**

- **Anzahl von ausgelösten Web Application Firewall-Regeln**

### <a name="backend-metrics"></a>Back-End-Metriken

Für Application Gateway werden folgende Metriken unterstützt:

- **Back-End-Antwortstatus**

  Anzahl der von den Back-Ends zurückgegebenen HTTP-Antwortstatuscodes. Dies schließt nicht die von Application Gateway generierten Antwortcodes ein. Die Antwortstatuscode-Verteilung kann weiter kategorisiert werden, um Antworten in 2xx-, 3xx-, 4xx- und 5xx-Kategorien anzuzeigen.

- **Anzahl von fehlerfreien Hosts**

  Die Anzahl der Back-Ends, die im Integritätstest als fehlerfrei ermittelt wurden. Sie können auf Back-End-Pool-Basis filtern, um fehlerfreie/fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.

- **Anzahl von fehlerhaften Hosts**

  Die Anzahl der Back-Ends, die im Integritätstest als fehlerhaft ermittelt wurden. Sie können auf Back-End-Pool-Basis filtern, um fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.

## <a name="metrics-supported-by-application-gateway-v1-sku"></a>Von der Application Gateway V1-SKU unterstützte Metriken

### <a name="application-gateway-metrics"></a>Application Gateway-Metriken

Für Application Gateway werden folgende Metriken unterstützt:

- **CPU-Auslastung**

  Zeigt die Auslastung der CPUs an, die dem Application Gateway zugeordnet sind.  Unter normalen Bedingungen sollte die CPU-Auslastung regelmäßig nicht 90 % überschreiten, da dies zu Wartezeiten bei den Websites führen kann, die hinter dem Application Gateway gehostet werden, sowie die Clienterfahrung merklich stören kann. Sie können die CPU-Auslastung indirekt steuern oder verbessern, indem Sie die Konfiguration des Application Gateway durch Erhöhen der Anzahl der Instanzen ändern oder durch Verschieben zu einer größeren SKU-Größe – oder durch beides.

- **Aktuelle Verbindungen**

  Anzahl von aktuellen Verbindungen mit Application Gateway

- **Anforderungsfehler**

  Anzahl von fehlerhaften Anforderungen über Application Gateway. Die Anzahl der Anforderungen kann weiter gefiltert werden, um die Anzahl für die einzelnen/spezifischen Kombinationen aus Back-End-Pools und HTTP-Einstellungen anzuzeigen.

- **Antwortstatus**

  Von Application Gateway zurückgegebener HTTP-Antwortstatus. Die Antwortstatuscode-Verteilung kann weiter kategorisiert werden, um Antworten in 2xx-, 3xx-, 4xx- und 5xx-Kategorien anzuzeigen.

- **Durchsatz**

  Anzahl von Bytes pro Sekunde, die das Application Gateway bereitgestellt hat

- **Anforderungen insgesamt**

  Anzahl von erfolgreichen Anforderungen über Application Gateway. Die Anzahl der Anforderungen kann weiter gefiltert werden, um die Anzahl für die einzelnen/spezifischen Kombinationen aus Back-End-Pools und HTTP-Einstellungen anzuzeigen.

- **Anzahl von übereinstimmenden Web Application Firewall-Regeln**

- **Anzahl von ausgelösten Web Application Firewall-Regeln**

### <a name="backend-metrics"></a>Back-End-Metriken

Für Application Gateway werden folgende Metriken unterstützt:

- **Anzahl von fehlerfreien Hosts**

  Die Anzahl der Back-Ends, die im Integritätstest als fehlerfrei ermittelt wurden. Sie können auf Back-End-Pool-Basis filtern, um fehlerfreie/fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.

- **Anzahl von fehlerhaften Hosts**

  Die Anzahl der Back-Ends, die im Integritätstest als fehlerhaft ermittelt wurden. Sie können auf Back-End-Pool-Basis filtern, um fehlerhafte Hosts in einem bestimmten Back-End-Pool anzuzeigen.

## <a name="metrics-visualization"></a>Metrikvisualisierung

Navigieren Sie zu einem Anwendungsgateway, und wählen Sie unter **Überwachung** die Option **Metriken** aus. Um die verfügbaren Werte anzuzeigen, wählen Sie die Dropdownliste **METRIK** aus.

In der folgenden Abbildung sehen Sie ein Beispiel mit drei Metriken für die letzten 30 Minuten:

[![](media/application-gateway-diagnostics/figure5.png "Metric view")](media/application-gateway-diagnostics/figure5-lb.png#lightbox)

Eine aktuelle Liste mit Metriken finden Sie unter [Unterstützte Metriken von Azure Monitor](../azure-monitor/platform/metrics-supported.md).

### <a name="alert-rules-on-metrics"></a>Warnungsregeln für Metriken

Sie können Warnungsregeln basierend auf Metriken für eine Ressource starten. Eine Warnung kann beispielsweise einen Webhook aufrufen oder eine E-Mail an einen Administrator senden, wenn der Durchsatz des Anwendungsgateways für einen angegebenen Zeitraum oberhalb oder unterhalb eines Schwellenwerts bzw. genau auf einem Schwellenwert liegt.

Im folgenden Beispiel wird schrittweise die Erstellung einer Warnungsregel beschrieben, die eine E-Mail an einen Administrator sendet, nachdem ein Durchsatzschwellenwert verletzt wurde:

1. Wählen Sie **Metrikwarnung hinzufügen** aus, um die Seite **Regel hinzufügen** zu öffnen. Sie können diese Seite auch über die Seite mit den Metriken erreichen.

   ![Schaltfläche „Metrikwarnung hinzufügen“][6]

2. Füllen Sie auf der Seite **Regel hinzufügen** die Abschnitte für den Namen, die Bedingung und die Benachrichtigung aus, und wählen Sie **OK** aus.

   * Wählen Sie unter **Bedingung** einen der vier Werte aus: **Größer als**, **Größer oder gleich**, **Kleiner als** oder **Kleiner oder gleich**.

   * Wählen Sie unter **Zeitraum** einen Zeitraum zwischen fünf Minuten und sechs Stunden aus.

   * Wenn Sie **E-Mail-Besitzer, Mitwirkende und Leser** wählen, kann die E-Mail-Adresse dynamisch basierend auf den Benutzern festgelegt werden, die auf diese Ressource zugreifen können. Andernfalls können Sie im Feld **Weitere Administrator-E-Mail(s)** eine durch Kommas getrennte Liste mit Benutzern angeben.

   ![Seite „Regel hinzufügen“][7]

Wenn der Schwellenwert überschritten wird, trifft eine E-Mail ein, die in etwa wie in der folgenden Abbildung aussieht:

![E-Mail zu überschrittenem Schwellenwert][8]

Nach dem Erstellen einer Metrikwarnung wird eine Liste mit Warnungen angezeigt. Dies ist eine Übersicht über alle Warnungsregeln.

![Liste mit Warnungen und Regeln][9]

Weitere Informationen zu Warnungsbenachrichtigungen finden Sie unter [Empfangen von Warnungsbenachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Weitere Informationen zu Webhooks und deren Verwendung mit Warnungen finden Sie unter [Konfigurieren eines Webhooks für eine Azure-Metrikwarnung](../azure-monitor/platform/alerts-webhooks.md).

## <a name="next-steps"></a>Nächste Schritte

* Visualisieren von Indikator- und Ereignisprotokollen mit [Azure Monitor-Protokollen](../azure-monitor/insights/azure-networking-analytics.md)
* Blogbeitrag [Visualize your Azure activity log with Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Visualisieren von Azure-Aktivitätsprotokollen mit Power BI)
* Blogbeitrag [View and analyze Azure activity logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr)

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
