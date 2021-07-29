---
title: Installieren Sie Event Grid auf einem Azure Arc-fähigen Kubernetes-Cluster
description: Dieser Artikel enthält Schritte zur Installation von Event Grid auf Azure Arc-fähigen Kubernetes-Clustern.
author: jfggdl
ms.author: jafernan
ms.subservice: kubernetes
ms.date: 05/26/2021
ms.topic: how-to
ms.openlocfilehash: d77d2c0c378d57c26f1496dc712c736a79c0c2ae
ms.sourcegitcommit: 6323442dbe8effb3cbfc76ffdd6db417eab0cef7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "110614543"
---
# <a name="install-event-grid-extension-on-azure-arc-enabled-kubernetes-cluster"></a>Installieren Sie die Event Grid-Erweiterung auf einem Azure Arc-fähigen Kubernetes-Cluster
Dieser Artikel führt Sie durch die Schritte zur Installation von Event Grid auf einem [Azure Arc-fähigen Kubernetes](../../azure-arc/kubernetes/overview.md)-Cluster.

Der Kürze halber wird in diesem Artikel „Event Grid auf der Kubernetes Erweiterung“ als „Event Grid auf Kubernetes“ oder einfach als „Event Grid“ bezeichnet.

[!INCLUDE [event-grid-preview-feature-note.md](../../../includes/event-grid-preview-feature-note.md)]


## <a name="supported-kubernetes-distributions"></a>Unterstützte Kubernetes-Distributionen
Nachfolgend sind die unterstützten Kubernetes-Versionen aufgeführt, auf denen Event Grid bereitgestellt und ausgeführt werden kann.

