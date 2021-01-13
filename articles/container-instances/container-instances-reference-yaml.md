---
title: YAML-Referenz für Containergruppe
description: Referenz für die von Azure Container Instances zum Konfigurieren einer Containergruppe unterstützte YAML-Datei
ms.topic: article
ms.date: 07/06/2020
ms.openlocfilehash: d0ec8d13eebba1c60f5a52f8c43bdd8b90eeb913
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87084759"
---
# <a name="yaml-reference-azure-container-instances"></a>YAML-Referenz: Azure Container Instances

Dieser Artikel behandelt die Syntax und Eigenschaften für die von Azure Container Instances zum Konfigurieren einer [Containergruppe](container-instances-container-groups.md) unterstützte YAML-Datei. Verwenden Sie eine YAML-Datei, um die Gruppenkonfiguration in den Befehl [az container create][az-container-create] in der Azure CLI einzugeben. 

Eine YAML-Datei ist eine bequeme Möglichkeit, um eine Containergruppe für reproduzierbare Bereitstellungen zu konfigurieren. Dies ist eine präzise Alternative zur Verwendung einer [Resource Manager-Vorlage](/azure/templates/Microsoft.ContainerInstance/2019-12-01/containerGroups) oder des Azure Container Instances SDKs zum Erstellen oder Aktualisieren einer Containergruppe.

> [!NOTE]
> Diese Referenz gilt für YAML-Dateien für Azure Container Instances-REST-API-Version `2019-12-01`.

## <a name="schema"></a>Schema 

