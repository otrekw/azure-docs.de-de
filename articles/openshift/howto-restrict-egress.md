---
title: Einschränken des ausgehenden Datenverkehrs in einem Azure Red Hat OpenShift-Cluster (ARO)
description: Es wird beschrieben, welche Ports und Adressen zur Steuerung des ausgehenden Datenverkehrs unter Azure Red Hat OpenShift (ARO) erforderlich sind.
author: sakthi-vetrivel
ms.author: jzim
ms.service: azure-redhat-openshift
ms.topic: article
ms.date: 04/09/2021
ms.openlocfilehash: c15e7261587952c93d059cfcfadd06c7d204d80d
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108135671"
---
# <a name="control-egress-traffic-for-your-azure-red-hat-openshift-aro-cluster-preview"></a>Steuern des ausgehenden Datenverkehrs für Ihren Azure Red Hat OpenShift-Cluster (ARO) (Vorschau)

Dieser Artikel enthält die erforderlichen Informationen, die Sie zum Schützen des ausgehenden Datenverkehrs Ihres Azure Red Hat OpenShift-Clusters (ARO) benötigen. Sie erhalten Informationen zu den Clusteranforderungen für eine grundlegende ARO-Bereitstellung sowie zu weiteren Anforderungen für optionale Red Hat- und Drittanbieterkomponenten. Am Ende des Artikels wird anhand eines [Beispiels](#private-aro-cluster-setup) veranschaulicht, wie diese Anforderungen mit Azure Firewall konfiguriert werden. Beachten Sie hierbei den Hinweis, dass Sie diese Informationen auch auf Azure Firewall oder jede andere Einschränkungsmethode oder Appliance für ausgehenden Datenverkehr anwenden können.

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird vorausgesetzt, dass Sie einen neuen Cluster erstellen. Falls Sie einen einfachen ARO-Cluster benötigen, helfen Ihnen die Informationen in der [Schnellstartanleitung zu ARO](./tutorial-create-cluster.md) weiter.

> [!IMPORTANT]
> ARO-Vorschaufunktionen stehen gemäß dem Self-Service- und Aktivierungsprinzip zur Verfügung. Vorschauversionen werden „wie besehen“ und „wie verfügbar“ bereitgestellt und sind von den Vereinbarungen zum Service Level und der eingeschränkten Garantie ausgeschlossen. ARO-Vorschauversionen werden teilweise vom Kundensupport auf Grundlage der bestmöglichen Leistung abgedeckt.

## <a name="minimum-required-fqdn--application-rules"></a>Mindestens erforderliche FQDNs/Anwendungsregeln

Diese Liste basiert auf der Liste mit den FQDNs in der OpenShift-Dokumentation unter https://docs.openshift.com/container-platform/4.6/installing/install_config/configuring-firewall.html.

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind erforderlich:

| Ziel-FQDN | Port | Zweck |
| ----------- | ----------- | ------------- |
| **`quay.io`** | **HTTPS: 443** | Ist obligatorisch für die Installation und wird vom Cluster genutzt. Wird vom Cluster verwendet, um die Plattformcontainerimages herunterzuladen. |
| **`registry.redhat.io`** | **HTTPS: 443** | Obligatorisch für wichtige Add-Ons. Wird vom Cluster zum Herunterladen von Kernkomponenten verwendet, z. B. Entwicklungstools, operatorbasierte Add-Ons und von Red Hat bereitgestellte Containerimages.
| **`mirror.openshift.com`** | **HTTPS: 443** | Wird in der VDI-Umgebung oder auf Ihrem Laptop benötigt, um auf gespiegelte Installationsinhalte und Images zuzugreifen. Ist im Cluster zum Herunterladen von Plattformfreigabesignaturen erforderlich, um ermitteln zu können, welche Images von „quay.io“ gepullt werden müssen. |
| **`api.openshift.com`** | **HTTPS: 443** | Hiermit kann der Cluster überprüfen, ob Updates verfügbar sind, bevor die Imagesignaturen heruntergeladen werden. |
| **`arosvc.azurecr.io`** | **HTTPS: 443** | Interne private Registrierung für ARO-Operators.  Erforderlich, wenn Sie „Microsoft.ContainerRegistry“ für Ihre Dienstendpunkte in Ihren Subnetzen nicht zulassen. |
| **`management.azure.com`** | **HTTPS: 443** | Wird vom Cluster für den Zugriff auf Azure-APIs verwendet. |
| **`login.microsoftonline.com`** | **HTTPS: 443** | Wird vom Cluster für die Authentifizierung bei Azure verwendet. |
| **`gcs.prod.monitoring.core.windows.net`** | **HTTPS: 443** | Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann. |
| **`*.blob.core.windows.net`** | **HTTPS: 443** | Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann. |
| **`*.servicebus.windows.net`** | **HTTPS: 443** | Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann. |
| **`*.table.core.windows.net`** | **HTTPS: 443** | Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann. |

> [!NOTE] 
> Für viele Kunden ist das Verfügbarmachen von „*.blob“, „*.table“ und anderen großen Adressräumen mit einem potenziellen Datenexfiltrationsrisiko verbunden. Es kann ratsam sein, die [Firewall für ausgehenden Datenverkehr von OpenShift](https://docs.openshift.com/container-platform/4.6/networking/openshift_sdn/configuring-egress-firewall.html) zum Schützen der im Cluster bereitgestellten Anwendungen zu verwenden, indem das Erreichen dieser Ziele verhindert wird. Azure Private Link kann für spezifische Anwendungsanforderungen genutzt werden.

---

## <a name="complete-list-of-required-and-optional-fqdns"></a>Vollständige Liste mit den erforderlichen und optionalen FQDNs

### <a name="first-group-installing-and-downloading-packages-and-tools"></a>ERSTE GRUPPE: INSTALLIEREN UND HERUNTERLADEN VON PAKETEN UND TOOLS

- **`quay.io`** : Ist obligatorisch für die Installation und wird vom Cluster genutzt. Wird vom Cluster verwendet, um die Plattformcontainerimages herunterzuladen.
- **`registry.redhat.io`** : Obligatorisch für wichtige Add-Ons. Wird vom Cluster verwendet, um Kernkomponenten herunterzuladen, z. B. Entwicklungstools, operatorbasierte Add-Ons oder von Red Hat bereitgestellte Containerimages (z. B. unsere Middleware, das universelle Basisimage usw.).
- **`sso.redhat.com`** : Wird in der VDI-Umgebung oder auf Ihrem Laptop benötigt, um eine Verbindung mit „cloud.redhat.com“ herzustellen. Dies ist die Website, auf der wir das Pullgeheimnis herunterladen und einige SaaS-Lösungen nutzen können, die wir in Red Hat anbieten, um beispielsweise die Überwachung Ihrer Abonnements, des Clusterbestands, der Berichte zur verbrauchsbasierten Kostenzuteilung usw. zu ermöglichen.
- **`openshift.org`** : Wird in der VDI-Umgebung oder auf Ihrem Laptop benötigt, um die Verbindung für den Download von RH CoreOS-Images herzustellen. Da diese Betriebssystemimages in Azure über den Marketplace ausgewählt werden, ist der Download in Azure nicht erforderlich.

---

### <a name="second-group-telemetry"></a>ZWEITE GRUPPE: TELEMETRIE

Dieser gesamte Abschnitt kann auch deaktiviert werden, aber es ist ratsam, sich vorher darüber zu informieren: https://docs.openshift.com/container-platform/4.6/support/remote_health_monitoring/about-remote-health-monitoring.html
- **`cert-api.access.redhat.com`** : Wird in Ihrer VDI-Umgebung oder auf Ihrem Laptop benötigt.
- **`api.access.redhat.com`** : Wird in Ihrer VDI-Umgebung oder auf Ihrem Laptop benötigt.
- **`infogw.api.openshift.com`** : Wird in Ihrer VDI-Umgebung oder auf Ihrem Laptop benötigt.
- **`https://cloud.redhat.com/api/ingress`** : Wird im Cluster für den Insights-Operator verwendet, der mit SaaS Red Hat Insights integriert wird.
Auf der OpenShift Container Platform können Kunden die Berichterstellung zur Integrität und Nutzung deaktivieren. Bei verbundenen Clustern kann Red Hat aber schneller auf Probleme reagieren, und die Kunden können besser unterstützt werden. Darüber hinaus kann auch ein besseres Verständnis von Produktupgrades für Cluster entwickelt werden. Weitere Informationen: https://docs.openshift.com/container-platform/4.6/support/remote_health_monitoring/opting-out-of-remote-health-reporting.html

---

### <a name="third-group-cloud-apis"></a>DRITTE GRUPPE: CLOUD-APIs

- **`management.azure.com`** : Wird vom Cluster für den Zugriff auf Azure-APIs verwendet.

---

### <a name="fourth-group-other-openshift-requirements"></a>VIERTE GRUPPE: ANDERE OPENSHIFT-ANFORDERUNGEN

- **`mirror.openshift.com`** : Wird in der VDI-Umgebung bzw. auf Ihrem Laptop benötigt, um auf gespiegelte Installationsinhalte und Images zuzugreifen. Im Cluster wird dieser FQDN zum Herunterladen von Plattformfreigabesignaturen benötigt, damit der Cluster über die Informationen dazu verfügt, welche Images von „quay.io“ gepullt werden müssen.
- **`storage.googleapis.com/openshift-release`** : Alternative Website zum Herunterladen von Plattformfreigabesignaturen, mit denen der Cluster ermitteln kann, welche Images von „quay.io“ gepullt werden müssen.
- **`*.apps.<cluster_name>.<base_domain>`** (ODER GLEICHWERTIGE ARO-URL): Wenn Domänen in Positivlisten eingefügt werden, wird dieser FQDN in Ihrem Unternehmensnetzwerk genutzt, um unter OpenShift bereitgestellte Anwendungen zu erreichen oder auf die OpenShift-Konsole zuzugreifen.
- **`api.openshift.com`** : Hiermit kann der Cluster überprüfen, ob Updates verfügbar sind, bevor die Imagesignaturen heruntergeladen werden.
- **`registry.access.redhat.com`** : Der Registrierungszugriff ist in Ihrer VDI-Umgebung bzw. auf Ihrem Laptop erforderlich, um bei Verwendung des ODO-CLI-Tools Entwicklungsimages herunterzuladen. (Dieses CLI-Tool ist eine Alternative für Entwickler, die nicht mit Kubernetes vertraut sind.) https://docs.openshift.com/container-platform/4.6/cli_reference/developer_cli_odo/understanding-odo.html

---

### <a name="fifth-group-microsoft--red-hat-aro-monitoring-service"></a>FÜNFTE GRUPPE: ÜBERWACHUNGSDIENST FÜR MICROSOFT UND RED HAT ARO

- **`login.microsoftonline.com`** : Wird vom Cluster für die Authentifizierung bei Azure verwendet.
- **`gcs.prod.monitoring.core.windows.net`** : Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann.
- **`*.blob.core.windows.net`** : Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann.
- **`*.servicebus.windows.net`** : Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann.
- **`*.table.core.windows.net`** : Wird für die Microsoft Geneva-Überwachung verwendet, damit das ARO-Team die Cluster des Kunden überwachen kann.

## <a name="aro-integrations"></a>ARO-Integrationen

### <a name="azure-monitor-for-containers"></a>Azure Monitor für Container

Der Zugriff auf Azure Monitor für Container kann auf zwei Arten ermöglicht werden: Sie können das Azure Monitor-Diensttag ([ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags)) zulassen **oder** Zugriff auf die erforderlichen FQDNs/Anwendungsregeln gewähren.  Eine Anleitung zum Hinzufügen von Azure Monitor zu Ihrem vorhandenen ARO-Cluster finden Sie [hier](../azure-monitor/containers/container-insights-azure-redhat4-setup.md).

#### <a name="required-network-rules"></a>Benötigte Netzwerkregeln

Die folgenden vollqualifizierten Domänennamen und Anwendungsregeln sind erforderlich:

| Zielendpunkt                                                             | Protokoll | Port    | Zweck  |
|----------------------------------------------------------------------------------|----------|---------|------|
| [ServiceTag](../virtual-network/service-tags-overview.md#available-service-tags) -  **`AzureMonitor:443`**  | TCP           | 443      | Dieser Endpunkt dient zum Senden von Metrikdaten und Protokollen an Azure Monitor und Log Analytics. |

#### <a name="required-fqdn--application-rules"></a>Benötigte FQDNs/Anwendungsregeln

Die folgenden vollqualifizierten Domänennamen bzw. Anwendungsregeln sind für ARO-Cluster mit Aktivierung von Azure Monitor für Container erforderlich:

| FQDN                                    | Port      | Zweck      |
|-----------------------------------------|-----------|----------|
| **`dc.services.visualstudio.com`** | **`HTTPS:443`**    | Dieser Endpunkt wird für Metriken sowie zur Überwachung von Telemetriedaten mithilfe von Azure Monitor verwendet. |
| **`*.ods.opinsights.azure.com`**    | **`HTTPS:443`**    | Dieser Endpunkt wird von Azure Monitor zur Erfassung von Log Analytics-Daten verwendet. |
| **`*.oms.opinsights.azure.com`** | **`HTTPS:443`** | Dieser Endpunkt wird von „omsagent“ verwendet. „omsagent“ dient zur Authentifizierung des Log Analytics-Diensts. |
| **`*.monitoring.azure.com`** | **`HTTPS:443`** | Dieser Endpunkt wird zum Senden von Metrikdaten an Azure Monitor verwendet. |


## <a name="private-aro-cluster-setup"></a>Einrichten eines privaten ARO-Clusters
Das Ziel ist der Schutz des ARO-Clusters, indem ausgehender Datenverkehr über eine Azure Firewall-Instanz geleitet wird.
### <a name="before"></a>Vorher:
![Vorher](media/concepts-networking/aro-private.jpg)
### <a name="after"></a>Nachher:
![Nach](media/concepts-networking/aro-fw.jpg)

## <a name="create-a-private-aro-cluster"></a>Erstellen eines privaten ARO-Clusters

### <a name="set-up-vars-for-your-environment"></a>Einrichten von Variablen für Ihre Umgebung
```bash

CLUSTER=aro-cluster # Name of your created cluster
RESOURCEGROUP=aro-rg # The name of your resource group where you created the ARO cluster
AROVNET=aro-vnet # The name of your vnet from your created ARO cluster
JUMPSUBNET=jump-subnet
LOCATION=eastus # The location where ARO cluster is deployed

```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
```bash
az group create -g "$RESOURCEGROUP" -l $LOCATION
```

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks
```bash
az network vnet create \
  -g $RESOURCEGROUP \
  -n $AROVNET \
  --address-prefixes 10.0.0.0/8
```

### <a name="add-two-empty-subnets-to-your-virtual-network"></a>Hinzufügen von zwei leeren Subnetzen zu Ihrem virtuellen Netzwerk
```bash
  az network vnet subnet create \
    -g "$RESOURCEGROUP" \
    --vnet-name $AROVNET \
    -n "$CLUSTER-master" \
    --address-prefixes 10.10.1.0/24 \
    --service-endpoints Microsoft.ContainerRegistry

  az network vnet subnet create \
    -g $RESOURCEGROUP \
    --vnet-name $AROVNET \
    -n "$CLUSTER-worker" \
    --address-prefixes 10.20.1.0/24 \
    --service-endpoints Microsoft.ContainerRegistry
```

### <a name="disable-network-policies-for-private-link-service-on-your-virtual-network-and-subnets-this-is-a-requirement-for-the-aro-service-to-access-and-manage-the-cluster"></a>Deaktivieren Sie Netzwerkrichtlinien für den Private Link-Dienst in Ihrem virtuellen Netzwerk und in den Subnetzen. Dies ist erforderlich, damit der ARO-Dienst auf den Cluster zugreifen und ihn verwalten kann.
```bash
az network vnet subnet update \
  -g "$RESOURCEGROUP" \
  --vnet-name $AROVNET \
  -n "$CLUSTER-master" \
  --disable-private-link-service-network-policies true
```
### <a name="create-a-firewall-subnet"></a>Erstellen eines Firewallsubnetzes
```bash
az network vnet subnet create \
    -g "$RESOURCEGROUP" \
    --vnet-name $AROVNET \
    -n "AzureFirewallSubnet" \
    --address-prefixes 10.100.1.0/26
```

## <a name="create-a-jump-host-vm"></a>Erstellen einer Jump-Host-VM
### <a name="create-a-jump-subnet"></a>Erstellen eines Jump-Subnetzes
```bash
  az network vnet subnet create \
    -g "$RESOURCEGROUP" \
    --vnet-name $AROVNET \
    -n $JUMPSUBNET \
    --address-prefixes 10.30.1.0/24 \
    --service-endpoints Microsoft.ContainerRegistry
```
### <a name="create-a-jump-host-vm"></a>Erstellen einer Jump-Host-VM
```bash
VMUSERNAME=aroadmin

az vm create --name ubuntu-jump \
             --resource-group $RESOURCEGROUP \
             --ssh-key-values ~/.ssh/id_rsa.pub \
             --admin-username $VMUSERNAME \
             --image UbuntuLTS \
             --subnet $JUMPSUBNET \
             --public-ip-address jumphost-ip \
             --vnet-name $AROVNET 
```

## <a name="create-an-azure-red-hat-openshift-cluster"></a>Erstellen eines Azure Red Hat OpenShift-Clusters
### <a name="get-a-red-hat-pull-secret-optional"></a>Abrufen eines Red Hat-Pullgeheimnisses (optional)

Ein Red Hat-Pullgeheimnis ermöglicht Ihrem Cluster den Zugriff auf Red Hat-Containerregistrierungen und andere Inhalte. Dieser Schritt ist optional, wird aber empfohlen.

1. **[Navigieren Sie zum Manager-Portal für Ihren Red Hat OpenShift-Cluster](https://cloud.redhat.com/openshift/install/azure/aro-provisioned), und melden Sie sich an.**

   Sie müssen sich bei Ihrem Red Hat-Konto anmelden oder ein neues Red Hat-Konto mit Ihrer Geschäfts-E-Mail-Adresse erstellen und den Geschäftsbedingungen zustimmen.

2. **Klicken Sie auf „Download pull secret“ (Pullgeheimnis herunterladen).**

Speichern Sie die Datei `pull-secret.txt` an einem sicheren Ort. Sie wird bei jeder Clustererstellung verwendet.

Wenn Sie den Befehl `az aro create` ausführen, können Sie mit dem Parameter `--pull-secret @pull-secret.txt` auf Ihr Pullgeheimnis verweisen. Führen Sie `az aro create` von dem Verzeichnis aus, in dem Sie Ihre `pull-secret.txt`-Datei gespeichert haben. Ersetzen Sie andernfalls `@pull-secret.txt` durch `@<path-to-my-pull-secret-file`.

Wenn Sie Ihr Pullgeheimnis kopieren oder es in anderen Skripts referenzieren, sollte Ihr Pullgeheimnis als gültige JSON-Zeichenfolge formatiert werden.

```bash
az aro create \
  -g "$RESOURCEGROUP" \
  -n "$CLUSTER" \
  --vnet $AROVNET \
  --master-subnet "$CLUSTER-master" \
  --worker-subnet "$CLUSTER-worker" \
  --apiserver-visibility Private \
  --ingress-visibility Private \
  --pull-secret @pull-secret.txt
```

## <a name="create-an-azure-firewall"></a>Erstellen einer Azure Firewall-Instanz

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
```bash
az network public-ip create -g $RESOURCEGROUP -n fw-ip --sku "Standard" --location $LOCATION
```
### <a name="update-install-azure-firewall-extension"></a>Aktualisieren der Installation der Azure Firewall-Erweiterung
```bash
az extension add -n azure-firewall
az extension update -n azure-firewall
```

### <a name="create-azure-firewall-and-configure-ip-config"></a>Erstellen der Azure Firewall-Instanz und Durchführen der IP-Konfiguration
```bash
az network firewall create -g $RESOURCEGROUP -n aro-private -l $LOCATION
az network firewall ip-config create -g $RESOURCEGROUP -f aro-private -n fw-config --public-ip-address fw-ip --vnet-name $AROVNET

```

### <a name="capture-azure-firewall-ips-for-a-later-use"></a>Erfassen von Azure Firewall-IP-Adressen zur späteren Verwendung
```bash
FWPUBLIC_IP=$(az network public-ip show -g $RESOURCEGROUP -n fw-ip --query "ipAddress" -o tsv)
FWPRIVATE_IP=$(az network firewall show -g $RESOURCEGROUP -n aro-private --query "ipConfigurations[0].privateIpAddress" -o tsv)

echo $FWPUBLIC_IP
echo $FWPRIVATE_IP
```

### <a name="create-a-udr-and-routing-table-for-azure-firewall"></a>Erstellen einer UDR und einer Routingtabelle für Azure Firewall
```bash
az network route-table create -g $RESOURCEGROUP --name aro-udr

az network route-table route create -g $RESOURCEGROUP --name aro-udr --route-table-name aro-udr --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP
```

### <a name="add-application-rules-for-azure-firewall"></a>Hinzufügen von Anwendungsregeln für Azure Firewall
Regel für OpenShift-Betrieb basierend auf [dieser Liste](https://docs.openshift.com/container-platform/4.3/installing/install_config/configuring-firewall.html#configuring-firewall_configuring-firewall):
```bash
az network firewall application-rule create -g $RESOURCEGROUP -f aro-private \
 --collection-name 'ARO' \
 --action allow \
 --priority 100 \
 -n 'required' \
 --source-addresses '*' \
 --protocols 'http=80' 'https=443' \
 --target-fqdns 'registry.redhat.io' '*.quay.io' 'sso.redhat.com' 'management.azure.com' 'mirror.openshift.com' 'api.openshift.com' 'quay.io' '*.blob.core.windows.net' 'gcs.prod.monitoring.core.windows.net' 'registry.access.redhat.com' 'login.microsoftonline.com' '*.servicebus.windows.net' '*.table.core.windows.net' 'grafana.com'
```
Optionale Regeln für Docker-Images:
```bash
az network firewall application-rule create -g $RESOURCEGROUP -f aro-private \
 --collection-name 'Docker' \
 --action allow \
 --priority 200 \
 -n 'docker' \
 --source-addresses '*' \
 --protocols 'http=80' 'https=443' \
 --target-fqdns '*cloudflare.docker.com' '*registry-1.docker.io' 'apt.dockerproject.org' 'auth.docker.io'
```

### <a name="associate-aro-subnets-to-fw"></a>Zuordnen von ARO-Subnetzen zur Firewall
```bash
az network vnet subnet update -g $RESOURCEGROUP --vnet-name $AROVNET --name "$CLUSTER-master" --route-table aro-udr
az network vnet subnet update -g $RESOURCEGROUP --vnet-name $AROVNET --name "$CLUSTER-worker" --route-table aro-udr
```

## <a name="test-the-configuration-from-the-jumpbox"></a>Testen der Konfiguration für die Jumpbox
Diese Schritte funktionieren nur, wenn Sie Regeln für Docker-Images hinzugefügt haben. 
### <a name="configure-the-jumpbox"></a>Konfigurieren der Jumpbox
Melden Sie sich bei einer Jumpbox-VM an, und installieren Sie die Hilfsprogramme `azure-cli`, `oc-cli` und `jq`. Informationen zur Installation von „openshift-cli“ finden Sie im Red Hat-Kundenportal.
```bash
#Install Azure-cli
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
#Install jq
sudo apt install jq -y
```
### <a name="log-into-the-aro-cluster"></a>Anmelden beim ARO-Cluster
Auflisten von Anmeldeinformationen für den Cluster:
```bash

# Login to Azure
az login
# Set Vars in Jumpbox
CLUSTER=aro-cluster # Name of your created cluster
RESOURCEGROUP=aro-rg # The name of your resource group where you created the ARO cluster

#Get the cluster credentials
ARO_PASSWORD=$(az aro list-credentials -n $CLUSTER -g $RESOURCEGROUP -o json | jq -r '.kubeadminPassword')
ARO_USERNAME=$(az aro list-credentials -n $CLUSTER -g $RESOURCEGROUP -o json | jq -r '.kubeadminUsername')
```
Abrufen eines API-Serverendpunkts:
```bash
ARO_URL=$(az aro show -n $CLUSTER -g $RESOURCEGROUP -o json | jq -r '.apiserverProfile.url')
```

### <a name="download-the-oc-cli-to-the-jumpbox"></a>Herunterladen von „oc-cli“ auf die Jumpbox
```bash
cd ~
wget https://mirror.openshift.com/pub/openshift-v4/clients/ocp/latest/openshift-client-linux.tar.gz

mkdir openshift
tar -zxvf openshift-client-linux.tar.gz -C openshift
echo 'export PATH=$PATH:~/openshift' >> ~/.bashrc && source ~/.bashrc
```

Anmeldung per `oc login`:
```bash
oc login $ARO_URL -u $ARO_USERNAME -p $ARO_PASSWORD
```

### <a name="run-centos-to-test-outside-connectivity"></a>Ausführen von CentOS zum Testen der Konnektivität in ausgehender Richtung
Erstellen eines Pods
```bash
cat <<EOF | oc apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: centos
spec:
  containers:
  - name: centos
    image: centos
    ports:
    - containerPort: 80
    command:
    - sleep
    - "3600"
EOF
```
Greifen Sie nach Beginn der Ausführung mit „exec“ auf den Pod zu, und testen Sie die Konnektivität in ausgehender Richtung.

```bash
oc exec -it centos -- /bin/bash
curl microsoft.com
```

## <a name="access-the-web-console-of-the-private-cluster"></a>Zugreifen auf die Webkonsole des privaten Clusters

### <a name="set-up-ssh-forwards-commands"></a>Einrichten von SSH-Weiterleitungsbefehlen

```bash
sudo ssh -i $SSH_PATH -L 443:$CONSOLE_URL:443 aroadmin@$JUMPHOST

example:
sudo ssh -i /Users/jimzim/.ssh/id_rsa -L 443:console-openshift-console.apps.d5xm5iut.eastus.aroapp.io:443 aroadmin@104.211.18.56
```

### <a name="modify-the-etc-hosts-file-on-your-local-machine"></a>Ändern der Datei „etc/hosts“ auf Ihrem lokalen Computer
```bash
##
# Host Database
#
127.0.0.1 console-openshift-console.apps.d5xm5iut.eastus.aroapp.io
127.0.0.1 oauth-openshift.apps.d5xm5iut.eastus.aroapp.io
```

### <a name="use-sshuttle-as-another-option"></a>Verwenden von sshuttle als weitere Option

[sshuttle](https://github.com/sshuttle/sshuttle)


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

```bash

# Clean up the ARO cluster, vnet, firewall and jumpbox

# Remove udr from master and worker subnets first or will get error when deleting ARO cluster
az network vnet subnet update --vnet-name $AROVNET -n aro-cluster-master -g $RESOURCEGROUP --route-table aro-udr --remove routeTable
az network vnet subnet update --vnet-name $AROVNET -n aro-cluster-worker -g $RESOURCEGROUP --route-table aro-udr --remove routeTable

# Remove ARO Cluster
az aro delete -n $CLUSTER -g $RESOURCEGROUP

# Remove the resource group that contains the firewall, jumpbox and vnet
az group delete -n $RESOURCEGROUP
```