1. Von Azure AKS [unterstützte Kubernetes-Distributionen](../../aks/supported-kubernetes-versions.md).
1. RedHat [OpenShift Container-Plattform](https://www.openshift.com/products/container-platform).

Weitere Distributionen werden entsprechend [dem Feedback der Benutzer](https://feedback.azure.com/forums/909934-azure-event-grid) und seiner [Unterstützung durch Azure Arc-fähigen Kubernetes integriert.](../../azure-arc/kubernetes/validation-program.md)

## <a name="event-grid-extension"></a>Bezeichnung der Event Grid-Erweiterung
Der Vorgang zum Installieren einer Event Grid-Serviceinstanz in einem Kubernetes-Cluster ist die Erstellung einer Azure Arc-Clustererweiterung, die sowohl einen **Event Grid-Broker** als auch einen **Event Grid-Operator** bereitstellt. Weitere Informationen zur Funktion des Brokers und Operators finden Sie unter [Event Grid für Kubernetes-Komponenten.](concepts.md#event-grid-on-kubernetes-components) Die [ Azure Arc-Cluster-Erweiterungsfunktion](../../azure-arc/kubernetes/conceptual-extensions.md) ermöglicht die Lebenszyklusverwaltung mit Azure Resource Manager(ARM)-Steuerungsebenen-Vorgängen die zu Event Grid in Azure Arc-fähigen Kubernetes-Clustern bereitgestellt werden.

> [!NOTE]
> Die Vorschauversion des Diensts unterstützt nur eine einzelne Instanz der Event Grid-Erweiterung in einem Kubernetes-Cluster, da die Event Grid-Erweiterung derzeit als Erweiterung im Clusterbereich definiert ist. Es werden noch keine Bereitstellungen auf Namespace-Ebene für Event Grid unterstützt, die es ermöglichen würden, mehrere Instanzen in einem Cluster bereitzustellen.  Weitere Informationen zu Erweiterungsumfang finden Sie unter [Erstellen einer Erweiterungsinstanz und](../../azure-arc/kubernetes/extensions.md#create-extensions-instance) Suchen nach ``scope``.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit der Installation von Event Grid fortfahren, stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind. 

1. Ein Cluster, der in einer der [unterstützten Kubernetes-Distributionen ausgeführt wird](#supported-kubernetes-distributions).
1. [Ein Azure-Abonnement](https://azure.microsoft.com/en-us/free/).
1. [PKI-Zertifikate,](#pki-certificate-requirements) die zum Herstellen einer HTTPS-Verbindung mit dem Event Grid Broker verwendet werden.
1. [Verbinden Sie Ihren Cluster mit Azure Arc](../../azure-arc/kubernetes/quickstart-connect-cluster.md).

## <a name="getting-support"></a>Anfordern von Support
Wenn Sie auf ein Problem stoßen, finden Sie im Abschnitt [Fehlerbehebung](#troubleshooting) Unterstützung für häufige Umstände. Wenn weiterhin Probleme auftreten, [erstellen Sie eine Azure-Supportanfrage](get-support.md#how-to-create-a-support-request).

## <a name="pki-certificate-requirements"></a>PKI-Zertifikatanforderungen
Der Event Grid Broker (Server) bedient zwei Arten von Clients. Die Serverauthentifizierung erfolgt mithilfe von Zertifikaten. Die Clientauthentifizierung erfolgt entweder anhand von Zertifikaten oder SAS-Schlüsseln basierend auf dem Clienttyp.

- Event Grid Operatoren, die Anforderungen auf Steuerungsebene an den Event Grid Broker stellen, werden mithilfe von Zertifikaten authentifiziert.
- Event Grid Herausgeber von Ereignissen für ein Event Grid-Thema werden anhand den Thema-bezogenen SAS-Schlüsseln authentifiziert.

Um eine sichere HTTPS-Kommunikation mit dem Event Grid-Broker und Event Grid-Operator zu gewährleisten, verwenden wir PKI-Zertifikate während der Installation der Event Grid-Erweiterung. Dies sind die allgemeinen Anforderungen für diese PKI-Zertifikate:

1. Die Zertifikate und Schlüssel müssen [X.509](https://en.wikipedia.org/wiki/X.509)-Zertifikat und [Privacy-Enhanced Mail](https://en.wikipedia.org/wiki/Privacy-Enhanced_Mail) PEM-verschlüsselt sein.
1. Um das Event Grid Brokerzertifikat (Server) während der Installation zu einzurichten, müssen Sie Folgendes bereitstellen:
    1. Ein CA-Zertifikat
    1. Ein öffentliches Zertifikat
    1. Ein privater Schlüssel
1. Zur Konfiguration des Event Grid-Operator-Zertifikats (Client) müssen Sie Folgendes bereitstellen:
    1. Ein CA-Zertifikat
    1. Ein öffentliches Zertifikat
    1. Ein privater Schlüssel

    Veröffentlichungsclients können das Event Grid-Broker-CA-Zertifikat verwenden, um den Server beim Veröffentlichen von Ereignissen in einem Thema zu validieren.

    > [!IMPORTANT]
    > Während eine mit einem Client verbundene Domäne über mehr als ein öffentliches Zertifikat verfügen kann, das von verschiedenen Zertifizierungsstellen ausgestellt wurde, erlaubt Event Grid auf Kubernetes nur das Hochladen eines einzigen CA-Zertifikats für Clients bei der Installation von Event Grid. Folglich sollten die Zertifikate für den Event-Grid-Betreiber von der gleichen CA ausgestellt (signiert) sein, zur erfolgreichen Validierung der Zertifikatskette und der erfolgreichen Einrichtung einer TLS-Session.
1. Wenn Sie den Common Name (CN) für Server- und Client-Zertifikate konfigurieren, stellen Sie sicher, dass sie sich von dem CN unterscheiden, der für das Zertifikat der Zertifizierungsstelle bereitgestellt wird.

    > [!IMPORTANT] 
    > Für anfängliche Proof-of-Concept-Phasen können selbstsignierte Zertifikate eine Option sein, aber im Allgemeinen sollten ordnungsgemäße PKI-Zertifikate, die von einer Zertifizierungsstelle (CA) signiert sind, beschaffen und verwendet werden.

## <a name="install-using-azure-portal"></a>Installation mit dem Azure-Portal

1. Suchen Sie auf dem Azure-Portal (obiges Feld) nach **Azure Arc**
1. Wählen Sie im linken Menü im Abschnitt **Infrastruktur** die Option **Kubernetes-Cluster** aus.
1. Suchen Sie in der Liste der Cluster nach dem Cluster, in dem Sie Event Grid installieren möchten, und wählen Sie ihn aus. Die **Übersichts-** Seite für den Cluster wird angezeigt.

    :::image type="content" source="./media/install-k8s-extension/select-kubernetes-cluster.png" alt-text="Auswählen Ihres Kubernetes-Clusters":::
1. Wählen Sie **Erweiterungen** in der Gruppe **Einstellungen** im linken Menü.
1. Klicken Sie auf **+ Hinzufügen**. Eine Seite mit den verfügbaren Azure Arc Kubernetes-Erweiterungen wird angezeigt.

    :::image type="content" source="./media/install-k8s-extension/cluster-extensions-add.png" alt-text="Clustererweiterungen – Schaltfläche „Hinzufügen“":::    
1. Wählen Sie auf der Seite **Neue Ressource** **Event Grid in Kubernetes-Erweiterung** aus.

    :::image type="content" source="./media/install-k8s-extension/select-event-grid-extension.png" alt-text="Auswählen von „Event Grid in Kubernetes-Erweiterung“":::        
1. Wählen Sie auf der Seite **Event Grid in Kubernetes-Erweiterung** die Option **Erstellen** aus.

    :::image type="content" source="./media/install-k8s-extension/select-create-extension.png" alt-text="Auswählen des Erstellens der Kubernetes-Erweiterung":::            
1. Führen Sie auf der Seite **Event Grid installieren** auf der Registerkarte **Grundlagen** die folgenden Schritte aus. 
    1. Im **Projektdetails**-Abschnitt werden schreibgeschützte Abonnement- und Ressourcengruppenwerte angezeigt, da Azure Arc-Erweiterungen unter demselben Azure-Abonnement und derselben Ressourcengruppe des verbundenen Clusters bereitgestellt werden, auf dem sie installiert sind.
    1. Geben Sie im Feld **Name der Event Grid Erweiterung** einen Namen an. Dieser Name sollte unter anderen Azure Arc-Erweiterungen, die auf demselben mit Azure Arc verbundenen Cluster bereitgestellt werden, eindeutig sein.
    1. Für den **Release-Namespace** können Sie den Namen eines Kubernetes-Namespace angeben, in dem Event Grid-Komponenten bereitgestellt werden. Angenommen, Sie möchten, dass für alle Dienste mit Azure Arc-Unterstützung in Ihrem Cluster ein einzelner Namespace bereitgestellt wird. Der Standardwert ist **eventgrid-system**. Wenn der angegebene Namespace nicht vorhanden ist, wird er für Sie erstellt.
    1. Im Abschnitt **Event Grid Broker**-Details wird der Service-Typ angezeigt. Der Event-Grid-Broker, der die Komponente ist, welche die Topic-Endpunkte bereitstellt, an die Ereignisse gesendet werden, wird als Kubernetes-Dienst vom Typ **ClusterIP** bereitgestellt. Daher verwenden die IPs, die allen Themen zugewiesen sind, den für den Cluster konfigurierten privaten IP-Raum.
    1. Geben Sie den **Speicherklassennamen** an, den Sie für den Broker verwenden möchten und der von Ihrer Kubernetes-Distribution unterstützt wird. Wenn Sie z. B. AKS nutzen, könnten Sie `azurefile` verwenden, welches wiederum Azure Storage Standard verwendet. Weitere Informationen zu vordefinierten Speicherklassen, die von AKS unterstützt werden, finden Sie unter [Speicherklassen in AKS](../../aks/concepts-storage.md#storage-classes). Wenn Sie andere Kubernetes-Distributionen verwenden, finden Sie in der Dokumentation zur Kubernetes-Verteilung Informationen zu unterstützten vordefinierten Speicherklassen oder zur Bereitstellung Ihrer eigener Speicherklassen.
    1. **Speichergröße**. Der Standardwert ist 1 GB. Berücksichtigen Sie die Erfassungsrate, wenn Sie die Größe Ihres Speichers bestimmen. Die Erfassungsrate in MiB/Sekunde, gemessen als Größe Ihrer Ereignisse, mit der Veröffentlichungsrate (Ereignisse pro Sekunde) multipliziert in allen Themen auf dem Event Grid Broker ist ein wichtiger Faktor bei der Speicherzuweisung. Ereignisse sind vorübergehender Natur, und sobald sie übermittelt wurden, ist kein Speicherverbrauch für diese Ereignisse vorhanden. Die Erfassungsrate ist zwar ein Haupttreiber für die Speichernutzung, jedoch nicht der einzige. Die Metadaten, welche die Themen- und Ereignisabonnementkonfiguration enthalten, verbrauchen ebenfalls Speicherplatz, das erfordert jedoch in der Regel eine geringere Menge an Speicherplatz als die Ereignisse, die von Event Grid aufgenommen und geliefert werden.
    1. **Arbeitsspeicherlimit**. Der Standardwert ist 1 GB. 
    1. **Arbeitsspeicheranforderung**. Der Standardwert ist 200 MB. Dieses Feld kann nicht bearbeitet werden.

        :::image type="content" source="./media/install-k8s-extension/basics-page.png" alt-text="Installieren der Event Grid Erweiterung – Grundlagen-Seite":::
    1. Wählen Sie **Weiter: Konfiguration** im unteren Teil der Seite aus.
1. Gehen Sie auf der Registerkarte **Konfiguration** der Seite **Event Grid installieren** wie folgt vor: 
    1. **HTTP-Kommunikation aktivieren (unsicher)** . Aktivieren Sie dieses Kontrollkästchen, wenn Sie einen nicht gesicherten Kanal verwenden möchten, wenn Clients mit dem Event Grid Broker kommunizieren.

        > [!IMPORTANT]
        > Wenn Sie diese Option aktivieren, wird für die gesamte Kommunikation mit dem Event Grid Broker HTTP als Transport verwendet. Daher können ein beliebiger Publishing-Client und der Event-Grid-Betreiber nicht sicher mit dem Event-Grid-Broker kommunizieren. Sie sollten diese Option nur in den frühen Phasen der Entwicklung verwenden.
    1. Wenn Sie die HTTP-Kommunikation nicht aktiviert haben, wählen Sie jede der PKI-Zertifikatdateien aus, die Sie beschaffen haben, und erfüllen Sie die [PKI-Zertifikatanforderungen](#pki-certificate-requirements).

        :::image type="content" source="./media/install-k8s-extension/configuration-page.png" alt-text="Installieren der Event Grid Erweiterung – Einstellungen-Seite":::
    1. Wählen Sie am Ende der Seite **Weiter: Überwachen** aus.
1. Gehen Sie auf der Registerkarte **Überwachung** der Seite **Event Grid installieren** wie folgt vor:
    1. Wählen Sie **Metriken aktivieren** aus (optional). Wenn Sie diese Option auswählen, stellt Event Grid in Kubernetes Metriken für Themen und Ereignisabonnements im [Prometheus-Darstellungsformat](https://prometheus.io/docs/instrumenting/exposition_formats/) dar.

        :::image type="content" source="./media/install-k8s-extension/monitoring-page.png" alt-text="Installieren der Event Grid Erweiterung – Überwachungsseite":::    
    1. Wählen Sie **Weiter: Tags** um zur **Tags**-Seite zu navigieren. 
1. Führen Sie auf der Seite **Tags** die folgenden Schritte aus:
    1. Definieren Sie [Tags](/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging) bei Bedarf.

        :::image type="content" source="./media/install-k8s-extension/tags-page.png" alt-text="Installieren der Event Grid Erweiterung – Tags-Seite":::
    1. Wählen Sie am unteren Rand der Seite die Option **Bewerten + erstellen** aus.
1. Wählen Sie auf der Registerkarte **Überprüfen + erstellen** die Option **Erstellen** aus.
    
    :::image type="content" source="./media/install-k8s-extension/review-create-page.png" alt-text="Installieren der Event Grid-Erweiterung – Überprüfen und Erstellen-Seite":::   
    
    > [!IMPORTANT]
    > Bei der Installation von Event Grid handelt es sich um einen asynchronen Vorgang, dessen Ausführung auf dem Kubernetes-Cluster die Zeit überschreiten kann, zu der Sie im Azure-Portal eine Benachrichtigung sehen, dass die Bereitstellung abgeschlossen ist. Warten Sie mindestens 5 Minuten, nachdem eine Benachrichtigung mit dem Hinweis angezeigt wird, dass Ihre Bereitstellung abgeschlossen ist, bevor Sie versuchen, einen benutzerdefinierten Speicherort zu erstellen (nächster Schritt). Wenn Sie über Zugriff auf dem Kubernetes-Cluster verfügen, können Sie in einer Bash-Session den folgenden Befehl ausführen, um zu überprüfen, ob sich die Pods des Event Grid-Brokers und Event Grid-Operators im Ausführungs-Status befinden, was darauf hindeuten würde, dass die Installation abgeschlossen wurde:

    ```bash
    kubectl get pods -n \<release-namespace-name\>
    ```

    Hier ist die Beispielausgabe:

    ```bash
    NAME                                  READY   STATUS    RESTARTS   AGE
    eventgrid-broker-568f75976-wxkd2      1/1     Running   0          2m28s
    eventgrid-operator-6c4c6c675d-ttjv5   1/1     Running   0          2m28s    
    ```

    > [!IMPORTANT]
    > Ein benutzerdefinierter Speicherort muss erstellt werden, bevor versucht wird, Event Grid bereitzustellen. Um einen benutzerdefinierten Speicherort zu erstellen, können Sie die **Kontext**-Seite in den letzten 5 Minuten nach der Anzeige des Bereitstellungs-Abschlusses auswählen. Alternativ können Sie einen benutzerdefinierten Speicherort mithilfe des [Azure-Portals](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.ExtendedLocation%2FCustomLocations) erstellen. Weitere Informationen finden Sie unter [Erstellen und Verwalten benutzerdefinierter Standorte in Kubernetes mit Azure Arc-Unterstützung](../../azure-arc/kubernetes/custom-locations.md).
1. Nach der erfolgreichen Bereitstellung wird auf der Seite **Erweiterungen** ein Eintrag mit dem Namen angezeigt, den Sie für Ihre Event Grid-Erweiterung angegeben haben. Wenn für den **Installationsstatus** **Ausstehend** angezeigt wird, warten Sie einige Minuten, und wählen Sie dann auf der Symbolleiste **Aktualisieren** aus. 

    :::image type="content" source="./media/install-k8s-extension/extension-installed.png" alt-text="Event Grid-Erweiterung – installiert":::   

## <a name="install-using-azure-cli"></a>Installation mit der Azure CLI

1. Starten Sie eine Shellsitzung. Sie können eine Sitzung auf Ihrem Computer starten oder einen Browser für [https://shell.azure.com](https://shell.azure.com) öffnen.
1. Erstellen Sie die Konfigurationsdatei ``protected-settings-extension.json``. Diese Datei wird beim Erstellen der Event Grid-Erweiterung als Parameter übergeben.

   Ersetzen Sie im folgenden Befehl und in jeder der Konfigurationszeilen ``filename`` durch den Namen, der das öffentliche Zertifikat, das Zertifizierungsstellenzertifikat oder den Schlüssel für den Operator (Client) oder Broker (Server) enthält. Alle bereitgestellten Zertifikate sollten base64-codiert ohne Zeilenumbruch sein. Daher die Verwendung des ``base64 --wrap=0``-Befehls. 

    ```bash
    echo "{ 
        \"eventgridoperator.identityCert.base64EncodedIdentityCert\":\"$(base64 <filename> --wrap=0)\",
        \"eventgridoperator.identityCert.base64EncodedIdentityKey\":\"$(base64 <filename> --wrap=0)\",
        \"eventgridoperator.identityCert.base64EncodedIdentityCaCert\":\"$(base64 <filename> --wrap=0)\",
        \"eventgridbroker.service.tls.base64EncodedServerCert\":  \"$(base64 <filename> --wrap=0)\" ,
        \"eventgridbroker.service.tls.base64EncodedServerKey\":  \"$(base64 <filename> --wrap=0)\" ,
        \"eventgridbroker.service.tls.base64EncodedServerCaCert\":  \"$(base64 <filename> --wrap=0)\" 
    }" > protected-settings-extension.json 
    ```
    
    Wenn das öffentliche Zertifikat für den Broker (erstes Konfigurationselement oben) beispielsweise ``client.cer`` heißt, sollte die erste Konfigurationszeile wie die folgende aussehen:

    ```bash
    \"eventgridoperator.identityCert.base64EncodedIdentityCert\":\"$(base64 client.cer --wrap=0)\",    
    ```

1. Erstellen Sie die Konfigurationsdatei ``settings-extension.json``. Diese Datei wird beim Erstellen der Event Grid-Erweiterung als Parameter übergeben.
    > [!IMPORTANT]
    > Sie dürfen die Werte für ``ServiceAccount`` und ``serviceType`` nicht ändern. In der Vorschauversion wird nur der Kubernetes-Diensttyp ``ClusterIP`` unterstützt.

    Geben Sie für ``storageClassName`` den Speicherklassennamen an, den Sie für den Broker verwenden möchten, und der von Ihrer Kubernetes-Distribution unterstützt wird. Wenn Sie z. B. AKS nutzen, könnten Sie `azurefile        ` verwenden, welches wiederum Azure Storage Standard verwendet. Weitere Informationen zu vordefinierten Speicherklassen, die von AKS unterstützt werden, finden Sie unter [Speicherklassen in AKS](../../aks/concepts-storage.md#storage-classes). Wenn Sie andere Kubernetes-Distributionen verwenden, finden Sie in der Dokumentation zur Kubernetes-Verteilung Informationen zu unterstützten vordefinierten Speicherklassen oder zur Bereitstellung Ihrer eigener Speicherklassen.

    Setzen Sie ``reporterType`` auf ``prometheus``, um Metriken für Themen und Ereignisabonnements im [Prometheus-Expositionsformat](https://prometheus.io/docs/instrumenting/exposition_formats/) zu aktivieren.  

    > [!IMPORTANT] 
    > Während der Vorschauversion ist die Verwendung eines Prometheus-Clients der einzige unterstützte Mechanismus zum Abrufen von Metriken. 

    ```bash
    echo "{
        \"Microsoft.CustomLocation.ServiceAccount\":\"eventgrid-operator\",
        \"eventgridbroker.service.serviceType\": \"ClusterIP\",
        \"eventgridbroker.dataStorage.storageClassName\": \"<storage_class_name>\",
        \"eventgridbroker.diagnostics.metrics.reporterType\":\"prometheus\"
    }" > settings-extension.json
    ```
    
1. Erstellen Sie eine Kubernetes-Erweiterung, die Event Grid-Komponenten in Ihrem Cluster installiert. 

   Für die Parameter ``cluster-name`` und ``resource-group`` müssen Sie die gleichen Namen verwenden, die beim [Verbinden Ihres Clusters mit Azure Arc](../../azure-arc/kubernetes/quickstart-connect-cluster.md) angegeben wurden.

   ``release-namespace`` ist der Namespace, in dem Event Grid-Komponenten bereitgestellt werden. Der Standardwert ist **eventgrid-system**. Möglicherweise möchten Sie einen Wert angeben, um den Standardwert zu überschreiben. Angenommen, Sie möchten, dass für alle Dienste mit Azure Arc-Unterstützung in Ihrem Cluster ein einzelner Namespace bereitgestellt wird. Wenn der angegebene Namespace nicht vorhanden ist, wird er für Sie erstellt.

    > [!IMPORTANT]
    > Während der Vorschauversion ist ``cluster`` der einzige Bereich, der beim Erstellen oder Aktualisieren einer Event Grid-Erweiterung unterstützt wird. Das bedeutet, dass der Dienst nur eine einzelne Instanz der Event Grid-Erweiterung in einem Kubernetes-Cluster unterstützt. Bereitstellungen im Namespacebereich werden noch nicht unterstützt. Weitere Informationen zu Erweiterungsumfang finden Sie unter [Erstellen einer Erweiterungsinstanz und](../../azure-arc/kubernetes/extensions.md#create-extensions-instance) Suchen nach ``scope``.

    ```azurecli-interactive
    az k8s-extension create --cluster-type connectedClusters --cluster-name <connected_cluster_name> --resource-group <resource_group_of_connected_cluster> --name <event_grid_extension_name> --extension-type Microsoft.EventGrid --scope cluster --auto-upgrade-minor-version true --release-train Stable --release-namespace <namespace_name> --configuration-protected-settings-file protected-settings-extension.json --configuration-settings-file settings-extension.json    
    ```
1. Überprüfen Sie, ob die Event Grid-Erweiterung erfolgreich installiert wurde.

    ```azurecli-interactive
    az k8s-extension show  --cluster-type connectedClusters --cluster-name <connected_cluster_name> --resource-group <resource_group_of_connected_cluster> --name <event_grid_extension_name>
    ```

    Die ``installedState``-Eigenschaft sollte ``Installed`` sein, wenn die Event Grid-Erweiterungskomponenten erfolgreich bereitgestellt wurden. 

### <a name="custom-location"></a>Benutzerdefinierter Speicherort

> [!IMPORTANT]
> Ein benutzerdefinierter Speicherort muss erstellt werden, bevor versucht wird, Event Grid bereitzustellen. Sie können mithilfe des [Azure-Portals](../../azure-arc/kubernetes/custom-locations.md#create-custom-location) einen benutzerdefinierten Speicherort erstellen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="azure-arc-connect-cluster-issues"></a>Azure Arc Connect-Cluster-Probleme

**Problem**: Wenn ich zu **Azure Arc** navigiere und im Menü auf der linken Seite auf **Kubernetes-Cluster** klicke, wird auf der angezeigten Seite nicht der Kubernetes-Cluster angezeigt, auf dem ich Event Grid installieren möchte.

**Lösung**: Ihr Kubernetes-Cluster ist nicht bei Azure registriert. Führen Sie die Schritte im Artikel [Verbinden eines vorhandenen Kubernetes-Clusters mit Azure Arc](../../azure-arc/kubernetes/quickstart-connect-cluster.md). Falls während dieses Schritts ein Problem auftritt, stellen Sie eine [Supportanfrage](#getting-support) an das Azure Arc Kubernetes-Team.

### <a name="event-grid-extension-issues"></a>Probleme mit der Event Grid-Erweiterung

**Problem:** Wenn Sie versuchen, eine „Event Grid-Erweiterung" zu installieren, erhalten Sie die folgende Meldung: „**Ungültiger Vorgang** - Eine Instanz des Event Grids wurde bereits auf diesem verbundenen Kubernetes-Cluster installiert. Die Event Grid-Erweiterung ist auf Clusterebene vorgesehen, was bedeutet, dass nur eine Instanz in einem Cluster installiert werden kann."
    
**Erklärung**: Sie haben bereits Event Grid installiert. Die Vorschauversion von Event Grid unterstützt nur eine Event Grid Erweiterungsinstanz, die in einem Cluster bereitgestellt wird.


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie einen benutzerdefinierten Speicherort](../../azure-arc/kubernetes/custom-locations.md), und befolgen Sie dann die Anweisungen im Schnellstart [Weiterleiten von Cloudereignissen an Webhooks mit Azure Event Grid auf Kubernetes](create-topic-subscription.md).
