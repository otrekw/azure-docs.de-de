---
title: Überwachungsmetriken und unformatierte Protokolle für Azure CDN von Microsoft
description: In diesem Artikel werden die Überwachungsmetriken und unformatierten Protokolle in Azure CDN von Microsoft beschrieben.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/25/2020
ms.author: allensu
ms.openlocfilehash: 2fbefd3b7761976cffbd6be8714cb849e1253aec
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2020
ms.locfileid: "92778034"
---
# <a name="monitoring-metrics-and-raw-logs-for-azure-cdn-from-microsoft"></a>Überwachungsmetriken und unformatierte Protokolle für Azure CDN von Microsoft
Mit Azure CDN von Microsoft können Sie Ressourcen auf folgende Weise überwachen, um Probleme zu behandeln, nachzuverfolgen und zu debuggen. 

* Unformatierte Protokolle enthalten umfangreiche Informationen zu jeder Anforderung, die in CDN empfangen wird. Unformatierte Protokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblicke in die Vorgänge, die für Azure-Ressourcen ausgeführt wurden.
* Metriken, die u. a. vier wichtige Metriken in CDN anzeigen: Bytetrefferquote, Anforderungsanzahl, Antwortgröße und Gesamtlatenz. Außerdem stehen unterschiedliche Dimensionen zur Aufschlüsselung von Metriken zur Verfügung.
* Kunden können Warnungen für wichtige Metriken einrichten.
* Kunden können über Azure Log Analytics zusätzliche nützliche Metriken aktivieren. Auch werden Abfragebeispiele für einige andere Metriken unter Azure Log Analytics bereitgestellt.

> [!IMPORTANT]
> Das Feature für unformatierte HTTP-Protokolle ist für Azure CDN von Microsoft verfügbar.

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="configuration---azure-portal"></a>Konfiguration – Azure-Portal

So konfigurieren Sie unformatierte Protokolle für Azure CDN über Microsoft-Profile 

1. Wählen Sie im Menü des Azure-Portals **Alle Ressourcen** >>  **\<your-CDN-profile>** aus.

2. Wählen von unter **Überwachung** die Option **Diagnoseeinstellungen** aus.

3. Wählen Sie **+Diagnoseeinstellung hinzufügen**  aus.

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-01.png" alt-text="„Diagnoseeinstellung hinzufügen“ für CDN-Profil" border="true":::
    
    > [!IMPORTANT]
    > Unformatierte Protokolle sind nur auf Profilebene verfügbar, während aggregierte HTTP-Statuscodeprotokolle auf Endpunktebene verfügbar sind.

4. Geben Sie unter **Diagnoseeinstellungen** einen Namen für die Diagnoseeinstellung unter **Name der Diagnoseeinstellungen** ein.

5. Wählen Sie **AzureCdnAccessLog** aus, und legen Sie die Aufbewahrungsdauer in Tagen fest.

6. Wählen Sie die **Zieldetails** aus. Folgende Optionen für das Ziel sind verfügbar:
    * **An Log Analytics senden**
        * Wählen Sie das **Abonnement** und den **Log Analytics-Arbeitsbereich** aus.
    * **In einem Speicherkonto archivieren**
        * Wählen Sie das **Abonnement** und das **Speicherkonto** aus.
    * **An einen Event Hub streamen**
        * Wählen Sie das **Abonnement** , den **Event Hub-Namespace** , den **Event Hub-Namen** (optional) und den **Event Hub-Richtliniennamen** aus.

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-02.png" alt-text="Konfigurieren des Ziels für Protokolleinstellungen" border="true":::

7. Wählen Sie **Speichern** aus.

## <a name="configuration---azure-powershell"></a>Konfiguration – Azure PowerShell

Verwenden Sie [Set-AzDiagnosticSetting](/powershell/module/az.monitor/set-azdiagnosticsetting), um die Diagnoseeinstellung für unformatierte Protokolle zu konfigurieren.

