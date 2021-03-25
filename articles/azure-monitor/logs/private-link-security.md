---
title: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor
description: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor
author: noakup
ms.author: noakuper
ms.topic: conceptual
ms.date: 10/05/2020
ms.openlocfilehash: 65af5810152034fd7b6014041edd07835eebd194
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102101476"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-monitor"></a>Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor

Mit [Azure Private Link](../../private-link/private-link-overview.md) können Sie Azure-PaaS-Dienste über private Endpunkte sicher mit Ihrem virtuellen Netzwerk verknüpfen. Bei vielen Diensten richten Sie einfach für jede Ressource einen Endpunkt ein. Azure Monitor ist jedoch eine Zusammenstellung verschiedener miteinander verbundener Dienste, die zur Überwachung Ihrer Workloads ineinandergreifen. Daher haben wir eine Ressource mit dem Namen Azure Monitor Private Link Scope (AMPLS) erstellt. Mit AMPLS können Sie die Grenzen Ihres Überwachungsnetzwerks definieren und eine Verbindung mit Ihrem virtuellen Netzwerk herstellen. In diesem Artikel wird erläutert, wie Sie einen Azure Monitor Private Link-Bereich einrichten und verwenden.

## <a name="advantages"></a>Vorteile

Private Link bietet folgende Möglichkeiten:

- Herstellen einer privaten Verbindung mit Azure Monitor ohne öffentlichen Netzwerkzugriff
- Sicherstellen, dass der Zugriff auf Überwachungsdaten nur über autorisierte private Netzwerke erfolgt
- Verhindern von Datenexfiltration aus Ihren privaten Netzwerken durch Definieren bestimmter Azure Monitor-Ressourcen, die eine Verbindung über Ihren privaten Endpunkt herstellen
- Sichere Verbindung zwischen Ihrem privaten lokalen Netzwerk und Azure Monitor über ExpressRoute und Private Link
- Kapseln des gesamten Datenverkehrs innerhalb des Microsoft Azure-Backbonenetzwerks

