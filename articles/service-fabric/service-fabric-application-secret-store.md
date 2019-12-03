---
title: Service Fabric-Geheimnisspeicher | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie den Service Fabric-Geheimnisspeicher verwenden.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: 5315a8806f45e40204e8500e97c3440bfa9ab8b2
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/14/2019
ms.locfileid: "74077336"
---
#  <a name="service-fabric-secrets-store"></a>Service Fabric-Geheimnisspeicher
In diesem Artikel wird beschrieben, wie Sie Geheimnisse in Service Fabric-Anwendungen mithilfe des Service Fabric-Geheimnissen Geheimnisspeichers (CSS) erstellen und verwenden. CSS ist ein lokaler Geheimnisspeichercache, der verwendet wird, um vertrauliche Daten wie Kennwörter, Token und Schlüssel im Arbeitsspeicher verschlüsselt zu halten.

## <a name="enabling-secrets-store"></a>Aktivieren des Geheimnisspeichers
 Fügen Sie das Folgende zu Ihrer Clusterkonfiguration unter `fabricSettings` hinzu, um CSS zu aktivieren. Es wird empfohlen, für CSS ein anderes Zertifikat als das Clusterzertifkat zu verwenden. Stellen Sie sicher, dass das Verschlüsselungszertifikat auf allen Knoten installiert ist und dass `NetworkService` über Leseberechtigung für den privaten Schlüssel des Zertifikats verfügt.
  ```json
    "fabricSettings": 
    [
        ...
    {
        "parameters":  [
            "name":  "CentralSecretService"
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "3"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                },
                 {
                    "name" : "EncryptionCertificateThumbprint",
                    "value": "<thumbprint>"
                 }
                ,
                ],
            },
            ]
```
## <a name="declare-secret-resource"></a>Deklarieren der Geheimnisressource
Sie können eine Geheimnisressource entweder mithilfe der Resource Manager-Vorlage oder mithilfe der REST-API erstellen.

* Verwenden der Resource Manager-Vorlage
```json
   "resources": [
      {
        "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
          }
        }
      ]
```
Die oben stehende Vorlage erstellt die Geheimnisressource `supersecret`, doch es ist noch kein Wert für die Geheimnisressource festgelegt.

* Verwenden der REST-API

Um die Geheimnisressource `supersecret` zu erstellen, nehmen Sie eine PUT-Anforderung an `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` vor. Sie benötigen das Clusterzertifikat oder das Administratorclientzertifikat, um ein Geheimnis zu erstellen.

```powershell
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint>
```

## <a name="set-secret-value"></a>Festlegen des Geheimniswerts
* Verwenden der Resource Manager-Vorlage

Die unten stehende Resource Manager-Vorlage erstellt den Wert für das Geheimnis `supersecret` mit der Version `ver1` und legt ihn fest.
```json
  {
  "parameters": {
  "supersecret": {
      "type": "string",
      "metadata": {
        "description": "supersecret value"
      }
   }
  },
  "resources": [
    {
      "apiVersion": "2018-07-01-preview",
        "name": "supersecret",
        "type": "Microsoft.ServiceFabricMesh/secrets",
        "location": "[parameters('location')]", 
        "dependsOn": [],
        "properties": {
          "kind": "inlinedValue",
            "description": "Application Secret",
            "contentType": "text/plain",
        }
    },
    {
      "apiVersion": "2018-07-01-preview",
      "name": "supersecret/ver1",
      "type": "Microsoft.ServiceFabricMesh/secrets/values",
      "location": "[parameters('location')]",
      "dependsOn": [
        "Microsoft.ServiceFabricMesh/secrets/supersecret"
      ],
      "properties": {
        "value": "[parameters('supersecret')]"
      }
    }
  ],
  ```
* Verwenden der REST-API

```powershell
$Params = @{"properties": {"value": "mysecretpassword"}}
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
## <a name="using-the-secret-in-your-application"></a>Verwenden des Geheimnisses in Ihrer Anwendung

1.  Fügen Sie in der Datei „settings.xml“ einen Abschnitt mit folgendem Inhalt hinzu. Beachten Sie, dass der Wert das Format `secretname:version` hat.

```xml
  <Section Name="testsecrets">
   <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
  </Section>
```
2. Importieren Sie jetzt den Abschnitt in „ApplicationManifest.xml“.
```xml
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ConfigPackagePolicies CodePackageRef="Code">
        <ConfigPackage Name="Config" SectionName="testsecrets" EnvironmentVariableName="SecretPath" />
        </ConfigPackagePolicies>
    </Policies>
  </ServiceManifestImport>
```

Die Umgebungsvariable „SecretPath“ verweist auf das Verzeichnis, in dem alle Geheimnisse gespeichert sind. Jeder unter dem Abschnitt `testsecrets` aufgeführte Parameter wird in einer separaten Datei gespeichert. Die Anwendung kann jetzt das Geheimnis wie unten gezeigt verwenden.
```C#
secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
```
3. Einbinden von Geheimnissen in einen Container

Die einzige Änderung, die Sie vornehmen müssen, um die Geheimnisse im Container verfügbar zu machen, besteht in der Angabe eines Bereitstellungspunkts in `<ConfigPackage>`.
Hier sehen Sie die geänderte Datei „ApplicationManifest.xml“.  

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="testservicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ConfigPackagePolicies CodePackageRef="Code">
        <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="C:\secrets" EnvironmentVariableName="SecretPath" />
        <!-- Linux Container
         <ConfigPackage Name="Config" SectionName="testsecrets" MountPoint="/mnt/secrets" EnvironmentVariableName="SecretPath" />
        -->
      </ConfigPackagePolicies>
    </Policies>
  </ServiceManifestImport>
```
Geheimnisse stehen unter dem Bereitstellungspunkt in Ihrem Container zur Verfügung.

4. Binden des Geheimnisses an eine Umgebungsvariable 

Sie können das Geheimnis an eine Prozessumgebungsvariable binden, indem Sie „Type='SecretsStoreRef'“ angeben. Im Folgenden finden Sie ein Beispiel dafür, wie Sie `supersecret`, Version `ver1`, an die Umgebungsvariable `MySuperSecret` in „ServiceManifest.xml“ binden.

```xml
<EnvironmentVariables>
  <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
</EnvironmentVariables>
```
## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Anwendungs- und Dienstsicherheit](service-fabric-application-and-service-security.md).