Aufbewahrungsdaten werden durch die Option **-RetentionInDays** im Befehl definiert.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="enable-diagnostic-logs-in-a-storage-account"></a>Aktivieren von Diagnoseprotokollen in einem Speicherkonto

1. Melden Sie sich bei Azure PowerShell an:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```

2. Geben Sie die folgenden Befehle ein, um Diagnoseprotokolle in einem Speicherkonto zu aktivieren. Ersetzen Sie die Variablen durch Ihre Werte:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $storageacct = <your-storage-account-name>
    $diagname = <your-diagnostic-setting-name>
    $days = '30'

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $storage = Get-AzStorageAccount -ResourceGroupName $rsg -Name $storageacct

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -StorageAccountId $storage.id -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```

### <a name="enable-diagnostics-logs-for-log-analytics-workspace"></a>Aktivieren von Diagnoseprotokollen für den Log Analytics-Arbeitsbereich

1. Melden Sie sich bei Azure PowerShell an:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```
2. Geben Sie diese Befehle ein, um Diagnoseprotokolle für einen Log Analytics-Arbeitsbereich zu aktivieren. Ersetzen Sie die Variablen durch Ihre Werte:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $workspacename = <your-log-analytics-workspace-name>
    $diagname = <your-diagnostic-setting-name>
    $days = '30'

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $workspace = Get-AzOperationalInsightsWorkspace -ResourceGroupName $rsg -Name $workspacename

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -WorkspaceId $workspace.ResourceId -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```
### <a name="enable-diagnostics-logs-for-event-hub-namespace"></a>Aktivieren von Diagnoseprotokollen für den Event Hub-Namespace

1. Melden Sie sich bei Azure PowerShell an:

    ```azurepowershell-interactive
    Connect-AzAccount 
    ```
2. Geben Sie diese Befehle ein, um Diagnoseprotokolle für einen Event Hub-Namespace zu aktivieren. Ersetzen Sie die Variablen durch Ihre Werte:

    ```azurepowershell-interactive
    ## Variables for the commands ##
    $rsg = <your-resource-group-name>
    $cdnprofile = <your-cdn-profile-name>
    $cdnendpoint = <your-cdn-endpoint-name>
    $evthubnamespace = <your-event-hub-namespace-name>
    $diagname = <your-diagnostic-setting-name>

    $cdn = Get-AzCdnEndpoint -ResourceGroupName $rsg -ProfileName $cdnprofile -EndpointName $cdnendpoint

    $eventhub = Get-AzEventHubNamespace -ResourceGroupName $rsg -Name $eventhubname

    Set-AzDiagnosticSetting -Name $diagname -ResourceId $cdn.id -EventHubName $eventhub.id -Enabled $true -Category AzureCdnAccessLog -RetentionEnabled 1 -RetentionInDays $days
    ```

## <a name="raw-logs-properties"></a>Eigenschaften von unformatierten Protokollen

Azure CDN vom Microsoft-Dienst stellt zurzeit unformatierte Protokolle bereit. Unformatierte Protokolle enthalten einzelne API-Anforderungen, wobei jeder Eintrag folgendem Schema entspricht: 

