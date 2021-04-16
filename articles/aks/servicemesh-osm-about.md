---
title: Öffnen des Dienst-Mesh (Vorschau)
description: Öffnen des Dienst-Mesh (OSM) in Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 3/12/2021
ms.custom: mvc
ms.author: pgibson
zone_pivot_groups: client-operating-system
ms.openlocfilehash: 0052c8d2f9b85c34d50a3e9d01253ecaf2d02bab
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2021
ms.locfileid: "106106712"
---
# <a name="open-service-mesh-aks-add-on-preview"></a>Open Service Mesh AKS-Zusatzmodul (Vorschau)

## <a name="overview"></a>Übersicht

[Öffnen des Dienst-Mesh (OSM)](https://docs.openservicemesh.io/) ist ein schlankes, erweiterbares, cloudbasiertes Dienst-Mesh, mit dem Benutzer die Standardfunktionen für die Wahrnehmbarkeit für sehr dynamische Microservice-Umgebungen einheitlich verwalten, sichern und erhalten können.

OSM führt eine Envoy-basierte Kontrollebene auf Kubernetes aus, kann mit [SMI](https://smi-spec.io/)-APIs konfiguriert werden und funktioniert, indem ein Envoy-Proxy als Sidecar-Container neben jeder Instanz Ihrer Anwendung injiziert wird. Der Envoy-Proxy enthält Regeln für Zugriffskontrollrichtlinien und führt diese aus, implementiert die Routing-Konfiguration und erfasst Metriken. Die Steuerebene konfiguriert die Proxys kontinuierlich, um sicherzustellen, dass die Richtlinien und Routing-Regeln auf dem neuesten Stand sind und dass die Proxys in Ordnung sind.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="capabilities-and-features"></a>Funktionen und Features

OSM bietet die folgenden Fähigkeiten und Funktionen, um ein Cloud-natives Service-Mesh für Ihre Azure Kubernetes Service (AKS)-Cluster bereitzustellen:

- Sichere Kommunikation von Dienst zu Dienst durch Aktivierung von mTLS

- Einfaches Onboarding von Anwendungen auf das Mesh durch Aktivierung der automatischen Sidecar-Injektion des Envoy-Proxys

- Einfache und transparente Konfigurationen für die Verlagerung des Datenverkehrs auf Bereitstellungen

- Fähigkeit, feinkörnige Zugriffskontrollrichtlinien für Dienste zu definieren und auszuführen

- Beobachtbarkeit und Einblicke in Anwendungsmetriken zur Fehlersuche und Überwachung von Diensten

- Integration mit externen Zertifikatsverwaltungsdiensten/-lösungen mit einer steckbaren Schnittstelle

## <a name="scenarios"></a>Szenarien

OSM kann Ihre AKS-Bereitstellungen in den folgenden Szenarien unterstützen:

- Verschlüsselte Kommunikation zwischen den im Cluster bereitgestellten Service-Endpunkten

- Verkehrsautorisierung von sowohl HTTP/HTTPS- als auch TCP-Verkehr im Netz

- Konfiguration von gewichteten Verkehrskontrollen zwischen zwei oder mehr Diensten für A/B- oder Canary-Bereitstellungen

- Erfassung und Anzeige von KPIs aus dem Anwendungsverkehr

## <a name="osm-service-quotas-and-limits-preview"></a>OSM-Dienst-Kontingente und-Grenzwerte (Vorschau)

Einschränkungen für die OSM-Vorschau für Dienst-Kontingente und-Grenzwerte finden Sie auf der [Seite AKS-Kontingente und regionale Grenzwerte](https://docs.microsoft.com/azure/aks/quotas-skus-regions).

::: zone pivot="client-operating-system-linux"

[!INCLUDE [Linux - download and install client binary](includes/servicemesh/osm/install-osm-binary-linux.md)]

::: zone-end

::: zone pivot="client-operating-system-macos"

[!INCLUDE [macOS - download and install client binary](includes/servicemesh/osm/install-osm-binary-macos.md)]

::: zone-end

::: zone pivot="client-operating-system-windows"

[!INCLUDE [Windows - download and install client binary](includes/servicemesh/osm/install-osm-binary-windows.md)]

::: zone-end

> [!WARNING]
> Versuchen Sie nicht, OSM mithilfe der Binärdatei zu installieren `osm install` . Dies führt zu einer Installation von OSM, die nicht als Add-on für AKS integriert ist.

### <a name="register-the-aks-openservicemesh-preview-feature"></a>Registrieren der Previewfunktion `AKS-OpenServiceMesh`

Um einen AKS-Cluster zu erstellen, der das Open Service Mesh-Add-on verwenden kann, müssen Sie das `AKS-OpenServiceMesh` Feature-Flag in Ihrem Abonnement aktivieren.

Registrieren Sie das Featureflag `AKS-OpenServiceMesh` mithilfe des Befehls [az feature register][az-feature-register], wie im folgenden Beispiel gezeigt:

```azurecli-interactive
az feature register --namespace "Microsoft.ContainerService" --name "AKS-OpenServiceMesh"
```

Es dauert einige Minuten, bis der Status _Registered (Registriert)_ angezeigt wird. Überprüfen Sie den Registrierungsstatus mithilfe des Befehls [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-OpenServiceMesh')].{Name:name,State:properties.state}"
```

Wenn der Vorgang abgeschlossen ist, können Sie die Registrierung des _Microsoft.ContainerService_-Ressourcenanbieters mit dem Befehl [az provider register][az-provider-register] aktualisieren:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="install-open-service-mesh-osm-azure-kubernetes-service-aks-add-on-for-a-new-aks-cluster"></a>Installieren von Open Service Mesh (OSM) Azure Kubernetes Service (AKS) Add-on für einen neuen AKS-Cluster

Für ein neues AKS-Cluster-Bereitstellungsszenario beginnen Sie mit einer brandneuen Bereitstellung eines AKS-Clusters und aktivieren das OSM-Add-on beim Erstellen des Clusters.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

In Azure ordnen Sie verwandte Ressourcen einer Ressourcengruppe zu. Erstellen Sie mit [az group create](/cli/azure/group#az-group-create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen _myOsmAksGroup_ am Standort _eastus2_ erstellt.

```azurecli-interactive
az group create --name <myosmaksgroup> --location <eastus2>
```

### <a name="deploy-an-aks-cluster-with-the-osm-add-on-enabled"></a>Bereitstellen eines AKS-Clusters mit aktiviertem OSM-Add-on

Sie werden nun einen neuen AKS-Cluster mit aktiviertem OSM-Add-on bereitstellen.

> [!NOTE]
> Bitte beachten Sie, dass der folgende AKS-Bereitstellungsbefehl ephemere Festplatten des Betriebssystems verwendet. Weitere Informationen zu [Kurzlebige Betriebssystemdatenträger für AKS](https://docs.microsoft.com/azure/aks/cluster-configuration#ephemeral-os)finden Sie unter

```azurecli-interactive
az aks create -n osm-addon-cluster -g <myosmaksgroup> --kubernetes-version 1.19.6 --node-osdisk-type Ephemeral --node-osdisk-size 30 --network-plugin azure --enable-managed-identity -a open-service-mesh
```

#### <a name="get-aks-cluster-access-credentials"></a>AKS-Cluster-Zugangsberechtigungen abrufen

Holen Sie sich die Zugangsdaten für den neuen Managed Kubernetes-Cluster.

```azurecli-interactive
az aks get-credentials -n <myosmakscluster> -g <myosmaksgroup>
```

## <a name="enable-open-service-mesh-osm-azure-kubernetes-service-aks-add-on-for-an-existing-aks-cluster"></a>Aktivieren von Open Service Mesh (OSM) Azure Kubernetes Service (AKS) Add-on für einen bestehenden AKS-Cluster

Für ein bestehendes AKS-Cluster-Szenario aktivieren Sie das OSM-Add-on für einen bestehenden AKS-Cluster, der bereits eingesetzt wurde.

### <a name="enable-the-osm-add-on-to-existing-aks-cluster"></a>Aktivieren des OSM-Add-on für einen vorhandenen AKS-Cluster

Um das AKS-OSM-Add-on zu aktivieren, müssen Sie den `az aks enable-addons --addons` Befehl ausführen, der den Parameter übergibt. `open-service-mesh`

```azurecli-interactive
az aks enable-addons --addons open-service-mesh -g <resource group name> -n <AKS cluster name>
```

Es sollte eine Ausgabe ähnlich der unten gezeigten Ausgabe angezeigt werden, um zu bestätigen, dass das AKS-OSM-Add-on installiert wurde.

```json
{- Finished ..
  "aadProfile": null,
  "addonProfiles": {
    "KubeDashboard": {
      "config": null,
      "enabled": false,
      "identity": null
    },
    "openServiceMesh": {
      "config": {},
      "enabled": true,
      "identity": {
...
```

## <a name="validate-the-aks-osm-add-on-installation"></a>Überprüfen der AKS-OSM-Add-on-Installation

Es gibt mehrere Befehle, die ausgeführt werden müssen, um zu überprüfen, ob alle Komponenten des AKS-OSM-Add-Ins aktiviert sind und ausgeführt werden:

Zuerst können wir die Add-on-Profile des Clusters-Abfragen, um den aktivierten Status der installierten Add-ons zu überprüfen. Der folgende Befehl sollte „true“ zurückgeben.

```azurecli-interactive
az aks list -g <resource group name> -o json | jq -r '.[].addonProfiles.openServiceMesh.enabled'
```

Die folgenden `kubectl` Befehle melden den Status des OSM-Controllers.

```azurecli-interactive
kubectl get deployments -n kube-system --selector app=osm-controller
kubectl get pods -n kube-system --selector app=osm-controller
kubectl get services -n kube-system --selector app=osm-controller
```

## <a name="accessing-the-aks-osm-add-on"></a>Zugreifen auf das AKS-OSM-Add-on

Zurzeit können Sie über die configMap auf die Konfiguration des OSM-Controllers zugreifen und diese konfigurieren. Um die Konfigurationseinstellungen des OSM-Controllers anzuzeigen, Fragen Sie die dieses von OSM-config mithilfe von `kubectl` ab, um die Konfigurationseinstellungen anzuzeigen.

```azurecli-interactive
kubectl get configmap -n kube-system osm-config -o json | jq '.data'
```

Die Ausgabe der OSM-Configmap sollte wie folgt aussehen:

```json
{
  "egress": "true",
  "enable_debug_server": "true",
  "enable_privileged_init_container": "false",
  "envoy_log_level": "error",
  "outbound_ip_range_exclusion_list": "169.254.169.254/32,168.63.129.16/32,<YOUR_API_SERVER_PUBLIC_IP>/32",
  "permissive_traffic_policy_mode": "true",
  "prometheus_scraping": "false",
  "service_cert_validity_duration": "24h",
  "use_https_ingress": "false"
}
```

Beachten Sie, dass die **permissive_traffic_policy_mode** auf **true** festgelegt ist. Der Modus „Permissive traffic policy“ in OSM ist ein Modus, in dem die Durchsetzung der [SMI](https://smi-spec.io/)-Verkehrsrichtlinie umgangen wird. In diesem Modus erkennt OSM automatisch Dienste, die Teil des Dienstnetzes sind, und programmiert Regeln für Datenverkehrsrichtlinien auf jedem Envoy-Proxy-Sidecar, um mit diesen Diensten kommunizieren zu können.

> [!WARNING]
> Bevor Sie fortfahren, vergewissern Sie sich bitte, dass der Modus „Permissive Traffic Policy“ auf **true** eingestellt ist; falls nicht, ändern Sie ihn bitte mit dem folgenden Befehl auf

```OSM Permissive Mode to True
kubectl patch ConfigMap -n kube-system osm-config --type merge --patch '{"data":{"permissive_traffic_policy_mode":"true"}}'
```

## <a name="deploy-a-new-application-to-be-managed-by-the-open-service-mesh-osm-azure-kubernetes-service-aks-add-on"></a>Bereitstellen einer neuen Anwendung, die vom Open Service Mesh (OSM) Azure Kubernetes Service (AKS) Add-on verwaltet werden soll

### <a name="before-you-begin"></a>Voraussetzungen

Die in dieser Anleitung beschriebenen Schritte gehen davon aus, dass Sie einen AKS-Cluster (Kubernetes `1.19+` und höher, mit aktiviertem Kubernetes RBAC) erstellt haben, eine `kubectl` Verbindung mit dem Cluster hergestellt haben (Wenn Sie bei einem dieser Punkte Hilfe benötigen, dann lesen Sie den [AKS-Schnellstart](./kubernetes-walkthrough.md) und haben das AKS OSM-Add-on installiert.

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI, Version 2.20.0 oder höher
- Erweiterung `aks-preview`, Version 0.5.5 oder höher
- OSM-Version v0.8.0 oder höher
- apt-get install jq

### <a name="create-namespaces-for-the-application"></a>Anlegen von Namensräumen für die Anwendung

In dieser exemplarischen Vorgehensweise wird die OSM Bookstore-Anwendung verwendet, die über die folgenden Kubernetes-Dienste verfügt:

- bookbuyer
- bookthief
- bookstore
- bookwarehouse

Legen Sie für jede dieser Anwendungskomponenten Namensräume an.

```azurecli-interactive
for i in bookstore bookbuyer bookthief bookwarehouse; do kubectl create ns $i; done
```

Die folgende Ausgabe wird angezeigt.

```Output
namespace/bookstore created
namespace/bookbuyer created
namespace/bookthief created
namespace/bookwarehouse created
```

### <a name="onboard-the-namespaces-to-be-managed-by-osm"></a>Einbindung der von OSM zu verwaltenden Namespaces

Wenn Sie die Namespaces zum OSM-Netz hinzufügen, ermöglicht dies dem OSM-Controller, die Envoy-Sidecar-Proxy-Container automatisch mit Ihrer Anwendung zu injizieren. Führen Sie den folgenden Befehl aus, um die Namespaces der OSM-Buchhandelsanwendung zu aktivieren.

```azurecli-interactive
osm namespace add bookstore bookbuyer bookthief bookwarehouse
```

Die folgende Ausgabe wird angezeigt.

```Output
Namespace [bookstore] successfully added to mesh [osm]
Namespace [bookbuyer] successfully added to mesh [osm]
Namespace [bookthief] successfully added to mesh [osm]
Namespace [bookwarehouse] successfully added to mesh [osm]
```

### <a name="deploy-the-bookstore-application-to-the-aks-cluster"></a>Bereitstellen der Bookstore-Anwendung auf dem AKS-Cluster

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookbuyer.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookthief.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookstore.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookwarehouse.yaml
```

Nachfolgend sind alle Bereitstellungen-Ausgänge zusammengefasst.

```Output
serviceaccount/bookbuyer created
service/bookbuyer created
deployment.apps/bookbuyer created

serviceaccount/bookthief created
service/bookthief created
deployment.apps/bookthief created

service/bookstore created
serviceaccount/bookstore created
deployment.apps/bookstore created

serviceaccount/bookwarehouse created
service/bookwarehouse created
deployment.apps/bookwarehouse created
```

### <a name="checkpoint-what-got-installed"></a>Prüfpunkt: Was wurde installiert?

Die Beispielanwendung „Bookstore“ ist eine mehrstufige App, die aus vier Diensten besteht, nämlich dem Bookbuyer, dem Bookhief, dem Bookstore und dem Bookwarehouse. Sowohl der bookbuyer- als auch der bookthief-Dienst kommunizieren mit dem bookstore-Dienst, um Bücher vom bookstore-Dienst abzurufen. Der Buchhandelsdienst holt Bücher aus dem Bookwarehouse, um den Bookbuyer und den Bookthief zu versorgen. Dies ist eine einfache mehrschichtige Anwendung, die gut zeigt, wie ein Dienstnetz zum Schutz und zur Autorisierung der Kommunikation zwischen den Anwendungsdiensten verwendet werden kann. Im weiteren Verlauf der Erläuterung werden wir die SMI-Richtlinien (Service Mesh Interface) aktivieren und deaktivieren, um den Diensten die Kommunikation über OSM zu erlauben oder zu verbieten. Unten sehen Sie ein Architekturdiagramm dessen, was für den Bookstore-Anwendung installiert wurde.

![Architektur der OSM-Bookbuyer-App](./media/aks-osm-addon/osm-bookstore-app-arch.png)

### <a name="verify-the-bookstore-application-running-inside-the-aks-cluster"></a>Überprüfen Sie die Bookstore-Anwendung, die innerhalb des AKS-Clusters läuft

Ab jetzt haben wir die Bookstore-Mulit-Container-Anwendung bereitgestellt, aber sie ist nur innerhalb des AKS-Clusters zugänglich. Spätere Tutorials werden Sie dabei unterstützen, die Anwendung außerhalb des Clusters über einen Eingangsdatencontroller zu exponieren. Für den Moment werden wir die Port-Weiterleitung nutzen, um auf die Bookbuyer-Anwendung innerhalb des AKS-Clusters zuzugreifen, um zu überprüfen, ob sie Bücher vom Bookstore-Service kauft.

Um zu überprüfen, dass die Anwendung innerhalb des Clusters läuft, werden wir einen Port-Forward verwenden, um sowohl die UI der Bookbuyer- als auch der Bookthief-Komponente anzuzeigen.

Lassen Sie uns zuerst den Namen des Bookbuyer-Pods ermitteln

```azurecli-interactive
kubectl get pod -n bookbuyer
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden. Ihrem Bookbuyer-Pod wird ein eindeutiger Name angefügt.

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookbuyer-7676c7fcfb-mtnrz   2/2     Running   0          7m8s
```

Sobald wir den Namen des Pods haben, können wir nun den Befehl port-forward verwenden, um einen Tunnel von unserem lokalen System zur Anwendung im AKS-Cluster einzurichten. Führen Sie den folgenden Befehl aus, um die Portweiterleitung für den lokalen Systemport 8080 einzurichten. Verwenden Sie erneut den angegebenen Bookbuyer-Pod-Namen.

> [!NOTE]
> Für alle Befehle zur Portweiterleitung verwenden Sie am besten ein zusätzliches Terminal, damit Sie diese Anleitung weiter durcharbeiten können und die Verbindung zum Tunnel nicht unterbrochen wird. Am besten ist es auch, wenn Sie den Port-Forward-Tunnel außerhalb der Azure Cloud Shell einrichten.

```Bash
kubectl port-forward bookbuyer-7676c7fcfb-mtnrz -n bookbuyer 8080:14001
```

Sie sollten eine ähnliche Ausgabe wie diese sehen.

```Output
Forwarding from 127.0.0.1:8080 -> 14001
Forwarding from [::1]:8080 -> 14001
```

Rufen Sie bei bestehender Portweiterleitung die folgende URL in einem Browser auf `http://localhost:8080`. Sie sollten nun in der Lage sein, die Benutzeroberfläche der Bookbuyer-Anwendung im Browser zu sehen, ähnlich wie in der Abbildung unten.

![OSM Bookbuyer App UI Image](./media/aks-osm-addon/osm-bookbuyer-service-ui.png)

Sie werden auch bemerken, dass die Anzahl der insgesamt gekauften Bücher weiter zum Bookstore v1 Dienst aufsteigt. Der Bookstore v2-Dienst wurde noch nicht bereitgestellt. Wir werden den Dienst „Bookstore v2“ einsetzen, wenn wir die SMI-Verkehrsteilungsrichtlinien demonstrieren.

Das Gleiche können Sie auch für den Bookthief-Dienst überprüfen.

```azurecli-interactive
kubectl get pod -n bookthief
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden. Ihrem Bookbuyer-Pod wird ein eindeutiger Name angefügt.

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookthief-59549fb69c-cr8vl   2/2     Running   0          15m54s
```

Port vorwärts an Bookdieb-Pod.

```Bash
kubectl port-forward bookthief-59549fb69c-cr8vl -n bookthief 8080:14001
```

Rufen Sie in einem Browser die folgende URL auf `http://localhost:8080`. Sie sollten sehen, dass der Bookthief gerade Bücher aus dem Bookstore-Dienst klaut! Später werden wir eine Datenverkehr-Richtlinie einführen, um den Bookthief zu stoppen.

![OSM Bookthief-App UI-Bild](./media/aks-osm-addon/osm-bookthief-service-ui.png)

### <a name="disable-osm-permissive-traffic-mode-for-the-mesh"></a>OSM-Modus „Permissiver Datenverkehr“ für das Netz deaktivieren

Wie bereits bei der Anzeige der OSM-Clusterkonfiguration erwähnt, ist die OSM-Konfiguration standardmäßig auf die Aktivierung der Richtlinie für den permissiven Datenverkehr-Modus eingestellt. In diesem Modus wird die Durchsetzung von Datenverkehrs-Richtlinien umgangen und OSM erkennt automatisch Dienste, die Teil des Dienstnetzes sind, und programmiert Datenverkehrs-Richtlinienregeln auf jedem Envoy-Proxy-Sidecar, um mit diesen Diensten kommunizieren zu können.

Nun deaktivieren wir die Richtlinie für den Zuordnungsmodus für den Datenverkehr, und OSM benötigt explizite [SMI](https://smi-spec.io/) -Richtlinien, die für den Cluster bereitgestellt werden, um die Kommunikation im Netz von jedem Dienst zuzulassen. Um den permissiven Datenverkehrsmodus zu deaktivieren, führen Sie den folgenden Befehl aus, um die Eigenschaft configmap zu aktualisieren, indem Sie den Wert von`true` auf `false` ändern.

```azurecli-interactive
kubectl patch ConfigMap -n kube-system osm-config --type merge --patch '{"data":{"permissive_traffic_policy_mode":"false"}}'
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden. Ihrem Bookbuyer-Pod wird ein eindeutiger Name angefügt.

```Output
configmap/osm-config patched
```

Um sicherzustellen, dass der Modus für den Datenverkehr deaktiviert wurde, wird der Anschluss zurück an den Bookbuyer- oder Bookthief-Pod weitergegeben, um die Benutzeroberfläche im Browser anzuzeigen und zu überprüfen, ob die gekauften Bücher oder die gestohlenen Bücher nicht mehr inkrementiert werden. Stellen Sie sicher, dass Sie den Browser neu laden. Wenn das Inkrementieren beendet wurde, wurde die Richtlinie ordnungsgemäß angewendet. Sie haben den Bücherdieb erfolgreich daran gehindert, Bücher zu stehlen, aber weder der Buchkäufer kann in der Buchhandlung einkaufen noch kann die Buchhandlung Bücher aus dem Buchlager holen. Als nächstes implementieren wir [SMI](https://smi-spec.io/) -Richtlinien, um nur die Dienste in dem Mesh zuzulassen, mit dem Sie kommunizieren möchten.

### <a name="apply-service-mesh-interface-smi-traffic-access-policies"></a>Anwenden der SMI-Richtlinien für den Datenverkehrs-Zugriff

Nachdem wir nun alle Kommunikationen im Mesh deaktiviert haben, erlauben wir unserem Dienst Buchkäufer, mit unserem Dienst Buchhandlung zu kommunizieren, um Bücher zu kaufen, und erlauben unserem Dienst Buchhandlung, mit unserem Dienst Buchlager zu kommunizieren, um Bücher zum Verkauf abzurufen.

Stellen Sie die folgenden [SMI](https://smi-spec.io/) -Richtlinien bereit.

```azurecli-interactive
kubectl apply -f - <<EOF
---
apiVersion: access.smi-spec.io/v1alpha3
kind: TrafficTarget
metadata:
  name: bookbuyer-access-bookstore
  namespace: bookstore
spec:
  destination:
    kind: ServiceAccount
    name: bookstore
    namespace: bookstore
  rules:
  - kind: HTTPRouteGroup
    name: bookstore-service-routes
    matches:
    - buy-a-book
    - books-bought
  sources:
  - kind: ServiceAccount
    name: bookbuyer
    namespace: bookbuyer
---
apiVersion: specs.smi-spec.io/v1alpha4
kind: HTTPRouteGroup
metadata:
  name: bookstore-service-routes
  namespace: bookstore
spec:
  matches:
  - name: books-bought
    pathRegex: /books-bought
    methods:
    - GET
    headers:
    - "user-agent": ".*-http-client/*.*"
    - "client-app": "bookbuyer"
  - name: buy-a-book
    pathRegex: ".*a-book.*new"
    methods:
    - GET
  - name: update-books-bought
    pathRegex: /update-books-bought
    methods:
    - POST
---
kind: TrafficTarget
apiVersion: access.smi-spec.io/v1alpha3
metadata:
  name: bookstore-access-bookwarehouse
  namespace: bookwarehouse
spec:
  destination:
    kind: ServiceAccount
    name: bookwarehouse
    namespace: bookwarehouse
  rules:
  - kind: HTTPRouteGroup
    name: bookwarehouse-service-routes
    matches:
    - restock-books
  sources:
  - kind: ServiceAccount
    name: bookstore
    namespace: bookstore
  - kind: ServiceAccount
    name: bookstore-v2
    namespace: bookstore
---
apiVersion: specs.smi-spec.io/v1alpha4
kind: HTTPRouteGroup
metadata:
  name: bookwarehouse-service-routes
  namespace: bookwarehouse
spec:
  matches:
    - name: restock-books
      methods:
      - POST
      headers:
      - host: bookwarehouse.bookwarehouse
EOF
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden.

```Output
traffictarget.access.smi-spec.io/bookbuyer-access-bookstore-v1 created
httproutegroup.specs.smi-spec.io/bookstore-service-routes created
traffictarget.access.smi-spec.io/bookstore-access-bookwarehouse created
httproutegroup.specs.smi-spec.io/bookwarehouse-service-routes created
```

Sie können nun eine Portweiterleitungssitzung auf dem Buchkäufer- oder dem Buchladen-Pod einrichten und sehen, dass sowohl die Metriken für gekaufte als auch für verkaufte Bücher wieder hochgezählt werden. Sie können dasselbe auch für den Buchdieb-Pod tun, um sicherzustellen, dass er immer noch nicht in der Lage ist, Bücher zu stehlen.

### <a name="apply-service-mesh-interface-smi-traffic-split-policies"></a>Anwenden von Richtlinien zur Aufteilung des Datenverkehrs an der Service-Mesh-Schnittstelle (SMI)

Für unsere abschließende Demonstration erstellen wir eine Richtlinie zum Aufteilen des [SMI](https://smi-spec.io/)-Datenverkehrs, um die Gewichtung der Kommunikation zwischen einem Dienst und mehreren Diensten als Backend zu konfigurieren. Mit der Traffic-Split-Funktion können Sie Verbindungen zu einem Dienst schrittweise auf einen anderen übertragen, indem Sie den Traffic auf einer Skala von 0 bis 100 gewichten.

Die folgende Grafik ist ein Diagramm der zu implementierenden [SMI](https://smi-spec.io/)-Traffic-Split-Richtlinie. Wir werden einen zusätzlichen Bookstore Version 2 bereitstellen und dann den eingehenden Datenverkehr vom Bookbuyer aufteilen, wobei wir 25 % des Datenverkehrs an den Bookstore v1-Dienst und 75 % an den Bookstore v2-Dienst gewichten.

![OSM-Bookbuyer-Traffic-Split-Diagramm](./media/aks-osm-addon/osm-bookbuyer-traffic-split-diagram.png)

Stellen Sie den Bookstore v2-Dienst bereit.

```azurecli-interactive
kubectl apply -f - <<EOF
---
apiVersion: v1
kind: Service
metadata:
  name: bookstore-v2
  namespace: bookstore
  labels:
    app: bookstore-v2
spec:
  ports:
  - port: 14001
    name: bookstore-port
  selector:
    app: bookstore-v2
---
# Deploy bookstore-v2 Service Account
apiVersion: v1
kind: ServiceAccount
metadata:
  name: bookstore-v2
  namespace: bookstore
---
# Deploy bookstore-v2 Deployment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bookstore-v2
  namespace: bookstore
spec:
  replicas: 1
  selector:
    matchLabels:
      app: bookstore-v2
  template:
    metadata:
      labels:
        app: bookstore-v2
    spec:
      serviceAccountName: bookstore-v2
      containers:
        - name: bookstore
          image: openservicemesh/bookstore:v0.8.0
          imagePullPolicy: Always
          ports:
            - containerPort: 14001
              name: web
          command: ["/bookstore"]
          args: ["--path", "./", "--port", "14001"]
          env:
            - name: BOOKWAREHOUSE_NAMESPACE
              value: bookwarehouse
            - name: IDENTITY
              value: bookstore-v2
---
kind: TrafficTarget
apiVersion: access.smi-spec.io/v1alpha3
metadata:
  name: bookbuyer-access-bookstore-v2
  namespace: bookstore
spec:
  destination:
    kind: ServiceAccount
    name: bookstore-v2
    namespace: bookstore
  rules:
  - kind: HTTPRouteGroup
    name: bookstore-service-routes
    matches:
    - buy-a-book
    - books-bought
  sources:
  - kind: ServiceAccount
    name: bookbuyer
    namespace: bookbuyer
EOF
```

Die folgende Ausgabe wird angezeigt.

```Output
service/bookstore-v2 configured
serviceaccount/bookstore-v2 created
deployment.apps/bookstore-v2 created
traffictarget.access.smi-spec.io/bookstore-v2 created
```

Verteilen Sie nun die Traffic-Split-Richtlinie, um den Bookbuyer-Verkehr auf die beiden Dienste Bookstore v1 und v2 aufzuteilen.

```azurecli-interactive
kubectl apply -f - <<EOF
apiVersion: split.smi-spec.io/v1alpha2
kind: TrafficSplit
metadata:
  name: bookstore-split
  namespace: bookstore
spec:
  service: bookstore.bookstore
  backends:
  - service: bookstore
    weight: 25
  - service: bookstore-v2
    weight: 75
EOF
```

Die folgende Ausgabe wird angezeigt.

```Output
trafficsplit.split.smi-spec.io/bookstore-split created
```

Richten Sie einen Port-Forward-Tunnel zum Bookbuyer-Pod ein und Sie sollten nun sehen, dass Bücher vom Bookstore v2-Dienst gekauft werden. Wenn Sie die Zunahme der Einkäufe weiter beobachten, sollten Sie eine schnellere Zunahme der Einkäufe über den Bookstore v2 Service feststellen.

![OSM-Bookbuyer-Bücher boough UI](./media/aks-osm-addon/osm-bookbuyer-traffic-split-ui.png)

## <a name="manage-existing-deployed-applications-to-be-managed-by-the-open-service-mesh-osm-azure-kubernetes-service-aks-add-on"></a>Verwaltung bestehender bereitgestellter Anwendungen, die vom Open Service Mesh (OSM) Azure Kubernetes Service (AKS) Add-on verwaltet werden sollen

### <a name="before-you-begin"></a>Voraussetzungen

Die in dieser Anleitung beschriebenen Schritte setzen voraus, dass Sie zuvor das OSM AKS-Add-on für Ihren AKS-Cluster aktiviert haben. Falls nicht, lesen Sie den Abschnitt [Open Service Mesh (OSM) Azure Kubernetes Service (AKS)-Add-on für einen vorhandenen AKS-Cluster aktivieren](#enable-open-service-mesh-osm-azure-kubernetes-service-aks-add-on-for-an-existing-aks-cluster), bevor Sie fortfahren. Außerdem muss Ihr AKS-Cluster Version Kubernetes `1.19+` und höher sein, die Kubernetes RBAC muss aktiviert sein und eine `kubectl` Verbindung mit dem Cluster hergestellt haben (wenn Sie Hilfe zu diesen Elementen benötigen, sehen Sie sich den [AKS-Schnellstart](./kubernetes-walkthrough.md)an und haben das AKS OSM-Add-on installiert.

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI, Version 2.20.0 oder höher
- Erweiterung `aks-preview`, Version 0.5.5 oder höher
- OSM-Version v0.8.0 oder höher
- apt-get install jq

### <a name="verify-the-open-service-mesh-osm-permissive-traffic-mode-policy"></a>Überprüfen Sie die OSM-Richtlinie (Open Service Mesh) für den Modus Permissiver Datenverkehr

Der Modus „Permissive Datenverkehr-Richtlinie“ in OSM ist ein Modus, in dem die Durchsetzung der [SMI](https://smi-spec.io/)-Verkehrsrichtlinie umgangen wird. In diesem Modus erkennt OSM automatisch Dienste, die Teil des Dienstnetzes sind, und programmiert Regeln für Datenverkehrsrichtlinien auf jedem Envoy-Proxy-Sidecar, um mit diesen Diensten kommunizieren zu können.

Führen Sie den folgenden Befehl aus, um den aktuellen Zuordnungsmodus des OSM für Ihren Cluster zu überprüfen:

```azurecli-interactive
kubectl get configmap -n kube-system osm-config -o json | jq '.data'
```

Die Ausgabe der OSM-Configmap sollte wie folgt aussehen:

```Output
{
  "egress": "true",
  "enable_debug_server": "true",
  "envoy_log_level": "error",
  "permissive_traffic_policy_mode": "true",
  "prometheus_scraping": "false",
  "service_cert_validity_duration": "24h",
  "use_https_ingress": "false"
}
```

Wenn die **permissive_traffic_policy_mode** auf „**true**“ festgelegt ist, können Sie Ihre Namespaces ohne jegliche Unterbrechung der Dienst-zu-Dienst-Kommunikation sicher integrieren. Wenn die **permissive_traffic_policy_mode** mit **false** konfiguriert ist, müssen Sie sicherstellen, dass die richtigen [SMI](https://smi-spec.io/) -Richtlinien für den Datenverkehrszugriff bereitgestellt werden, und dass Sie über ein Dienstkonto verfügen, das jeden in Namespace bereitgestellten Dienst repräsentiert. Befolgen Sie die Anleitungen für das Integrieren vorhandener bereitgestellter [Anwendungen mit einer Richtlinie für den Open Service Mesh (OSM), die als falsch konfiguriert ist](#onboard-existing-deployed-applications-with-open-service-mesh-osm-permissive-traffic-policy-configured-as-false)

### <a name="onboard-existing-deployed-applications-with-open-service-mesh-osm-permissive-traffic-policy-configured-as-true"></a>Onboarding bestehender bereitgestellter Anwendungen mit Open Service Mesh (OSM) Permissive Traffic-Richtlinie konfiguriert als True

Als Erstes fügen wir den/die bereitgestellten Namespace(s) der Anwendung dem OSM zur Verwaltung hinzu.

```azurecli-interactive
osm namespace add bookstore
```

Die folgende Ausgabe wird angezeigt.

```Output
Namespace [bookstore] successfully added to mesh [osm]
```

Als nächstes sehen wir uns die aktuelle Pod-Bereitstellung in Namespace an. Führen Sie den folgenden Befehl aus, um die Pods im angegebenen Namespace anzuzeigen.

```azurecli-interactive
kubectl get pod -n bookbuyer
```

Die folgende, ähnliche Ausgabe wird angezeigt:

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookbuyer-78666dcff8-wh6wl   1/1     Running   0          43s
```

Beachten Sie, dass die Spalte **BEREIT** mit dem Wert **1/1** angezeigt wird, was bedeutet, dass der Anwendungs-Pod nur einen Container hat. Als nächstes müssen wir Ihre Anwendungsbereitstellungen neu starten, damit OSM den Envoy-Sidecar-Proxy-Container mit Ihrem Anwendungs-Pod injizieren kann. Lassen Sie sich eine Liste der Bereitstellungen in dem Namespace anzeigen.

```azurecli-interactive
kubectl get deployment -n bookbuyer
```

Die folgende Ausgabe wird angezeigt.

```Output
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
bookbuyer   1/1     1            1           23h
```

Jetzt starten wir die Bereitstellung neu, um den Envoy-Sidecar-Proxy-Container mit Ihrem Anwendungs-Pod zu injizieren. Führen Sie den folgenden Befehl aus.

```azurecli-interactive
kubectl rollout restart deployment bookbuyer -n bookbuyer
```

Die folgende Ausgabe wird angezeigt.

```Output
deployment.apps/bookbuyer restarted
```

Sehen wir uns die Pods im Namespace erneut an:

```azurecli-interactive
kubectl get pod -n bookbuyer
```

Nun sehen Sie, dass in der Spalte **BEREIT** nun **2/2** Container angezeigt werden, die für Ihren Pod bereit sind. Der zweite Container ist der Envoy-Sidecar-Proxy.

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookbuyer-84446dd5bd-j4tlr   2/2     Running   0          3m30s
```

Wir können den Pod weiter untersuchen, um den Envoy-Proxy anzuzeigen, indem wir den Befehl beschreiben ausführen, um die Konfiguration anzuzeigen.

```azurecli-interactive
kubectl describe pod bookbuyer-84446dd5bd-j4tlr -n bookbuyer
```

```Output
Containers:
  bookbuyer:
    Container ID:  containerd://b7503b866f915711002292ea53970bd994e788e33fb718f1c4f8f12cd4a88198
    Image:         openservicemesh/bookbuyer:v0.8.0
    Image ID:      docker.io/openservicemesh/bookbuyer@sha256:813874bd2dc9c5a259b9657995348cf0822b905e29c4e86f21fdefa0ef21dcee
    Port:          <none>
    Host Port:     <none>
    Command:
      /bookbuyer
    State:          Running
      Started:      Tue, 23 Mar 2021 10:52:53 -0400
    Ready:          True
    Restart Count:  0
    Environment:
      BOOKSTORE_NAMESPACE:  bookstore
      BOOKSTORE_SVC:        bookstore
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from bookbuyer-token-zft2r (ro)
  envoy:
    Container ID:  containerd://f5f1cb5db8d5304e23cc984eb08146ea162a3e82d4262c4472c28d5579c25e10
    Image:         envoyproxy/envoy-alpine:v1.17.1
    Image ID:      docker.io/envoyproxy/envoy-alpine@sha256:511e76b9b73fccd98af2fbfb75c34833343d1999469229fdfb191abd2bbe3dfb
    Ports:         15000/TCP, 15003/TCP, 15010/TCP
    Host Ports:    0/TCP, 0/TCP, 0/TCP
```

Vergewissern Sie sich, dass Ihre Anwendung nach der Injektion des Envoy-Sidecar-Proxys noch funktionsfähig ist.

### <a name="onboard-existing-deployed-applications-with-open-service-mesh-osm-permissive-traffic-policy-configured-as-false"></a>Onboarding bestehender bereitgestellter Anwendungen mit Open Service Mesh (OSM) Permissive Traffic-Richtlinie konfiguriert als False

Wenn die OSM-Konfiguration für die permissive Datenverkehrsrichtlinie auf gesetzt ist, benötigt`false`OSM explizite [SMI](https://smi-spec.io/)-Datenverkehrs-Zugriffsrichtlinien, die für die Dienst-zu-Dienst-Kommunikation innerhalb Ihres Clusters eingesetzt werden. Derzeit verwendet OSM auch Kubernetes-Dienstkonten als Teil der Autorisierung der Dienst-zu-Dienst-Kommunikation. Um sicherzustellen, dass Ihre vorhandenen bereitgestellten Anwendungen kommunizieren, wenn Sie vom OSM-Mesh verwaltet werden, müssen wir überprüfen, ob ein Dienstkonto vorhanden ist, um Sie zu verwenden, die Anwendungsbereitstellung mit den Dienst Kontoinformationen aktualisieren und die Richtlinien für den [SMI](https://smi-spec.io/) -Datenverkehr anwenden.

#### <a name="verify-kubernetes-service-accounts"></a>Kubernetes-Dienstkonten verifizieren

Überprüfen Sie, ob Sie über ein Kubernetes-Dienstkonto im Namespace verfügen, in dem Ihre Anwendung bereitgestellt wird.

```azurecli-interactive
kubectl get serviceaccounts -n bookbuyer
```

Im Folgenden befindet sich ein Dienstkonto namens `bookbuyer` im Bookbuyer-Namespace.

```Output
NAME        SECRETS   AGE
bookbuyer   1         25h
default     1         25h
```

Wenn Sie kein anderes Dienstkonto als das Standardkonto aufgeführt haben, müssen Sie eines für Ihre Anwendung erstellen. Verwenden Sie den folgenden Befehl als Beispiel, um ein Dienstkonto im bereitgestellten Namespace der Anwendung zu erstellen.

```azurecli-interactive
kubectl create serviceaccount myserviceaccount -n bookbuyer
```

```Output
serviceaccount/myserviceaccount created
```

#### <a name="view-your-applications-current-deployment-specification"></a>Anzeigen der aktuellen Bereitstellungsspezifikation Ihrer Anwendung

Wenn Sie ein Dienstkonto aus dem vorherigen Abschnitt erstellen mussten, ist Ihre Anwendungsbereitstellung wahrscheinlich nicht mit einem bestimmten `serviceAccountName` in der Bereitstellungsspezifikation konfiguriert. Sie können die Bereitstellungsspezifikation Ihrer Anwendung mit den folgenden Befehlen anzeigen:

```azurecli-interactive
kubectl get deployment -n bookbuyer
```

Eine Liste der Bereitstellungen wird in der Ausgabe aufgeführt.

```Output
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
bookbuyer   1/1     1            1           25h
```

Wir beschreiben nun die Bereitstellung als Überprüfung, ob ein Dienstkonto im Abschnitt Pod-Vorlage aufgeführt ist.

```azurecli-interactive
kubectl describe deployment bookbuyer -n bookbuyer
```

In dieser speziellen Bereitstellung sehen Sie, dass ein Dienstkonto mit der im Abschnitt Pod-Vorlage aufgeführten Bereitstellung verknüpft ist. Bei dieser Bereitstellung wird das Dienstkonto „Buchkäufer“ verwendet. Wenn die Eigenschaft **Dienstkonto:** nicht angezeigt wird, ist die Bereitstellung nicht für die Verwendung eines Dienstkontos konfiguriert.

```Output
Pod Template:
  Labels:           app=bookbuyer
                    version=v1
  Annotations:      kubectl.kubernetes.io/restartedAt: 2021-03-23T10:52:49-04:00
  Service Account:  bookbuyer
  Containers:
   bookbuyer:
    Image:      openservicemesh/bookbuyer:v0.8.0

```

Es gibt mehrere Verfahren zum Aktualisieren Ihrer Bereitstellung, um ein Kubernetes-Dienstkonto hinzuzufügen. Lesen Sie die Dokumentation zu Kubernetes, um [eine Bereitstellung](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) Inline zu aktualisieren, oder [Konfigurieren Sie Dienstkonten für Pods](https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/). Nachdem Sie Ihre Bereitstellungsspezifikation mit dem Dienstkonto aktualisiert haben, stellen Sie die Bereitstellung für den Cluster erneut bereit (kubectl apply-f your-deployment.yaml).

#### <a name="deploy-the-necessary-service-mesh-interface-smi-policies"></a>Bereitstellen der erforderlichen SMI-Richtlinien

Der letzte Schritt, den autorisierten Datenverkehr im Mesh zuzulassen, besteht darin, die erforderlichen [SMI](https://smi-spec.io/)-Richtlinien für den Datenverkehr für Ihre Anwendung bereitzustellen. Der Umfang der Konfiguration, die Sie mit Richtlinien für den [SMI](https://smi-spec.io/)-Datenverkehr erreichen können, geht über den Rahmen dieser exemplarischen Vorgehensweise hinaus, aber wir erläutern einige der allgemeinen Komponenten der Spezifikation und zeigen Ihnen, wie Sie sowohl eine einfache TrafficTarget-als auch eine HTTPRouteGroup-Richtlinie konfigurieren, um die Kommunikation zwischen Diensten für Ihre Anwendung zu ermöglichen.

Mithilfe der [SMI](https://smi-spec.io/) - [**Datenverkehr-Zugriffssteuerung**](https://github.com/servicemeshinterface/smi-spec/blob/main/apis/traffic-access/v1alpha3/traffic-access.md#traffic-access-control)-Spezifikation können Benutzer die Zugriffssteuerungs-Richtlinie für Ihre Anwendungen definieren. Wir werden uns auf die api-Ressourcen **TrafficTarget** und **HTTPRoutGroup** konzentrieren.

Die Ressource TrafficTarget besteht aus drei wesentlichen Konfigurationseinstellungen Ziel, Regeln und Quellen. Ein Beispiel für ein TrafficTarget ist unten dargestellt.

```TrafficTarget Example spec
apiVersion: access.smi-spec.io/v1alpha3
kind: TrafficTarget
metadata:
  name: bookbuyer-access-bookstore-v1
  namespace: bookstore
spec:
  destination:
    kind: ServiceAccount
    name: bookstore
    namespace: bookstore
  rules:
  - kind: HTTPRouteGroup
    name: bookstore-service-routes
    matches:
    - buy-a-book
    - books-bought
  sources:
  - kind: ServiceAccount
    name: bookbuyer
    namespace: bookbuyer
```

In der obigen Spezifikation TrafficTarget bezeichnet das `destination` Dienstkonto, das für den Zielquellendienst konfiguriert ist. Merken Sie sich, dass das Dienstkonto, das der Bereitstellung hinzugefügt wurde, verwendet wird, um den Zugriff auf die Bereitstellung zu autorisieren. Der `rules` Abschnitt in diesem speziellen Beispiel definiert den Typ des HTTP-Datenverkehrs, der über die Verbindung zugelassen wird. Sie können feinkörnige Regex-Muster für die HTTP-Header konfigurieren, um genau festzulegen, welcher Datenverkehr über HTTP erlaubt ist. Der `sources` Abschnitt ist der Dienst, von dem die Kommunikation ausgeht. Diese Spezifikation liest Buchkäufer für die Kommunikation mit der Buchhandlung.

Die HTTPRouteGroup-Ressource besteht aus einem oder einem Array von Übereinstimmungen von HTTP-Header Informationen und ist eine Voraussetzung für die Spezifikation-Traffictarget. Im folgenden Beispiel können Sie sehen, dass HTTPRouteGroup drei HTTP-Aktionen autorisiert: zwei GET-und ein POST-Vorgang.

```HTTPRouteGroup Example Spec
apiVersion: specs.smi-spec.io/v1alpha4
kind: HTTPRouteGroup
metadata:
  name: bookstore-service-routes
  namespace: bookstore
spec:
  matches:
  - name: books-bought
    pathRegex: /books-bought
    methods:
    - GET
    headers:
    - "user-agent": ".*-http-client/*.*"
    - "client-app": "bookbuyer"
  - name: buy-a-book
    pathRegex: ".*a-book.*new"
    methods:
    - GET
  - name: update-books-bought
    pathRegex: /update-books-bought
    methods:
    - POST
```

Wenn Sie nicht wissen, welche Art von HTTP-Datenverkehr Ihre Front-End-Anwendung an andere Ebenen der Anwendung weiterleitet, können Sie, da die TrafficTarget-Spezifikation eine Regel erfordert, das Äquivalent einer "allow all"-Regel erstellen, indem Sie die unten stehende Spezifikation für HTTPRouteGroup verwenden.

```HTTPRouteGroup Allow All Example
apiVersion: specs.smi-spec.io/v1alpha4
kind: HTTPRouteGroup
metadata:
  name: allow-all
  namespace: yournamespace
spec:
  matches:
  - name: allow-all
    pathRegex: '.*'
    methods: ["GET","PUT","POST","DELETE","PATCH"]
```

Nachdem Sie die Spezifikationen TrafficTarget und HTTPRouteGroup konfiguriert haben, können Sie diese als eine YAML und bereitstellen. Im folgenden finden Sie die Beispielkonfiguration für die Buchhandlung.

```Bookstore Example TrafficTarget and HTTPRouteGroup configuration
kubectl apply -f - <<EOF
---
apiVersion: access.smi-spec.io/v1alpha3
kind: TrafficTarget
metadata:
  name: bookbuyer-access-bookstore-v1
  namespace: bookstore
spec:
  destination:
    kind: ServiceAccount
    name: bookstore
    namespace: bookstore
  rules:
  - kind: HTTPRouteGroup
    name: bookstore-service-routes
    matches:
    - buy-a-book
    - books-bought
  sources:
  - kind: ServiceAccount
    name: bookbuyer
    namespace: bookbuyer
---
apiVersion: specs.smi-spec.io/v1alpha4
kind: HTTPRouteGroup
metadata:
  name: bookstore-service-routes
  namespace: bookstore
spec:
  matches:
  - name: books-bought
    pathRegex: /books-bought
    methods:
    - GET
    headers:
    - "user-agent": ".*-http-client/*.*"
    - "client-app": "bookbuyer"
  - name: buy-a-book
    pathRegex: ".*a-book.*new"
    methods:
    - GET
  - name: update-books-bought
    pathRegex: /update-books-bought
    methods:
    - POST
EOF
```

Ausführlichere Informationen zur Spezifikation finden Sie auf der [SMI](https://smi-spec.io/)-Website.

### <a name="manage-the-applications-namespace-with-osm"></a>Verwalten des Namespace der Anwendung mit OSM

Als Nächstes konfigurieren Sie OSM, um den Namespace zu verwalten und die Bereitstellungen neu zu starten, damit der Envoy Sidecar Proxy mit der Anwendung eingefügt wird.

Führen Sie den folgenden Befehl aus, um den `azure-vote` Namespace für die Verwaltung meines OSM zu konfigurieren.

```azurecli-interactive
osm namespace add azure-vote
```

```Output
Namespace [azure-vote] successfully added to mesh [osm]
```

Starten Sie anschließend beide Bereitstellungen `azure-vote-front` mit `azure-vote-back` den folgenden Befehlen neu.

```azurecli-interactive
kubectl rollout restart deployment azure-vote-front -n azure-vote
kubectl rollout restart deployment azure-vote-back -n azure-vote
```

```Output
deployment.apps/azure-vote-front restarted
deployment.apps/azure-vote-back restarted
```

Wenn wir die Pods für den `azure-vote` Namespace anzeigen, wird die **fertige** Phase von `azure-vote-front` und `azure-vote-back` als 2/2 angezeigt, was bedeutet, dass der Envoy Sidecar Proxy neben der Anwendung eingefügt wurde.

## <a name="tutorial-deploy-an-application-managed-by-open-service-mesh-osm-with-nginx-ingress"></a>Tutorial: Bereitstellen einer von Open Service Mesh (OSM) verwalteten Anwendung mit NGINX-Ingress

Open Service Mesh (OSM) ist ein leichtgewichtiges, erweiterbares, Cloud Native Service Mesh, das es Anwendern ermöglicht, hochdynamische Microservice-Umgebungen einheitlich zu verwalten, zu sichern und Out-of-the-Box Observability-Funktionen zu erhalten.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Anzeigen der aktuellen OSM-Cluster-Konfiguration
> - Erstellen der Namespaces für OSM zum Verwalten bereitgestellter Anwendungen in den Namespaces
> - Einbindung der von OSM zu verwaltenden Namespaces
> - Bereitstellen der Beispielanwendung
> - Überprüfen Sie die Anwendung, die innerhalb des AKS-Clusters läuft
> - Erstellen Sie einen NGINX-Ingress-Controller, der für die Anwendung verwendet wird
> - Verfügbar machen eines Diensts über das Azure-Anwendung Gateway Ingress über das Internet

### <a name="before-you-begin"></a>Voraussetzungen

Die in dieser Artikel beschriebenen Schritte gehen davon aus, dass Sie einen AKS-Cluster (Kubernetes `1.19+` und höher, mit aktiviertem Kubernetes RBAC) erstellt haben, eine `kubectl` Verbindung mit dem Cluster hergestellt haben (Wenn Sie bei einem dieser Punkte Hilfe benötigen, dann lesen Sie den [AKS-Schnellstart](./kubernetes-walkthrough.md) und haben das AKS OSM-Add-on installiert.

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI, Version 2.20.0 oder höher
- Erweiterung `aks-preview`, Version 0.5.5 oder höher
- OSM-Version v0.8.0 oder höher
- apt-get install jq

### <a name="view-and-verify-the-current-osm-cluster-configuration"></a>Anzeigen und Überprüfen der aktuellen OSM-Cluster Konfiguration

Nachdem das OSM-Add-on für AKS auf dem AKS-Cluster aktiviert wurde, können Sie die aktuellen Konfigurationsparameter in der Datei OSM-Config Kubernetes ConfigMap anzeigen. Führen Sie den folgenden Befehl aus, um die Eigenschaften der ConfigMap anzuzeigen:

```azurecli-interactive
kubectl get configmap -n kube-system osm-config -o json | jq '.data'
```

Ausgabe zeigt die aktuelle OSM-Konfiguration für den Cluster an.

```json
{
  "egress": "true",
  "enable_debug_server": "true",
  "enable_privileged_init_container": "false",
  "envoy_log_level": "error",
  "outbound_ip_range_exclusion_list": "169.254.169.254,168.63.129.16,20.193.57.43",
  "permissive_traffic_policy_mode": "false",
  "prometheus_scraping": "false",
  "service_cert_validity_duration": "24h",
  "use_https_ingress": "false"
}
```

Beachten Sie, dass die **permissive_traffic_policy_mode** auf **true** festgelegt ist. Der Modus „Permissive traffic policy“ in OSM ist ein Modus, in dem die Durchsetzung der [SMI](https://smi-spec.io/)-Verkehrsrichtlinie umgangen wird. In diesem Modus erkennt OSM automatisch Dienste, die Teil des Dienstnetzes sind, und programmiert Regeln für Datenverkehrsrichtlinien auf jedem Envoy-Proxy-Sidecar, um mit diesen Diensten kommunizieren zu können.

### <a name="create-namespaces-for-the-application"></a>Anlegen von Namensräumen für die Anwendung

In diesem Lernprogramm verwenden wir die OSM-Buchhandelsanwendung, die aus den folgenden Anwendungskomponenten besteht:

- bookbuyer
- bookthief
- bookstore
- bookwarehouse

Legen Sie für jede dieser Anwendungskomponenten Namensräume an.

```azurecli-interactive
for i in bookstore bookbuyer bookthief bookwarehouse; do kubectl create ns $i; done
```

Die folgende Ausgabe wird angezeigt.

```Output
namespace/bookstore created
namespace/bookbuyer created
namespace/bookthief created
namespace/bookwarehouse created
```

### <a name="onboard-the-namespaces-to-be-managed-by-osm"></a>Einbindung der von OSM zu verwaltenden Namespaces

Das Hinzufügen der Namespaces zum OSM-Mesh ermöglicht es dem OSM-Controller, die Envoy-Sidecar-Proxy-Container automatisch mit Ihrer Anwendung zu injizieren. Führen Sie den folgenden Befehl aus, um die Namespaces der OSM-Buchhandelsanwendung zu aktivieren.

```azurecli-interactive
osm namespace add bookstore bookbuyer bookthief bookwarehouse
```

Die folgende Ausgabe wird angezeigt.

```Output
Namespace [bookstore] successfully added to mesh [osm]
Namespace [bookbuyer] successfully added to mesh [osm]
Namespace [bookthief] successfully added to mesh [osm]
Namespace [bookwarehouse] successfully added to mesh [osm]
```

### <a name="deploy-the-bookstore-application-to-the-aks-cluster"></a>Bereitstellen der Bookstore-Anwendung auf dem AKS-Cluster

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookbuyer.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookthief.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookstore.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookwarehouse.yaml
```

Nachfolgend sind alle Bereitstellungen-Ausgänge zusammengefasst.

```Output
serviceaccount/bookbuyer created
service/bookbuyer created
deployment.apps/bookbuyer created

serviceaccount/bookthief created
service/bookthief created
deployment.apps/bookthief created

service/bookstore created
serviceaccount/bookstore created
deployment.apps/bookstore created

serviceaccount/bookwarehouse created
service/bookwarehouse created
deployment.apps/bookwarehouse created
```

### <a name="update-the-bookbuyer-service"></a>Aktualisieren des Buchkäufer-Dienstanbieter

Aktualisieren Sie den Buchkäufer-Dienst auf die richtige Konfiguration des Eingangsports mit dem folgenden Service-Manifest.

```azurecli-interactive
kubectl apply -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: bookbuyer
  namespace: bookbuyer
  labels:
    app: bookbuyer
spec:
  ports:
  - port: 14001
    name: inbound-port
  selector:
    app: bookbuyer
EOF
```

### <a name="verify-the-bookstore-application-running-inside-the-aks-cluster"></a>Überprüfen Sie die Bookstore-Anwendung, die innerhalb des AKS-Clusters läuft

Ab jetzt haben wir die Bookstore-Mulit-Container-Anwendung bereitgestellt, aber sie ist nur innerhalb des AKS-Clusters zugänglich. Später werden wir den Azure Application Gateway Ingress Controller hinzufügen, um die Anwendung außerhalb des AKS-Clusters zu exponieren. Um zu überprüfen, dass die Anwendung innerhalb des Clusters läuft, werden wir einen Port-Forward verwenden, um sowohl die UI der Buchkäufer- als auch der Buchdieb-Komponente anzuzeigen.

Lassen Sie uns zuerst den Namen des Bookbuyer-Pods ermitteln

```azurecli-interactive
kubectl get pod -n bookbuyer
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden. Ihrem Bookbuyer-Pod wird ein eindeutiger Name angefügt.

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookbuyer-7676c7fcfb-mtnrz   2/2     Running   0          7m8s
```

Sobald wir den Namen des Pods haben, können wir nun den Befehl port-forward verwenden, um einen Tunnel von unserem lokalen System zur Anwendung im AKS-Cluster einzurichten. Führen Sie den folgenden Befehl aus, um die Portweiterleitung für den lokalen Systemport 8080 einzurichten. Verwenden Sie erneut den angegebenen Bookbuyer-Pod-Namen.

```azurecli-interactive
kubectl port-forward bookbuyer-7676c7fcfb-mtnrz -n bookbuyer 8080:14001
```

Sie sollten eine ähnliche Ausgabe wie diese sehen.

```Output
Forwarding from 127.0.0.1:8080 -> 14001
Forwarding from [::1]:8080 -> 14001
```

Rufen Sie bei bestehender Portweiterleitung die folgende URL in einem Browser auf `http://localhost:8080`. Sie sollten nun in der Lage sein, die Benutzeroberfläche der Bookbuyer-Anwendung im Browser zu sehen, ähnlich wie in der Abbildung unten.

![OSM-Buchkäufer-App für NGINX UI-Image](./media/aks-osm-addon/osm-agic-bookbuyer-img.png)

### <a name="create-an-nginx-ingress-controller-in-azure-kubernetes-service-aks"></a>Erstellen eines Ingress-Controllers in Azure Kubernetes Service (AKS)

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne IP-Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden.

Wir werden den Ingress-Controller verwenden, um die von OSM verwaltete Anwendung dem Internet auszusetzen. Verwenden Sie zum Erstellen des Eingangscontrollers Helm, um nginx-ingress zu installieren. Für zusätzliche Redundanz werden zwei Replikate der NGINX-Eingangscontroller mit dem Parameter `--set controller.replicaCount` bereitgestellt. Um vollständig von der Ausführung von Replikaten des Eingangscontrollers zu profitieren, stellen Sie sicher, dass sich mehr als ein Knoten im AKS-Cluster befindet.

Der Eingangscontroller muss ebenfalls auf einem Linux-Knoten geplant werden. Windows Server-Knoten dürfen nicht auf dem Eingangscontroller ausgeführt werden. Ein Knotenselektor wird mit dem Parameter `--set nodeSelector` angegeben, um den Kubernetes-Scheduler anzuweisen, den NGINX-Eingangscontroller auf einem Linux-basierten Knoten auszuführen.

> [!TIP]
> Im folgenden Beispiel wird der Kubernetes-Namespace _ingress-basic_ für die Eingangsressourcen erstellt. Geben Sie ggf. einen Namespace für Ihre eigene Umgebung an.

```azurecli-interactive
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Add the ingress-nginx repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Update the helm repo(s)
helm repo update

# Use Helm to deploy an NGINX ingress controller in the ingress-basic namespace
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress-basic \
    --set controller.replicaCount=1 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set controller.admissionWebhooks.patch.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Wird der Kubernetes-Lastenausgleichsdienst für den NGINX-Eingangscontroller erstellt, wird eine dynamische IP-Adresse zugewiesen, wie in der folgenden Beispielausgabe gezeigt:

```Output
$ kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-ingress-nginx-controller

NAME                                     TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)                      AGE   SELECTOR
nginx-ingress-ingress-nginx-controller   LoadBalancer   10.0.74.133   EXTERNAL_IP     80:32486/TCP,443:30953/TCP   44s   app.kubernetes.io/component=controller,app.kubernetes.io/instance=nginx-ingress,app.kubernetes.io/name=ingress-nginx
```

Es wurden noch keine Eingangsregeln erstellt, sodass die Standard-404-Seite des NGINX-Eingangscontrollers angezeigt wird, wenn Sie zur internen IP-Adresse navigieren. Eingangsregeln werden in den folgenden Schritten konfiguriert.

### <a name="expose-the-bookbuyer-service-to-the-internet"></a>Den Buchkäufer-Dienst für das Internet freigeben

```azurecli-interactive
kubectl apply -f - <<EOF
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: bookbuyer-ingress
  namespace: bookbuyer
  annotations:
    kubernetes.io/ingress.class: nginx

spec:

  rules:
    - host: bookbuyer.contoso.com
      http:
        paths:
        - path: /
          backend:
            serviceName: bookbuyer
            servicePort: 14001

  backend:
    serviceName: bookbuyer
    servicePort: 14001
EOF
```

Die folgende Ausgabe wird angezeigt.

```Output
Warning: extensions/v1beta1 Ingress is deprecated in v1.14+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
ingress.extensions/bookbuyer-ingress created
```

### <a name="view-the-nginx-logs"></a>Anzeigen der NGINX-Protokolle

```azurecli-interactive
POD=$(kubectl get pods -n ingress-basic | grep 'nginx-ingress' | awk '{print $1}')

kubectl logs $POD -n ingress-basic -f
```

Ausgabe zeigt den Status des NGINX-Ingress-Controllers an, wenn die Eingangsregel erfolgreich angewendet wurde:

```Output
I0321 <date>       6 event.go:282] Event(v1.ObjectReference{Kind:"Pod", Namespace:"ingress-basic", Name:"nginx-ingress-ingress-nginx-controller-54cf6c8bf4-jdvrw", UID:"3ebbe5e5-50ef-481d-954d-4b82a499ebe1", APIVersion:"v1", ResourceVersion:"3272", FieldPath:""}): type: 'Normal' reason: 'RELOAD' NGINX reload triggered due to a change in configuration
I0321 <date>        6 event.go:282] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"bookbuyer", Name:"bookbuyer-ingress", UID:"e1018efc-8116-493c-9999-294b4566819e", APIVersion:"networking.k8s.io/v1beta1", ResourceVersion:"5460", FieldPath:""}): type: 'Normal' reason: 'Sync' Scheduled for sync
I0321 <date>        6 controller.go:146] "Configuration changes detected, backend reload required"
I0321 <date>        6 controller.go:163] "Backend successfully reloaded"
I0321 <date>        6 event.go:282] Event(v1.ObjectReference{Kind:"Pod", Namespace:"ingress-basic", Name:"nginx-ingress-ingress-nginx-controller-54cf6c8bf4-jdvrw", UID:"3ebbe5e5-50ef-481d-954d-4b82a499ebe1", APIVersion:"v1", ResourceVersion:"3272", FieldPath:""}): type: 'Normal' reason: 'RELOAD' NGINX reload triggered due to a change in configuration
```

### <a name="view-the-nginx-services-and-bookbuyer-service-externally"></a>Extern Anzeigen der NGINX-Dienste und des Buchkäufer-Diensts

```azurecli-interactive
kubectl get services -n ingress-basic
```

```Output
NAME                                               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
nginx-ingress-ingress-nginx-controller             LoadBalancer   10.0.100.23   20.193.1.74   80:31742/TCP,443:32683/TCP   4m15s
nginx-ingress-ingress-nginx-controller-admission   ClusterIP      10.0.163.98   <none>        443/TCP                      4m15s
```

Da der Host-Name im Ingress-Manifest ein Pseudo-Name ist, der zum Testen verwendet wird, ist der DNS-Name nicht im Internet verfügbar. Alternativ können wir das curl-Programm verwenden und über den Hostname-Header der öffentlichen NGINX-IP-Adresse verfügen und einen 200-Code empfangen, der uns erfolgreich mit dem Buchkäufer-Dienst verbindet.

```azurecli-interactive
curl -H 'Host: bookbuyer.contoso.com' http://EXTERNAL-IP/
```

Die folgende Ausgabe wird angezeigt.

```Output
<!doctype html>
<html itemscope="" itemtype="http://schema.org/WebPage" lang="en">
  <head>
      <meta content="Bookbuyer" name="description">
      <meta content="text/html; charset=UTF-8" http-equiv="Content-Type">
      <title>Bookbuyer</title>
      <style>
        #navbar {
            width: 100%;
            height: 50px;
            display: table;
            border-spacing: 0;
            white-space: nowrap;
            line-height: normal;
            background-color: #0078D4;
            background-position: left top;
            background-repeat-x: repeat;
            background-image: none;
            color: white;
            font: 2.2em "Fira Sans", sans-serif;
        }
        #main {
            padding: 10pt 10pt 10pt 10pt;
            font: 1.8em "Fira Sans", sans-serif;
        }
        li {
            padding: 10pt 10pt 10pt 10pt;
            font: 1.2em "Consolas", sans-serif;
        }
      </style>
      <script>
        setTimeout(function(){window.location.reload(1);}, 1500);
      </script>
  </head>
  <body bgcolor="#fff">
    <div id="navbar">
      &#128214; Bookbuyer
    </div>
    <div id="main">
      <ul>
        <li>Total books bought: <strong>1833</strong>
          <ul>
            <li>from bookstore V1: <strong>277</strong>
            <li>from bookstore V2: <strong>1556</strong>
          </ul>
        </li>
      </ul>
    </div>

    <br/><br/><br/><br/>
    <br/><br/><br/><br/>
    <br/><br/><br/><br/>

    Current Time: <strong>Fri, 26 Mar 2021 15:02:53 UTC</strong>
  </body>
</html>
```

## <a name="tutorial-deploy-an-application-managed-by-open-service-mesh-osm-using-azure-application-gateway-ingress-aks-add-on"></a>Tutorial: Bereitstellen einer von Open Service Mesh (OSM) verwalteten Anwendung mit dem Azure Application Gateway Ingress AKS Add-on

Open Service Mesh (OSM) ist ein leichtgewichtiges, erweiterbares, Cloud Native Service Mesh, das es Anwendern ermöglicht, hochdynamische Microservice-Umgebungen einheitlich zu verwalten, zu sichern und Out-of-the-Box Observability-Funktionen zu erhalten.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Anzeigen der aktuellen OSM-Cluster-Konfiguration
> - Erstellen der Namespaces für OSM zum Verwalten bereitgestellter Anwendungen in den Namespaces
> - Einbindung der von OSM zu verwaltenden Namespaces
> - Bereitstellen der Beispielanwendung
> - Überprüfen Sie die Anwendung, die innerhalb des AKS-Clusters läuft
> - Erstellen Sie ein Azure-Anwendung Gateway, das als Eingangscontroller für die Anwendung verwendet werden soll
> - Verfügbar machen eines Diensts über das Azure-Anwendung Gateway Ingress über das Internet

### <a name="before-you-begin"></a>Voraussetzungen

Die in diesem Artikel beschriebenen Schritte gehen davon aus, dass Sie einen AKS-Cluster (Kubernetes `1.19+` und höher, mit aktiviertem Kubernetes RBAC) erstellt haben, eine `kubectl` Verbindung mit dem Cluster hergestellt haben (Wenn Sie bei einem dieser Punkte Hilfe benötigen, lesen Sie den [AKS-Schnellstart](./kubernetes-walkthrough.md), haben das AKS OSM-Add-on installiert und werden ein neues Azure Application Gateway für den Ingress erstellen.

Die folgenden Ressourcen müssen installiert sein:

- Azure CLI, Version 2.20.0 oder höher
- Erweiterung `aks-preview`, Version 0.5.5 oder höher
- AKS-Cluster Version 1,19 + Verwenden von Azure CNI-Netzwerken (verbunden mit einem Azure-Vnet)
- OSM-Version v0.8.0 oder höher
- apt-get install jq

### <a name="view-and-verify-the-current-osm-cluster-configuration"></a>Anzeigen und Überprüfen der aktuellen OSM-Cluster Konfiguration

Nachdem das OSM-Add-on für AKS auf dem AKS-Cluster aktiviert wurde, können Sie die aktuellen Konfigurationsparameter in der Datei OSM-Config Kubernetes ConfigMap anzeigen. Führen Sie den folgenden Befehl aus, um die Eigenschaften der ConfigMap anzuzeigen:

```azurecli-interactive
kubectl get configmap -n kube-system osm-config -o json | jq '.data'
```

Ausgabe zeigt die aktuelle OSM-Konfiguration für den Cluster an.

```json
{
  "egress": "true",
  "enable_debug_server": "true",
  "enable_privileged_init_container": "false",
  "envoy_log_level": "error",
  "outbound_ip_range_exclusion_list": "169.254.169.254,168.63.129.16,20.193.57.43",
  "permissive_traffic_policy_mode": "false",
  "prometheus_scraping": "false",
  "service_cert_validity_duration": "24h",
  "use_https_ingress": "false"
}
```

Beachten Sie, dass die **permissive_traffic_policy_mode** auf **true** festgelegt ist. Der Modus „Permissive traffic policy“ in OSM ist ein Modus, in dem die Durchsetzung der [SMI](https://smi-spec.io/)-Verkehrsrichtlinie umgangen wird. In diesem Modus erkennt OSM automatisch Dienste, die Teil des Dienstnetzes sind, und programmiert Regeln für Datenverkehrsrichtlinien auf jedem Envoy-Proxy-Sidecar, um mit diesen Diensten kommunizieren zu können.

### <a name="create-namespaces-for-the-application"></a>Anlegen von Namensräumen für die Anwendung

In diesem Lernprogramm verwenden wir die OSM-Buchhandelsanwendung, die aus den folgenden Anwendungskomponenten besteht:

- bookbuyer
- bookthief
- bookstore
- bookwarehouse

Legen Sie für jede dieser Anwendungskomponenten Namensräume an.

```azurecli-interactive
for i in bookstore bookbuyer bookthief bookwarehouse; do kubectl create ns $i; done
```

Die folgende Ausgabe wird angezeigt.

```Output
namespace/bookstore created
namespace/bookbuyer created
namespace/bookthief created
namespace/bookwarehouse created
```

### <a name="onboard-the-namespaces-to-be-managed-by-osm"></a>Einbindung der von OSM zu verwaltenden Namespaces

Wenn Sie die Namespaces zum OSM-Netz hinzufügen, ermöglicht dies dem OSM-Controller, die Envoy-Sidecar-Proxy-Container automatisch mit Ihrer Anwendung zu injizieren. Führen Sie den folgenden Befehl aus, um die Namespaces der OSM-Buchhandelsanwendung zu aktivieren.

```azurecli-interactive
osm namespace add bookstore bookbuyer bookthief bookwarehouse
```

Die folgende Ausgabe wird angezeigt.

```Output
Namespace [bookstore] successfully added to mesh [osm]
Namespace [bookbuyer] successfully added to mesh [osm]
Namespace [bookthief] successfully added to mesh [osm]
Namespace [bookwarehouse] successfully added to mesh [osm]
```

### <a name="deploy-the-bookstore-application-to-the-aks-cluster"></a>Bereitstellen der Bookstore-Anwendung auf dem AKS-Cluster

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookbuyer.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookthief.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookstore.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/release-v0.8/docs/example/manifests/apps/bookwarehouse.yaml
```

Nachfolgend sind alle Bereitstellungen-Ausgänge zusammengefasst.

```Output
serviceaccount/bookbuyer created
service/bookbuyer created
deployment.apps/bookbuyer created

serviceaccount/bookthief created
service/bookthief created
deployment.apps/bookthief created

service/bookstore created
serviceaccount/bookstore created
deployment.apps/bookstore created

serviceaccount/bookwarehouse created
service/bookwarehouse created
deployment.apps/bookwarehouse created
```

### <a name="update-the-bookbuyer-service"></a>Aktualisieren des Buchkäufer-Dienstanbieter

Aktualisieren Sie den Buchkäufer-Dienst auf die richtige Konfiguration des Eingangsports mit dem folgenden Service-Manifest.

```azurecli-interactive
kubectl apply -f - <<EOF
apiVersion: v1
kind: Service
metadata:
  name: bookbuyer
  namespace: bookbuyer
  labels:
    app: bookbuyer
spec:
  ports:
  - port: 14001
    name: inbound-port
  selector:
    app: bookbuyer
EOF
```

### <a name="verify-the-bookstore-application-running-inside-the-aks-cluster"></a>Überprüfen Sie die Bookstore-Anwendung, die innerhalb des AKS-Clusters läuft

Ab jetzt haben wir die Buchladen-Multicontainer-Anwendung bereitgestellt, aber sie ist nur innerhalb des AKS-Clusters zugänglich. Später werden wir den Azure Application Gateway Ingress Controller hinzufügen, um die Anwendung außerhalb des AKS-Clusters zu exponieren. Um zu überprüfen, dass die Anwendung innerhalb des Clusters läuft, werden wir einen Port-Forward verwenden, um sowohl die UI der Buchkäufer- als auch der Buchdieb-Komponente anzuzeigen.

Lassen Sie uns zuerst den Namen des Bookbuyer-Pods ermitteln

```azurecli-interactive
kubectl get pod -n bookbuyer
```

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden. Ihrem Bookbuyer-Pod wird ein eindeutiger Name angefügt.

```Output
NAME                         READY   STATUS    RESTARTS   AGE
bookbuyer-7676c7fcfb-mtnrz   2/2     Running   0          7m8s
```

Sobald wir den Namen des Pods haben, können wir nun den Befehl port-forward verwenden, um einen Tunnel von unserem lokalen System zur Anwendung im AKS-Cluster einzurichten. Führen Sie den folgenden Befehl aus, um die Portweiterleitung für den lokalen Systemport 8080 einzurichten. Verwenden Sie wieder Ihren spezifischen Buchkäufer-Pod-Namen.

```azurecli-interactive
kubectl port-forward bookbuyer-7676c7fcfb-mtnrz -n bookbuyer 8080:14001
```

Sie sollten eine ähnliche Ausgabe wie diese sehen.

```Output
Forwarding from 127.0.0.1:8080 -> 14001
Forwarding from [::1]:8080 -> 14001
```

Rufen Sie bei bestehender Portweiterleitung die folgende URL in einem Browser auf `http://localhost:8080`. Sie sollten nun in der Lage sein, die Benutzeroberfläche der Bookbuyer-Anwendung im Browser zu sehen, ähnlich wie in der Abbildung unten.

![OSM-Buchkäufer-App für App Gateway UI-Bild](./media/aks-osm-addon/osm-agic-bookbuyer-img.png)

### <a name="create-an-azure-application-gateway-to-expose-the-bookbuyer-application-outside-the-aks-cluster"></a>Erstellen eines Azure-Anwendung Gateways, um das Buchkäufer-Anwendung außerhalb des AKS-Clusters verfügbar zu machen

> [!NOTE]
> In den folgenden Anweisungen wird eine neue Instanz des Azure-Anwendung Gateways erstellt, das für den Eingang verwendet werden soll. Wenn Sie ein vorhandenes Azure Application Gateway verwenden möchten, fahren Sie mit dem Abschnitt zur Aktivierung des Add-ons Application Gateway Ingress Controller fort.

#### <a name="deploy-a-new-application-gateway"></a>Bereitstellen eines neuen Application Gateway

> [!NOTE]
> Wir verweisen auf die bestehende Dokumentation zur Aktivierung des Add-ons Application Gateway Ingress Controller für einen bestehenden AKS-Cluster. Es wurden einige Anpassungen an die OSM-Materialien vorgenommen. Eine ausführlichere Dokumentation zu diesem Thema finden Sie [hier](https://docs.microsoft.com/azure/application-gateway/tutorial-ingress-controller-add-on-existing).

Sie stellen nun ein neues Application Gateway bereit, um ein vorhandenes Application Gateway zu simulieren, das Sie für den Lastenausgleich des Datenverkehrs zum AKS-Cluster _myCluster_ verwenden möchten. Das Application Gateway erhält den Namen _myApplicationGateway_, Sie müssen jedoch zunächst eine Ressource mit öffentlicher IP-Adresse und dem Namen _myPublicIp_ und ein neues virtuelles Netzwerk mit dem Namen _myVnet_ und dem Adressraum 11.0.0.0/8 erstellen sowie ein Subnetz mit dem Adressraum 11.1.0.0/16 und dem Namen _mySubnet_. Stellen Sie dann das Application Gateway in _mySubnet_ mithilfe von _myPublicIp_ bereit.

Wenn Sie ein AKS-Cluster und ein Application Gateway in separaten virtuellen Netzwerken verwenden, dürfen sich die Adressräume der beiden virtuellen Netzwerke nicht überlappen. Der Standardadressraum, in dem ein AKS-Cluster bereitgestellt wird, ist 10.0.0.0/8. Daher legen Sie das Adresspräfix für das virtuelle Netzwerk mit dem Application Gateway auf 11.0.0.0/8 fest.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2
az network public-ip create -n myPublicIp -g MyResourceGroup --allocation-method Static --sku Standard
az network vnet create -n myVnet -g myResourceGroup --address-prefix 11.0.0.0/8 --subnet-name mySubnet --subnet-prefix 11.1.0.0/16
az network application-gateway create -n myApplicationGateway -l eastus2 -g myResourceGroup --sku Standard_v2 --public-ip-address myPublicIp --vnet-name myVnet --subnet mySubnet
```

> [!NOTE]
> Das AGIC-Add-On (Application Gateway Ingress Controller, Application Gateway-Eingangscontroller) unterstützt **nur** Application Gateway v2-SKUs (Standard und WAF) und **keine** Application Gateway v1-SKUs.

#### <a name="enable-the-agic-add-on-for-an-existing-aks-cluster-through-azure-cli"></a>Aktivieren des AGIC-Add-Ons im vorhandenen AKS-Cluster über die Azure CLI

Wenn Sie weiter mit der Azure CLI arbeiten möchten, können Sie nun fortfahren und das AGIC-Add-On in dem von Ihnen erstellten AKS-Cluster _myCluster_ aktivieren und angeben, dass das AGIC-Add-On die vorhandene Application Gateway-Instanz _myApplicationGateway_ verwenden soll, die Sie zuvor erstellt haben.

```azurecli-interactive
appgwId=$(az network application-gateway show -n myApplicationGateway -g myResourceGroup -o tsv --query "id")
az aks enable-addons -n myCluster -g myResourceGroup -a ingress-appgw --appgw-id $appgwId
```

Sie können überprüfen, ob das Azure-Anwendung Gateway-AKS-Add-on mit dem folgenden Befehl aktiviert wurde.

```azurecli-interactive
az aks list -g osm-aks-rg -o json | jq -r .[].addonProfiles.ingressApplicationGateway.enabled
```

Mit diesem Befehl sollte die Ausgabe als angezeigt werden `true` .

#### <a name="peer-the-two-virtual-networks-together"></a>Peering zwischen den beiden virtuellen Netzwerken

Da der AKS-Cluster in einem eigenen virtuellen Netzwerk und das Application Gateway in einem anderen virtuellen Netzwerk bereitgestellt wurde, müssen Sie die beiden virtuellen Netzwerke per Peering miteinander verknüpfen, damit Datenverkehr vom Application Gateway zu den Pods im Cluster fließt. Für das Peering der beiden virtuellen Netzwerke muss der Azure CLI-Befehl zweimal ausgeführt werden, um sicherzustellen, dass die Verbindung bidirektional ist. Mit dem ersten Befehl wird eine Peeringverbindung vom virtuellen Netzwerk mit dem Application Gateway zum virtuellen AKS-Netzwerk erstellt. Mit dem zweiten Befehl wird eine Peeringverbindung in umgekehrter Richtung erstellt.

```azurecli-interactive
nodeResourceGroup=$(az aks show -n myCluster -g myResourceGroup -o tsv --query "nodeResourceGroup")
aksVnetName=$(az network vnet list -g $nodeResourceGroup -o tsv --query "[0].name")

aksVnetId=$(az network vnet show -n $aksVnetName -g $nodeResourceGroup -o tsv --query "id")
az network vnet peering create -n AppGWtoAKSVnetPeering -g myResourceGroup --vnet-name myVnet --remote-vnet $aksVnetId --allow-vnet-access

appGWVnetId=$(az network vnet show -n myVnet -g myResourceGroup -o tsv --query "id")
az network vnet peering create -n AKStoAppGWVnetPeering -g $nodeResourceGroup --vnet-name $aksVnetName --remote-vnet $appGWVnetId --allow-vnet-access
```

### <a name="expose-the-bookbuyer-service-to-the-internet"></a>Den Buchkäufer-Dienst für das Internet freigeben

Wenden Sie das folgende Ingress-Manifest auf den AKS-Cluster an, um den Buchkäufer-Dienst über das Azure Application Gateway dem Internet auszusetzen.

```azurecli-interactive
kubectl apply -f - <<EOF
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: bookbuyer-ingress
  namespace: bookbuyer
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway

spec:

  rules:
    - host: bookbuyer.contoso.com
      http:
        paths:
        - path: /
          backend:
            serviceName: bookbuyer
            servicePort: 14001

  backend:
    serviceName: bookbuyer
    servicePort: 14001
EOF
```

Die folgende Ausgabe muss angezeigt werden:

```Output
Warning: extensions/v1beta1 Ingress is deprecated in v1.14+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
ingress.extensions/bookbuyer-ingress created
```

Da der Host-Name im Ingress-Manifest ein Pseudo-Name ist, der zum Testen verwendet wird, ist der DNS-Name nicht im Internet verfügbar. Alternativ können wir das curl-Programm verwenden und über den Hostname-Header der öffentlichen Azure Application Gateway-IP-Adresse verfügen und einen 200-Code empfangen, der uns erfolgreich mit dem Buchkäufer-Dienst verbindet.

```azurecli-interactive
appGWPIP=$(az network public-ip show -g MyResourceGroup -n myPublicIp -o tsv --query "ipAddress")
curl -H 'Host: bookbuyer.contoso.com' http://$appGWPIP/
```

Die folgende Ausgabe muss angezeigt werden:

```Output
<!doctype html>
<html itemscope="" itemtype="http://schema.org/WebPage" lang="en">
  <head>
      <meta content="Bookbuyer" name="description">
      <meta content="text/html; charset=UTF-8" http-equiv="Content-Type">
      <title>Bookbuyer</title>
      <style>
        #navbar {
            width: 100%;
            height: 50px;
            display: table;
            border-spacing: 0;
            white-space: nowrap;
            line-height: normal;
            background-color: #0078D4;
            background-position: left top;
            background-repeat-x: repeat;
            background-image: none;
            color: white;
            font: 2.2em "Fira Sans", sans-serif;
        }
        #main {
            padding: 10pt 10pt 10pt 10pt;
            font: 1.8em "Fira Sans", sans-serif;
        }
        li {
            padding: 10pt 10pt 10pt 10pt;
            font: 1.2em "Consolas", sans-serif;
        }
      </style>
      <script>
        setTimeout(function(){window.location.reload(1);}, 1500);
      </script>
  </head>
  <body bgcolor="#fff">
    <div id="navbar">
      &#128214; Bookbuyer
    </div>
    <div id="main">
      <ul>
        <li>Total books bought: <strong>5969</strong>
          <ul>
            <li>from bookstore V1: <strong>277</strong>
            <li>from bookstore V2: <strong>5692</strong>
          </ul>
        </li>
      </ul>
    </div>

    <br/><br/><br/><br/>
    <br/><br/><br/><br/>
    <br/><br/><br/><br/>

    Current Time: <strong>Fri, 26 Mar 2021 16:34:30 UTC</strong>
  </body>
</html>
```

### <a name="troubleshooting"></a>Problembehandlung

- [AGIC Dokumentation zur Problembehandlung](https://docs.microsoft.com/azure/application-gateway/ingress-controller-troubleshoot)
- [Weitere Tools zur Problembehandlung sind im GitHub-Repository von Agic verfügbar.](https://github.com/Azure/application-gateway-kubernetes-ingress/blob/master/docs/troubleshootings/troubleshooting-installing-a-simple-application.md)

## <a name="open-service-mesh-osm-monitoring-and-observability-using-azure-monitor-and-applications-insights"></a>Open Service Mesh (OSM)-Überwachung und-Observability mithilfe von Azure Monitor und Application Insights

Sowohl Azure Monitor als auch Azure Application Insights helfen Ihnen, die Verfügbarkeit und Leistung Ihrer Anwendungen und Dienste zu maximieren, indem sie eine umfassende Lösung zum Sammeln, Analysieren und Verarbeiten von Telemetriedaten aus Ihren Cloud- und On-Premises-Umgebungen bereitstellen.

Das OSM AKS-Add-on wird über tiefe Integrationen in diese beiden Azure-Dienste verfügen und eine nahtlose Azure-Erfahrung für die Anzeige und Reaktion auf kritische KPIs bieten, die von OSM-Metriken bereitgestellt werden. Weitere Informationen zum Aktivieren und Konfigurieren dieser Dienste für das OSM AKS-Add-on finden Sie auf der Seite [Azure Monitor für OSM](https://aka.ms/azmon/osmpreview) .

## <a name="tutorial-manually-deploy-prometheus-grafana-and-jaeger-to-view-open-service-mesh-osm-metrics-for-observability"></a>Tutorial: Manuelles Bereitstellen von Prometheus, Grafana und Jaeger zur Anzeige von Open Service Mesh (OSM)-Metriken für die Beobachtbarkeit

> [!WARNING]
> Die Installation von Prometheus, Grafana und Jaeger dient als allgemeine Anleitung, um zu zeigen, wie diese Tools zur Anzeige von OSM-Metrikdaten verwendet werden können. Die Installationsanleitung ist nicht für einen Produktionseinrichtung zu verwenden. Bitte lesen Sie in der Dokumentation des jeweiligen Tools nach, wie Sie die Installationen am besten an Ihre Bedürfnisse anpassen. Am bemerkenswertesten ist das Fehlen von persistentem Speicher, was bedeutet, dass alle Daten verloren gehen, sobald ein Prometheus-Grafana- und/oder Jaeger-Pod(s) beendet wird.

Open Service Mesh (OSM) generiert detaillierte Metriken in Bezug auf den gesamten Datenverkehr innerhalb des Mesh. Diese Metriken bieten Einblicke in das Verhalten von Anwendungen im Mesh und helfen Anwendern bei der Problembehandlung, Wartung und Analyse ihrer Anwendungen.

Ab heute sammelt OSM Metriken direkt von den Sidecar-Proxys (Envoy). OSM bietet umfangreiche Metriken für den ein- und ausgehenden Verkehr für alle Dienste im Mesh. Mit diesen Metriken kann der Benutzer-Informationen über das Gesamtvolumen des Datenverkehrs, Fehler innerhalb des Datenverkehrs und die Antwortzeit für Anfragen erhalten.

OSM verwendet Prometheus, um konsistente Verkehrsmetriken und Statistiken für alle im Mesh laufenden Anwendungen zu sammeln und zu speichern. Prometheus ist ein Open-Source-Überwachungs- und Alarmierungs-Toolkit, das häufig in (aber nicht nur) Kubernetes- und Service Mesh-Umgebungen eingesetzt wird.

Jede Anwendung, die Teil des Mesh ist, läuft in einem Pod, der einen Envoy-Sidecar enthält, der Metriken (Proxy-Metriken) im Prometheus-Format bereitstellt. Außerdem verfügt jeder Pod, der Teil des Netzes ist, über Prometheus-Annotationen, was es dem Prometheus-Server ermöglicht, die Anwendung dynamisch zu scrapen. Dieser Mechanismus ermöglicht automatisch das Scraping von Metriken, wenn ein neuer Namespace/Pod/Service zum Mesh hinzugefügt wird.

OSM-Metriken können mit Grafana, einer Open-Source-Visualisierungs- und Analysesoftware, angezeigt werden. Damit können Sie Ihre Metriken abfragen, visualisieren und untersuchen sowie Warnungen erstellen.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
>
> - Erstellen und Bereitstellen einer Prometheus-Instanz
> - Konfigurieren Sie OSM so, dass Prometheus Scraping erlaubt
> - Aktualisieren der Prometheus-Configmap
> - Erstellen und Bereitstellen einer Grafana-Instanz
> - Konfigurieren Sie Grafana mit der Prometheus-Datenquelle
> - Importieren des OSM-Dashboards für Grafana
> - Erstellen und Bereitstellen einer Jaeger-Instanz
> - Konfigurieren der Jaeger-Ablaufverfolgung für OSM

### <a name="deploy-and-configure-a-prometheus-instance-for-osm"></a>Bereitstellen und Konfigurieren einer Prometheus-Instanz für OSM

Wir werden Helm verwenden, um die Prometheus-Instanz bereitzustellen. Führen Sie die folgenden Befehle aus, um Prometheus über Helm zu installieren:

```azurecli-interactive
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install stable prometheus-community/prometheus
```

Wenn die Installation erfolgreich war, sollte unten eine ähnliche Ausgabe angezeigt werden. Notieren Sie sich den Port des Prometheus-Servers und den DNS-Namen des Clusters. Diese Informationen werden später verwendet, um Prometheus als Datenquelle für Grafana zu konfigurieren.

```Output
NAME: stable
LAST DEPLOYED: Fri Mar 26 13:34:51 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
The Prometheus server can be accessed via port 80 on the following DNS name from within your cluster:
stable-prometheus-server.default.svc.cluster.local


Get the Prometheus server URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace default -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace default port-forward $POD_NAME 9090


The Prometheus alertmanager can be accessed via port 80 on the following DNS name from within your cluster:
stable-prometheus-alertmanager.default.svc.cluster.local


Get the Alertmanager URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace default -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace default port-forward $POD_NAME 9093
#################################################################################
######   WARNING: Pod Security Policy has been moved to a global property.  #####
######            use .Values.podSecurityPolicy.enabled with pod-based      #####
######            annotations                                               #####
######            (e.g. .Values.nodeExporter.podSecurityPolicy.annotations) #####
#################################################################################


The Prometheus PushGateway can be accessed via port 9091 on the following DNS name from within your cluster:
stable-prometheus-pushgateway.default.svc.cluster.local


Get the PushGateway URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace default -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace default port-forward $POD_NAME 9091

For more information on running Prometheus, visit:
https://prometheus.io/
```

#### <a name="configure-osm-to-allow-prometheus-scraping"></a>Konfigurieren Sie OSM so, dass Prometheus Scraping erlaubt

Um sicherzustellen, dass die OSM-Komponenten für Prometheus-Scrapes konfiguriert sind, sollten Sie die **prometheus_scraping** Konfiguration in der Konfigurationsdatei „OSM-config“ überprüfen. Zeigen Sie die Konfiguration mit dem folgenden Befehl an:

```azurecli-interactive
kubectl get configmap -n kube-system osm-config -o json | jq '.data.prometheus_scraping'
```

Die Ausgabe des vorherigen Befehls sollte zurückgegeben `true`werden, wenn OSM für Prometheus Scraping konfiguriert ist. Wenn der zurückgegebene Wert `false` ist, müssen wir die Konfiguration so aktualisieren, dass Sie lautet `true`. Führen Sie den folgenden Befehl **aus** , um OSM Prometheus-Scraping zu aktivieren:

```azurecli-interactive
kubectl patch ConfigMap -n kube-system osm-config --type merge --patch '{"data":{"prometheus_scraping":"true"}}'
```

Die folgende Ausgabe wird angezeigt.

```Output
configmap/osm-config patched
```

#### <a name="update-the-prometheus-configmap"></a>Aktualisieren der Prometheus-Configmap

Die Standardinstallation von Prometheus enthält zwei Kubernetes configmaps. Sie können die Liste der Prometheus-configmaps mit dem folgenden Befehl anzeigen.

```azurecli-interactive
kubectl get configmap | grep prometheus
```

```Output
stable-prometheus-alertmanager   1      4h34m
stable-prometheus-server         5      4h34m
```

Wir müssen die prometheus.yml-Konfiguration, die sich in der configmap **stable-prometheus-server** befindet, durch die folgende OSM-Konfiguration ersetzen. Es gibt verschiedene Techniken zur Dateibearbeitung, um diese Aufgabe zu erfüllen. Ein einfacher und sicherer Weg ist es, die Configmap zu exportieren, eine Kopie davon zur Sicherung zu erstellen und diese dann mit einem Editor wie Visual Studio Code zu bearbeiten.

> [!NOTE]
> Wenn Sie Visual Studio Code nicht installiert haben, können Sie es [hier](https://code.visualstudio.com/Download)herunterladen und installieren.

Lassen Sie uns zunächst die Configmap von **stable-prometheus-server** exportieren und dann eine Kopie zur Sicherung erstellen.

```azurecli-interactive
kubectl get configmap stable-prometheus-server -o yaml > cm-stable-prometheus-server.yml
cp cm-stable-prometheus-server.yml cm-stable-prometheus-server.yml.copy
```

Öffnen Sie als nächstes die Datei mit Visual Studio Code, um Sie zu bearbeiten.

```azurecli-interactive
code cm-stable-prometheus-server.yml
```

Nachdem Sie die dieses im Visual Studio Code-Editor geöffnet haben, ersetzen Sie die Datei „Prometheus.yml“ durch die unten stehende OSM-Konfiguration, und speichern Sie die Datei.

> [!WARNING]
> Es ist äußerst wichtig, dass Sie darauf achten, dass Sie die Einziehungsstruktur der yaml-Datei beibehalten. Änderungen an der Struktur der yaml-Datei können dazu führen, dass die configmap nicht mehr angewendet werden kann.

```OSM Prometheus Configmap Configuration
prometheus.yml: |
    global:
      scrape_interval: 10s
      scrape_timeout: 10s
      evaluation_interval: 1m

    scrape_configs:
      - job_name: 'kubernetes-apiservers'
        kubernetes_sd_configs:
        - role: endpoints
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
          # TODO need to remove this when the CA and SAN match
          insecure_skip_verify: true
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        metric_relabel_configs:
        - source_labels: [__name__]
          regex: '(apiserver_watch_events_total|apiserver_admission_webhook_rejection_count)'
          action: keep
        relabel_configs:
        - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
          action: keep
          regex: default;kubernetes;https

      - job_name: 'kubernetes-nodes'
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        kubernetes_sd_configs:
        - role: node
        relabel_configs:
        - action: labelmap
          regex: __meta_kubernetes_node_label_(.+)
        - target_label: __address__
          replacement: kubernetes.default.svc:443
        - source_labels: [__meta_kubernetes_node_name]
          regex: (.+)
          target_label: __metrics_path__
          replacement: /api/v1/nodes/${1}/proxy/metrics

      - job_name: 'kubernetes-pods'
        kubernetes_sd_configs:
        - role: pod
        metric_relabel_configs:
        - source_labels: [__name__]
          regex: '(envoy_server_live|envoy_cluster_upstream_rq_xx|envoy_cluster_upstream_cx_active|envoy_cluster_upstream_cx_tx_bytes_total|envoy_cluster_upstream_cx_rx_bytes_total|envoy_cluster_upstream_cx_destroy_remote_with_active_rq|envoy_cluster_upstream_cx_connect_timeout|envoy_cluster_upstream_cx_destroy_local_with_active_rq|envoy_cluster_upstream_rq_pending_failure_eject|envoy_cluster_upstream_rq_pending_overflow|envoy_cluster_upstream_rq_timeout|envoy_cluster_upstream_rq_rx_reset|^osm.*)'
          action: keep
        relabel_configs:
        - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
          action: keep
          regex: true
        - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
          action: replace
          target_label: __metrics_path__
          regex: (.+)
        - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
          action: replace
          regex: ([^:]+)(?::\d+)?;(\d+)
          replacement: $1:$2
          target_label: __address__
        - source_labels: [__meta_kubernetes_namespace]
          action: replace
          target_label: source_namespace
        - source_labels: [__meta_kubernetes_pod_name]
          action: replace
          target_label: source_pod_name
        - regex: '(__meta_kubernetes_pod_label_app)'
          action: labelmap
          replacement: source_service
        - regex: '(__meta_kubernetes_pod_label_osm_envoy_uid|__meta_kubernetes_pod_label_pod_template_hash|__meta_kubernetes_pod_label_version)'
          action: drop
        # for non-ReplicaSets (DaemonSet, StatefulSet)
        # __meta_kubernetes_pod_controller_kind=DaemonSet
        # __meta_kubernetes_pod_controller_name=foo
        # =>
        # workload_kind=DaemonSet
        # workload_name=foo
        - source_labels: [__meta_kubernetes_pod_controller_kind]
          action: replace
          target_label: source_workload_kind
        - source_labels: [__meta_kubernetes_pod_controller_name]
          action: replace
          target_label: source_workload_name
        # for ReplicaSets
        # __meta_kubernetes_pod_controller_kind=ReplicaSet
        # __meta_kubernetes_pod_controller_name=foo-bar-123
        # =>
        # workload_kind=Deployment
        # workload_name=foo-bar
        # deplyment=foo
        - source_labels: [__meta_kubernetes_pod_controller_kind]
          action: replace
          regex: ^ReplicaSet$
          target_label: source_workload_kind
          replacement: Deployment
        - source_labels:
          - __meta_kubernetes_pod_controller_kind
          - __meta_kubernetes_pod_controller_name
          action: replace
          regex: ^ReplicaSet;(.*)-[^-]+$
          target_label: source_workload_name

      - job_name: 'smi-metrics'
        kubernetes_sd_configs:
        - role: pod
        relabel_configs:
        - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
          action: keep
          regex: true
        - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
          action: replace
          target_label: __metrics_path__
          regex: (.+)
        - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
          action: replace
          regex: ([^:]+)(?::\d+)?;(\d+)
          replacement: $1:$2
          target_label: __address__
        metric_relabel_configs:
        - source_labels: [__name__]
          regex: 'envoy_.*osm_request_(total|duration_ms_(bucket|count|sum))'
          action: keep
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_(\d{3})_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: response_code
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_(.*)_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: source_namespace
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_(.*)_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: source_kind
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_(.*)_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: source_name
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_(.*)_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: source_pod
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_(.*)_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: destination_namespace
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_(.*)_destination_name_.*_destination_pod_.*_osm_request_total
          target_label: destination_kind
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_(.*)_destination_pod_.*_osm_request_total
          target_label: destination_name
        - source_labels: [__name__]
          action: replace
          regex: envoy_response_code_\d{3}_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_(.*)_osm_request_total
          target_label: destination_pod
        - source_labels: [__name__]
          action: replace
          regex: .*(osm_request_total)
          target_label: __name__

        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_(.*)_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: source_namespace
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_(.*)_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: source_kind
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_(.*)_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: source_name
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_(.*)_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: source_pod
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_(.*)_destination_kind_.*_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: destination_namespace
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_(.*)_destination_name_.*_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: destination_kind
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_(.*)_destination_pod_.*_osm_request_duration_ms_(bucket|sum|count)
          target_label: destination_name
        - source_labels: [__name__]
          action: replace
          regex: envoy_source_namespace_.*_source_kind_.*_source_name_.*_source_pod_.*_destination_namespace_.*_destination_kind_.*_destination_name_.*_destination_pod_(.*)_osm_request_duration_ms_(bucket|sum|count)
          target_label: destination_pod
        - source_labels: [__name__]
          action: replace
          regex: .*(osm_request_duration_ms_(bucket|sum|count))
          target_label: __name__

      - job_name: 'kubernetes-cadvisor'
        scheme: https
        tls_config:
          ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
        kubernetes_sd_configs:
        - role: node
        metric_relabel_configs:
        - source_labels: [__name__]
          regex: '(container_cpu_usage_seconds_total|container_memory_rss)'
          action: keep
        relabel_configs:
        - action: labelmap
          regex: __meta_kubernetes_node_label_(.+)
        - target_label: __address__
          replacement: kubernetes.default.svc:443
        - source_labels: [__meta_kubernetes_node_name]
          regex: (.+)
          target_label: __metrics_path__
          replacement: /api/v1/nodes/${1}/proxy/metrics/cadvisor
```

Wenden Sie die aktualisierte configmap yaml-Datei mit dem folgenden Befehl an.

```azurecli-interactive
kubectl apply -f cm-stable-prometheus-server.yml
```

```Output
configmap/stable-prometheus-server configured
```

> [!NOTE]
> Möglicherweise erhalten Sie eine Meldung über eine fehlende benötigte Kubernetes-Anmerkung. Dies kann vorerst ignoriert werden.

#### <a name="verify-prometheus-is-configured-to-scrape-the-osm-mesh-and-api-endpoints"></a>Überprüfen Sie, ob Prometheus für das Scrapen des OSM-Netzes und der API-Endpunkte konfiguriert ist

Um zu überprüfen, ob Prometheus richtig konfiguriert ist, um das OSM-Mesh und die API-Endpunkte zu scrapen, werden wir zum Prometheus-Pod portieren und die Zielkonfiguration anzeigen. Führen Sie die folgenden Befehle aus:

```azurecli-interactive
PROM_POD_NAME=$(kubectl get pods -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace <promNamespace> port-forward $PROM_POD_NAME 9090
```

Öffnen Sie einen Browser bis zu `http://localhost:9090/targets`

Wenn Sie nach unten scrollen, sollten Sie alle SMI-Metrik-Endpunkte mit dem Status **AUF** sowie andere OSM-Metriken sehen, die wie unten abgebildet definiert sind.

![OSM Prometheus Zielmetriken Benutzeroberfläche Bild](./media/aks-osm-addon/osm-prometheus-smi-metrics-target-scrape.png)

### <a name="deploy-and-configure-a-grafana-instance-for-osm&quot;></a>Bereitstellen und Konfigurieren einer Grafana-Instanz für OSM

Wir verwenden Helm zum Bereitstellen der Grafana-Instanz. Führen Sie die folgenden Befehle aus, um Prometheus über Helm zu installieren:

```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install osm-grafana grafana/grafana
```

Als nächstes holen wir uns das Standard-Grafana-Passwort, um uns auf der Grafana-Seite anzumelden.

```azurecli-interactive
kubectl get secret --namespace default osm-grafana -o jsonpath=&quot;{.data.admin-password}&quot; | base64 --decode ; echo
```

Aktualisieren des Grafana-Kennworts.

Als Nächstes rufen wir den Grafana-Pod ab, um das Grafana-Dashboard für die Anmeldung zu portieren.

```azurecli-interactive
GRAF_POD_NAME=$(kubectl get pods -l &quot;app.kubernetes.io/name=grafana&quot; -o jsonpath=&quot;{.items[0].metadata.name}")
kubectl port-forward $GRAF_POD_NAME 3000
```

Öffnen Sie einen Browser bis zu `http://localhost:3000`

Geben Sie auf dem unten abgebildeten Anmeldebildschirm **Admin** als Benutzername ein, und verwenden Sie das zuvor erfasste Grafana-Kennwort.

![OSM Grafana Login-Seite UI-Bild](./media/aks-osm-addon/osm-grafana-ui-login.png)

#### <a name="configure-the-grafana-prometheus-data-source"></a>Konfigurieren Sie die Grafana Prometheus-Datenquelle

Nachdem Sie sich erfolgreich in Grafana eingeloggt haben, ist der nächste Schritt, Prometheus als Datenquellen für Grafana hinzuzufügen. Navigieren Sie hierzu im Menü auf der linken Seite zum Konfigurationssymbol, und wählen Sie wie unten dargestellt die Option Datenquellen aus.

![OSM Grafana Datenquellen Seite UI Bild](./media/aks-osm-addon/osm-grafana-ui-datasources.png)

Klicken Sie auf die Schaltfläche **Datenquelle hinzufügen**, und wählen Sie unter Zeitreihen-Datenbanken

![Die OSM Grafana-Datenquellen Auswahlseite Benutzeroberfläche Bild](./media/aks-osm-addon/osm-grafana-ui-datasources-select-prometheus.png)

Geben Sie auf der Seite **Konfigurieren Sie Ihre Prometheus-Datenquelle unten** den Kubernetes Cluster-voll qualifizierten Namen für den Prometheus-Dienst für die HTTP-URL-Einstellung ein. Der Standardport sollte `stable-prometheus-server.default.svc.cluster.local`sein. Sobald Sie den Endpunkt des Prometheus-Dienstes eingegeben haben, scrollen Sie zum Ende der Seite und wählen Sie **Speichern & Testen**. Sie sollten ein grünes Kontrollkästchen erhalten, das anzeigt, dass die Datenquelle funktioniert.

#### <a name="importing-osm-dashboards"></a>Importieren von OSM-Dashboards

OSM-Dashboards sind über Folgendes verfügbar:

- [Unser Repository](/charts/osm/grafana)kann als JSON-BLOB über das Webverwaltungs-Portal importiert werden.
- oder [Online unter Grafana.com](https://grafana.com/grafana/dashboards/14145)

Um ein Dashboard zu importieren, suchen `+` Sie im Menü auf der linken Seite nach dem Zeichen, und wählen Sie aus `import` .
Sie können das Dashboard direkt anhand seiner ID importieren auf `Grafana.com`. `OSM Mesh Details`Das Dashboard verwendet beispielsweise ID `14145`, Sie können die ID direkt auf dem Formular verwenden und Folgendes auswählen `import` :

![OSM Grafana Dashboard Import Seite Benutzeroberfläche Bild](./media/aks-osm-addon/osm-grafana-dashboard-import.png)

Sobald Sie importieren auswählen, werden Sie automatisch in Ihr importiertes Dashboard gebracht.

![OSM Grafana Dashboard Mesh Details Seite Benutzeroberfläche Bild](./media/aks-osm-addon/osm-grafana-mesh-dashboard-details.png)

### <a name="deploy-and-configure-a-jaeger-operator-on-kubernetes-for-osm"></a>Bereitstellen und Konfigurieren eines Jaeger-Operators auf Kubernetes für OSM

[Jaeger](https://www.jaegertracing.io/) ist ein Open Source-Ablaufverfolgungs-System, das für die Überwachung und Problembehandlung verteilter Systeme verwendet wird. Es kann mit OSM als neue Instanz bereitgestellt werden oder Sie können Ihre eigene Instanz mitbringen. Mit den folgenden Anweisungen wird eine neue Instanz von Jaeger im Namespace auf dem`jaeger` AKS-Cluster bereitgestellt.

#### <a name="deploy-jaeger-to-the-aks-cluster"></a>Bereitstellen von Jaeger auf dem AKS-Cluster

Wenden Sie das folgende Manifest an, um Jaeger zu installieren:

```azurecli-interactive
kubectl apply -f - <<EOF
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jaeger
  namespace: jaeger
  labels:
    app: jaeger
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jaeger
  template:
    metadata:
      labels:
        app: jaeger
    spec:
      containers:
        - name: jaeger
          image: jaegertracing/all-in-one
          args:
          - --collector.zipkin.host-port=9411
          imagePullPolicy: IfNotPresent
          ports:
          - containerPort: 9411
          resources:
            limits:
              cpu: 500m
              memory: 512M
            requests:
              cpu: 100m
              memory: 256M
---
kind: Service
apiVersion: v1
metadata:
  name: jaeger
  namespace: jaeger
  labels:
    app: jaeger
spec:
  selector:
    app: jaeger
  ports:
  - protocol: TCP
    # Service port and target port are the same
    port: 9411
  type: ClusterIP
EOF
```

```Output
deployment.apps/jaeger created
service/jaeger created
```

#### <a name="enable-tracing-for-the-osm-add-on"></a>Aktivieren der Ablaufverfolgung für das OSM-Add-on

Als nächstes müssen wir die Ablaufverfolgung für das OSM-Add-on aktivieren.

> [!NOTE]
> Ab sofort sind die Verfolgungseigenschaften in der osm-config configmap nicht mehr sichtbar. Dies wird in einer neuen Version des OSM AKS-Add-ons sichtbar gemacht werden.

Führen Sie den folgenden Befehl aus, um die Ablaufverfolgung für das OSM-Add-on zu aktivieren:

```azurecli-interactive
kubectl patch configmap osm-config -n kube-system -p '{"data":{"tracing_enable":"true", "tracing_address":"jaeger.jaeger.svc.cluster.local", "tracing_port":"9411", "tracing_endpoint":"/api/v2/spans"}}' --type=merge
```

```Output
configmap/osm-config patched
```

#### <a name="view-the-jaeger-ui-with-port-forwarding"></a>Anzeigen der Jaeger-Benutzeroberfläche mit Portweiterleitung

Die Benutzeroberfläche von Jaeger läuft auf Port 16686. Um die Web-Benutzeroberfläche anzuzeigen, können Sie kubectl Port-Forward verwenden:

```azurecli-interactive
JAEGER_POD=$(kubectl get pods -n jaeger --no-headers  --selector app=jaeger | awk 'NR==1{print $1}')
kubectl port-forward -n jaeger $JAEGER_POD  16686:16686
http://localhost:16686/
```

Im Browser sollten Sie ein Service-Dropdown sehen, mit dem Sie aus den verschiedenen Anwendungen auswählen können, die von der Buchladen-Demo bereitgestellt werden. Wählen Sie einen Dienst aus, um alle Spans davon anzuzeigen. Wenn Sie z. B. bookbuyer mit einem Lookback von einer Stunde auswählen, können Sie seine Interaktionen mit bookstore-v1 und bookstore-v2 nach Zeit sortiert sehen.

![OSM Jaeger Verfolgungsseite UI-Bild](./media/aks-osm-addon/osm-jaeger-trace-view-ui.png)

Wählen Sie ein beliebiges Element aus, um es im Detail zu betrachten. Wählen Sie mehrere Elemente aus, um Spuren zu vergleichen. Zum Beispiel können Sie die Interaktionen des Buchkäufers mit Buchhandlung und Buchhandlung-v2 zu einem bestimmten Zeitpunkt vergleichen.

Sie können auch die Registerkarte Systemarchitektur auswählen, um eine Grafik der Interaktion/Kommunikation der verschiedenen Anwendungen anzuzeigen. Dies bietet einen Überblick darüber, wie der Datenverkehr zwischen den Anwendungen fließt.

![OSM Jaeger Systemarchitektur Benutzeroberfläche Bild](./media/aks-osm-addon/osm-jaeger-sys-arc-view-ui.png)

## <a name="open-service-mesh-osm-aks-add-on-troubleshooting-guides"></a>Open Service Mesh (OSM) Add-on AKS Anleitungen zur Fehlerbehebung

Wenn Sie das OSM AKS-Add-on einsetzen, kann gelegentlich ein Problem auftreten. Die folgenden Leitfäden helfen Ihnen beim Beheben von Fehlern und Beheben allgemeiner Probleme.

### <a name="verifying-and-troubleshooting-osm-components"></a>Überprüfen und Problembehandlung von OSM-Komponenten

#### <a name="check-osm-controller-deployment"></a>OSM-Controller Bereitstellung überprüfen

```azurecli-interactive
kubectl get deployment -n kube-system --selector app=osm-controller
```

Ein fehlerfreier OSM-Controller würde wie folgt aussehen:

```Output
NAME             READY   UP-TO-DATE   AVAILABLE   AGE
osm-controller   1/1     1            1           59m
```

#### <a name="check-the-osm-controller-pod"></a>Überprüfen des OSM-Controller-Pods

```azurecli-interactive
kubectl get pods -n kube-system --selector app=osm-controller
```

Ein fehlerfreier OSM-Pod würde wie folgt aussehen:

```Output
NAME                            READY   STATUS    RESTARTS   AGE
osm-controller-b5bd66db-wglzl   0/1     Evicted   0          61m
osm-controller-b5bd66db-wvl9w   1/1     Running   0          31m
```

Obwohl ein Controller zu einem bestimmten Zeitpunkt entfernt wurde, haben wir einen anderen Controller, der bereit ist 1/1 und mit 0 Neustarts. Wenn die Spalte BEREIT einen anderen Wert als 1/1 hat, befindet sich das Dienstnetz in einem defekten Zustand.
Die Spalte BEREIT mit 0/1 zeigt an, dass der Steuerebenen-Container abstürzt - wir müssen uns Protokolle besorgen. Siehe Abschnitt OSM-Controller-Protokolle vom Azure Support Center abrufen unten. Die Spalte BEREIT mit einer Zahl größer als 1 nach dem / würde anzeigen, dass Seitenwagen installiert sind. OSM Controller würde höchstwahrscheinlich nicht mit angeschlossenen Seitenwagen funktionieren.

> [!NOTE]
> Ab Version v0.8.2 befindet sich der OSM-Controller nicht im HA-Modus und läuft in einer Bereitstellung mit Replikatanzahl von 1 - Single Pod. Der Pod verfügt über Integritätsprüfungen und wird bei Bedarf vom Kubelet neu gestartet.

#### <a name="check-osm-controller-service"></a>OSM-Controller-Dienst prüfen

```azurecli-interactive
kubectl get service -n kube-system osm-controller
```

Ein gesunder OSM-Controller-Dienst würde folgendermaßen aussehen:

```Output
NAME             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)              AGE
osm-controller   ClusterIP   10.0.31.254   <none>        15128/TCP,9092/TCP   67m
```

> [!NOTE]
> Die CLUSTER-IP wäre eine andere. Der Dienst NAME und PORT(S) müssen mit dem obigen Beispiel übereinstimmen.

#### <a name="check-osm-controller-endpoints"></a>OSM-Controller-Endpunkte überprüfen

```azurecli-interactive
kubectl get endpoints -n kube-system osm-controller
```

Ein fehlerfreier OSM-Controller-Endpunkt bzw. gesunde OSM-Controller-Endpunkte würden wie folgt aussehen:

```Output
NAME             ENDPOINTS                              AGE
osm-controller   10.240.1.115:9092,10.240.1.115:15128   69m
```

#### <a name="check-osm-injector-deployment"></a>OSM-Injektor-Bereitstellung prüfen

```azurecli-interactive
kubectl get pod -n kube-system --selector app=osm-injector
```

Eine fehlerfreier OSM-Injektor-Bereitstellung würde wie folgt aussehen:

```Output
NAME                            READY   STATUS    RESTARTS   AGE
osm-injector-5986c57765-vlsdk   1/1     Running   0          73m
```

#### <a name="check-osm-injector-pod"></a>OSM-Injektor-Pod prüfen

```azurecli-interactive
kubectl get pod -n kube-system --selector app=osm-injector
```

Ein fehlerfreier OSM-Pod würde wie folgt aussehen:

```Output
NAME                            READY   STATUS    RESTARTS   AGE
osm-injector-5986c57765-vlsdk   1/1     Running   0          73m
```

#### <a name="check-osm-injector-service"></a>OSM-Injektordienst überprüfen

```azurecli-interactive
kubectl get service -n kube-system osm-injector
```

Ein fehlerfreier OSM-Injektordienst würde wie folgt aussehen:

```Output
NAME           TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
osm-injector   ClusterIP   10.0.39.54   <none>        9090/TCP   75m
```

#### <a name="check-osm-endpoints"></a>OSM-Endpunkte überprüfen

```azurecli-interactive
kubectl get endpoints -n kube-system osm-injector
```

Ein fehlerfreier OSM-Endpunkt würde wie folgt aussehen:

```Output
NAME           ENDPOINTS           AGE
osm-injector   10.240.1.172:9090   75m
```

#### <a name="check-validating-and-mutating-webhooks"></a>Prüfen Validieren und Mutieren von Webhooks

```azurecli-interactive
kubectl get ValidatingWebhookConfiguration --selector app=osm-controller
```

Ein fehlerfreier OSM Validating Webhook würde so aussehen:

```Output
NAME              WEBHOOKS   AGE
aks-osm-webhook-osm   1      81m
```

```azurecli-interactive
kubectl get MutatingWebhookConfiguration --selector app=osm-injector
```

Ein fehlerfreier OSM Mutating Webhook würde wie folgt aussehen:

```Output
NAME              WEBHOOKS   AGE
aks-osm-webhook-osm   1      102m
```

#### <a name="check-for-the-service-and-the-ca-bundle-of-the-validating-webhook"></a>Prüfen Sie auf den Dienst und das CA-Bundle des Validating Webhook

```azurecli-interactive
kubectl get ValidatingWebhookConfiguration aks-osm-webhook-osm -o json | jq '.webhooks[0].clientConfig.service'
```

Eine gut konfigurierte Validating-Webhook-Konfiguration würde genau so aussehen:

```json
{
  "name": "osm-config-validator",
  "namespace": "kube-system",
  "path": "/validate-webhook",
  "port": 9093
}
```

#### <a name="check-for-the-service-and-the-ca-bundle-of-the-mutating-webhook"></a>Prüfen Sie auf den Dienst und das CA-Bundle des mutierenden Webhooks

```azurecli-interactive
kubectl get MutatingWebhookConfiguration aks-osm-webhook-osm -o json | jq '.webhooks[0].clientConfig.service'
```

Eine gut konfigurierte Mutating-Webhook-Konfiguration würde genau so aussehen:

```json
{
  "name": "osm-injector",
  "namespace": "kube-system",
  "path": "/mutate-pod-creation",
  "port": 9090
}
```

#### <a name="check-whether-osm-controller-has-given-the-validating-or-mutating-webhook-a-ca-bundle"></a>Prüfen Sie, ob der OSM-Controller dem validierenden (oder mutierenden) Webhook ein CA-Bundle gegeben hat

> [!NOTE]
> Ab v0.8.2 ist es wichtig zu wissen, dass AKS RP den Validating Webhook installiert, AKS Reconciler dafür sorgt, dass er existiert, aber OSM Controller derjenige ist, der das CA-Bundle befüllt.

```azurecli-interactive
kubectl get ValidatingWebhookConfiguration aks-osm-webhook-osm -o json | jq -r '.webhooks[0].clientConfig.caBundle' | wc -c
```

```azurecli-interactive
kubectl get MutatingWebhookConfiguration aks-osm-webhook-osm -o json | jq -r '.webhooks[0].clientConfig.caBundle' | wc -c
```

```Example Output
1845
```

Diese Zahl gibt die Anzahl der Bytes oder die Größe des Zertifizierungsstellen Pakets an. Wenn dieser Wert leer, 0 oder eine Zahl unter 1000 ist, deutet dies darauf hin, dass das CA-Bundle nicht korrekt bereitgestellt wird. Ohne ein korrektes CA-Bundle würde der Validating Webhook fehlschlagen und den Benutzer daran hindern, Änderungen an der osm-config ConfigMap im kube-system-Namespace vorzunehmen.

Ein Beispielfehler, wenn das CA-Bündel falsch ist:

- Ein Versuch, die osm-config ConfigMap zu ändern:

```azurecli-interactive
kubectl patch ConfigMap osm-config -n kube-system --type merge --patch '{"data":{"config_resync_interval":"2m"}}'
```

- Error:

```
Error from server (InternalError): Internal error occurred: failed calling webhook "osm-config-webhook.k8s.io": Post https://osm-config-validator.kube-system.svc:9093/validate-webhook?timeout=30s: x509: certificate signed by unknown authority
```

Workaround für den Fall, dass die **Validierende** Webhook-Konfiguration ein schlechtes Zertifikat hat:

- Option 1: Starten Sie den OSM-Controller neu. Hierdurch wird der OSM-Controller neu gestartet. Beim Start wird das CA-Bundle der beiden Webhooks Mutating und Validating überschrieben.

```azurecli-interactive
kubectl rollout restart deployment -n kube-system osm-controller
```

- Option 2 - Option 2. Löschen Sie den Validierungs-Webhook - durch das Entfernen des Validierungs-Webhooks werden die Mutationen der `osm-config` ConfigMap nicht mehr validiert. Jeder Patch wird durchlaufen. Der AKS Reconciler wird irgendwann sicherstellen, dass der validierende Webhook existiert und ihn neu erstellen. Der OSM-Controller muss möglicherweise neu gestartet werden, um das CA-Bundle schnell neu zu schreiben.

```azurecli-interactive
kubectl delete ValidatingWebhookConfiguration aks-osm-webhook-osm
```

- Option 3 - Löschen und patchen: Mit dem folgenden Befehl wird der validierende Webhook gelöscht, so dass wir beliebige Werte hinzufügen können, und es wird sofort versucht, einen Patch anzuwenden. Höchstwahrscheinlich wird der AKS Reconciler nicht genug Zeit haben, um den validierenden Webhook abzugleichen und wiederherzustellen, was uns die Möglichkeit gibt, eine Änderung als letzten Ausweg anzuwenden:

```azurecli-interactive
kubectl delete ValidatingWebhookConfiguration aks-osm-webhook-osm; kubectl patch ConfigMap osm-config -n kube-system --type merge --patch '{"data":{"config_resync_interval":"15s"}}'
```

#### <a name="check-the-osm-config-configmap"></a>Überprüfen Sie die `osm-config` **ConfigMap**

> [!NOTE]
> Für den OSM-Controller ist es nicht erforderlich, `osm-config` dass die ConfigMap im kube-system-Namensraum vorhanden ist. Der Controller verfügt über angemessene Standardwerte für die Konfiguration und kann ohne ihn arbeiten.

Überprüfen des Vorhandenseins einer Datei:

```azurecli-interactive
kubectl get ConfigMap -n kube-system osm-config
```

Prüfen Sie den Inhalt der osm-config ConfigMap

```azurecli-interactive
kubectl get ConfigMap -n kube-system osm-config -o json | jq '.data'
```

```json
{
  "egress": "true",
  "enable_debug_server": "true",
  "enable_privileged_init_container": "false",
  "envoy_log_level": "error",
  "outbound_ip_range_exclusion_list": "169.254.169.254,168.63.129.16,20.193.20.233",
  "permissive_traffic_policy_mode": "true",
  "prometheus_scraping": "false",
  "service_cert_validity_duration": "24h",
  "use_https_ingress": "false"
}
```

`osm-config` ConfigMap-Werte:

| Schlüssel                              | type   | Zulässige Werte                                          | Standardwert                          | Funktion                                                                                                                                                                                                                                |
| -------------------------------- | ------ | ------------------------------------------------------- | -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| egress                           | bool   | true, false                                             | `"false"`                              | Ermöglicht ausgehenden Datenverkehr im Mesh.                                                                                                                                                                                                             |
| enable_debug_server              | bool   | true, false                                             | `"true"`                               | Aktiviert einen Debug-Endpunkt auf dem osm-controller-Pod, um Informationen zum Mesh wie Proxy-Verbindungen, Zertifikate und SMI-Richtlinien aufzulisten.                                                                                    |
| enable_privileged_init_container | bool   | true, false                                             | `"false"`                              | Aktiviert privilegierte Init-Container für Pods in Mesh. Wenn false, haben init-Container nur NET_ADMIN.                                                                                                                                   |
| envoy_log_level                  | Zeichenfolge | verfolgen, debuggen, info, warnung, warnung, fehler, kritisch, aus | `"error"`                              | Legt die Verbosität der Protokollierung des Envoy-Proxy-Sidecars fest, gilt nur für neu erstellte Pods, die dem Netz beitreten. Um die Protokollebene für vorhandene Pods zu aktualisieren, starten Sie die Bereitstellung mit `kubectl rollout restart` .                            |
| outbound_ip_range_exclusion_list | Zeichenfolge | Komma-getrennte Liste von IP-Bereichen der Form a.b.c.d/x | `-`                                    | Globale Liste der IP-Adressbereiche, die vom Abfangen des ausgehenden Datenverkehrs durch den Sidecar-Proxy ausgeschlossen werden sollen.                                                                                                                                    |
| permissive_traffic_policy_mode   | bool   | true, false                                             | `"false"`                              | Die Einstellung auf,  `true` aktiviert den allow-all-Modus im Mesh, d. h. keine Durchsetzung von Verkehrsrichtlinien im Mesh. Wenn der Wert auf festgelegt ist `false` , wird die Richtlinie für den gesamten Datenverkehr in Mesh aktiviert `SMI Traffic Target` |
| prometheus_scraping              | bool   | true, false                                             | `"true"`                               | Ermöglicht Prometheus Metrik-Scraping auf Sidecar-Proxys.                                                                                                                                                                                 |
| service_cert_validity_duration   | Zeichenfolge | 24 Std. 1 Std. 30 min (beliebige Zeitdauer)                          | `"24h"`                                | Legt die Gültigkeitsdauer des Service-Zertifikats fest, dargestellt als eine Folge von Dezimalzahlen, jeweils mit optionalem Bruch und einem Einheitensuffix.                                                                                             |
| tracing_enable                   | bool   | true, false                                             | `"false"`                              | Aktiviert die Jaeger-Ablaufverfolgung für das Netz.                                                                                                                                                                                                    |
| tracing_address                  | Zeichenfolge | jaeger.mesh-namespace.svc.cluster.local                 | `jaeger.kube-system.svc.cluster.local` | Adresse der Jaeger-Bereitstellung, wenn die Verfolgung aktiviert ist.                                                                                                                                                                                |
| tracing_endpoint                 | Zeichenfolge | /api/v2/spans                                           | /api/v2/spans                          | Endpunkt für Ablaufverfolgungsdaten, wenn Ablaufverfolgung aktiviert ist.                                                                                                                                                                                          |
| tracing_port                     | INT    | beliebiger ganzzahliger Wert ungleich Null                              | `"9411"`                               | Der Port, für den die Ablaufverfolgung aktiviert ist.                                                                                                                                                                                                       |
| use_https_ingress                | bool   | true, false                                             | `"false"`                              | Aktiviert den HTTPS-Ingress auf dem Mesh.                                                                                                                                                                                                      |
| config_resync_interval           | Zeichenfolge | unter 1 Minute schaltet dies ab                            | 0 (deaktiviert)                           | Wenn ein Wert über 1m (60s) angegeben wird, sendet der OSM-Controller alle verfügbaren Konfigurationen im angegebenen Intervall an jedes verbundene Envoy                                                                                                    |

#### <a name="check-namespaces"></a>Namespaces überprüfen

> [!NOTE]
> Der kube-system-Namespace wird nie an einem Service Mesh teilnehmen und wird nie mit den unten stehenden Schlüsseln/Werten beschriftet und/oder annotiert.

Wir verwenden den `osm namespace add` Befehl, um Namespaces mit einem bestimmten Service Mesh zu verknüpfen.
Wenn ein k8s-Namespace Teil des Netzes ist (oder damit er Teil des Netzes sein kann), muss Folgendes zutreffen:

Anzeigen der Anmerkungen mit

```azurecli-interactive
kubectl get namespace bookbuyer -o json | jq '.metadata.annotations'
```

Die folgenden Eigenschaften müssen vorhanden sein:

```Output
{
  "openservicemesh.io/sidecar-injection": "enabled"
}
```

Anzeigen der Etiketten mit

```azurecli-interactive
kubectl get namespace bookbuyer -o json | jq '.metadata.labels'
```

Die folgenden Eigenschaften müssen vorhanden sein:

```Output
{
  "openservicemesh.io/monitored-by": "osm"
}
```

Wenn ein Namespace nicht mit `"openservicemesh.io/sidecar-injection": "enabled"` annotiert oder nicht mit `"openservicemesh.io/monitored-by": "osm"`dem OSM-Injektor beschriftet ist, werden keine Envoy-Sidecars hinzugefügt.

> Hinweis: Nachdem aufgerufen wurde, werden `osm namespace add` nur **neue** Pods mit einem „Envoy Sidecar“ eingefügt. Vorhandene Pods müssen neu gestartet werden mit `kubectl rollout restart deployment ...`

#### <a name="verify-the-smi-crds"></a>Überprüfen Sie die SMI-CRDs:

Überprüfen Sie, ob der Cluster über die erforderlichen CRDs verfügt:

```azurecli-interactive
kubectl get crds
```

Folgendes muss auf Ihrem Computer installiert sein:

- httproutegroups.specs.smi-spec.io
- tcproutes.specs.smi-spec.io
- trafficsplits.split.smi-spec.io
- traffictargets.access.smi-spec.io
- udproutes.specs.smi-spec.io

Mit diesem Befehl können Sie die Versionen der installierten CRDs abrufen:

```azurecli-interactive
for x in $(kubectl get crds --no-headers | awk '{print $1}' | grep 'smi-spec.io'); do
    kubectl get crd $x -o json | jq -r '(.metadata.name, "----" , .spec.versions[].name, "\n")'
done
```

Erwartete Ausgabe:

```Output
httproutegroups.specs.smi-spec.io
----
v1alpha4
v1alpha3
v1alpha2
v1alpha1


tcproutes.specs.smi-spec.io
----
v1alpha4
v1alpha3
v1alpha2
v1alpha1


trafficsplits.split.smi-spec.io
----
v1alpha2


traffictargets.access.smi-spec.io
----
v1alpha3
v1alpha2
v1alpha1


udproutes.specs.smi-spec.io
----
v1alpha4
v1alpha3
v1alpha2
v1alpha1
```

OSM Controller v 0.8.2 erfordert die folgenden Versionen:

- traffictargets.access.smi-spec.io - [v1alpha3](https://github.com/servicemeshinterface/smi-spec/blob/v0.6.0/apis/traffic-access/v1alpha3/traffic-access.md)
- httproutegroups.specs.smi-spec.io - [v1alpha4](https://github.com/servicemeshinterface/smi-spec/blob/v0.6.0/apis/traffic-specs/v1alpha4/traffic-specs.md#httproutegroup)
- tcproutes.specs.smi-spec.io - [v1alpha4](https://github.com/servicemeshinterface/smi-spec/blob/v0.6.0/apis/traffic-specs/v1alpha4/traffic-specs.md#tcproute)
- udproutes.specs.smi-spec.io - Nicht unterstützt
- trafficsplits.split.smi-spec.io - [v1alpha2](https://github.com/servicemeshinterface/smi-spec/blob/v0.6.0/apis/traffic-split/v1alpha2/traffic-split.md)
- \*.metrics.smi-spec.io - [v1alpha1](https://github.com/servicemeshinterface/smi-spec/blob/v0.6.0/apis/traffic-metrics/v1alpha1/traffic-metrics.md)

Wenn CRDs fehlen, verwenden Sie die folgenden Befehle, um diese im Cluster zu installieren:

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/v0.8.2/charts/osm/crds/access.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/v0.8.2/charts/osm/crds/specs.yaml
```

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/openservicemesh/osm/v0.8.2/charts/osm/crds/split.yaml
```

## <a name="disable-open-service-mesh-osm-add-on-for-your-aks-cluster"></a>Deaktivieren Sie das OSM-Add-on für Ihren AKS-Cluster

Führen Sie den folgenden Befehl aus, um das OSM-Add-On zu deaktivieren:

```azurecli-interactive
az aks disable-addons -n <AKS-cluster-name> -g <AKS-resource-group-name> -a open-service-mesh
```

<!-- LINKS - internal -->

[kubernetes-service]: concepts-network.md#services
[az-feature-register]: /cli/azure/feature?view=azure-cli-latest&preserve-view=true#az_feature_register
[az-feature-list]: /cli/azure/feature?view=azure-cli-latest&preserve-view=true#az_feature_list
[az-provider-register]: /cli/azure/provider?view=azure-cli-latest&preserve-view=true#az_provider_register