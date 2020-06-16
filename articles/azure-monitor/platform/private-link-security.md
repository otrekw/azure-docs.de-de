---
title: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor
description: Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor
author: nkiest
ms.author: nikiest
ms.topic: conceptual
ms.date: 05/20/2020
ms.subservice: ''
ms.openlocfilehash: 95345ba864d498190186e1a366c8551be97c33f5
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/02/2020
ms.locfileid: "84299667"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-monitor"></a>Verwenden von Azure Private Link zum sicheren Verbinden von Netzwerken mit Azure Monitor

> [!IMPORTANT]
> Zurzeit müssen Sie **Zugriff anfordern**, um diese Funktion verwenden zu können. Sie können den Zugriff über das [Registrierungsformular](https://aka.ms/AzMonPrivateLinkSignup) beantragen.


Mit [Azure Private Link](../../private-link/private-link-overview.md) können Sie Azure-PaaS-Dienste über private Endpunkte sicher mit Ihrem virtuellen Netzwerk verknüpfen. Bei vielen Diensten richten Sie einfach für jede Ressource einen Endpunkt ein. Azure Monitor ist jedoch eine Zusammenstellung verschiedener miteinander verbundener Dienste, die zur Überwachung Ihrer Workloads ineinandergreifen. Aus diesem Grund haben wir eine neue Ressource erstellt: den Azure Monitor Private Link-Bereich. Mit einem solchen Bereich können Sie die Grenzen Ihres Überwachungsnetzwerks definieren und eine Verbindung mit Ihrem virtuellen Netzwerk herstellen. In diesem Artikel wird erläutert, wie Sie einen Azure Monitor Private Link-Bereich einrichten und verwenden.

## <a name="advantages"></a>Vorteile

Private Link bietet folgende Möglichkeiten:

- Herstellen einer privaten Verbindung mit Azure Monitor ohne öffentlichen Netzwerkzugriff
- Sicherstellen, dass der Zugriff auf Überwachungsdaten nur über autorisierte private Netzwerke erfolgt
- Verhindern von Datenexfiltration aus Ihren privaten Netzwerken durch Definieren bestimmter Azure Monitor-Ressourcen, die eine Verbindung über Ihren privaten Endpunkt herstellen
- Sichere Verbindung zwischen Ihrem privaten lokalen Netzwerk und Azure Monitor über ExpressRoute und Private Link
- Kapseln des gesamten Datenverkehrs innerhalb des Microsoft Azure-Backbonenetzwerks

Weitere Informationen finden Sie unter [Hauptvorteile von Private Link](../../private-link/private-link-overview.md#key-benefits).

## <a name="how-it-works"></a>Funktionsweise

Ein Azure Monitor Private Link-Bereich (Azure Monitor Private Link Scope, AMPLS) ist eine Gruppierungsressource, mit der Sie mehrere private Endpunkte (und damit auch die virtuellen Netzwerke, in denen sich die Endpunkte befinden) mit einer oder mehreren Azure Monitor-Ressourcen verbinden können. Die Ressourcen umfassen Log Analytics-Arbeitsbereiche und Application Insights-Komponenten.

![Diagramm der Ressourcentopologie](./media/private-link-security/private-link-topology-1.png)

> [!NOTE]
> Eine einzelne Azure Monitor-Ressource kann zu mehreren Azure Monitor Private Link-Bereichen gehören, aber Sie können ein einzelnes virtuelles Netzwerk nicht mit mehr als einem Bereich verbinden. 

## <a name="planning-based-on-your-network"></a>Planung basierend auf dem Netzwerk

Bevor Sie Ihre AMPLS-Ressourcen einrichten, überprüfen Sie Ihre Anforderungen an die Netzwerkisolation. Bewerten Sie den Zugriff Ihrer virtuellen Netzwerke auf das öffentliche Internet sowie die Zugriffsbeschränkungen der einzelnen Azure Monitor-Ressourcen (also der Application Insights-Komponenten und der Log Analytics-Arbeitsbereiche).

### <a name="evaluate-which-virtual-networks-should-connect-to-a-private-link"></a>Bewerten, welche virtuellen Netzwerke eine Verbindung mit Private Link herstellen sollen

Bewerten Sie zunächst, welche Ihrer virtuellen Netzwerke eingeschränkten Zugriff auf das Internet haben. Virtuelle Netzwerke mit freiem Internetzugang benötigen möglicherweise keine Private Link-Instanz, um auf Ihre Azure Monitor-Ressourcen zuzugreifen. Die Überwachungsressourcen, mit denen Ihre virtuellen Netzwerke Verbindungen herstellen, schränken eingehenden Datenverkehr möglicherweise ein und benötigen eine Private Link-Verbindung (zur Protokollerfassung oder für Abfragen). In solchen Fällen muss auch ein virtuelles Netzwerk, das auf das öffentliche Internet zugreifen kann, über Private Link und einen Azure Monitor Private Link-Bereich eine Verbindung mit diesen Ressourcen herstellen.

### <a name="evaluate-which-azure-monitor-resources-should-have-a-private-link"></a>Bewerten, welche Azure Monitor-Ressourcen eine Private Link-Instanz benötigen

Überprüfen Sie jede einzelne Azure Monitor-Ressource:

- Soll die Ressource nur die Erfassung von Protokollen aus Ressourcen zulassen, die sich in bestimmten virtuellen Netzwerken befinden?
- Soll die Ressource nur von Clients abgefragt werden, die sich in bestimmten virtuellen Netzwerken befinden?

Wenn die Antwort auf eine dieser Fragen „Ja“ lautete, legen Sie die Einschränkungen so fest, wie unter [Konfigurieren von Log Analytics-Arbeitsbereichen](#configure-log-analytics) und [Konfigurieren von Application Insights-Komponenten](#configure-application-insights) erläutert, und ordnen Sie diese Ressourcen mindestens einem Azure Monitor Private Link-Bereich zu. Virtuelle Netzwerke, die auf diese Überwachungsressourcen zugreifen sollen, benötigen einen privaten Endpunkt, der eine Verbindung mit dem entsprechenden Azure Monitor Private Link-Bereich herstellt.
Denken Sie daran, dass Sie ein und denselben Arbeitsbereich bzw. ein und dieselbe Anwendung mit mehreren Azure Monitor Private Link-Bereichen verbinden können, damit verschiedene Netzwerke darauf zugreifen können.

### <a name="group-together-monitoring-resources-by-network-accessibility"></a>Gruppieren von Überwachungsressourcen anhand des Netzwerkzugriffs

Da jedes virtuelle Netzwerk nur mit einer einzigen AMPLS-Ressource verbunden werden kann, müssen Sie Überwachungsressourcen gruppieren, auf die vom gleichen Netzwerk aus zugegriffen werden soll. Die einfachste Möglichkeit hierfür besteht darin, einen Azure Monitor Private Link-Bereich pro virtuellem Netzwerk zu erstellen und die Ressourcen auszuwählen, die eine Verbindung mit diesem Netzwerk herstellen sollen. Um den Ressourcenbedarf zu senken und die Verwaltbarkeit zu verbessern, können Sie einen Azure Monitor Private Link-Bereich auch für mehrere Netzwerke verwenden.

Ein Beispiel: Ihre internen virtuellen Netzwerke VNet1 und VNet2 sollen eine Verbindung mit den Arbeitsbereichen Arbeitsbereich1 und Arbeitsbereich2 sowie der Application Insights-Komponente ApplicationInsights3 herstellen. In diesem Fall ordnen Sie alle drei Ressource demselben Azure Monitor Private Link-Bereich zu. Wenn VNet3 nur auf Arbeitsbereich1 zugreifen soll, erstellen Sie eine weitere AMPLS-Ressource, ordnen Sie dieser Arbeitsbereich1 zu, und stellen Sie eine Verbindung mit VNet3 her, wie in den folgenden Diagrammen veranschaulicht:

![Diagramm der Topologie für AMPLS A](./media/private-link-security/ampls-topology-a-1.png)

![Diagramm der Topologie für AMPLS B](./media/private-link-security/ampls-topology-b-1.png)

## <a name="example-connection"></a>Beispiel für eine Verbindung

Erstellen Sie als Erstes eine Ressource für einen Azure Monitor Private Link-Bereich.

1. Wechseln Sie im Azure-Portal zu **Ressource erstellen**, und suchen Sie nach **Azure Monitor Private Link-Bereich**.

   ![Suchen des Azure Monitor-Private Link-Bereichs](./media/private-link-security/ampls-find-1c.png)

2. Klicken Sie auf **Create** (Erstellen).
3. Wählen Sie ein Abonnement und eine Ressourcengruppe aus.
4. Benennen Sie den Azure Monitor Private Link-Bereich. Der Name sollte verdeutlichen, zu welchem Zweck und in welcher Sicherheitsgrenze der Bereich verwendet wird, sodass Sicherheitsgrenzen nicht versehentlich verletzt werden. Beispiel: „AppServerProdTelem“.
5. Klicken Sie auf **Überprüfen und erstellen**. 

   ![Erstellen des Azure Monitor-Private Link-Bereichs](./media/private-link-security/ampls-create-1d.png)

6. Warten Sie die Überprüfung ab, und klicken Sie dann auf **Erstellen**.

## <a name="connect-azure-monitor-resources"></a>Verbinden von Azure Monitor-Ressourcen

Sie können Ihren Azure Monitor Private Link-Bereich zuerst mit privaten Endpunkten und dann mit Azure Monitor-Ressourcen oder umgekehrt verbinden. Der Verbindungsprozess erfolgt aber schneller, wenn Sie mit den Azure Monitor-Ressourcen beginnen. So stellen Sie eine Verbindung zwischen den Azure Monitor-Ressourcen – Log Analytics-Arbeitsbereiche und Application Insights-Komponenten – und einem Azure Monitor Private Link-Bereich her:

1. Klicken Sie in Ihrem Azure Monitor Private Link-Bereich im Menü links auf **Azure Monitor-Ressourcen**. Klicken Sie auf die Schaltfläche **Hinzufügen** .
2. Fügen Sie den Arbeitsbereich oder die Komponente hinzu. Durch Klicken auf die Schaltfläche **Hinzufügen** wird ein Dialogfeld geöffnet, in dem Sie Azure Monitor-Ressourcen auswählen können. Sie können Ihre Abonnements und Ressourcengruppen durchsuchen oder zum Filtern den entsprechenden Namen eingeben. Wählen Sie den Arbeitsbereich bzw. die Komponente aus, und klicken Sie auf **Anwenden**, um die entsprechende Ressource zum Bereich hinzuzufügen.

    ![Screenshot der Benutzeroberfläche zur Auswahl eines Bereichs](./media/private-link-security/ampls-select-2.png)

### <a name="connect-to-a-private-endpoint"></a>Herstellen einer Verbindung mit einem privaten Endpunkt

Nachdem Sie Ihre Ressource mit dem Azure Monitor Private Link-Bereich verbunden haben, erstellen Sie jetzt einen privaten Endpunkt für die Verbindung mit dem Netzwerk. Diesen Schritt können Sie im [Private Link-Center im Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints) oder – wie im gezeigten Beispiel – im Azure Monitor Private Link-Bereich ausführen.

1. Klicken Sie in Ihrer AMPLS-Ressource im Ressourcenmenü links auf **Private Endpunktverbindungen**. Klicken Sie auf **Privater Endpunkt**, um den Prozess zum Erstellen des Endpunkts zu starten. Sie können hier auch Verbindungen genehmigen, die im Private Link-Center gestartet wurden, indem Sie die entsprechende Verbindung auswählen und auf **Genehmigen** klicken.

    ![Screenshot der Benutzeroberfläche für Verbindungen mit privaten Endpunkten](./media/private-link-security/ampls-select-private-endpoint-connect-3.png)

2. Wählen Sie das Abonnement, die Ressourcengruppe, den Namen des Endpunkts sowie die Region aus, in der sich der Endpunkt befinden soll. Die Region muss dieselbe Region sein, in der sich das virtuelle Netzwerk befindet, mit dem eine Verbindung hergestellt werden soll.

3. Klicken Sie auf **Weiter: Ressource** aus. 

4. Gehen Sie auf dem Bildschirm „Ressource“ folgendermaßen vor:

   a. Wählen Sie das **Abonnement** aus, das Ihre Ressource für den Azure Monitor Private Link-Bereich enthält. 

   b. Wählen Sie als **Ressourcentyp** die Option **Microsoft.insights/privateLinkScopes** aus. 

   c. Wählen Sie aus der Dropdownliste **Ressource** den Private Link-Bereich aus, den Sie weiter oben erstellt haben. 

   d. Klicken Sie auf **Weiter: Konfiguration >** .
      ![Screenshot der Auswahlmöglichkeiten auf dem Bildschirm „Privaten Endpunkt erstellen“](./media/private-link-security/ampls-select-private-endpoint-create-4.png)

5. Gehen Sie im Bereich „Konfiguration“ folgendermaßen vor:

   a.    Wählen Sie das **virtuelle Netzwerk** und das **Subnetz** aus, mit dem Sie Ihre Azure Monitor-Ressourcen verbinden möchten. 
 
   b.    Wählen Sie für **In private DNS-Zone integrieren** die Option **Ja** aus, und lassen Sie die neue private DNS-Zone automatisch erstellen. 
 
   c.    Klicken Sie auf **Überprüfen + erstellen**.
 
   d.    Warten Sie die Überprüfung ab. 
 
   e.    Klicken Sie auf **Erstellen**. 

    ![Screenshot 2 der Auswahlmöglichkeiten auf dem Bildschirm „Privaten Endpunkt erstellen“](./media/private-link-security/ampls-select-private-endpoint-create-5.png)

Nun haben Sie einen neuen privaten Endpunkt erstellt, der mit diesem Azure Monitor-Private Link-Bereich verbunden ist.

## <a name="configure-log-analytics"></a>Konfigurieren von Log Analytics

Öffnen Sie das Azure-Portal. In Ihrem Log Analytics-Arbeitsbereich (Ihrer Azure Monitor-Ressource) befindet sich auf der linken Seite ein Menüelement **Netzwerkisolation**. Über dieses Menü können Sie zwei verschiedene Zustände steuern. 

![Log Analytics-Netzwerkisolation](./media/private-link-security/ampls-log-analytics-lan-network-isolation-6.png)

Erstens können Sie diese Log Analytics-Ressource mit allen Azure Monitor Private Link-Bereichen verbinden, auf die Sie zugreifen können. Klicken Sie auf **Hinzufügen**, und wählen Sie den Azure Monitor Private Link-Bereich aus.  Klicken Sie auf **Übernehmen**, um die Verbindung herzustellen. Alle verbundenen Bereiche werden in diesem Bildschirm angezeigt. Durch Herstellen dieser Verbindung kann der Netzwerkdatenverkehr in den verbundenen virtuellen Netzwerken diesen Arbeitsbereich erreichen. Dieser Vorgang hat den gleichen Effekt wie das Herstellen einer Verbindung aus dem Bereich, wie es in [Herstellen einer Verbindung mit Azure Monitor-Ressourcen](#connect-azure-monitor-resources) ausgeführt wurde.  

Zweitens können Sie steuern, wie von außerhalb der oben aufgeführten Private Link-Bereiche auf diese Ressource zugegriffen werden kann. Wenn Sie **Zugriff auf öffentliche Netzwerke für Erfassung zulassen** auf **Nein** festlegen, können Computer außerhalb der verbundenen Bereiche keine Daten in diesen Arbeitsbereich hochladen. Wenn Sie **Zugriff aus öffentlichen Netzwerken für Abfragen zulassen** auf **Nein** festlegen, können Computer außerhalb der Bereiche nicht auf Daten in diesem Arbeitsbereich zugreifen. Dies schließt auch den Zugriff auf Arbeitsmappen, Dashboards, auf Abfrage-APIs basierende Clientfunktionen, Erkenntnisse im Azure-Portal und vieles mehr ein. Funktionen außerhalb des Azure-Portals, die Log Analytics-Daten verwenden, müssen auch innerhalb des über Private Link verbundenen virtuellen Netzwerks ausgeführt werden.

Der auf diese Weise eingeschränkte Zugriff gilt nur für Daten im Arbeitsbereich. Konfigurationsänderungen – einschließlich Aktivierung und Deaktivierung dieser Zugriffseinstellungen – werden von Azure Resource Manager verwaltet. Beschränken Sie den Zugriff auf Resource Manager mithilfe der geeigneten Rollen, Berechtigungen, Netzwerksteuerungen und Überwachungsfunktionen. Weitere Informationen finden Sie unter [Rollen, Berechtigungen und Sicherheit in Azure Monitor](roles-permissions-security.md).

> [!NOTE]
> Protokolle und Metriken, die über [Diagnoseeinstellungen](diagnostic-settings.md) in einen Arbeitsbereich hochgeladen werden, werden über einen sicheren privaten Microsoft-Kanal geleitet und nicht durch diese Einstellungen gesteuert.

## <a name="configure-application-insights"></a>Application Insights konfigurieren

Öffnen Sie das Azure-Portal. In Ihrer Application Insights-Komponente (Ihrer Azure Monitor-Ressource) befindet sich auf der linken Seite ein Menüelement **Netzwerkisolation**. Über dieses Menü können Sie zwei verschiedene Zustände steuern.

![Application Insights-Netzwerkisolation](./media/private-link-security/ampls-application-insights-lan-network-isolation-6.png)

Erstens können Sie diese Application Insights-Ressource mit den Azure Monitor Private Link-Bereichen verbinden, auf die Sie zugreifen können. Klicken Sie auf **Hinzufügen**, und wählen Sie den **Azure Monitor Private Link-Bereich** aus. Klicken Sie auf „Übernehmen“, um die Verbindung herzustellen. Alle verbundenen Bereiche werden in diesem Bildschirm angezeigt. Durch Herstellen dieser Verbindung kann der Netzwerkdatenverkehr in den verbundenen virtuellen Netzwerken diese Komponente erreichen. Dieser Vorgang hat den gleichen Effekt wie das Herstellen einer Verbindung aus dem Bereich, wie es in [Herstellen einer Verbindung mit Azure Monitor-Ressourcen](#connect-azure-monitor-resources) ausgeführt wurde. 

Zweitens können Sie steuern, wie von außerhalb der oben aufgeführten Private Link-Bereiche auf diese Ressource zugegriffen werden kann. Wenn Sie **Zugriff auf öffentliche Netzwerke für Erfassung zulassen** auf **Nein** festlegen, können Computer oder SDKs außerhalb der verbundenen Bereiche keine Daten in diese Komponente hochladen. Wenn Sie **Zugriff aus öffentlichen Netzwerken für Abfragen zulassen** auf **Nein** festlegen, können Computer außerhalb der Bereiche nicht auf Daten in dieser Application Insights-Ressource zugreifen. Dies schließt auch den Zugriff auf APM-Protokolle, Metriken, den Livemetrikstream sowie darauf aufsetzende Funktionalität ein, wie z. B. Arbeitsmappen, Dashboards, auf Abfrage-APIs basierende Clientfunktionen, Erkenntnisse im Azure-Portal und vieles mehr. 

Beachten Sie, dass Nicht-Portal-Verbrauchsfunktionen ebenfalls in dem privat verknüpften virtuellen Netzwerk ausgeführt werden müssen, in dem sich die überwachten Workloads befinden. 

Sie müssen Ressourcen zum Hosten der überwachten Workloads im Private Link-Dienst hinzufügen. Hier finden Sie die [Dokumentation](https://docs.microsoft.com/azure/app-service/networking/private-endpoint) für diesen Vorgang für App Services.

Der auf diese Weise eingeschränkte Zugriff gilt nur für Daten in der Application Insights-Ressource. Konfigurationsänderungen – einschließlich Aktivierung und Deaktivierung dieser Zugriffseinstellungen – werden von Azure Resource Manager verwaltet. Beschränken Sie stattdessen den Zugriff auf Resource Manager mithilfe der geeigneten Rollen, Berechtigungen, Netzwerksteuerungen und Überwachungsfunktionen. Weitere Informationen finden Sie unter [Rollen, Berechtigungen und Sicherheit in Azure Monitor](roles-permissions-security.md).

> [!NOTE]
> Um arbeitsbereichsbasierte Application Insights-Ressourcen vollständig zu schützen, müssen Sie den Zugriff sowohl auf die Application Insights-Ressource als auch auf den zugrunde liegenden Log Analytics-Arbeitsbereich sperren.

> [!NOTE]
> Diagnosefunktionen auf Codeebene (Profiler/Debugger) unterstützen Private Link derzeit nicht.

## <a name="use-apis-and-command-line"></a>Verwenden von APIs und der Befehlszeile

Sie können den oben beschriebenen Prozess mithilfe von Azure Resource Manager-Vorlagen und Befehlszeilenschnittstellen automatisieren.

Zum Erstellen und Verwalten von Private Link-Bereichen verwenden Sie [az monitor private-link-scope](https://docs.microsoft.com/cli/azure/monitor/private-link-scope?view=azure-cli-latest). Mit diesem Befehl können Sie Bereiche erstellen, Log Analytics-Arbeitsbereiche und Application Insights-Komponenten zuordnen und private Endpunkte hinzufügen/entfernen/genehmigen.

Verwenden Sie zum Verwalten des Netzwerkzugriffs die Flags `[--ingestion-access {Disabled, Enabled}]` und `[--query-access {Disabled, Enabled}]`in [Log Analytics-Arbeitsbereichen](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace?view=azure-cli-latest) oder [Application Insights-Komponenten](https://docs.microsoft.com/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest).

## <a name="collect-custom-logs-over-private-link"></a>Erfassen von benutzerdefinierten Protokollen über Private Link

Beim Erfassungsprozess für benutzerdefinierte Protokolle werden Speicherkonten verwendet. Standardmäßig sind dies dienstseitig verwaltete Speicherkonten. Um jedoch benutzerdefinierte Protokolle in Private Links zu erfassen, müssen Sie Ihre eigenen Speicherkonten verwenden und sie Log Analytics-Arbeitsbereichen zuordnen. Informieren Sie sich in diesem Artikel zur [Befehlszeile](https://docs.microsoft.com/cli/azure/monitor/log-analytics/workspace/linked-storage?view=azure-cli-latest), wie Sie solche Konten einrichten.

Weitere Informationen zum Einbinden Ihres eigenen Speicherkontos finden Sie unter [Kundeneigene Speicherkonten für die Protokollerfassung](private-storage.md).

## <a name="restrictions-and-limitations"></a>Einschränkungen

### <a name="agents"></a>Agents

In privaten Netzwerken müssen die neuesten Versionen von Windows- und Linux-Agents verwendet werden, um eine sichere Telemetriedatenerfassung in Log Analytics-Arbeitsbereichen zu ermöglichen. Ältere Versionen können keine Überwachungsdaten in ein privates Netzwerk hochladen.

**Log Analytics-Windows-Agent**

Verwenden Sie die Log Analytics-Agent-Version 10.20.18038.0 oder höher.

**Log Analytics-Linux-Agent**

Verwenden Sie die Agent-Version 1.12.25 oder höher. Falls dies nicht möglich ist, führen Sie die folgenden Befehle auf Ihrem virtuellen Computer aus.

```cmd
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -X
$ sudo /opt/microsoft/omsagent/bin/omsadmin.sh -w <workspace id> -s <workspace key>
```

### <a name="azure-portal"></a>Azure-Portal

Um Azure Monitor-Portalfunktionalität wie Application Insights und Log Analytics zu verwenden, müssen Sie den Zugriff auf die Erweiterungen für Azure-Portal und Azure Monitor in den privaten Netzwerken zulassen. Fügen Sie [Diensttags](../../firewall/service-tags.md) für **AzureActiveDirectory**, **AzureResourceManager**, **AzureFrontDoor.FirstParty** und **AzureFrontdoor.Frontend** Ihrer Firewall hinzu.

### <a name="programmatic-access"></a>Programmgesteuerter Zugriff

Um die REST-API, die [CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest) oder PowerShell mit Azure Monitor in private Netzwerken zu verwenden, fügen Sie die [Diensttags](https://docs.microsoft.com/azure/virtual-network/service-tags-overview) für **AzureActiveDirectory** und **AzureResourceManager** zu Ihrer Firewall hinzu.

Durch Hinzufügen dieser Tags können Sie Aktionen wie das Abfragen von Protokolldaten sowie das Erstellen und Verwalten von Log Analytics-Arbeitsbereichen und Application Insights-Komponenten ausführen.

### <a name="application-insights-sdk-downloads-from-a-content-delivery-network"></a>Application Insights SDK-Downloads aus einem Content Delivery Network

Bündeln Sie den JavaScript-Code in Ihrem Skript, sodass der Browser nicht versucht, Code aus einem CDN herunterzuladen. Ein Beispiel finden Sie auf [GitHub](https://github.com/microsoft/ApplicationInsights-JS#npm-setup-ignore-if-using-snippet-setup).

### <a name="log-analytics-solution-download"></a>Download der Log Analytics-Lösung

Damit der Log Analytics-Agent Lösungspakete herunterladen kann, fügen Sie die entsprechenden vollqualifizierten Domänennamen zu Ihrer Zulassungsliste für die Firewall hinzu. 


| Cloudumgebung | Agent-Ressource | Ports | Direction |
|:--|:--|:--|:--|
|Azure – Öffentlich     | scadvisor.blob.core.windows.net         | 443 | Ausgehend
|Azure Government | usbn1oicore.blob.core.usgovcloudapi.net | 443 |  Ausgehend
|Azure China 21Vianet      | mceast2oicore.blob.core.chinacloudapi.cn| 443 | Ausgehend

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [privaten Speicher](private-storage.md).