| Eigenschaft              | BESCHREIBUNG                                                                                                                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrackingReference     | Die eindeutige Verweiszeichenfolge zur Identifizierung einer Anforderung, die von der Front Door-Instanz verarbeitet wird. Diese wird auch als X-Azure-Ref-Header an den Client gesendet. Sie ist für die Suche nach Informationen in den Zugriffsprotokollen für eine bestimmte Anforderung erforderlich. |
| HttpMethod            | Von der Anforderung verwendete HTTP-Methode                                                                                                                                                                     |
| HttpVersion           | Typ der Anforderung oder Verbindung.                                                                                                                                                                   |
| RequestUri            | URI der empfangenen Anforderung                                                                                                                                                                         |
| RequestBytes          | Die Größe der HTTP-Anforderungsnachricht in Byte, einschließlich Anforderungsheader und -text.                                                                                                   |
| ResponseBytes         | Die vom Back-End-Server als Antwort gesendeten Bytes.                                                                                                                                                    |
| UserAgent             | Der vom Client verwendete Browsertyp                                                                                                                                                               |
| ClientIp              | Die IP-Adresse des Clients, der die Anforderung gestellt hat.                                                                                                                                                  |
| TimeTaken             | Die Dauer der Aktion in Millisekunden.                                                                                                                                            |
| SecurityProtocol      | Die TLS-/SSL-Protokollversion, die von der Anforderung verwendet wird, oder NULL, wenn keine Verschlüsselung verwendet wird.                                                                                                                           |
| Endpunkt              | Der CDN-Endpunkthost wurde unter dem übergeordneten CDN-Profil konfiguriert.                                                                                                                                   |
| Back-End-Hostname     | Der Name des Back-End-Hosts oder Ursprungs, an den Anforderungen gesendet werden.                                                                                                                                |
| An Origin Shield gesendet </br> (veraltet) * **Siehe nachfolgenden Hinweis zur eingestellten Unterstützung.** | Wenn der Wert TRUE ist, wurde die Anforderung vom Origin Shield-Cache anstelle des Edge-POP beantwortet. Origin Shield ist ein übergeordneter Cache zum Verbessern der Cachetrefferquote.                                       |
| isReceivedFromClient | Wenn dies „true“ ergibt, stammte die Anforderung vom Client. Wenn dies „false“ ergibt, ist die Anforderung ein Fehler in der Edge (untergeordneter POP) und wird von Origin Shield (übergeordneter POP) beantwortet. 
| HttpStatusCode        | Der HTTP-Statuscode, der vom Proxy zurückgegeben wurde.                                                                                                                                                        |
| HttpStatusDetails     | Resultierender Status in der Anforderung. Informationen zur Bedeutung dieses Zeichenfolgenwerts finden Sie in der Verweistabelle zum Status.                                                                                              |
| POP                   | Der Edge-POP, der auf die Benutzeranforderung geantwortet hat. Die POP-Abkürzungen sind Flughafencodes der jeweiligen Regionen.                                                                                   |
| Cachestatus          | Gibt an, ob das Objekt aus dem Cache zurückgegeben wurde oder vom Ursprung stammt.                                                                                                             |
> [!NOTE]
> Die Protokolle können unter Ihrem Log Analytics-Profil durch Ausführen einer Abfrage angezeigt werden. Eine Beispielabfrage sieht wie folgt aus:
    ```
    AzureDiagnostics | where Category == "AzureCdnAccessLog"
    ```

### <a name="sent-to-origin-shield-deprecation"></a>An eingestellte Unterstützung von Origin Shield gesendet
Die unformatierte Protokolleigenschaft **isSentToOriginShield** wurde eingestellt und durch ein neues Feld, **isReceivedFromClient** , ersetzt. Verwenden Sie das neue Feld, wenn Sie bereits das veraltete Feld verwenden. 

Unformatierte Protokolle umfassen Protokolle, die sowohl über CDN Edge (untergeordneter POP) als auch Origin Shield generiert wurden. Origin Shield bezieht sich auf übergeordnete Knoten, die strategisch über den Globus verteilt sind. Diese Knoten kommunizieren mit den Ursprungsservern und reduzieren den Datenverkehr am Ursprung. 

Für jede an Origin Shield gerichtete Anforderung gibt es zwei Protokolleinträge:

* Einen für Edgeknoten.
* Einen für Origin Shield. 

Um die ausgehenden Daten oder Antworten von den Edgeknoten von denen von Origin Shield zu unterscheiden, können Sie das Feld **isReceivedFromClient** verwenden, um die richtigen Daten zu erhalten. 