Weitere Informationen finden Sie unter [Hauptvorteile von Private Link](../../private-link/private-link-overview.md#key-benefits).

## <a name="how-it-works"></a>Funktionsweise

Azure Monitor Private Link Scope (AMPLS) stellt Verbindungen privater Endpunkte (und der darin enthaltenen VNETs) mit einer oder mehreren Azure Monitor-Ressourcen her – Log Analytics-Arbeitsbereichen und Application Insights-Komponenten.

![Diagramm der grundlegenden Ressourcentopologie](./media/private-link-security/private-link-basic-topology.png)

* Der private Endpunkt in Ihrem VNET ermöglicht, Azure Monitor-Endpunkte über private IP-Adressen aus dem Pool Ihres Netzwerks zu erreichen, anstatt die öffentlichen IPs dieser Endpunkte zu verwenden. Auf diese Weise können Sie Ihre Azure Monitor-Ressourcen weiterhin verwenden, ohne das VNET für nicht benötigten ausgehenden Datenverkehr zu öffnen. 
* Der Datenverkehr vom privaten Endpunkt zu Ihren Azure Monitor-Ressourcen wird über das Microsoft Azure-Backbone und nicht über öffentliche Netzwerke weitergeleitet. 
* Sie können Ihre einzelnen Arbeitsbereiche oder Komponenten so konfigurieren, dass Erfassung und Abfragen von öffentlichen Netzwerken zugelassen oder verweigert werden. Dies bietet Schutz auf Ressourcenebene, sodass Sie den Datenverkehr zu bestimmten Ressourcen kontrollieren können.

> [!NOTE]
> Eine einzelne Azure Monitor-Ressource kann zu mehreren Azure Monitor Private Link-Bereichen gehören, aber Sie können ein einzelnes virtuelles Netzwerk nicht mit mehr als einem Bereich verbinden. 

## <a name="planning-your-private-link-setup"></a>Planen Ihres Private Link-Setups

Vor dem Einrichten Ihres Azure Monitor Private Link-Setups sollten Sie Ihre Netzwerktopologie und insbesondere Ihre DNS-Routingtopologie berücksichtigen. 

### <a name="the-issue-of-dns-overrides"></a>Das Problem der DNS-Außerkraftsetzungen
Einige Azure Monitor-Dienste verwenden globale Endpunkte. Dies bedeutet, dass sie Anforderungen aller Arbeitsbereiche und Komponenten erfüllen. Beispiele hierfür sind der Application Insights-Erfassungsendpunkt und der Abfrageendpunkt sowohl von Application Insights als auch Log Analytics.

Wenn Sie eine Private Link-Verbindung einrichten, wird das DNS so aktualisiert, dass Azure Monitor-Endpunkte den privaten IP-Adressen aus dem IP-Adressbereich Ihres VNET zugeordnet werden. Durch diese Änderung wird jede vorherige Zuordnung dieser Endpunkte außer Kraft gesetzt, was sinnvolle Auswirkungen haben kann, wie Sie im Folgenden erfahren werden. 

### <a name="azure-monitor-private-link-applies-to-all-azure-monitor-resources---its-all-or-nothing"></a>Azure Monitor Private Link wird auf alle Azure Monitor-Ressourcen angewandt – hier gilt alles oder nichts
Da einige Azure Monitor-Endpunkte global sind, ist es nicht möglich, eine Private Link-Verbindung mit einer bestimmten Komponente oder einem bestimmten Arbeitsbereich herzustellen. Wenn Sie stattdessen eine Private Link-Verbindung mit einer einzelnen Application Insights-Komponente herstellen, werden Ihre DNS-Einträge für **alle** Application Insights-Komponenten aktualisiert. Jeder Versuch, eine Komponente zu erfassen oder abzufragen, erfolgt über Private Link, und möglicherweise kann dabei ein Fehler auftreten. Entsprechend führt das Einrichten einer Private Link-Instanz für einen einzelnen Arbeitsbereich dazu, dass alle Log Analytics-Abfragen den Private Link-Abfrageendpunkt durchlaufen (aber nicht Erfassungsanforderungen, die arbeitsbereichsspezifische Endpunkte aufweisen).

![Diagramm der DNS-Außerkraftsetzungen in einem einzelnen VNET](./media/private-link-security/dns-overrides-single-vnet.png)

Dies gilt nicht nur für ein bestimmtes VNET, sondern für alle VNETs, die denselben DNS-Server nutzen (siehe [Das Problem der DNS-Außerkraftsetzungen](#the-issue-of-dns-overrides)). Beispielsweise werden an eine beliebige Application Insights-Komponente gerichtete Anforderungen zum Erfassen von Protokollen immer über die Private Link-Route gesendet. Komponenten, die nicht mit dem AMPLS verknüpft sind, bestehen die Private Link-Validierung nicht und können nicht passieren.

> [!NOTE]
> Zusammenfassend: Nachdem Sie eine Private Link-Verbindung mit einer einzelnen Ressource eingerichtet haben, wird sie auf alle Azure Monitor-Ressourcen in Ihrem Netzwerk angewandt – hier gilt alles oder nichts. Dies bedeutet, dass Sie alle Azure Monitor-Ressourcen in Ihrem Netzwerk oder keine Ihrer AMPLS-Instanz hinzufügen sollten.

### <a name="azure-monitor-private-link-applies-to-your-entire-network"></a>Azure Monitor Private Link gilt für Ihr gesamtes Netzwerk
Einige Netzwerke bestehen aus mehreren VNETs. Wenn die VNETs denselben DNS-Server verwenden, setzen sie sich gegenseitig ihre DNS-Zuordnungen außer Kraft und unterbrechen sich möglicherweise gegenseitig die Kommunikation mit Azure Monitor (siehe [Das Problem der DNS-Außerkraftsetzungen](#the-issue-of-dns-overrides)). Letztendlich kann nur das letzte VNET mit Azure Monitor kommunizieren, da Azure Monitor-Endpunkte vom DNS privaten IP-Adressen aus dem Bereich dieses VNET zugeordnet werden (der möglicherweise von anderen VNETs aus nicht erreichbar ist).

![Diagramm der DNS-Außerkraftsetzungen in mehreren VNETs](./media/private-link-security/dns-overrides-multiple-vnets.png)

Im obigen Diagramm stellt VNET 10.0.1.x zuerst eine Verbindung mit AMPLS1 her und ordnet IP-Adressen aus seinem Bereich die globalen Azure Monitor-Endpunkte zu. Später stellt VNET 10.0.2.x eine Verbindung mit AMPLS2 her und setzt mit IP-Adressen aus seinem Bereich die DNS-Zuordnung der *gleichen globalen Endpunkte* außer Kraft. Da zwischen diesen VNETs keine Peerbeziehung besteht, kann das erste VNET diese Endpunkte nun nicht erreichen.

> [!NOTE]
> Zusammenfassend: Das AMPLS-Setup wirkt sich auf alle Netzwerke aus, die dieselben DNS-Zonen verwenden. Um das gegenseitige Außerkraftsetzen der DNS-Endpunktzuordnungen zu vermeiden, sollte ein einzelner privater Endpunkt in einem Peeringnetzwerk (z. B. einem Hub-VNET) eingerichtet oder die Netzwerke auf der DNS-Ebene getrennt werden (z. B. durch ausschließliche Verwendung von DNS-Weiterleitungen oder separaten DNS-Servern).

### <a name="hub-spoke-networks"></a>Hub-Spoke-Netzwerke
Hub-Spoke-Topologien können das Problem von DNS-Außerkraftsetzungen durch Einrichtung einer Private Link-Instanz auf dem Hub-VNET (Haupt-VNET), anstatt eine Private Link-Instanz für jedes VNET separat einzurichten, vermeiden. Dieses Setup ist insbesondere dann sinnvoll, wenn die von den Spoke-VNETs verwendeten Azure Monitor-Ressourcen gemeinsam genutzt werden. 

![Hub-and-Spoke: privater Endpunkt](./media/private-link-security/hub-and-spoke-with-single-private-endpoint.png)

> [!NOTE]
> Möglicherweise bevorzugen Sie absichtlich separate Private Link-Instanzen für Ihre Spoke-VNETs, um z. B. jedem VNET Zugriff auf eine begrenzte Anzahl von Überwachungsressourcen zu gewähren. In solchen Fällen können Sie einen dedizierten privaten Endpunkt und eine AMPLS-Instanz für jedes VNET erstellen, müssen jedoch auch überprüfen, ob sie nicht die gleichen DNS-Zonen gemeinsam verwenden, um DNS-Außerkraftsetzungen zu vermeiden.


### <a name="consider-limits"></a>Beachten von Einschränkungen

Wie unter [Einschränkungen](#restrictions-and-limitations) aufgeführt, gelten für das AMPLS-Objekt eine Reihe von Grenzwerten, die in der folgenden Topologie angezeigt werden:
* Jedes VNET stellt eine Verbindung zu **einem einzigen** AMPLS-Objekt her.
* AMPLS B ist mit privaten Endpunkten von zwei VNETs (VNet2 und VNet3) verbunden und verwendet damit zwei von zehn der möglichen Verbindungen mit privaten Endpunkten.
* AMPLS A stellt eine Verbindung mit zwei Arbeitsbereichen und einer Application Insights-Komponente her und verwendet damit drei von 50 der möglichen Verbindungen mit Azure Monitor-Ressourcen.
* Workspace2 stellt eine Verbindung mit AMPLS A und AMPLS B her und verwendet damit zwei von fünf der möglichen AMPLS-Verbindungen.

![Diagramm der AMPLS-Einschränkungen](./media/private-link-security/ampls-limits.png)


## <a name="example-connection"></a>Beispiel für eine Verbindung

Erstellen Sie als Erstes eine Ressource für einen Azure Monitor Private Link-Bereich.

1. Wechseln Sie im Azure-Portal zu **Ressource erstellen**, und suchen Sie nach **Azure Monitor Private Link-Bereich**.

   ![Suchen des Azure Monitor-Private Link-Bereichs](./media/private-link-security/ampls-find-1c.png)

2. Klicken Sie auf **Erstellen**.
3. Wählen Sie ein Abonnement und eine Ressourcengruppe aus.
4. Benennen Sie den Azure Monitor Private Link-Bereich. Es empfiehlt sich, einen aussagekräftigen Namen wie „AppServerProdTelem“ zu verwenden.
5. Klicken Sie auf **Überprüfen + erstellen**. 

   ![Erstellen des Azure Monitor-Private Link-Bereichs](./media/private-link-security/ampls-create-1d.png)

6. Warten Sie die Überprüfung ab, und wählen Sie dann **Erstellen** aus.

### <a name="connect-azure-monitor-resources"></a>Verbinden von Azure Monitor-Ressourcen

Stellen Sie eine Verbindung zwischen Azure Monitor-Ressourcen (Log Analytics-Arbeitsbereiche und Application Insights-Komponenten) und Ihrem Azure Monitor-Private Link-Bereich her.

1. Wählen Sie in Ihrem Azure Monitor Private Link-Bereich im Menü links **Azure Monitor-Ressourcen** aus. Wählen Sie die Schaltfläche **Hinzufügen** aus.
2. Fügen Sie den Arbeitsbereich oder die Komponente hinzu. Durch Auswahl der Schaltfläche **Hinzufügen** wird ein Dialogfeld geöffnet, in dem Sie Azure Monitor-Ressourcen auswählen können. Sie können Ihre Abonnements und Ressourcengruppen durchsuchen oder zum Filtern den entsprechenden Namen eingeben. Wählen Sie den Arbeitsbereich bzw. die Komponente und dann **Anwenden** aus, um die entsprechende Ressource dem Bereich hinzuzufügen.

    ![Screenshot der Benutzeroberfläche zur Auswahl eines Bereichs](./media/private-link-security/ampls-select-2.png)

> [!NOTE]
> Das Löschen von Azure Monitor-Ressourcen erfordert, dass Sie diese zunächst von allen AMPLS-Objekten trennen, mit denen sie verbunden sind. Es ist nicht möglich, Ressourcen zu löschen, die mit einem AMPLS verbunden sind.

### <a name="connect-to-a-private-endpoint"></a>Herstellen einer Verbindung mit einem privaten Endpunkt

Nachdem Sie Ihre Ressource mit dem Azure Monitor Private Link-Bereich verbunden haben, erstellen Sie jetzt einen privaten Endpunkt für die Verbindung mit dem Netzwerk. Diesen Schritt können Sie im [Private Link-Center im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints) oder – wie im gezeigten Beispiel – im Azure Monitor Private Link-Bereich ausführen.

1. Wählen Sie in Ihrer Bereichsressource im Ressourcenmenü links **Private Endpunktverbindungen** aus. Wählen Sie **Privater Endpunkt** aus, um den Prozess zum Erstellen des Endpunkts zu starten. Sie können hier auch Verbindungen genehmigen, die im Private Link-Center gestartet wurden, indem Sie die entsprechende Verbindung und dann **Genehmigen** auswählen.

    ![Screenshot der Benutzeroberfläche für Verbindungen mit privaten Endpunkten](./media/private-link-security/ampls-select-private-endpoint-connect-3.png)

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Namen des Endpunkts sowie die Region aus, in der sich der Endpunkt befinden soll. Die Region muss mit der Region des VNET übereinstimmen, mit dem Sie die Verbindung herstellen.

3. Klicken Sie auf **Weiter: Ressource** aus. 

4. Gehen Sie auf dem Bildschirm „Ressource“ folgendermaßen vor:

   a. Wählen Sie das **Abonnement** aus, das Ihre Ressource für den Azure Monitor Private Link-Bereich enthält. 

   b. Wählen Sie als **Ressourcentyp** die Option **Microsoft.insights/privateLinkScopes** aus. 

   c. Wählen Sie aus der Dropdownliste **Ressource** den Private Link-Bereich aus, den Sie weiter oben erstellt haben. 

   d. Klicken Sie auf **Weiter: Konfiguration >** .
      ![Screenshot der Auswahlmöglichkeiten auf dem Bildschirm „Privaten Endpunkt erstellen“](./media/private-link-security/ampls-select-private-endpoint-create-4.png)

5. Gehen Sie im Bereich „Konfiguration“ folgendermaßen vor:

   a.    Wählen Sie das **virtuelle Netzwerk** und das **Subnetz** aus, mit dem Sie Ihre Azure Monitor-Ressourcen verbinden möchten. 
 
   b.    Wählen Sie für **In private DNS-Zone integrieren** die Option **Ja** aus, und lassen Sie die neue private DNS-Zone automatisch erstellen. Die tatsächlichen DNS-Zonen können von den im folgenden Screenshot dargestellten Werten abweichen. 
   > [!NOTE]
   > Wenn Sie **Nein** wählen und die manuelle Verwaltung von DNS-Einträgen bevorzugen, müssen Sie zuerst die Einrichtung Ihrer Private Link-Instanz einschließlich dieses privaten Endpunkts und der AMPLS-Konfiguration abschließen. Konfigurieren Sie anschließend Ihr DNS gemäß den Anweisungen unter [DNS-Konfiguration für private Azure-Endpunkte ](../../private-link/private-endpoint-dns.md). Stellen Sie sicher, dass Sie keine leeren Datensätze als Vorbereitung für Ihre Private Link-Einrichtung erstellen. Die von Ihnen erstellten DNS-Einträge können bestehende Einstellungen überschreiben und Ihre Konnektivität mit Azure Monitor beeinträchtigen.
 
   c.    Klicken Sie auf **Überprüfen + erstellen**.
 
   d.    Warten Sie die Überprüfung ab. 
 
   e.    Wählen Sie **Erstellen** aus. 

    ![Screenshot der ausgewählten Details für den privaten Endpunkt](./media/private-link-security/ampls-select-private-endpoint-create-5.png)

Nun haben Sie einen neuen privaten Endpunkt erstellt, der mit dieser AMPLS-Instanz verbunden ist.

## <a name="review-and-validate-your-private-link-setup"></a>Überprüfen und Validieren Ihres Private Link-Setups

### <a name="reviewing-your-endpoints-dns-settings"></a>Überprüfen der DNS-Einstellungen Ihres Endpunkts
Für den von Ihnen erstellten privaten Endpunkt sollten nun vier DNS-Zonen konfiguriert sein:

[![Screenshot der DNS-Zonen des privaten Endpunkts](./media/private-link-security/private-endpoint-dns-zones.png)](./media/private-link-security/private-endpoint-dns-zones-expanded.png#lightbox)

* privatelink-monitor-azure-com
* privatelink-oms-opinsights-azure-com
* privatelink-ods-opinsights-azure-com
* privatelink-agentsvc-azure-automation-net

> [!NOTE]
> Jede dieser Zonen ordnet bestimmte Azure Monitor-Endpunkte privaten IP-Adressen aus dem IP-Adresspool des VNET zu. Die IP-Adressen, die in den folgenden Abbildungen gezeigt werden, sind lediglich Beispiele. Ihre Konfiguration sollte stattdessen private IP-Adressen aus Ihrem eigenen Netzwerk enthalten.

#### <a name="privatelink-monitor-azure-com"></a>Privatelink-monitor-azure-com
Diese Zone deckt die globalen Endpunkte ab, die von Azure Monitor verwendet werden. Das bedeutet, dass diese Endpunkte Anforderungen verarbeiten, bei denen alle Ressourcen und nicht nur eine bestimmte Ressource berücksichtigt werden. Dieser Zone sollten folgende Endpunkte zugeordnet sein:
* `in.ai`: Application Insights-Erfassungsendpunkt, es werden ein globaler und ein regionaler Eintrag angezeigt
* `api`: Application Insights- und Log Analytics-API-Endpunkt
* `live`: Application Insights-Livemetrikendpunkt
* `profiler`: Application Insights-Profiler-Endpunkt
* `snapshot`: Application Insights-Momentaufnahmenendpunkt [ ![ Screenshot der privaten DNS-Zone „monitor-azure-com“](./media/private-link-security/dns-zone-privatelink-monitor-azure-com.png)](./media/private-link-security/dns-zone-privatelink-monitor-azure-com-expanded.png#lightbox)

#### <a name="privatelink-oms-opinsights-azure-com"></a>privatelink-oms-opinsights-azure-com
Diese Zone deckt die arbeitsbereichsspezifische Zuordnung zu OMS-Endpunkten ab. Es sollte ein Eintrag für jeden Arbeitsbereich angezeigt werden, der mit dem mit diesem privaten Endpunkt verbundenen AMPLS verknüpft ist.
[![Screenshot der privaten DNS-Zone „oms-opinsights-azure-com“](./media/private-link-security/dns-zone-privatelink-oms-opinsights-azure-com.png)](./media/private-link-security/dns-zone-privatelink-oms-opinsights-azure-com-expanded.png#lightbox)

#### <a name="privatelink-ods-opinsights-azure-com"></a>privatelink-ods-opinsights-azure-com
Diese Zone deckt die arbeitsbereichsspezifische Zuordnung zu ODS-Endpunkten ab – dem Erfassungsendpunkt von Log Analytics. Es sollte ein Eintrag für jeden Arbeitsbereich angezeigt werden, der mit dem mit diesem privaten Endpunkt verbundenen AMPLS verknüpft ist.
[![Screenshot der privaten DNS-Zone „ods-opinsights-azure-com“](./media/private-link-security/dns-zone-privatelink-ods-opinsights-azure-com.png)](./media/private-link-security/dns-zone-privatelink-ods-opinsights-azure-com-expanded.png#lightbox)

#### <a name="privatelink-agentsvc-azure-automation-net"></a>privatelink-agentsvc-azure-automation-net
Diese Zone deckt die arbeitsbereichsspezifische Zuordnung zu den Automatisierungsendpunkten des Agent-Diensts ab. Es sollte ein Eintrag für jeden Arbeitsbereich angezeigt werden, der mit dem mit diesem privaten Endpunkt verbundenen AMPLS verknüpft ist.
[![Screenshot der privaten DNS-Zone „agentsvc-azure-automation-net“](./media/private-link-security/dns-zone-privatelink-agentsvc-azure-automation-net.png)](./media/private-link-security/dns-zone-privatelink-agentsvc-azure-automation-net-expanded.png#lightbox)

### <a name="validating-you-are-communicating-over-a-private-link"></a>Überprüfen der Kommunikation über Private Link
* Um zu überprüfen, ob Ihre Anforderungen nun über den privaten Endpunkt und an die Endpunkte gesendet werden, die privaten IP-Adressen zugeordneten sind, können Sie diese mit einem Tool für die Netzwerknachverfolgung oder sogar mit Ihrem Browser überprüfen. Wenn Sie beispielsweise versuchen, Ihren Arbeitsbereich oder Ihre Anwendung abzufragen, stellen Sie sicher, dass die Anforderung an die private IP-Adresse gesendet wird, die dem API-Endpunkt zugeordnet ist (in diesem Beispiel *172.17.0.9*).

    Hinweis: In einigen Browsern können andere DNS-Einstellungen verwendet werden (siehe [Browser-DNS-Einstellungen](#browser-dns-settings)). Stellen Sie sicher, dass Ihre DNS-Einstellungen gültig sind.

* Um sicherzustellen, dass Ihr Arbeitsbereich oder Ihre Komponente keine Anforderungen von öffentlichen Netzwerken empfängt (nicht über AMPLS verbunden), legen Sie die Flags für öffentliche Erfassung und Abfrage auf *Nein* fest, wie unter [Verwalten des Zugriffs von Punkten außerhalb von Private Link-Bereichen](#manage-access-from-outside-of-private-links-scopes) erläutert.

* Verwenden Sie von einem Client in Ihrem geschützten Netzwerk aus `nslookup` für einen in Ihren DNS-Zonen aufgelisteten Endpunkt. Dieser sollte von Ihrem DNS-Server in die zugeordnete private IP-Adresse, anstelle der standardmäßig verwendeten öffentlichen IP-Adresse aufgelöst werden.


## <a name="configure-log-analytics"></a>Konfigurieren von Log Analytics

Öffnen Sie das Azure-Portal. In Ihrer Log Analytics-Arbeitsbereichsressource befindet sich auf der linken Seite das Element **Netzwerkisolation**. Über dieses Menü können Sie zwei verschiedene Zustände steuern.

![Log Analytics-Netzwerkisolation](./media/private-link-security/ampls-log-analytics-lan-network-isolation-6.png)

### <a name="connected-azure-monitor-private-link-scopes"></a>Verbundene Azure Monitor-Private Link-Bereiche
Alle mit diesem Arbeitsbereich verbundenen Bereiche werden in diesem Bildschirm angezeigt. Durch das Herstellen einer Verbindung mit Azure Monitor-Private Link-Bereichen kann Netzwerkdatenverkehr aus dem mit jedem AMPLS verbundenen virtuellen Netzwerk an diesen Arbeitsbereich fließen. Diese Art der Verbindungsherstellung hat den gleichen Effekt wie das Einrichten einer Verbindung im AMPLS, wie in [Herstellen einer Verbindung mit Azure Monitor-Ressourcen](#connect-azure-monitor-resources) gezeigt. Zum Hinzufügen einer neuen Verbindung wählen Sie **Hinzufügen** und den Azure Monitor Private Link-Bereich aus. Wählen Sie **Übernehmen** aus, um die Verbindung damit herzustellen. Beachten Sie, dass ein Arbeitsbereich keine Verbindung mit fünf AMPLS-Objekten herstellen kann, wie unter [Einschränkungen](#restrictions-and-limitations) erläutert. 

### <a name="manage-access-from-outside-of-private-links-scopes"></a>Verwalten des Zugriffs von Punkten außerhalb von Private Link-Bereichen
Die Einstellungen im unteren Bereich dieser Seite steuern den Zugriff von öffentlichen Netzwerken, also Netzwerken, die nicht über die oben aufgeführten Bereiche verbunden sind. Bei Festlegung von **Zugriff auf öffentliche Netzwerke für Erfassung zulassen** auf **Nein** wird die Erfassung von Protokollen von Computern außerhalb der verbundenen Bereiche blockiert. Bei Festlegung von **Zugriff auf öffentliche Netzwerke für Abfragen zulassen** auf **Nein** werden Abfragen von Computern außerhalb der Bereiche blockiert. Dies schließt auch Abfragen über Arbeitsmappen, Dashboards, API-basierte Clientfunktionen, Insights im Azure-Portal und in vielen weiteren Bereichen ein. Funktionen außerhalb des Azure-Portals und solche, die Log Analytics-Daten abfragen, müssen auch innerhalb des über Private Link verbundenen virtuellen Netzwerks ausgeführt werden.

### <a name="exceptions"></a>Ausnahmen
Das Beschränken des Zugriffs wie oben erläutert gilt nicht für Azure Resource Manager und weist daher die folgenden Einschränkungen auf:
* Zugriff auf Daten: Während Abfragen aus öffentlichen Netzwerken für die meisten Log Analytics-Funktionen blockiert oder zugelassen werden können, fragen einige Funktionen Daten über Azure Resource Manager ab. Daher sind solche Datenabfragen nur möglich, wenn Private Link-Einstellungen auch auf Resource Manager angewendet werden (Feature bald verfügbar). Beispiele sind Azure Monitor-Lösungen, Arbeitsmappen und Insights sowie der LogicApp-Connector.
* Arbeitsbereichsverwaltung: Änderungen der Arbeitsbereichseinstellung und -konfiguration (einschließlich Aktivierung und Deaktivierung dieser Zugriffseinstellungen) werden von Azure Resource Manager verwaltet. Beschränken Sie den Zugriff auf die Arbeitsbereichsverwaltung mithilfe der geeigneten Rollen, Berechtigungen, Netzwerksteuerungen und Überwachungsfunktionen. Weitere Informationen finden Sie unter [Rollen, Berechtigungen und Sicherheit in Azure Monitor](../roles-permissions-security.md).

> [!NOTE]
> Protokolle und Metriken, die über [Diagnoseeinstellungen](../essentials/diagnostic-settings.md) in einen Arbeitsbereich hochgeladen werden, werden über einen sicheren privaten Microsoft-Kanal geleitet und nicht durch diese Einstellungen gesteuert.

### <a name="log-analytics-solution-packs-download"></a>Herunterladen von Log Analytics-Lösungspaketen

Damit der Log Analytics-Agent Lösungspakete herunterladen kann, fügen Sie die entsprechenden vollqualifizierten Domänennamen zu Ihrer Zulassungsliste für die Firewall hinzu. 


| Cloudumgebung | Agent-Ressource | Ports | Direction |
|:--|:--|:--|:--|
|Azure – Öffentlich     | scadvisorcontent.blob.core.windows.net         | 443 | Ausgehend
|Azure Government | usbn1oicore.blob.core.usgovcloudapi.net | 443 |  Ausgehend
|Azure China 21Vianet      | mceast2oicore.blob.core.chinacloudapi.cn| 443 | Ausgehend

## <a name="configure-application-insights"></a>Application Insights konfigurieren

Öffnen Sie das Azure-Portal. In Ihrer Application Insights-Komponente (Ihrer Azure Monitor-Ressource) befindet sich auf der linken Seite ein Menüelement **Netzwerkisolation**. Über dieses Menü können Sie zwei verschiedene Zustände steuern.

![Application Insights-Netzwerkisolation](./media/private-link-security/ampls-application-insights-lan-network-isolation-6.png)

Erstens können Sie diese Application Insights-Ressource mit den Azure Monitor Private Link-Bereichen verbinden, auf die Sie zugreifen können. Wählen Sie **Hinzufügen** und dann den **Azure Monitor Private Link-Bereich** aus. Wählen Sie „Übernehmen“ aus, um die Verbindung damit herzustellen. Alle verbundenen Bereiche werden in diesem Bildschirm angezeigt. Wenn Sie diese Verbindung herstellen, kann der Netzwerkdatenverkehr in den verbundenen virtuellen Netzwerken diese Komponente erreichen und hat dieselbe Auswirkung wie das Herstellen einer Verbindung vom Bereich aus, wie in [Verbinden von Azure Monitor-Ressourcen](#connect-azure-monitor-resources) beschrieben. 

Zweitens können Sie steuern, wie von außerhalb der oben aufgeführten Private Link-Bereiche (AMPLS) auf diese Ressource zugegriffen werden kann. Wenn Sie **Zugriff auf öffentliche Netzwerke für Erfassung zulassen** auf **Nein** festlegen, können Computer oder SDKs außerhalb der verbundenen Bereiche keine Daten in diese Komponente hochladen. Wenn Sie **Zugriff aus öffentlichen Netzwerken für Abfragen zulassen** auf **Nein** festlegen, können Computer außerhalb der Bereiche nicht auf Daten in dieser Application Insights-Ressource zugreifen. Dies schließt auch den Zugriff auf APM-Protokolle, Metriken, den Livemetrikstream sowie darauf aufsetzende Funktionalität ein, wie z. B. Arbeitsmappen, Dashboards, auf Abfrage-APIs basierende Clientfunktionen, Erkenntnisse im Azure-Portal und vieles mehr. 

> [!NOTE]
> Nicht-Portal-Verbrauchsfunktionen müssen ebenfalls in dem privat verknüpften virtuellen Netzwerk ausgeführt werden, in dem sich die überwachten Workloads befinden.

Sie müssen Ressourcen zum Hosten der überwachten Workloads im Private Link-Dienst hinzufügen. Ein Beispiel finden Sie unter [Verwenden privater Endpunkte für eine Azure-Web-App](../../app-service/networking/private-endpoint.md).

Der auf diese Weise eingeschränkte Zugriff gilt nur für Daten in der Application Insights-Ressource. Konfigurationsänderungen – einschließlich Aktivierung und Deaktivierung dieser Zugriffseinstellungen – werden jedoch von Azure Resource Manager verwaltet. Sie sollten sich also stattdessen auf den Zugriff auf Resource Manager mithilfe der geeigneten Rollen, Berechtigungen, Netzwerksteuerungen und Überwachungsfunktionen beschränken. Weitere Informationen finden Sie unter [Rollen, Berechtigungen und Sicherheit in Azure Monitor](../roles-permissions-security.md).

> [!NOTE]
> Um arbeitsbereichsbasierte Application Insights-Ressourcen vollständig zu schützen, müssen Sie den Zugriff sowohl auf die Application Insights-Ressource als auch auf den zugrunde liegenden Log Analytics-Arbeitsbereich sperren.
>
> Bei der Diagnose auf Codeebene (Profiler/Debugger) müssen Sie [Ihr eigenes Speicherkonto angeben](../app/profiler-bring-your-own-storage.md), damit private Links unterstützt werden.

### <a name="handling-the-all-or-nothing-nature-of-private-links"></a>Umgang mit dem „Alles-oder-nichts“-Prinzip von Private Link-Instanzen
Wie in [Planen Ihres Private Link-Setups](#planning-your-private-link-setup) erläutert, wirkt sich auch das Einrichten einer Private Link-Instanz für eine einzelne Ressource auf alle Azure Monitor-Ressourcen in diesen Netzwerken und in anderen Netzwerken mit demselben DNS aus. Dieses Verhalten kann Ihren Integrationsprozess erschweren. Ziehen Sie folgende Möglichkeiten in Betracht:

* Alles einbeziehen: Der einfachste und sicherste Ansatz ist, alle Ihre Application Insights-Komponenten AMPLS hinzuzufügen. Für Komponenten, auf die Sie weiterhin von anderen Netzwerken aus zugreifen möchten, lassen Sie die Flags „Öffentlichen Internetzugriff für die Erfassung/Abfrage zulassen“ auf „Ja“ (Standardeinstellung) festgelegt.
* Isolieren von Netzwerken: Wenn Sie Spoke-VNETs verwenden (oder sich daran ausrichten können), befolgen Sie die Anweisungen in [Hub-Spoke-Netzwerktopologie in Azure](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). Richten Sie dann separate Private Link-Einstellungen in den entsprechenden Spoke-VNETs ein. Stellen Sie auch sicher, dass DNS-Zonen getrennt werden, da die gemeinsame Nutzung von DNS-Zonen mit anderen Spoke-Netzwerken zu [DNS-Außerkraftsetzungen](#the-issue-of-dns-overrides) führt.
* Verwenden von benutzerdefinierten DNS-Zonen für bestimmte Apps: Diese Lösung ermöglicht den Zugriff auf ausgewählte Application Insights-Komponenten über Private Link, während der gesamte andere Datenverkehr über die öffentlichen Routen beibehalten wird.
    - Richten Sie eine [benutzerdefinierte private DNS-Zone](../../private-link/private-endpoint-dns.md) ein, und benennen Sie sie mit einem eindeutigen Namen wie „internal.monitor.azure.com“
    - Erstellen Sie eine AMPLS-Instanz und einen privaten Endpunkt, und wählen Sie **nicht** die automatische Integration in die private DNS-Zone
    - Wechseln Sie zu „Privater Endpunkt > DNS-Konfiguration“, und überprüfen Sie die vorgeschlagene Zuordnung von FQDNs.
    - Wählen Sie „Konfiguration hinzufügen“ und dann die soeben erstellte Zone „internal.monitor.azure.com“ aus
    - Fügen Sie Datensätze für den obigen ![Screenshot der konfigurierten DNS-Zone](./media/private-link-security/private-endpoint-global-dns-zone.png) hinzu
    - Wechseln Sie zu Ihrer Application Insights-Komponente, und kopieren Sie deren [Verbindungszeichenfolge](../app/sdk-connection-string.md).
    - Apps oder Skripte, die diese Komponente über Private Link abrufen möchten, sollten die Verbindungszeichenfolge mit dem EndpointSuffix=internal.monitor.azure.com verwenden.
* Zuordnen von Endpunkten über Hostsdateien anstelle von DNS: So ermöglichen Sie einen Private Link-Zugriff nur über einen bestimmten Computer/virtuellen Computer in Ihrem Netzwerk:
    - Richten Sie eine AMPLS-Instanz und einen privaten Endpunkt ein, und wählen Sie **nicht** die automatische Integration in die private DNS-Zone 
    - Konfigurieren Sie die obigen A-Einträge auf einem Computer, auf dem die App in der Hostsdatei ausgeführt wird


## <a name="use-apis-and-command-line"></a>Verwenden von APIs und der Befehlszeile

Sie können den oben beschriebenen Prozess mithilfe von Azure Resource Manager-Vorlagen, REST- und Befehlszeilenschnittstellen automatisieren.

Zum Erstellen und Verwalten von Private Link-Bereichen verwenden Sie die [REST-API](/rest/api/monitor/private%20link%20scopes%20(preview)) oder die [Azure CLI (az monitor private-link-scope)](/cli/azure/monitor/private-link-scope).

Verwenden Sie zum Verwalten des Netzwerkzugriffs die Flags `[--ingestion-access {Disabled, Enabled}]` und `[--query-access {Disabled, Enabled}]`in [Log Analytics-Arbeitsbereichen](/cli/azure/monitor/log-analytics/workspace) oder [Application Insights-Komponenten](/cli/azure/ext/application-insights/monitor/app-insights/component).

## <a name="collect-custom-logs-and-iis-log-over-private-link"></a>Erfassen von benutzerdefinierten Protokollen und des IIS-Protokolls über Private Link

Beim Erfassungsprozess für benutzerdefinierte Protokolle werden Speicherkonten verwendet. Standardmäßig sind dies dienstseitig verwaltete Speicherkonten. Um jedoch benutzerdefinierte Protokolle in Private Links zu erfassen, müssen Sie Ihre eigenen Speicherkonten verwenden und sie Log Analytics-Arbeitsbereichen zuordnen. Informieren Sie sich in diesem Artikel zur [Befehlszeile](/cli/azure/monitor/log-analytics/workspace/linked-storage), wie Sie solche Konten einrichten.

Weitere Informationen zum Einbinden Ihres eigenen Speicherkontos finden Sie unter [Kundeneigene Speicherkonten für die Protokollerfassung](private-storage.md).

## <a name="restrictions-and-limitations"></a>Einschränkungen

### <a name="ampls"></a>AMPLS
Es gibt eine Reihe von Einschränkungen für das AMPLS-Objekt, die beim Planen der Einrichtung von Private Link zu beachten sind:

* Ein VNET kann nur eine Verbindung mit einem (1) AMPLS-Objekt herstellen. Dies bedeutet, dass das AMPLS-Objekt Zugriff auf alle Azure Monitor-Ressourcen bieten muss, auf die das VNET Zugriff haben sollte.
* Eine Azure Monitor-Ressource (Arbeitsbereich oder Application Insights-Komponente) kann eine Verbindung mit höchstens fünf AMPLS-Objekten herstellen.
* Ein AMPLS-Objekt kann eine Verbindung mit höchstens 50 Azure Monitor-Ressourcen herstellen.
* Ein AMPLS-Objekt kann eine Verbindung mit höchstens zehn privaten Endpunkten herstellen.

Weitere Informationen zu diesen Grenzwerten finden Sie unter [Beachten von Einschränkungen](#consider-limits).

### <a name="agents"></a>Agents

Die neuesten Versionen von Windows- und Linux-Agents müssen verwendet werden, um eine sichere Erfassung in Log Analytics-Arbeitsbereichen zu ermöglichen. Ältere Versionen können keine Überwachungsdaten über ein privates Netzwerk hochladen.

**Log Analytics-Windows-Agent**

Verwenden Sie die Log Analytics-Agent-Version 10.20.18038.0 oder höher.

**Log Analytics-Linux-Agent**

Verwenden Sie die Agent-Version 1.12.25 oder höher. Falls dies nicht möglich ist, führen Sie die folgenden Befehle auf Ihrem virtuellen Computer aus.

```cmd
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -X
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <workspace key>
```

### <a name="azure-portal"></a>Azure-Portal

Um Azure Monitor-Portalfunktionalität wie Application Insights und Log Analytics zu verwenden, müssen Sie den Zugriff auf die Erweiterungen für Azure-Portal und Azure Monitor in den privaten Netzwerken zulassen. Fügen Sie [Diensttags](../../firewall/service-tags.md) für **AzureActiveDirectory**, **AzureResourceManager**, **AzureFrontDoor.FirstParty** und **AzureFrontdoor.Frontend** Ihrer Netzwerksicherheitsgruppe hinzu.

### <a name="querying-data"></a>Abfragen von Daten
Der [`externaldata`-Operator](/azure/data-explorer/kusto/query/externaldata-operator?pivots=azuremonitor) wird von Private Link nicht unterstützt, da er zwar Daten aus Speicherkonten liest, jedoch nicht sicherstellt, dass privat auf den Speicher zugegriffen wird.

### <a name="programmatic-access"></a>Programmgesteuerter Zugriff

Um die REST-API, die [CLI](/cli/azure/monitor) oder PowerShell mit Azure Monitor in privaten Netzwerken zu verwenden, fügen Sie die [Diensttags](../../virtual-network/service-tags-overview.md) für **AzureActiveDirectory** und **AzureResourceManager** Ihrer Firewall hinzu.

### <a name="application-insights-sdk-downloads-from-a-content-delivery-network"></a>Application Insights SDK-Downloads aus einem Content Delivery Network

Bündeln Sie den JavaScript-Code in Ihrem Skript, sodass der Browser nicht versucht, Code aus einem CDN herunterzuladen. Ein Beispiel finden Sie auf [GitHub](https://github.com/microsoft/ApplicationInsights-JS#npm-setup-ignore-if-using-snippet-setup).

### <a name="browser-dns-settings"></a>Browser-DNS-Einstellungen

Wenn Sie eine Verbindung mit den Azure Monitor-Ressourcen über Private Link herstellen, muss der Datenverkehr zu diesen Ressourcen über den privaten Endpunkt erfolgen, der in Ihrem Netzwerk konfiguriert ist. Um den privaten Endpunkt zu aktivieren, aktualisieren Sie Ihre DNS-Einstellungen, wie unter [Herstellen einer Verbindung mit einem privaten Endpunkt](#connect-to-a-private-endpoint) beschrieben. Einige Browser verwenden eigene DNS-Einstellungen anstelle der von Ihnen festgelegten DNS-Einstellungen. Der Browser versucht möglicherweise, eine Verbindung mit öffentlichen Azure Monitor-Endpunkten herzustellen und den privaten Link vollständig zu umgehen. Vergewissern Sie sich, dass die Browsereinstellungen DNS-Einstellungen nicht überschreiben und keine alten Einstellungen zwischenspeichern. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [privaten Speicher](private-storage.md).