Im Folgenden finden Sie das Schema für die YAML-Datei, einschließlich Kommentaren zur Hervorhebung von wichtigen Eigenschaften. Eine Beschreibung der Eigenschaften in diesem Schema finden Sie im Abschnitt [Eigenschaftswerte](#property-values).

```yml
name: string  # Name of the container group
apiVersion: '2019-12-01'
location: string
tags: {}
identity: 
  type: string
  userAssignedIdentities: {}
properties: # Properties of container group
  containers: # Array of container instances in the group
  - name: string # Name of an instance
    properties: # Properties of an instance
      image: string # Container image used to create the instance
      command:
      - string
      ports: # External-facing ports exposed on the instance, must also be set in group ipAddress property 
      - protocol: string
        port: integer
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      resources: # Resource requirements of the instance
        requests:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
        limits:
          memoryInGB: number
          cpu: number
          gpu:
            count: integer
            sku: string
      volumeMounts: # Array of volume mounts for the instance
      - name: string
        mountPath: string
        readOnly: boolean
      livenessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
      readinessProbe:
        exec:
          command:
          - string
        httpGet:
          path: string
          port: integer
          scheme: string
        initialDelaySeconds: integer
        periodSeconds: integer
        failureThreshold: integer
        successThreshold: integer
        timeoutSeconds: integer
  imageRegistryCredentials: # Credentials to pull a private image
  - server: string
    username: string
    password: string
  restartPolicy: string
  ipAddress: # IP address configuration of container group
    ports:
    - protocol: string
      port: integer
    type: string
    ip: string
    dnsNameLabel: string
  osType: string
  volumes: # Array of volumes available to the instances
  - name: string
    azureFile:
      shareName: string
      readOnly: boolean
      storageAccountName: string
      storageAccountKey: string
    emptyDir: {}
    secret: {}
    gitRepo:
      directory: string
      repository: string
      revision: string
  diagnostics:
    logAnalytics:
      workspaceId: string
      workspaceKey: string
      logType: string
      metadata: {}
  networkProfile: # Virtual network profile for container group
    id: string
  dnsConfig: # DNS configuration for container group
    nameServers:
    - string
    searchDomains: string
    options: string
  sku: string # SKU for the container group
  encryptionProperties:
    vaultBaseUrl: string
    keyName: string
    keyVersion: string
  initContainers: # Array of init containers in the group
  - name: string
    properties:
      image: string
      command:
      - string
      environmentVariables:
      - name: string
        value: string
        secureValue: string
      volumeMounts:
      - name: string
        mountPath: string
        readOnly: boolean
```

## <a name="property-values"></a>Eigenschaftswerte

In den folgenden Tabellen sind die Werte beschrieben, die Sie im Schema festlegen müssen.



### <a name="microsoftcontainerinstancecontainergroups-object"></a>Microsoft.ContainerInstance/containerGroups-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Containergruppe. |
|  apiVersion | enum | Ja | 2018-10-01 |
|  location | Zeichenfolge | Nein | Der Ressourcenspeicherort. |
|  tags | Objekt (object) | Nein | Die Ressourcentags. |
|  identity | Objekt (object) | Nein | Die Identität der Containergruppe, sofern konfiguriert. - [-Objekt](#containergroupidentity-object) |
|  properties | Objekt (object) | Ja | [ContainerGroupProperties-Objekt](#containergroupproperties-object) |




### <a name="containergroupidentity-object"></a>ContainerGroupIdentity-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  type | enum | Nein | Der für die Containergruppe verwendete Identitätstyp. Der Typ „SystemAssigned, UserAssigned“ umfasst sowohl eine implizit erstellte Identität als auch einen Satz von Benutzern zugewiesener Identitäten. Der Typ „None“ entfernt alle Identitäten aus der Containergruppe. – SystemAssigned, UserAssigned, SystemAssigned, UserAssigned, None |
|  userAssignedIdentities | Objekt (object) | Nein | Die Liste der der Containergruppe zugeordneten Benutzeridentitäten. Die Schlüsselverweise des Benutzeridentitäts-Wörterbuchs sind Azure Resource Manager-Ressourcen-IDs im Format: '/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{identityName}'. |




### <a name="containergroupproperties-object"></a>ContainerGroupProperties-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  containers | array | Ja | Die Container innerhalb der Containergruppe. - [Container-Objekt](#container-object) |
|  imageRegistryCredentials | array | Nein | Die Anmeldeinformationen für die Imageregistrierung, mit denen die Containergruppe erstellt wird. - [ImageRegistryCredential-Objekt](#imageregistrycredential-object) |
|  restartPolicy | enum | Nein | Neustartrichtlinie für alle Container innerhalb der Containergruppe. - `Always` Immer neu starten- `OnFailure` Neustart bei Fehler- `Never` Nie neu starten. – Always, OnFailure, Never |
|  ipAddress | Objekt (object) | Nein | Der IP-Adressentyp der Containergruppe. - [IpAddress-Objekt](#ipaddress-object) |
|  osType | enum | Ja | Der für die Container in der Containergruppe erforderliche Betriebssystemtyp. – Windows oder Linux |
|  volumes | array | Nein | Die Liste der Volumes, die von Containern in dieser Containergruppe eingebunden werden können. - [Volume-Objekt](#volume-object) |
|  Diagnose | Objekt (object) | Nein | Die Diagnoseinformationen für eine Containergruppe. - [ContainerGroupDiagnostics-Objekt](#containergroupdiagnostics-object) |
|  networkProfile | Objekt (object) | Nein | Die Netzwerkprofilinformationen für eine Containergruppe. - [ContainerGroupNetworkProfile-Objekt](#containergroupnetworkprofile-object) |
|  dnsConfig | Objekt (object) | Nein | Die DNS-Konfigurationsinformationen für eine Containergruppe. - [DnsConfiguration-Objekt](#dnsconfiguration-object) |
| sku | enum | Nein | SKU für eine Containergruppe: Standard oder Dedicated |
| encryptionProperties | Objekt (object) | Nein | Die Verschlüsselungseigenschaften für eine Containergruppe. - [EncryptionProperties-Objekt](#encryptionproperties-object) | 
| initContainers | array | Nein | Die Init-Container für eine Containergruppe - [InitContainerDefinition-Objekt](#initcontainerdefinition-object) |




### <a name="container-object"></a>Container-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der vom Benutzer bereitgestellte Name der Containerinstanz. |
|  properties | Objekt (object) | Ja | Der Eigenschaften der Containerinstanz. - [ContainerProperties-Objekt](#containerproperties-object) |




### <a name="imageregistrycredential-object"></a>ImageRegistryCredential-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  server | Zeichenfolge | Ja | Der Docker-Imageregistrierungsserver ohne ein Protokoll wie „HTTP“ oder „HTTPS“. |
|  username | Zeichenfolge | Ja | Der Benutzername für die private Registrierung. |
|  password | Zeichenfolge | No | Das Kennwort für die private Registrierung. |




### <a name="ipaddress-object"></a>IpAddress-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  ports | array | Ja | Die Liste der in der Containergruppe verfügbar gemachten Ports. - [Port-Objekte](#port-object) |
|  type | enum | Ja | Gibt an, ob die IP dem öffentlichen Internet oder dem privaten VNET verfügbar gemacht ist. – Public oder Private |
|  ip | Zeichenfolge | No | Die dem öffentlichen Internet verfügbar gemachte IP-Adresse. |
|  dnsNameLabel | Zeichenfolge | No | Die DNS-Namensbezeichnung für die IP-Adresse. |




### <a name="volume-object"></a>Volume-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name des Volumes. |
|  azureFile | Objekt (object) | Nein | Das Azure File-Volume. - [AzureFileVolume-Objekt](#azurefilevolume-object) |
|  emptyDir | Objekt (object) | Nein | Das leere Verzeichnis-Volume. |
|  secret | Objekt (object) | Nein | Das geheime Volume. |
|  gitRepo | Objekt (object) | Nein | Das Git-Repository-Volume. - [GitRepoVolume-Objekt](#gitrepovolume-object) |




### <a name="containergroupdiagnostics-object"></a>ContainerGroupDiagnostics-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  logAnalytics | Objekt (object) | Nein | Containergruppen-Protokollanalyseinformationen. - [LogAnalytics-Objekt](#loganalytics-object) |




### <a name="containergroupnetworkprofile-object"></a>ContainerGroupNetworkProfile-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  id | Zeichenfolge | Ja | Der Bezeichner für ein Netzwerkprofil. |




### <a name="dnsconfiguration-object"></a>DnsConfiguration-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  nameServers | array | Ja | Die DNS-Server für die Containergruppe. – string |
|  searchDomains | Zeichenfolge | No | Die DNS-Suchdomänen für das Nachschlagen von Hostnamen in der Containergruppe. |
|  Optionen | Zeichenfolge | No | Die DNS-Optionen für die Containergruppe. |


### <a name="encryptionproperties-object"></a>EncryptionProperties-Objekt

| Name  | Typ  | Erforderlich  | Wert |
|  ---- | ---- | ---- | ---- |
| vaultBaseUrl  | Zeichenfolge    | Ja   | Die Basis-URL für den Schlüsseltresor. |
| keyName   | Zeichenfolge    | Ja   | Der Name des Verschlüsselungsschlüssels |
| keyVersion    | Zeichenfolge    | Ja   | Die Version des Verschlüsselungsschlüssels |

### <a name="initcontainerdefinition-object"></a>InitContainerDefinition-Objekt

| Name  | Typ  | Erforderlich  | Wert |
|  ---- | ---- | ---- | ---- |
| name  | Zeichenfolge |  Ja | Der Name für den Init-Container |
| properties    | Objekt (object)    | Ja   | Die Eigenschaften für den Init-Container. - [InitContainerPropertiesDefinition-Objekt](#initcontainerpropertiesdefinition-object)


### <a name="containerproperties-object"></a>ContainerProperties-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  image | Zeichenfolge | Ja | Der Name des zum Erstellen der Containerinstanz verwendeten Images. |
|  command | array | Nein | Die innerhalb der Containerinstanz auszuführenden Befehle im EXEC-Format. – string |
|  ports | array | Nein | Die in der Containerinstanz verfügbar gemachten Ports. - [ContainerPort-Objekt](#containerport-object) |
|  environmentVariables | array | Nein | Die in der Containerinstanz festzulegenden Umgebungsvariablen. - [EnvironmentVariable-Objekt](#environmentvariable-object) |
|  ressourcen | Objekt (object) | Ja | Die Ressourcenanforderungen der Containerinstanz. - [ResourceRequirements-Objekt](#resourcerequirements-object) |
|  volumeMounts | array | Nein | Die für die Containerinstanz verfügbaren Volumebereitstellungen. - [VolumeMount-Objekt](#volumemount-object) |
|  livenessProbe | Objekt (object) | Nein | Der Livetest. - [ContainerProbe-Objekt](#containerprobe-object) |
|  readinessProbe | Objekt (object) | Nein | Der Bereitschaftstest. - [ContainerProbe-Objekt](#containerprobe-object) |




### <a name="port-object"></a>Port-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  Protokoll | enum | Nein | Das dem Port zugeordnete Protokoll. – TCP oder UDP |
|  port | integer | Ja | Die Portnummer. |




### <a name="azurefilevolume-object"></a>AzureFileVolume-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  shareName | Zeichenfolge | Ja | Der Name der als Volume bereitzustellenden Azure File-Freigabe. |
|  readOnly | boolean | Nein | Das Flag, das anzeigt, ob die als Volume bereitgestellte Azure File-Freigabe schreibgeschützt ist. |
|  storageAccountName | Zeichenfolge | Ja | Der Name des Speicherkontos, das die Azure File-Freigabe enthält. |
|  storageAccountKey | Zeichenfolge | No | Der Zugriffsschlüssel des Speicherkontos, der für den Zugriff auf die Azure File-Freigabe verwendet wird. |




### <a name="gitrepovolume-object"></a>GitRepoVolume-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  directory | Zeichenfolge | No | Zielverzeichnisname. Darf „..“ weder enthalten noch damit beginnen.  Wenn „.“ angegeben wird, ist das Volumeverzeichnis das Git-Repository.  Andernfalls, falls angegeben, enthält das Volume das Git-Repository in einem Unterverzeichnis mit dem angegebenen Namen. |
|  repository | Zeichenfolge | Ja | Repository-URL |
|  revision | Zeichenfolge | Nein | Commit-Hash für die angegebene Revision. |



### <a name="loganalytics-object"></a>LogAnalytics-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  workspaceId | Zeichenfolge | Ja | Die Arbeitsbereichs-ID für Log Analytics. |
|  workspaceKey | Zeichenfolge | Ja | Der Arbeitsbereichsschlüssel für Log Analytics. |
|  logType | enum | Nein | Der zu verwendende Protokolltyp. – ContainerInsights oder ContainerInstanceLogs |
|  metadata | Objekt (object) | Nein | Metadaten für Log Analytics. |


### <a name="initcontainerpropertiesdefinition-object"></a>InitContainerPropertiesDefinition-Objekt

| Name  | Typ  | Erforderlich  | Wert |
|  ---- | ---- | ---- | ---- |
| image | Zeichenfolge    | No    | Das Image des Init-Containers. |
| command   | array | Nein    | Der Befehl, der im Init-Container im Ausführungsformat ausgeführt werden soll. – string |
| environmentVariables | array  | Nein |Die Umgebungsvariablen, die im Init-Container festgelegt werden sollen. - [EnvironmentVariable-Objekt](#environmentvariable-object)
| volumeMounts |array   | Nein    | Die für den Init-Container verfügbaren Volumebereitstellungen. - [VolumeMount-Objekt](#volumemount-object)

### <a name="containerport-object"></a>ContainerPort-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  Protokoll | enum | Nein | Das dem Port zugeordnete Protokoll. – TCP oder UDP |
|  port | integer | Ja | Die innerhalb der Containergruppe verfügbar gemachte Portnummer. |




### <a name="environmentvariable-object"></a>EnvironmentVariable-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Umgebungsvariablen. |
|  value | Zeichenfolge | No | Der Wert der Umgebungsvariablen. |
|  secureValue | Zeichenfolge | No | Der Wert der sicheren Umgebungsvariablen. |




### <a name="resourcerequirements-object"></a>ResourceRequirements-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  requests | Objekt (object) | Ja | Die Ressourcenanforderungen dieser Containerinstanz. - [ResourceRequests-Objekt](#resourcerequests-object) |
|  Grenzwerte | Objekt (object) | Nein | Die Ressourcenlimits dieser Containerinstanz. - [ResourceLimits-Objekt](#resourcelimits-object) |




### <a name="volumemount-object"></a>VolumeMount-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  name | Zeichenfolge | Ja | Der Name der Volumebereitstellung. |
|  mountPath | Zeichenfolge | Ja | Der Pfad innerhalb des Containers, in dem das Volume bereitgestellt werden soll. Darf keinen Doppelpunkt (:) enthalten. |
|  readOnly | boolean | Nein | Das Flag, das anzeigt, ob die Volumebereitstellung schreibgeschützt ist. |




### <a name="containerprobe-object"></a>ContainerProbe-Objekt

|  Name | type | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  exec | Objekt (object) | Nein | Der zu testende Ausführungsbefehl – [ContainerExec-Objekt](#containerexec-object) |
|  httpGet | Objekt (object) | Nein | Die zu testenden HTTP GET-Einstellungen – [ContainerHttpGet-Objekt](#containerhttpget-object) |
|  initialDelaySeconds | integer | Nein | Die anfänglichen Verzögerungssekunden. |
|  periodSeconds | integer | Nein | Die Zeitraumsekunden. |
|  failureThreshold | integer | Nein | Der Fehlerschwellenwert. |
|  successThreshold | integer | Nein | Der Schwellenwert für erfolgreiche Tests. |
|  timeoutSeconds | integer | Nein | Die Timeoutsekunden. |




### <a name="resourcerequests-object"></a>ResourceRequests-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Ja | Die Arbeitsspeicheranforderung dieser Containerinstanz in GB. |
|  cpu | number | Ja | Die CPU-Anforderungen dieser Containerinstanz. |
|  gpu | Objekt (object) | Nein | Die GPU-Anforderungen dieser Containerinstanz. - [GpuResource-Objekt](#gpuresource-object) |




### <a name="resourcelimits-object"></a>ResourceLimits-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  memoryInGB | number | Nein | Das Arbeitsspeicherlimit dieser Containerinstanz in GB. |
|  cpu | number | Nein | Das CPU-Limit dieser Containerinstanz. |
|  gpu | Objekt (object) | Nein | Das GPU-Limit dieser Containerinstanz. - [GpuResource-Objekt](#gpuresource-object) |




### <a name="containerexec-object"></a>ContainerExec-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  command | array | Nein | Die innerhalb des Containers auszuführenden Befehle. – string |




### <a name="containerhttpget-object"></a>ContainerHttpGet-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  path | Zeichenfolge | Nein | Der zu testende Pfad. |
|  port | integer | Ja | Die zu testende Portnummer. |
|  scheme | enum | Nein | Das Schema. – HTTP oder HTTPS |




### <a name="gpuresource-object"></a>GpuResource-Objekt

|  Name | Typ | Erforderlich | Wert |
|  ---- | ---- | ---- | ---- |
|  count | integer | Ja | Die Anzahl der GPU-Ressourcen. |
|  sku | enum | Ja | Die SKU der GPU-Ressource. – K80, P100, V100 |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie im Tutorial [Bereitstellen einer Gruppe mit mehreren Containern mithilfe einer YAML-Datei](container-instances-multi-container-yaml.md).

Beispiele für die Verwendung einer YAML-Datei zum Bereitstellen von Containergruppen in einem [virtuellen Netzwerk](container-instances-vnet.md) oder für das [Einbinden eines externen Volumes](container-instances-volume-azure-files.md).

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create