Wenn der Wert „false“ ergibt, bedeutet dies, dass die Anforderung von Origin Shield bis zu den Edgeknoten beantwortet wird. Dieser Ansatz ist effektiv, um unformatierte Protokolle mit Abrechnungsdaten zu vergleichen. Für die ausgehenden Daten von Origin Shield zu den Edgeknoten fallen keine Gebühren an. Es fallen Gebühren für ausgehende Daten von den Edgeknoten zu Clients an. 

**Beispiel für eine Kusto-Abfrage zum Ausschließen von Protokollen, die von Origin Shield in Log Analytics generiert werden.**

```kusto
AzureDiagnostics 
| where OperationName == "Microsoft.Cdn/Profiles/AccessLog/Write" and Category == "AzureCdnAccessLog"  
| where isReceivedFromClient == true

```

> [!IMPORTANT]
> Das Feature der unformatierten HTTP-Protokolle ist automatisch für alle Profile verfügbar, die nach dem **25. Februar 2020** erstellt oder aktualisiert wurden. Bei früher erstellten CDN-Profilen sollte der CDN-Endpunkt nach der Einrichtung der Protokollierung aktualisiert werden. Sie können z. B. zur geografischen Filterung unter CDN-Endpunkte navigieren und jedes Land/jede Region, das bzw. die für ihre Workload nicht relevant ist, blockieren und auf „Speichern“ klicken.


## <a name="metrics"></a>Metriken
Azure CDN von Microsoft ist mit Azure Monitor integriert und veröffentlicht vier CDN-Metriken, um Probleme zu behandeln, nachzuverfolgen und zu debuggen. 

Die Metriken werden in Diagrammen angezeigt und können über PowerShell, die Befehlszeilenschnittstelle und die API aufgerufen werden. Die CDN-Metriken sind kostenlos verfügbar.

Die Metriken von Azure CDN von Microsoft werden in Intervallen von 60 Sekunden gemessen und gesendet. Es kann bis zu 3 Minuten dauern, bis die Metriken im Portal angezeigt werden. 

Weitere Informationen finden Sie unter [Azure Monitor-Metriken](../azure-monitor/platform/data-platform-metrics.md).

**In Azure CDN von Microsoft unterstützte Metriken**

| Metriken         | BESCHREIBUNG                                                                                                      | Dimension                                                                                   |
|-----------------|------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| Bytetrefferquote* | Der Prozentsatz der ausgehenden Daten aus dem CDN-Cache, berechnet im Vergleich zu den ausgehenden Gesamtdaten                                      | Endpunkt                                                                                    |
| RequestCount    | Die Anzahl der in CDN bereitgestellten Clientanforderungen                                                                     | Endpunkt </br> Land des Clients </br> Region des Clients </br> HTTP-Status </br> HTTP-Statusgruppe |
| ResponseSize    | Die Anzahl der vom CDN-Edge als Antworten an Clients gesendeten Bytes                                                  |Endpunkt </br> Land des Clients </br> Region des Clients </br> HTTP-Status </br> HTTP-Statusgruppe                                                                                          |
| TotalLatency    | Die Gesamtzeit zwischen dem Empfang der Clientanforderung in CDN **und dem letzten von CDN an den Client gesendeten Antwortbyte** |Endpunkt </br> Land des Clients </br> Region des Clients </br> HTTP-Status </br> HTTP-Statusgruppe                                                                                             |

**_Bytetrefferquote = (vom Edge ausgehende Daten – vom Ursprung ausgehende Daten) : vom Edge ausgehende Daten_*

Von der Berechnung der Bytetrefferquote ausgeschlossene Szenarien:

* Sie konfigurieren entweder über die Regel-Engine oder das Verhalten beim Zwischenspeichern von Abfragezeichenfolgen explizit keinen Cache.
* Sie konfigurieren die Cache-Control-Direktive explizit mit „no-store“ oder „private cache“.

### <a name="metrics-configuration"></a>Metrikkonfiguration

1. Wählen Sie im Menü des Azure-Portals **Alle Ressourcen** >>  **\<your-CDN-profile>** aus.

