---
title: Metrikwarnungen aus Azure Monitor für Container | Microsoft-Dokumentation
description: In diesem Artikel werden die empfohlenen Metrikwarnungen erläutert, die in Azure Monitor für Container in der öffentlichen Vorschauversion verfügbar sind.
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 1826896ad2d5c64d389219018f51238826c840d0
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563363"
---
# <a name="recommended-metric-alerts-preview-from-azure-monitor-for-containers"></a>Empfohlene Metrikwarnungen (Vorschau) aus Azure Monitor für Container

Um Warnungen zu Problemen mit Systemressourcen zu erhalten, sobald Bedarfsspitzen auftreten und die Kapazität nahezu erreicht ist, erstellen Sie mit Azure Monitor für Container eine Protokollwarnung basierend auf Leistungsdaten, die in Azure Monitor-Protokollen gespeichert werden. Azure Monitor für Container enthält jetzt vorkonfigurierte Metrikwarnungsregeln für Ihren AKS-Cluster, der sich in der öffentlichen Vorschau befindet.

In diesem Artikel werden die Funktionen beschrieben. Außerdem sind Anleitungen zum Konfigurieren und Verwalten dieser Warnungsregeln enthalten.

Wenn Sie mit Azure Monitor-Warnungen nicht vertraut sind, lesen Sie zunächst den [Überblick über Warnungen in Microsoft Azure](../platform/alerts-overview.md). Weitere Informationen zu Metrikwarnungen finden Sie unter [Metrikwarnungen in Azure Monitor](../platform/alerts-metric-overview.md).

## <a name="prerequisites"></a>Voraussetzungen

Vergewissern Sie sich, dass folgende Voraussetzungen erfüllt sind, bevor Sie beginnen:

