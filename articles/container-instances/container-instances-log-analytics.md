---
title: Erfassen und Analysieren von Ressourcenprotokollen
description: Erfahren Sie, wie Sie Ressourcenprotokolle und Ereignisdaten aus Containergruppen in Azure Container Instances an Azure Monitor-Protokolle senden können.
ms.topic: article
ms.date: 07/13/2020
ms.openlocfilehash: b110ba46bdcf2741e5f16845f28fe8305bcee1a1
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2020
ms.locfileid: "92148649"
---
# <a name="container-group-and-instance-logging-with-azure-monitor-logs"></a>Protokollierung für Containergruppen und -instanzen mit Azure Monitor-Protokollen

Log Analytics-Arbeitsbereiche bieten einen zentralen Ort zum Speichern und Abfragen von Protokolldaten – sowohl für Azure-Ressourcen als auch für lokale Ressourcen und Ressourcen in anderen Clouds. In Azure Container Instances ist die Unterstützung für das Senden von Protokollen und Ereignisdaten an Azure Monitor-Protokolle bereits integriert.

Um Containergruppenprotokolle und -ereignisdaten an Azure Monitor-Protokolle zu senden, geben Sie beim Konfigurieren einer Containergruppe eine ID und den Arbeitsbereichsschlüssel eines vorhandenen Log Analytics-Arbeitsbereichs an. 

In den folgenden Abschnitten wird beschrieben, wie Sie eine Containergruppe mit aktivierter Protokollierung erstellen und Protokolle abfragen. Sie können [eine Containergruppe auch mit einer Arbeitsbereichs-ID und einem Arbeitsbereichsschlüssel aktualisieren](container-instances-update.md), um die Protokollierung zu aktivieren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> Derzeit können Sie nur Ereignisdaten von Linux-Containerinstanzen an Log Analytics senden.

## <a name="prerequisites"></a>Voraussetzungen

Zum Aktivieren der Protokollierung in Ihren Containerinstanzen benötigen Sie Folgendes:

