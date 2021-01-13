---
title: Erstellen eines Datencontrollers mit der Azure Data CLI (azdata)
description: Erstellen Sie einen Azure Arc-Datencontroller in einem typischen Kubernetes-Cluster mit mehreren Knoten, den Sie bereits mithilfe der Azure Data CLI (azdata) erstellt haben.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 0b4cf72622df78e13add723853d935fc97649b4a
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/12/2020
ms.locfileid: "97358995"
---
# <a name="create-azure-arc-data-controller-using-the-azure-data-cli-azdata"></a>Erstellen eines Azure Arc-Datencontrollers mithilfe von [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="prerequisites"></a>Voraussetzungen

Lesen Sie die Übersicht im Thema [Erstellen des Azure Arc-Datencontrollers](create-data-controller.md).

Zum Erstellen des Azure Arc-Datencontrollers mithilfe der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] muss die [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)] installiert sein.

   [Installieren der [!INCLUDE [azure-data-cli-azdata](../../../includes/azure-data-cli-azdata.md)]](install-client-tools.md)

Unabhängig von der ausgewählten Zielplattform müssen Sie vor der Erstellung die folgenden Umgebungsvariablen für den Datencontrolleradministrator festlegen. Sie können diese Anmeldeinformationen anderen Benutzern mitteilen, die ggf. Administratorzugriff auf den Datencontroller benötigen.

**AZDATA_USERNAME**: Ein Benutzername Ihrer Wahl für den Datencontrolleradministrator. Beispiel: `arcadmin`

**AZDATA_PASSWORD**: Ein Kennwort Ihrer Wahl für den Datencontrolleradministrator. Das Kennwort muss mindestens acht Zeichen enthalten, die aus drei der folgenden vier Kategorien stammen: Großbuchstaben, Kleinbuchstaben, Ziffern und Symbole.

### <a name="linux-or-macos"></a>Linux oder macOS

```console
export AZDATA_USERNAME="<your username of choice>"
export AZDATA_PASSWORD="<your password of choice>"
```

### <a name="windows-powershell"></a>Windows PowerShell

```console
$ENV:AZDATA_USERNAME="<your username of choice>"
$ENV:AZDATA_PASSWORD="<your password of choice>"
```

Sie müssen eine Verbindung mit einem Kubernetes-Cluster herstellen und sich authentifizieren sowie einen vorhandenen Kubernetes-Kontext auswählen, bevor Sie mit der Erstellung des Azure Arc-Datencontrollers beginnen. Wie Sie eine Verbindung mit einem Kubernetes-Cluster oder -Dienst herstellen, ist von Fall zu Fall unterschiedlich. Informieren Sie sich in der Dokumentation zur verwendeten Kubernetes-Distribution oder zum verwendeten Kubernetes-Dienst darüber, wie eine Verbindung mit dem Kubernetes-API-Server hergestellt wird.

Mit den folgenden Befehlen können Sie überprüfen, ob Sie über eine aktuelle Kubernetes-Verbindung verfügen, und den aktuellen Kontext bestätigen.

```console
kubectl get namespace
kubectl config current-context
```

### <a name="connectivity-modes"></a>Konnektivitätsmodi