* Benutzerdefinierte Metriken sind nur in einigen Azure-Regionen verfügbar. Eine Liste der unterstützten Regionen finden Sie [hier](../platform/metrics-custom-overview.md#supported-regions).

* Zur Unterstützung von Metrikwarnungen und der Einführung zusätzlicher Metriken ist mindestens die Agent-Version **microsoft/oms:ciprod05262020** erforderlich.

    Sie haben folgende Möglichkeiten, um zu überprüfen, ob in Ihrem Cluster die neuere Version des Agents ausgeführt wird:

    * Führen Sie den Befehl `kubectl describe <omsagent-pod-name> --namespace=kube-system` aus. Achten Sie im zurückgegebenen Status auf den Wert für „omsagent“ unter **Image** im Abschnitt *Container* der Ausgabe. 
    * Wählen Sie auf der Registerkarte **Knoten** den Clusterknoten aus, und sehen Sie sich rechts im Bereich **Eigenschaften** den Wert unter **Agent-Imagetag** an.

    Bei dem angezeigten Wert sollte es sich um eine neuere Version als **ciprod05262020** handeln. Wenn in Ihrem Cluster eine ältere Version vorhanden ist, führen Sie die Schritte für ein [Upgrade des Agents im AKS-Cluster](container-insights-manage-agent.md#upgrade-agent-on-aks-cluster) durch, um die neueste Version zu erhalten.
    
    Weitere Informationen zur Agent-Version finden Sie im [Agent-Versionsverlauf](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). Um festzustellen, ob Metriken gesammelt werden, können Sie im Azure Monitor-Metrik-Explorer überprüfen, ob unter **Metriknamespace** der Eintrag **insights** aufgeführt ist. Wenn das der Fall ist, können Sie mit dem Einrichten der Warnungen beginnen. Wenn keine Metriken erfasst werden, fehlen dem Clusterdienstprinzipal oder der MSI die erforderlichen Berechtigungen. Um zu überprüfen, ob der SPN oder die MSI Mitglied der Rolle **Herausgeber von Überwachungsmetriken** ist, führen Sie die im Abschnitt [Aktualisieren pro Cluster mit der Azure-Befehlszeilenschnittstelle](container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli) beschriebenen Schritte aus, mit denen Sie die Rollenzuweisung bestätigen und festlegen können.

## <a name="alert-rules-overview"></a>Übersicht über Warnungsregeln

Um Warnungen zu wichtigen Aspekten zu erhalten, enthält Azure Monitor für Container die folgenden Metrikwarnungen für Ihre AKS-Cluster:

|Name| BESCHREIBUNG |Standardschwellenwert |
|----|-------------|------------------|
|Durchschnittliche CPU-Nutzung für Container in % |Berechnet die durchschnittliche CPU-Nutzung pro Container.|Wenn die durchschnittliche CPU-Nutzung pro Container größer als 95 % ist.| 
|Durchschnittliche Speichernutzung für Arbeitssatz für Container in % |Berechnet die durchschnittliche Speichernutzung für den Arbeitssatz pro Container.|Wenn die durchschnittliche Speichernutzung für den Arbeitssatz pro Container größer als 95 % ist. |
|Durchschnittliche CPU-Nutzung in % |Berechnet die durchschnittliche CPU-Nutzung pro Knoten. |Wenn die durchschnittliche CPU-Nutzung durch den Knoten größer als 80 % ist. |
|Durchschnittliche Datenträgernutzung in % |Berechnet die durchschnittliche Datenträgernutzung für einen Knoten.|Wenn die Datenträgernutzung für einen Knoten größer als 80 % ist. |
|Durchschnittliche Speichernutzung für Arbeitssatz in % |Berechnet die durchschnittliche Speichernutzung für den Arbeitssatz für einen Knoten. |Wenn die durchschnittliche Speichernutzung für den Arbeitssatz für einen Knoten größer als 80 % ist. |
|Anzahl der neu gestarteten Container |Berechnet die Anzahl der neu gestarteten Container. | Wenn die Anzahl der Containerneustarts größer als 0 (null) ist. |
|Anzahl fehlerhafter Pods |Berechnet, ob ein Pod einen fehlerhaften Zustand aufweist.|Wenn die Anzahl von Pods in fehlerhaftem Zustand größer als 0 (null) ist. |
|Knotenstatus „NotReady“ |Berechnet, ob sich ein Knoten im Zustand „NotReady“ befindet.|Wenn die Anzahl von Knoten im Zustand „NotReady“ größer als 0 (null) ist. |
|Aufgrund von Arbeitsspeichermangel beendete Container |Berechnet die Anzahl der aufgrund von Arbeitsspeichermangel beendeten Container. |Wenn die Anzahl der aufgrund von Arbeitsspeichermangel beendeten Container größer als 0 (null) ist. |
|Bereite Pods in % |Berechnet den durchschnittlichen Bereitschaftszustand von Pods. |Wenn der Bereitschaftszustand von Pods kleiner als 80 % ist.|
|Anzahl abgeschlossener Aufträge |Berechnet die Anzahl der Aufträge, die vor mehr als sechs Stunden abgeschlossen wurden. |Wenn die Anzahl veralteter Aufträge, die älter als sechs Stunden sind, größer als 0 (null) ist.|

Es gibt allgemeine Eigenschaften für alle diese Warnungsregeln:

* Alle Warnungsregeln sind metrikbasiert.

* Alle Warnungsregeln sind standardmäßig deaktiviert.

* Alle Warnungsregeln werden einmal pro Minute ausgewertet, und es werden die Daten der letzten fünf Minuten einbezogen.

* Warnungsregeln sind standardmäßig keine Aktionsgruppen zugewiesen. Sie können der Warnung eine [Aktionsgruppe](../platform/action-groups.md) hinzufügen, indem Sie beim Bearbeiten der Warnungsregel eine bestehende Aktionsgruppe auswählen oder eine neue Aktionsgruppe erstellen.

* Sie können den Schwellenwert für Warnungsregeln durch direktes Bearbeiten ändern. Beachten Sie jedoch die Anweisungen in den einzelnen Warnungsregeln, bevor Sie den Schwellenwert ändern.

Die folgenden warnungsbasierten Metriken weisen im Vergleich zu den anderen Metriken besondere Verhaltensmerkmale auf:

* Die *completedJobsCount*-Metrik wird nur gesendet, wenn Aufträge vorhanden sind, die vor mehr als sechs Stunden abgeschlossen wurden.

* Die *containerRestartCount*-Metrik wird nur gesendet, wenn Container neu gestartet werden.

* Die *oomKilledContainerCount*-Metrik wird nur gesendet, wenn Container aufgrund von Arbeitsspeichermangel beendet wurden.

* Die Metriken *cpuExceededPercentage*, *memoryRssExceededPercentage* und *memoryWorkingSetExceededPercentage* werden gesendet, wenn die Werte für die CPU-Nutzung, den Arbeitsspeicher-RSS und den Arbeitssatz für Arbeitsspeicher den konfigurierten Schwellenwert (Standardschwellenwert: 95 %) überschreiten. Bei diesen Schwellenwerten ist der für die entsprechende Warnungsregel angegebene Schwellenwert für die Warnungsbedingung nicht enthalten. Das heißt, wenn Sie diese Metriken sammeln und aus dem [Metrik-Explorer](../platform/metrics-getting-started.md) heraus analysieren möchten, empfiehlt es sich, den Schwellenwert in der Konfiguration auf einen niedrigeren Wert als den Schwellenwert für Warnungen festzulegen. Die Konfiguration in Bezug auf die Sammlungseinstellungen für die Schwellenwerte der Containerressourcenauslastung kann in der Datei „ConfigMaps“ im Abschnitt `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` überschrieben werden. Weitere Informationen zum Konfigurieren der Konfigurationsdatei „ConfigMap“ finden Sie im Abschnitt [Konfigurieren von Warnungsmetriken in „ConfigMaps“](#configure-alertable-metrics-in-configmaps).

## <a name="metrics-collected"></a>Gesammelte Metriken

Die folgenden Metriken werden, sofern nicht anders angegeben, als Teil dieser Funktion aktiviert und gesammelt:

|Metriknamespace |Metrik |BESCHREIBUNG |
|---------|----|------------|
|Insights.container/nodes |cpuUsageMillicores |CPU-Auslastung in Millicores nach Host.|
|Insights.container/nodes |cpuUsagePercentage |Prozentuale CPU-Auslastung nach Knoten.|
|Insights.container/nodes |memoryRssBytes |Arbeitsspeicher-RSS-Auslastung in Byte nach Host.|
|Insights.container/nodes |memoryRssPercentage |Prozentuale Arbeitsspeicher-RSS-Auslastung nach Host.|
|Insights.container/nodes |memoryWorkingSetBytes |Auslastung des Arbeitssatzes für Arbeitsspeicher in Byte nach Host.|
|Insights.container/nodes |memoryWorkingSetPercentage |Prozentuale Auslastung des Arbeitssatzes für Arbeitsspeicher nach Host.|
|Insights.container/nodes |nodesCount |Anzahl von Knoten nach Status.|
|Insights.container/nodes |diskUsedPercentage |Prozentsatz der Datenträgernutzung auf dem Knoten nach Gerät.|
|Insights.container/pods |podCount |Anzahl von Pods nach Controller, Namespace, Knoten und Phase.|
|Insights.container/pods |completedJobsCount |Anzahl der abgeschlossenen Aufträge, die älter als der vom Benutzer konfigurierbare Schwellenwert sind (Standardwert: sechs Stunden), nach Controller, Kubernetes-Namespace. |
|Insights.container/pods |restartingContainerCount |Anzahl der Containerneustarts nach Controller, Kubernetes-Namespace.|
|Insights.container/pods |oomKilledContainerCount |Anzahl der aufgrund von Arbeitsspeichermangel beendeten Container nach Controller, Kubernetes-Namespace.|
|Insights.container/pods |podReadyPercentage |Prozentsatz der Pods im Bereitschaftszustand nach Controller, Kubernetes-Namespace.|
|Insights.container/containers |cpuExceededPercentage |Prozentuale CPU-Auslastung für Container, die den vom Benutzer konfigurierbaren Schwellenwert überschreiten (Standardwert: 95,0), nach Containername, Controllername, Kubernetes-Namespace, Podname.<br> Gesammelt  |
|Insights.container/containers |memoryRssExceededPercentage |Prozentualer Arbeitsspeicher-RSS für Container, die den vom Benutzer konfigurierbaren Schwellenwert überschreiten (Standardwert: 95,0), nach Containername, Controllername, Kubernetes-Namespace, Podname.|
|Insights.container/containers |memoryWorkingSetExceededPercentage |Prozentualer Arbeitssatz für Arbeitsspeicher für Container, die den vom Benutzer konfigurierbaren Schwellenwert überschreiten (Standardwert: 95,0), nach Containername, Controllername, Kubernetes-Namespace, Podname.|

## <a name="enable-alert-rules"></a>Aktivieren von Warnungsregeln

Führen Sie die folgenden Schritte aus, um die Metrikwarnungen in Azure Monitor über das Azure-Portal zu aktivieren. Informationen zum Aktivieren mithilfe einer Resource Manager-Vorlage finden Sie unter [Aktivieren mit einer Resource Manager-Vorlage](#enable-with-a-resource-manager-template).

### <a name="from-the-azure-portal"></a>Über das Azure-Portal

In diesem Abschnitt wird das Aktivieren von Metrikwarnungen (Vorschau) in Azure Monitor für Container über das Azure-Portal erläutert.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Der Zugriff auf die Funktion für Metrikwarnungen (Vorschau) in Azure Monitor für Container ist direkt aus einem AKS-Cluster möglich. Dazu wählen Sie im Azure-Portal im linken Bereich die Option **Insights** aus.

3. Wählen Sie in der Befehlsleiste **Empfohlene Warnungen** aus.

    ![Option „Empfohlene Warnungen“ in Azure Monitor für Container](./media/container-insights-metric-alerts/command-bar-recommended-alerts.png)

4. Der Eigenschaftenbereich **Empfohlene Warnungen** wird automatisch rechts auf der Seite angezeigt. Standardmäßig sind alle Warnungsregeln in der Liste deaktiviert. Nachdem Sie **Aktivieren** ausgewählt haben, wird die Warnungsregel erstellt und der Regelname so aktualisiert, dass er einen Link zur Warnungsressource enthält.

    ![Eigenschaftenbereich „Empfohlene Warnungen“](./media/container-insights-metric-alerts/recommended-alerts-pane.png)

    Nachdem Sie den Umschalter **Aktivieren/Deaktivieren** auf das Aktivieren der Warnung gesetzt haben, wird eine Warnungsregel erstellt und der Regelname so aktualisiert, dass er einen Link zur tatsächlichen Warnungsressource enthält.

    ![Benachrichtigungsregel aktivieren](./media/container-insights-metric-alerts/recommended-alerts-pane-enable.png)

5. Warnungsregeln sind keine [Aktionsgruppen](../platform/action-groups.md) zugeordnet, um Benutzer zu benachrichtigen, dass eine Warnung ausgelöst wurde. Wählen Sie **Keine Aktionsgruppe zugewiesen** aus, und geben Sie auf der Seite **Aktionsgruppen** eine vorhandene Gruppe an, oder erstellen Sie eine Aktionsgruppe, indem Sie **Hinzufügen** bzw. **Erstellen** auswählen.

    ![Auswählen einer Aktionsgruppe](./media/container-insights-metric-alerts/select-action-group.png)

### <a name="enable-with-a-resource-manager-template"></a>Aktivieren mit einer Resource Manager-Vorlage

Sie können eine Azure Resource Manager-Vorlage und eine Parameterdatei verwenden, um die enthaltenen Metrikwarnungen in Azure Monitor zu erstellen.

Die grundlegenden Schritte lauten wie folgt:

1. Laden Sie eine oder alle verfügbaren Vorlagen, die beschreiben, wie die Warnung erstellt wird, aus [GitHub](https://github.com/microsoft/Docker-Provider/tree/ci_dev/alerts/recommended_alerts_ARM) herunter.

2. Erstellen und verwenden Sie eine [Parameterdatei](../../azure-resource-manager/templates/parameter-files.md) in Form einer JSON-Datei, um die zum Erstellen der Warnungsregel erforderlichen Werte festzulegen.

3. Stellen Sie die Vorlage über das Azure-Portal, PowerShell oder die Azure CLI bereit.

#### <a name="deploy-through-azure-portal"></a>Bereitstellen über das Azure-Portal

1. Laden Sie die Azure Resource Manager-Vorlage und die Parameterdatei herunter, und speichern Sie sie in einem lokalen Ordner, um mithilfe der folgenden Befehle die Warnungsregel zu erstellen:

2. Wählen Sie zum Bereitstellen einer benutzerdefinierten Vorlage über das Portal die Option **Ressource erstellen** im [Azure-Portal](https://portal.azure.com) aus.

3. Suchen Sie nach **Vorlage**, und wählen Sie dann **Vorlage** aus. Bereitstellung.

4. Klicken Sie auf **Erstellen**.

5. Es werden mehrere Optionen zum Erstellen einer Vorlage angezeigt. Wählen Sie **Eigene Vorlage im Editor erstellen** aus.

6. Wählen Sie auf der Seite **Vorlage bearbeiten** die Option **Datei laden** aus, und wählen Sie dann die Vorlagendatei aus.

7. Wählen Sie auf der Seite **Vorlage bearbeiten** die Option **Speichern** aus.

8. Machen Sie auf der Seite **Benutzerdefinierte Bereitstellung** die folgenden Angaben, und wählen Sie anschließend **Kaufen** aus, um die Vorlage bereitzustellen und die Warnungsregel zu erstellen.

    * Resource group
    * Standort
    * Name der Warnung
    * Clusterressourcen-ID

#### <a name="deploy-with-azure-powershell-or-cli"></a>Bereitstellen mit Azure PowerShell oder CLI

1. Laden Sie die Azure Resource Manager-Vorlage und die Parameterdatei herunter, und speichern Sie sie in einem lokalen Ordner, um mithilfe der folgenden Befehle die Warnungsregel zu erstellen:

2. Sie können die metrische Warnung mithilfe der Vorlage und Parameterdatei mit PowerShell oder Azure CLI erstellen.

    Verwenden von Azure PowerShell

    ```powershell
    Connect-AzAccount

    Select-AzSubscription -SubscriptionName <yourSubscriptionName>
    New-AzResourceGroupDeployment -Name CIMetricAlertDeployment -ResourceGroupName ResourceGroupofTargetResource `
    -TemplateFile templateFilename.json -TemplateParameterFile templateParameterFilename.parameters.json
    ```

    Verwenden der Azure-Befehlszeilenschnittstelle

    ```azurecli
    az login

    az group deployment create \
    --name AlertDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file templateFileName.json \
    --parameters @templateParameterFilename.parameters.json
    ```

    >[!NOTE]
    >Die metrische Warnung könnte zwar in einer anderen Ressourcengruppe erstellt werden als der, wo sich die Zielressource befindet, Sie sollten jedoch die Ressourcengruppe der Zielressource verwenden.

## <a name="edit-alert-rules"></a>Bearbeiten von Warnungsregeln

Sie können Warnungsregeln in Azure Monitor für Container anzeigen und verwalten, um den Schwellenwert zu bearbeiten oder eine [Aktionsgruppe](../platform/action-groups.md) für Ihren AKS-Cluster zu konfigurieren. Sie können diese Aktionen über das Azure-Portal und mithilfe der Azure CLI ausführen, doch ist dies auch direkt aus dem AKS-Cluster in Azure Monitor für Container möglich.

1. Wählen Sie in der Befehlsleiste **Empfohlene Warnungen** aus.

2. Zum Ändern des Schwellenwerts wählen Sie im Bereich **Empfohlene Warnungen** die aktivierte Warnung aus. Wählen Sie unter **Regel bearbeiten** die **Warnungskriterien** aus, die Sie bearbeiten möchten.

    * Zum Ändern des Schwellenwerts für die Warnungsregel wählen Sie die **Bedingung** aus.
    * Wenn Sie eine vorhandene Aktionsgruppe angeben oder eine neue Gruppe erstellen möchten, wählen Sie unter **Aktionsgruppe** die Option **Hinzufügen** bzw.  **Erstellen** aus.

Zum Anzeigen von Warnungen, die für die aktivierten Regeln erstellt wurden, wählen Sie im Bereich **Empfohlene Warnungen** die Option **In Warnungen anzeigen** aus. Sie werden zum Warnungsmenü für den AKS-Cluster weitergeleitet, in dem Sie alle zurzeit für den Cluster erstellten Warnungen sehen können.

## <a name="configure-alertable-metrics-in-configmaps"></a>Konfigurieren von Warnungsmetriken in „ConfigMaps“

Führen Sie die folgenden Schritte aus, um die Konfigurationsdatei „ConfigMap“ so zu konfigurieren, dass die Standardschwellenwerte für die Containerressourcennutzung überschrieben werden. Diese Schritte gelten nur für die folgenden Warnungsmetriken.

* *cpuExceededPercentage*
* *memoryRssExceededPercentage*
* *memoryWorkingSetExceededPercentage*

1. Bearbeiten Sie in der YAML-Datei „ConfigMap“ den Abschnitt `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]`.

2. Um den Schwellenwert für *cpuExceededPercentage* in 90 % zu ändern und mit dem Sammeln dieser Metrik zu beginnen, wenn dieser Schwellenwert erreicht und überschritten wird, konfigurieren Sie die Datei „ConfigMap“ anhand des folgenden Beispiels.

    ```
    container_cpu_threshold_percentage = 90.0
    # Threshold for container memoryRss, metric will be sent only when memory rss exceeds or becomes equal to the following percentage
    container_memory_rss_threshold_percentage = 95.0
    # Threshold for container memoryWorkingSet, metric will be sent only when memory working set exceeds or becomes equal to the following percentage
    container_memory_working_set_threshold_percentage = 95.0
    ```

3. Führen Sie den folgenden kubectl-Befehl aus: `kubectl apply -f <configmap_yaml_file.yaml>`.

    Beispiel: `kubectl apply -f container-azm-ms-agentconfig.yaml`.

Es kann einige Minuten dauern, bis die Konfigurationsänderungen übernommen werden, und alle omsagent-Pods im Cluster werden neu gestartet. Der Neustart aller omsagent-Pods erfolgt gleitend, nicht alle werden gleichzeitig neu gestartet. Wenn die Neustarts abgeschlossen sind, wird eine Meldung angezeigt, die der folgenden ähnelt und das Ergebnis anzeigt: `configmap "container-azm-ms-agentconfig" created`.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Beispiele zu Protokollabfragen](container-insights-log-search.md#search-logs-to-analyze-data) an, die auch vordefinierte Abfragen enthalten. Mit diesen Materialien können Sie Auswertungen von bzw. Anpassungen für Warnungen, Visualisierungen und Analysen von Clustern vornehmen.

- Weitere Informationen zu Azure Monitor und darüber, wie Sie andere Aspekte Ihres Kubernetes-Clusters überwachen können, finden Sie unter [Anzeigen der Leistung von Kubernetes-Clustern](container-insights-analyze.md).
