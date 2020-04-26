---
title: Azure Service Fabric Central Secrets Store
description: In diesem Artikel wird beschrieben, wie Sie den Central Secrets Store in Azure Service Fabric verwenden.
ms.topic: conceptual
ms.date: 07/25/2019
ms.openlocfilehash: 4087e7ccdcb2281c4a08af155d35a10c66147a85
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770416"
---
# <a name="central-secrets-store-in-azure-service-fabric"></a>Central Secrets Store in Azure Service Fabric 
In diesem Artikel wird beschrieben, wie Sie den Central Secrets Store (CSS) in Azure Service Fabric zum Erstellen von Geheimnissen in Service Fabric-Anwendungen verwenden. CSS ist ein lokaler Geheimnisspeichercache, der verwendet wird, um vertrauliche Daten wie Kennwörter, Token und Schlüssel im Arbeitsspeicher verschlüsselt zu speichern.

## <a name="enable-central-secrets-store"></a>Aktivieren von Central Secrets Store
Fügen Sie Ihrer Clusterkonfiguration das folgende Skript unter `fabricSettings` hinzu, um CSS zu aktivieren. Es wird empfohlen, ein anderes Zertifikat als ein Clusterzertifikat für CSS zu verwenden. Stellen Sie sicher, dass das Verschlüsselungszertifikat auf allen Knoten installiert ist und dass `NetworkService` über Leseberechtigung für den privaten Schlüssel des Zertifikats verfügt.
  ```json
    "fabricSettings": 
    [
        ...
    {
        "name":  "CentralSecretService",
        "parameters":  [
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
     }
        ...
     ]
```
## <a name="declare-a-secret-resource"></a>Deklarieren der Geheimnisressource
Sie können eine Geheimnisressource erstellen, indem Sie die REST-API verwenden.
  > [!NOTE] 
  > Wenn der Cluster Windows-Authentifizierung verwendet, wird die REST-Anforderung über einen ungesicherten HTTP-Kanal gesendet. Es wird empfohlen, einen X509-basierten Cluster mit sicheren Endpunkten zu verwenden.

Um eine `supersecret`-Geheimnisressource mit der Rest-API zu erstellen, nehmen Sie eine PUT-Anforderung an `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` vor. Sie benötigen das Clusterzertifikat oder das Administratorclientzertifikat, um eine Geheimnisressource zu erstellen.

```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

## <a name="set-the-secret-value"></a>Festlegen des Geheimniswerts

Verwenden Sie das folgende Skript, um die REST-API zum Festlegen des Geheimniswerts zu verwenden.
```powershell
$Params = '{"properties": {"value": "mysecretpassword"}}'
Invoke-WebRequest -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret/values/ver1?api-version=6.4-preview -Method PUT -Body $Params -CertificateThumbprint <ClusterCertThumbprint>
```
### <a name="examine-the-secret-value"></a>Untersuchen des Geheimniswerts
```powershell
Invoke-WebRequest -CertificateThumbprint <ClusterCertThumbprint> -Method POST -Uri "https:<clusterfqdn>/Resources/Secrets/supersecret/values/ver1/list_value?api-version=6.4-preview"
```
## <a name="use-the-secret-in-your-application"></a>Verwenden des Geheimnisses in Ihrer Anwendung

Führen Sie die folgenden Schritte aus, um das Geheimnis in Ihrer Service Fabric Anwendung zu verwenden.

1. Fügen Sie in der Datei **settings.xml** einen Abschnitt mit dem folgenden Codeausschnitt hinzu. Beachten Sie dabei, dass der Wert das Format {`secretname:version`} aufweist.

   ```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
   ```

1. Importieren Sie den Abschnitt in **ApplicationManifest.xml**.
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

   Die Umgebungsvariable `SecretPath` verweist auf das Verzeichnis, in dem alle Geheimnisse gespeichert sind. Jeder unter dem Abschnitt `testsecrets` aufgeführte Parameter wird in einer separaten Datei gespeichert. Die Anwendung kann das Geheimnis jetzt wie folgt verwenden:
   ```C#
   secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
   ```
1. Binden Sie die Geheimnisse in einen Container ein. Die einzige Änderung, die Sie vornehmen müssen, um die Geheimnisse im Container verfügbar zu machen, besteht im `specify` eines Bereitstellungspunkts in `<ConfigPackage>`.
Der folgende Codeausschnitt ist die geänderte Datei **ApplicationManifest.xml**.  

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

1. Sie können das Geheimnis an eine Prozessumgebungsvariable binden, indem Sie `Type='SecretsStoreRef` angeben. Der folgende Codeausschnitt ist ein Beispiel dafür, wie Sie `supersecret` (Version `ver1`) an die Umgebungsvariable `MySuperSecret` in **ServiceManifest.xml** binden.

   ```xml
   <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
   </EnvironmentVariables>
   ```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Anwendungs- und Dienstsicherheit](service-fabric-application-and-service-security.md).