2. Wählen Sie unter **Überwachung** die Option **Metriken** aus:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-03.png" alt-text="Metriken für CDN-Profil" border="true":::

3. Wählen Sie **Metrik hinzufügen** und dann die hinzuzufügende Metrik aus:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-04.png" alt-text="Hinzufügen und Auswählen der Metrik für CDN-Profil" border="true":::

4. Wählen Sie **Filter hinzufügen** aus, um einen Filter hinzuzufügen:
    
    :::image type="content" source="./media/cdn-raw-logs/raw-logs-05.png" alt-text="Anwenden eines Filters auf die Metrik" border="true":::

5. Wählen Sie **Apply splitting** (Teilung anwenden) aus, um Trends nach verschiedenen Dimensionen anzuzeigen:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-06.png" alt-text="Anwenden der Teilung auf die Metrik" border="true":::

6. Wählen Sie **Neues Diagramm** aus, um ein neues Diagramm hinzuzufügen:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-07.png" alt-text="Hinzufügen eines neuen Diagramms in der Metrikansicht" border="true":::

### <a name="alerts"></a>Alerts

Durch Auswählen von **Überwachung** >> **Warnungen** können Sie Warnungen für Microsoft CDN einrichten.

Wählen Sie **Neue Warnungsregel** für die im Abschnitt „Metriken“ aufgeführten Metriken aus:

:::image type="content" source="./media/cdn-raw-logs/raw-logs-08.png" alt-text="Konfigurieren von Warnungen für CDN-Endpunkt" border="true":::

Die Warnung wird basierend auf Azure Monitor abgerechnet. Weitere Informationen zu Warnungen finden Sie unter [Azure Monitor-Warnungen](../azure-monitor/platform/alerts-overview.md).

### <a name="additional-metrics"></a>Zusätzliche Metriken
Sie können zusätzliche Metriken über Azure Log Analytics und gegen einen Aufpreis unformatierte Protokolle aktivieren.

1. Führen Sie die Schritte oben zum Aktivieren der Diagnose aus, um unformatierte Protokolle an Log Analytics zu senden.

2. Wählen Sie den erstellten Log Analytics-Arbeitsbereich aus:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-09.png" alt-text="Auswählen des Log Analytics-Arbeitsbereichs" border="true":::   

3. Wählen Sie im Log Analytics-Arbeitsbereich unter **Allgemein** die Option **Protokolle** aus.  Wählen Sie dann **Erste Schritte** aus:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-10.png" alt-text="Log Analytics-Arbeitsbereich der Ressource" border="true":::   
 
4. Wählen Sie **CDN-Profile** aus.  Wählen Sie eine Beispielabfrage für die Ausführung aus, oder schließen Sie das Beispielfenster, um eine benutzerdefinierte Abfrage einzugeben:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-11.png" alt-text="Fenster mit Beispielabfragen" border="true":::   

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-12.png" alt-text="Abfrageausführung" border="true":::   

4. Wählen Sie **Diagramm** aus, um Daten nach Diagrammen anzuzeigen.  Wählen Sie **An Dashboard anheften** aus, um das Diagramm an das Azure-Dashboard anzuheften:

    :::image type="content" source="./media/cdn-raw-logs/raw-logs-13.png" alt-text="Heften Sie ein Diagramm an ein Dashboard an." border="true"::: 

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie unformatierte HTTP-Protokolle für den Microsoft CDN-Dienst aktiviert.

Weitere Informationen zu Azure CDN und den anderen in diesem Artikel erwähnten Azure-Diensten finden Sie unter den folgenden Quellen:

* [Analysieren](cdn-log-analysis.md) Sie Azure CDN-Verwendungsmuster.

* Weitere Informationen zu Azure Monitor finden Sie [hier](../azure-monitor/overview.md).

* Konfigurieren Sie [Log Analytics in Azure Monitor](../azure-monitor/log-query/get-started-portal.md).