* [Log Analytics-Arbeitsbereich](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (oder [Cloud Shell](../cloud-shell/overview.md))

## <a name="get-log-analytics-credentials"></a>Abrufen der Log Analytics-Anmeldeinformationen

Azure Container Instances benötigt die Berechtigung zum Senden von Daten an Ihren Log Analytics-Arbeitsbereich. Wenn Sie diese Berechtigung erteilen und die Protokollierung aktivieren möchten, müssen Sie beim Erstellen der Containergruppe die Log Analytics-Arbeitsbereichs-ID und einen der Schlüssel (Primär- oder Sekundärschlüssel) angeben.

So erhalten Sie die Log Analytics-Arbeitsbereichs-ID und den Primärschlüssel:

1. Navigieren Sie im Azure-Portal zu Ihrem Log Analytics-Arbeitsbereich.
1. Wählen Sie unter **Einstellungen** die Option **Agent-Verwaltung** aus.
1. Notieren Sie sich Folgendes:
   * **Arbeitsbereichs-ID**
   * **Primärschlüssel**

## <a name="create-container-group"></a>Erstellen einer Containergruppe

Mit der Log Analytics-Arbeitsbereichs-ID und dem Primärschlüssel können Sie nun eine protokollierungsfähige Containergruppe erstellen.

In den folgenden Beispielen werden zwei Möglichkeiten zum Erstellen einer Containergruppe gezeigt, die aus einem einzelnen [fluentd][fluentd]-Container besteht: Azure CLI und Azure CLI mit einer YAML-Vorlage. Der fluentd-Container generiert mehrere Ausgabezeilen in der Standardkonfiguration. Da diese Ausgabe an Ihren Log Analytics-Arbeitsbereich gesendet wird, eignet sie sich gut zur Veranschaulichung der Anzeige und Abfrage von Protokollen.

### <a name="deploy-with-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Für eine Bereitstellung über die Azure-Befehlszeilenschnittstelle geben Sie im Befehl [az container create][az-container-create] die Parameter `--log-analytics-workspace` und `--log-analytics-workspace-key` an. Ersetzen Sie die beiden Arbeitsbereichswerte durch die Werten, die Sie im vorherigen Schritt erhalten haben (und aktualisieren Sie den Ressourcengruppennamen), bevor Sie den folgenden Befehl ausführen.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>Bereitstellen mit YAML

Verwenden Sie diese Methode, wenn Sie Containergruppen lieber mit YAML bereitstellen möchten. Der folgende YAML-Code definiert eine Containergruppe mit einem einzelnen Container. Kopieren Sie den YAML-Code in eine neue Datei, und ersetzen Sie `LOG_ANALYTICS_WORKSPACE_ID` und `LOG_ANALYTICS_WORKSPACE_KEY` durch die im vorherigen Schritt erhaltenen Werte. Speichern Sie die Datei unter dem Namen **deploy-aci.yaml**.

```yaml
apiVersion: 2019-12-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Führen Sie als Nächstes den folgenden Befehl aus, um die Containergruppe bereitzustellen. Ersetzen Sie `myResourceGroup` durch eine Ressourcengruppe in Ihrem Abonnement (oder erstellen Sie zuerst eine Ressourcengruppe namens „myResourceGroup“):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Sie sollten kurz nach Ausgabe des Befehls eine Antwort von Azure mit den Bereitstellungsdetails erhalten.

## <a name="view-logs"></a>Anzeigen von Protokollen

Nach der Bereitstellung der Containergruppe kann es bis zu 10 Minuten dauern, bis die ersten Protokolleinträge im Azure-Portal angezeigt werden. 

So zeigen Sie die Protokolle der Containergruppen in der Tabelle `ContainerInstanceLog_CL` an:

1. Navigieren Sie im Azure-Portal zu Ihrem Log Analytics-Arbeitsbereich.
1. Wählen Sie unter **Allgemein** die Option **Protokolle** aus.  
1. Geben Sie die folgende Abfrage ein: `ContainerInstanceLog_CL | limit 50`.
1. Wählen Sie **Ausführen** aus.

Von der Abfrage sollten mehrere Ergebnisse angezeigt werden. Falls anfangs keine Ergebnisse angezeigt werden, warten Sie einige Minuten, und wählen Sie dann die Schaltfläche **Ausführen** aus, um die Abfrage erneut auszuführen. Protokolleinträge werden standardmäßig im Format **Tabelle** angezeigt. Sie können dann eine Zeile erweitern, um den Inhalt eines einzelnen Protokolleintrags anzuzeigen.

![Protokollsuchergebnisse im Azure-Portal][log-search-01]

## <a name="view-events"></a>Anzeigen von Ereignissen

Sie können auch Ereignisse für Containerinstanzen im Azure-Portal anzeigen. Die Ereignisse beinhalten den Erstellungszeitpunkt und die Startzeit der Instanz. So zeigen Sie die Ereignisdaten in der Tabelle `ContainerEvent_CL` an:

1. Navigieren Sie im Azure-Portal zu Ihrem Log Analytics-Arbeitsbereich.
1. Wählen Sie unter **Allgemein** die Option **Protokolle** aus.  
1. Geben Sie die folgende Abfrage ein: `ContainerEvent_CL | limit 50`.
1. Wählen Sie **Ausführen** aus.

Von der Abfrage sollten mehrere Ergebnisse angezeigt werden. Falls anfangs keine Ergebnisse angezeigt werden, warten Sie einige Minuten, und wählen Sie dann die Schaltfläche **Ausführen** aus, um die Abfrage erneut auszuführen. Einträge werden standardmäßig im **Tabellenformat** angezeigt. Sie können dann eine Zeile erweitern, um den Inhalt eines einzelnen Eintrags anzuzeigen.

![Ereignissuchergebnisse im Azure-Portal][log-search-02]

## <a name="query-container-logs"></a>Abfragen von Containerprotokollen

Azure Monitor-Protokolle beinhalten eine umfassende [Abfragesprache][query_lang], um Informationen aus Tausenden von Protokollausgabezeilen abzurufen.

Die Grundstruktur einer Abfrage ist die Quelltabelle (in diesem Artikel `ContainerInstanceLog_CL` oder `ContainerEvent_CL`), gefolgt von einer Reihe von Operatoren, die durch einen senkrechten Strich (`|`) getrennt sind. Sie können mehrere Operatoren verketten, um die Ergebnisse einzugrenzen und erweiterte Funktionen auszuführen.

Fügen Sie zum Anzeigen von Beispielabfrageergebnissen die folgende Abfrage in das Abfragetextfeld ein, und wählen Sie die Schaltfläche **Ausführen** aus, um die Abfrage auszuführen. Diese Abfrage zeigt alle Protokolleinträge an, deren Feld „Message“ das Wort „warn“ enthält:

```query
ContainerInstanceLog_CL
| where Message contains "warn"
```

Komplexere Abfragen werden ebenfalls unterstützt. Diese Abfrage zeigt beispielsweise nur die Protokolleinträge für die Containergruppe „mycontainergroup001“ an, die innerhalb der letzten Stunde generiert wurden:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Nächste Schritte

### <a name="azure-monitor-logs"></a>Azure Monitor-Protokolle

Weitere Informationen zum Abfragen von Protokollen und Konfigurieren von Warnungen in Azure Monitor-Protokollen finden Sie in den folgenden Themen:

* [Analysieren von Protokolldaten in Azure Monitor](../azure-monitor/log-query/log-query-overview.md)
* [Die neue Oberfläche „Warnungen“ in Azure Monitor](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>Überwachen von Container-CPU und -Arbeitsspeicher

Informationen zur Überwachung der CPU- und Arbeitsspeicherressourcen von Containerinstanzen finden Sie unter folgendem Link:

* [Überwachen von Containerressourcen in Azure Container Instances](container-instances-monitor.md)

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png
[log-search-02]: ./media/container-instances-log-analytics/portal-query-02.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: /azure/data-explorer/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create