Wie in [Konnektivitätsmodi und Anforderungen](https://docs.microsoft.com/azure/azure-arc/data/connectivity) beschrieben, kann der Azure Arc-Datencontroller mit dem Konnektivitätsmodus `direct` oder `indirect` bereitgestellt werden. Mit dem Konnektivitätsmodus `direct` werden Verwendungsdaten automatisch und kontinuierlich an Azure gesendet. In diesen Artikeln geben die Beispiele den Konnektivitätsmodus `direct` wie folgt an:

   ```console
   --connectivity-mode direct
   ```

   Um den Controller mit dem Konnektivitätsmodus `indirect` zu erstellen, aktualisieren Sie die Skripts im Beispiel wie unten angegeben:

   ```console
   --connectivity-mode indirect
   ```

#### <a name="create-service-principal"></a>Erstellen eines Dienstprinzipals

Wenn Sie den Azure Arc-Datencontroller mit dem Konnektivitätsmodus `direct` bereitstellen, sind die Anmeldeinformationen des Dienstprinzipals für die Azure-Konnektivität erforderlich. Der Dienstprinzipal wird verwendet, um Nutzungs- und Metrikdaten hochzuladen. 

Führen Sie die folgenden Befehle aus, um den Dienstprinzipal für den Metrikupload zu erstellen:

> [!NOTE]
> Zum Erstellen eines Dienstprinzipals sind [bestimmte Berechtigungen in Azure](../../active-directory/develop/howto-create-service-principal-portal.md#permissions-required-for-registering-an-app) erforderlich.

Aktualisieren Sie zum Erstellen eines Dienstprinzipals das folgende Beispiel. Ersetzen Sie `<ServicePrincipalName>` durch den Namen des Dienstprinzipals, und führen Sie den folgenden Befehl aus:

```azurecli
az ad sp create-for-rbac --name <ServicePrincipalName>
``` 

Wenn Sie den Dienstprinzipal zuvor erstellt haben und nur die aktuellen Anmeldeinformationen abrufen müssen, führen Sie den folgenden Befehl aus, um die Anmeldeinformationen zurückzusetzen:

```azurecli
az ad sp credential reset --name <ServicePrincipalName>
```

Führen Sie zum Erstellen eines Dienstprinzipals mit dem Namen `azure-arc-metrics` beispielsweise den folgenden Befehl aus:

```console
az ad sp create-for-rbac --name azure-arc-metrics
```

Beispielausgabe:

```output
"appId": "2e72adbf-de57-4c25-b90d-2f73f126e123",
"displayName": "azure-arc-metrics",
"name": "http://azure-arc-metrics",
"password": "5039d676-23f9-416c-9534-3bd6afc78123",
"tenant": "72f988bf-85f1-41af-91ab-2d7cd01ad1234"
```

Speichern Sie die Werte `appId`, `password` und `tenant` in einer Umgebungsvariable, um sie später zu verwenden. 

#### <a name="save-environment-variables-in-windows"></a>Speichern von Umgebungsvariablen unter Windows

```console
SET SPN_CLIENT_ID=<appId>
SET SPN_CLIENT_SECRET=<password>
SET SPN_TENANT_ID=<tenant>
SET SPN_AUTHORITY=https://login.microsoftonline.com
```

#### <a name="save-environment-variables-in-linux-or-macos"></a>Speichern von Umgebungsvariablen unter Linux oder macOS

```console
export SPN_CLIENT_ID='<appId>'
export SPN_CLIENT_SECRET='<password>'
export SPN_TENANT_ID='<tenant>'
export SPN_AUTHORITY='https://login.microsoftonline.com'
```

#### <a name="save-environment-variables-in-powershell"></a>Speichern von Umgebungsvariablen in PowerShell

```console
$Env:SPN_CLIENT_ID="<appId>"
$Env:SPN_CLIENT_SECRET="<password>"
$Env:SPN_TENANT_ID="<tenant>"
$Env:SPN_AUTHORITY="https://login.microsoftonline.com"
```

Weisen Sie nach der Erstellung des Dienstprinzipals diesen der entsprechenden Rolle zu. 

### <a name="assign-roles-to-the-service-principal"></a>Zuweisen von Rollen zum Dienstprinzipal

Führen Sie den folgenden Befehl aus, um dem Dienstprinzipal die Rolle `Monitoring Metrics Publisher` für das Abonnement zuzuweisen, in dem sich Ihre Datenbankinstanzressourcen befinden:

#### <a name="run-the-command-on-windows"></a>Ausführen des Befehls unter Windows

> [!NOTE]
> Bei der Ausführung in einer Windows-Umgebung müssen Sie für Rollennamen doppelte Anführungszeichen verwenden.

```azurecli
az role assignment create --assignee <appId> --role "Monitoring Metrics Publisher" --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role "Contributor" --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-on-linux-or-macos"></a>Ausführen des Befehls unter Linux oder macOS

```azurecli
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

#### <a name="run-the-command-in-powershell"></a>Ausführen des Befehls in PowerShell

```powershell
az role assignment create --assignee <appId> --role 'Monitoring Metrics Publisher' --scope subscriptions/<Subscription ID>
az role assignment create --assignee <appId> --role 'Contributor' --scope subscriptions/<Subscription ID>
```

```output
{
  "canDelegate": null,
  "id": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleAssignments/f82b7dc6-17bd-4e78-93a1-3fb733b912d",
  "name": "f82b7dc6-17bd-4e78-93a1-3fb733b9d123",
  "principalId": "5901025f-0353-4e33-aeb1-d814dbc5d123",
  "principalType": "ServicePrincipal",
  "roleDefinitionId": "/subscriptions/<Subscription ID>/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c39005123",
  "scope": "/subscriptions/<Subscription ID>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

Nachdem der Dienstprinzipal der entsprechenden Rolle zugewiesen und die Umgebungsvariablen festgelegt wurden, können Sie mit dem Erstellen des Datencontrollers fortfahren. 

## <a name="create-the-azure-arc-data-controller"></a>Erstellen des Azure Arc-Datencontrollers

> [!NOTE]
> In den folgenden Beispielen können Sie verschiedene Werte für den Parameter `--namespace` des Befehls „azdata arc dc create“ verwenden. Achten Sie jedoch darauf, dass Sie diesen Namespacenamen für `--namespace parameter` in allen anderen unten aufgeführten Befehlen verwenden.

- [Erstellen im Azure Kubernetes Service (AKS)](#create-on-azure-kubernetes-service-aks)
- [Erstellen in der AKS-Engine in Azure Stack Hub](#create-on-aks-engine-on-azure-stack-hub)
- [Erstellen in AKS in Azure Stack HCI](#create-on-aks-on-azure-stack-hci)
- [Erstellen in Azure Red Hat OpenShift (ARO)](#create-on-azure-red-hat-openshift-aro)
- [Erstellen auf der Red Hat OpenShift Container Platform (OCP)](#create-on-red-hat-openshift-container-platform-ocp)
- [Erstellen in der Open-Source-Upstreamversion von Kubernetes (kubeadm)](#create-on-open-source-upstream-kubernetes-kubeadm)
- [Erstellen im AWS Elastic Kubernetes Service (EKS)](#create-on-aws-elastic-kubernetes-service-eks)
- [Erstellen im Google Cloud Kubernetes Engine Service (GKE)](#create-on-google-cloud-kubernetes-engine-service-gke)

### <a name="create-on-azure-kubernetes-service-aks"></a>Erstellen im Azure Kubernetes Service (AKS)

Das AKS-Bereitstellungsprofil nutzt standardmäßig die Speicherklasse `managed-premium`. Die Speicherklasse `managed-premium` funktioniert nur, wenn Sie über VMs verfügen, die mithilfe von VM-Images mit Premium-Datenträgern bereitgestellt wurden.

Wenn Sie `managed-premium` als Speicherklasse verwenden möchten, können Sie den folgenden Befehl ausführen, um den Datencontroller zu erstellen. Ersetzen Sie die Platzhalter im Befehl durch den Namen Ihrer Ressourcengruppe, die Abonnement-ID und den Azure-Standort.

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Wenn Sie nicht sicher sind, welche Speicherklasse Sie verwenden sollen, verwenden Sie die Speicherklasse `default`, die unabhängig vom verwendeten VM-Typ unterstützt wird. Eine Einschränkung besteht jedoch darin, dass sie nicht die höchste Leistung bietet.

Wenn Sie die Speicherklasse `default` verwenden möchten, können Sie den folgenden Befehl ausführen:

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-aks-engine-on-azure-stack-hub"></a>Erstellen in der AKS-Engine in Azure Stack Hub

Das Bereitstellungsprofil nutzt standardmäßig die Speicherklasse `managed-premium`. Die Speicherklasse `managed-premium` funktioniert nur, wenn Sie über Worker-VMs verfügen, die mithilfe von VM-Images mit Premium-Datenträgern in Azure Stack Hub bereitgestellt wurden.

Sie können den folgenden Befehl ausführen, um den Datencontroller unter Verwendung der Speicherklasse „managed-premium“ zu erstellen.

```console
azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Wenn Sie nicht sicher sind, welche Speicherklasse Sie verwenden sollen, verwenden Sie die Speicherklasse `default`, die unabhängig vom verwendeten VM-Typ unterstützt wird. In Azure Stack Hub werden Premium-Datenträger und Standard-Datenträger von derselben Speicherinfrastruktur in unterstützt. Daher wird erwartet, dass sie die gleiche allgemeine Leistung, jedoch mit unterschiedlichen IOPS-Grenzwerten bieten.

Wenn Sie die Speicherklasse `default` verwenden möchten, können Sie den folgenden Befehl ausführen.

```console
azdata arc dc create --profile-name azure-arc-aks-default-storage --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-premium-storage --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-aks-on-azure-stack-hci"></a>Erstellen in AKS in Azure Stack HCI

Das Bereitstellungsprofil nutzt standardmäßig die Speicherklasse mit dem Namen `default` und den Diensttyp `LoadBalancer`.

Sie können den folgenden Befehl ausführen, um den Datencontroller unter Verwendung der Speicherklasse `default` und des Diensttyps `LoadBalancer` zu erstellen.

```console
azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-aks-hci --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.


### <a name="create-on-azure-red-hat-openshift-aro"></a>Erstellen in Azure Red Hat OpenShift (ARO)

#### <a name="apply-the-scc"></a>Anwenden von Sicherheitskontexteinschränkungen

Bevor Sie den Datencontroller in Azure Red Hat OpenShift erstellen, müssen Sie bestimmte Sicherheitskontexteinschränkungen (Security Context Constraints, SCC) anwenden. Für das Vorschaurelease lockern diese die Sicherheitseinschränkungen. In zukünftigen Releases werden aktualisierte Sicherheitskontexteinschränkungen bereitgestellt.

1. Laden Sie die benutzerdefinierte Sicherheitskontexteinschränkung (Security Context Constraint, SCC) herunter. Verwenden Sie einen der folgenden Werte: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/master/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - ([Raw](https://raw.githubusercontent.com/microsoft/azure_arc/master/arc_data_services/deploy/yaml/arc-data-scc.yaml))
   - `curl` Mit dem folgenden Befehl wird „arc-data-scc.yaml“ heruntergeladen:

      ```console
      curl https://raw.githubusercontent.com/microsoft/azure_arc/master/arc_data_services/deploy/yaml/arc-data-scc.yaml -o arc-data-scc.yaml
      ```

1. Create SCC (SCC erstellen).

   ```console
   oc create -f arc-data-scc.yaml
   ```

1. Wenden Sie die Sicherheitskontexteinschränkung auf das Dienstkonto an.

   > [!NOTE]
   > Verwenden Sie hier und im Befehl `azdata arc dc create` unten denselben Namespace. Beispiel: `arc`.

   ```console
   oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
   ```


#### <a name="create-custom-deployment-profile"></a>Erstellen eines benutzerdefinierten Bereitstellungsprofils

Verwenden Sie das Profil `azure-arc-azure-openshift` für Azure RedHat Open Shift.

```console
azdata arc dc config init --source azure-arc-azure-openshift --path ./custom
```

#### <a name="create-data-controller"></a>Erstellen eines Datencontrollers

Führen Sie den folgenden Befehl zum Erstellen des Datencontrollers aus.

> [!NOTE]
> Verwenden Sie hier und in den Befehlen `oc adm policy add-scc-to-user` oben denselben Namespace. Beispiel: `arc`.

```console
azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example
#azdata arc dc create --profile-name azure-arc-azure-openshift --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-red-hat-openshift-container-platform-ocp"></a>Erstellen auf der Red Hat OpenShift Container Platform (OCP)

> [!NOTE]
> Bei Verwendung der Red Hat OpenShift Container Platform in Azure empfiehlt es sich, die neueste verfügbare Version zu verwenden.

#### <a name="apply-the-scc"></a>Anwenden von Sicherheitskontexteinschränkungen

Bevor Sie den Datencontroller in Red Hat OCP erstellen, müssen Sie bestimmte Sicherheitskontexteinschränkungen (Security Context Constraints, SCC) anwenden. Für das Vorschaurelease lockern diese die Sicherheitseinschränkungen. In zukünftigen Releases werden aktualisierte Sicherheitskontexteinschränkungen bereitgestellt.

1. Laden Sie die benutzerdefinierte Sicherheitskontexteinschränkung (Security Context Constraint, SCC) herunter. Verwenden Sie einen der folgenden Werte: 
   - [GitHub](https://github.com/microsoft/azure_arc/tree/master/arc_data_services/deploy/yaml/arc-data-scc.yaml) 
   - ([Raw](https://raw.githubusercontent.com/microsoft/azure_arc/master/arc_data_services/deploy/yaml/arc-data-scc.yaml))
   - `curl` Mit dem folgenden Befehl wird „arc-data-scc.yaml“ heruntergeladen:

      ```console
      curl https://raw.githubusercontent.com/microsoft/azure_arc/master/arc_data_services/deploy/yaml/arc-data-scc.yaml -o arc-data-scc.yaml
      ```

1. Create SCC (SCC erstellen).

   ```console
   oc create -f arc-data-scc.yaml
   ```

1. Wenden Sie die Sicherheitskontexteinschränkung auf das Dienstkonto an.

   > [!NOTE]
   > Verwenden Sie hier und im Befehl `azdata arc dc create` unten denselben Namespace. Beispiel: `arc`.

   ```console
   oc adm policy add-scc-to-user arc-data-scc --serviceaccount default --namespace arc
   ```

#### <a name="determine-storage-class"></a>Bestimmen der Speicherklasse

Sie müssen außerdem bestimmen, welche Speicherklasse verwendet werden soll, indem Sie den folgenden Befehl ausführen.

```console
kubectl get storageclass
```

#### <a name="create-custom-deployment-profile"></a>Erstellen eines benutzerdefinierten Bereitstellungsprofils

Erstellen Sie eine neue benutzerdefinierte Datei für das Bereitstellungsprofil auf Grundlage des Bereitstellungsprofils `azure-arc-openshift`, indem Sie den folgenden Befehl ausführen. Mit diesem Befehl werden das Verzeichnis `custom` im aktuellen Arbeitsverzeichnis und die benutzerdefinierte Bereitstellungsprofildatei `control.json` in diesem Verzeichnis erstellt.

Verwenden Sie das Profil `azure-arc-openshift` für die OpenShift Container Platform.

```console
azdata arc dc config init --source azure-arc-openshift --path ./custom
```

#### <a name="set-storage-class"></a>Festlegen der Speicherklasse 

Legen Sie nun die gewünschte Speicherklasse fest, indem Sie `<storageclassname>` im folgenden Befehl durch den Namen der gewünschten Speicherklasse ersetzen, die Sie mithilfe des oben angegebenen Befehls `kubectl get storageclass` ermittelt haben.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

#### <a name="set-loadbalancer-optional"></a>Festlegen von LoadBalancer (optional)

Das `azure-arc-openshift`-Bereitstellungsprofil verwendet standardmäßig `NodePort` als Diensttyp. Wenn Sie einen OpenShift-Cluster mit integriertem Lastenausgleich verwenden, können Sie die Konfiguration mithilfe des folgenden Befehls so ändern, dass der Diensttyp `LoadBalancer` verwendet wird:

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

#### <a name="verify-security-policies"></a>Überprüfen der Sicherheitsrichtlinien

Bei Verwendung von OpenShift kann es vorkommen, dass Sie die Umgebung mit den standardmäßigen Sicherheitsrichtlinien in OpenShift ausführen oder die Umgebung prinzipiell stärker als üblich einschränken möchten. Sie können optional einige Funktionen deaktivieren, um die zur Bereitstellungszeit und zur Laufzeit erforderlichen Berechtigungen zu minimieren, indem Sie die folgenden Befehle ausführen.

Dieser Befehl deaktiviert die Erfassung von Metriken zu Pods. Wenn Sie diese Funktion deaktivieren, werden in den Grafana-Dashboards keine Metriken zu Pods angezeigt. Der Standardwert ist "True".

```console
azdata arc dc config replace -p ./custom/control.json --json-values spec.security.allowPodMetricsCollection=false
```

Dieser Befehl deaktiviert die Erfassung von Metriken zu Knoten. Wenn Sie diese Funktion deaktivieren, werden in den Grafana-Dashboards keine Metriken zu Knoten angezeigt. Der Standardwert ist "True".

```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowNodeMetricsCollection=false
```

Mit diesem Befehl wird die Möglichkeit deaktiviert, Speicherabbilder zur Problembehandlung zu erstellen.
```console
azdata arc dc config replace --path ./custom/control.json --json-values spec.security.allowDumps=false
```

#### <a name="create-data-controller"></a>Erstellen eines Datencontrollers

Nun können Sie den Datencontroller mit dem folgenden Befehl erstellen.

> [!NOTE]
>   Verwenden Sie hier und in den Befehlen `oc adm policy add-scc-to-user` oben denselben Namespace. Beispiel: `arc`.

> [!NOTE]
>   Der Parameter `--path` muss auf das _Verzeichnis_ mit der Datei „control.json“ und nicht auf die Datei „control.json“ selbst verweisen.


```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-open-source-upstream-kubernetes-kubeadm"></a>Erstellen in der Open-Source-Upstreamversion von Kubernetes (kubeadm)

Das kubeadm-Bereitstellungsprofil nutzt standardmäßig eine Speicherklasse mit dem Namen `local-storage` und den Diensttyp `NodePort`. Wenn dies akzeptabel ist, können Sie die nachfolgenden Anweisungen überspringen, mit deren Hilfe die gewünschte Speicherklasse und der Diensttyp festgelegt werden, und den Befehl `azdata arc dc create` unten direkt ausführen.

Wenn Sie Ihr Bereitstellungsprofil anpassen möchten, um eine bestimmte Speicherklasse und/oder einen bestimmten Diensttyp anzugeben, erstellen Sie zunächst eine neue benutzerdefinierte Bereitstellungsprofildatei auf Grundlage des kubeadm-Bereitstellungsprofils, indem Sie den folgenden Befehl ausführen. Mit diesem Befehl werden das Verzeichnis `custom` im aktuellen Arbeitsverzeichnis und die benutzerdefinierte Bereitstellungsprofildatei `control.json` in diesem Verzeichnis erstellt.

```console
azdata arc dc config init --source azure-arc-kubeadm --path ./custom
```

Sie können die verfügbaren Speicherklassen mithilfe des folgenden Befehls ermitteln.

```console
kubectl get storageclass
```

Legen Sie nun die gewünschte Speicherklasse fest, indem Sie `<storageclassname>` im folgenden Befehl durch den Namen der gewünschten Speicherklasse ersetzen, die Sie mithilfe des oben angegebenen Befehls `kubectl get storageclass` ermittelt haben.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=<storageclassname>"
azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=<storageclassname>"

#Example:
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.data.className=mystorageclass"
#azdata arc dc config replace --path ./custom/control.json --json-values "spec.storage.logs.className=mystorageclass"
```

Das kubeadm-Bereitstellungsprofil nutzt standardmäßig `NodePort` als Diensttyp. Wenn Sie einen Kubernetes-Cluster mit integriertem Lastenausgleich verwenden, können Sie die Konfiguration mithilfe des folgenden Befehls ändern.

```console
azdata arc dc config replace --path ./custom/control.json --json-values "$.spec.services[*].serviceType=LoadBalancer"
```

Nun können Sie den Datencontroller mit dem folgenden Befehl erstellen.

```console
azdata arc dc create --path ./custom --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --path ./custom --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-aws-elastic-kubernetes-service-eks"></a>Erstellen im AWS Elastic Kubernetes Service (EKS)

Standardmäßig lauten die EKS-Speicherklasse `gp2` und der Diensttyp `LoadBalancer`.

Führen Sie den folgenden Befehl aus, um den Datencontroller mit dem bereitgestellten EKS-Bereitstellungsprofil zu erstellen.

```console
azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-eks --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

### <a name="create-on-google-cloud-kubernetes-engine-service-gke"></a>Erstellen im Google Cloud Kubernetes Engine Service (GKE)

Standardmäßig lauten die GKE-Speicherklasse `standard` und der Diensttyp `LoadBalancer`.

Führen Sie den folgenden Befehl aus, um den Datencontroller mit dem bereitgestellten GKE-Bereitstellungsprofil zu erstellen.

```console
azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription <subscription id> --resource-group <resource group name> --location <location> --connectivity-mode direct

#Example:
#azdata arc dc create --profile-name azure-arc-gke --namespace arc --name arc --subscription 1e5ff510-76cf-44cc-9820-82f2d9b51951 --resource-group my-resource-group --location eastus --connectivity-mode direct
```

Nachdem Sie den Befehl ausgeführt haben, fahren Sie mit [Überwachen des Erstellungsstatus](#monitoring-the-creation-status) fort.

## <a name="monitoring-the-creation-status"></a>Überwachen des Erstellungsstatus

Das Erstellen des Controllers dauert einige Minuten. Mithilfe der folgenden Befehle können Sie den Status in einem anderen Terminalfenster überwachen:

> [!NOTE]
>  Bei den folgenden Beispielbefehlen wird davon ausgegangen, dass Sie einen Datencontroller und Kubernetes-Namespace mit dem Namen `arc` erstellt haben. Wenn Sie einen anderen Namespace-/Datencontrollernamen verwendet haben, können Sie `arc` durch diesen Namen ersetzen.

```console
kubectl get datacontroller/arc --namespace arc
```

```console
kubectl get pods --namespace arc
```

Sie können auch den Erstellungsstatus eines bestimmten Pods überprüfen, indem Sie einen Befehl wie den folgenden ausführen. Dies ist besonders bei der Problembehandlung hilfreich.

```console
kubectl describe po/<pod name> --namespace arc

#Example:
#kubectl describe po/control-2g7bl --namespace arc
```

## <a name="troubleshooting-creation-problems"></a>Beheben von Problemen bei der Erstellung

Wenn Probleme bei der Erstellung auftreten, finden Sie weitere Informationen im [Handbuch zur Problembehandlung](troubleshoot-guide.md).
