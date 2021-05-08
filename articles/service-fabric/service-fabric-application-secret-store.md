---
title: Azure Service Fabric – Central Secret Service
description: In diesem Artikel wird beschrieben, wie Sie den Central Secret Service in Azure Service Fabric verwenden.
author: amenarde
ms.topic: conceptual
ms.date: 04/06/2021
ms.author: anmenard
ms.openlocfilehash: 735f0b375d26d00dce43fd9d64131fa194958a73
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108131077"
---
# <a name="central-secret-service-in-azure-service-fabric"></a>Central Secret Service in Azure Service Fabric 
Central Secret Service (CSS), auch als Central Secret Store bezeichnet, ist ein Service Fabric-Systemdienst zum Schutz von Geheimnissen in einem Cluster. CSS vereinfacht die Verwaltung von Geheimnissen für Service Fabric-Anwendungen, sodass keine verschlüsselten Parameter erforderlich sind.

Central Secret Service ist ein dauerhafter, replizierter Geheimniscache im Cluster. In CSS gespeicherte Geheimnisse werden im Ruhezustand mit einem vom Kunden bereitgestellten Verschlüsselungszertifikat verschlüsselt. CSS stellt Client-APIs für die Geheimnisverwaltung bereit. Diese sind für Entitäten zugänglich, die sich als Cluster oder Clusteradministratorbenutzer authentifizieren. Das Anwendungsmodell der Service Fabric-Runtime ist in CSS integriert und ermöglicht die Deklaration von Anwendungsparametern als CSS-Geheimnisverweise. 

CSS ist darüber hinaus sehr hilfreich beim Bereitstellen von als [KeyVault-Geheimnis-URIs](service-fabric-keyvault-references.md) deklarierten Anwendungsgeheimnissen in Kombination mit einer [verwalteten Identität für in Azure bereitgestellte Service Fabric-Anwendungen](concepts-managed-identity.md).

Central Secret Service ist nicht als Ersatz für einen dedizierten, externen Geheimnisverwaltungsdienst wie z. B. Azure Key Vault vorgesehen. 

  > [!NOTE] 
  > Beim Aktivieren von CSS in einem Service Fabric-Cluster, in dem eine Version vor [7.1. CU3](service-fabric-versions.md#service-fabric-version-name-and-number-reference) ausgeführt wird, kommt es bei der Aktivierung möglicherweise zu einem Fehler, und CSS wechselt in einen dauerhaften Fehlerzustand, wenn der Cluster für die Windows-Authentifizierung konfiguriert, `EncryptionCertificateThumbprint` falsch deklariert oder das entsprechende Zertifikat nicht installiert ist. Sie sollten den Cluster auf eine höhere Service Fabric-Runtimeversion aktualisieren als Version 7.1. CU3, bevor Sie fortfahren. 
  
## <a name="enable-central-secrets-service"></a>Aktivieren von Central Secret Service
Aktualisieren Sie die Clusterkonfiguration wie nachfolgend beschrieben, um Central Secret Service zu aktivieren. Es wird empfohlen, ein Verschlüsselungszertifikat zu verwenden, das sich von Ihrem Clusterzertifikat unterscheidet. Dieses Zertifikat muss auf allen Knoten installiert werden.
```json
{ 
    "fabricSettings": [
        {
            "name":  "CentralSecretService",
            "parameters":  [
                {
                    "name":  "DeployedState",
                    "value":  "enabled"
                },
                {
                    "name" : "EncryptionCertificateThumbprint",
                    "value": "<thumbprint>"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "1"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                }
            ]
        }
    ]
}
```
  > [!NOTE] 
  > Die Konfigurationseinstellung „DeployedState“, die in [Version 8.0](service-fabric-versions.md#service-fabric-version-name-and-number-reference) von Service Fabric eingeführt wurde, ist der bevorzugte Mechanismus zum Aktivieren oder Deaktivieren von CSS. Diese Funktion wurde in früheren Versionen über die Konfigurationseinstellung „IsEnabled“ gesteuert, die jetzt als veraltet eingestuft ist.

## <a name="central-secret-service-secret-model"></a>Central Secret Service-Geheimnismodell
Die Central Secret Service-API macht zwei Typen verfügbar: Geheimnisressource und Geheimnisversion. Der Geheimnisressourcentyp repräsentiert konzeptionell eine Familie von Versionen eines einzelnen Geheimnisses, das für einen bestimmten Zweck verwendet wird. Beispiele hierfür sind etwa eine Verbindungszeichenfolge, ein Kennwort und ein Endpunktzertifikat. Ein Objekt mit dem Geheimnisressourcentyp enthält Metadaten, die diesem Geheimnis zugeordnet sind, insbesondere Art, Inhaltstyp und Beschreibung. Der Geheimnisversionstyp repräsentiert eine bestimmte Instanz des zugeordneten Geheimnisses und speichert den Geheimnisklartext (verschlüsselt). In den obigen Beispielen enthält eine Geheimnisversion den aktuellen Kennwortwert, ein Zertifikatobjekt, das bis zum Ende des Monats gültig ist usw. Nach der Erneuerung dieser Geheimnisse müssen neue Geheimnisversionen erstellt (und CSS hinzugefügt) werden.

Zur Formalisierung des Modells sind nachfolgend die Regeln aufgeführt, die in der CSS-Implementierung umgesetzt und erzwungen werden:

- Eine Geheimnisressource kann null, eine oder mehrere Versionen aufweisen.
- Jede Geheimnisversion ist einer bestimmten Geheimnisressource untergeordnet. Eine Version darf nur eine übergeordnete Ressource umfassen.
- Eine einzelne Geheimnisversion kann gelöscht werden, ohne dass sich dies auf andere Versionen desselben Geheimnisses auswirkt.
- Das Löschen einer Geheimnisressource führt zur Löschung aller zugehörigen Versionen.  
- Der Wert einer Geheimnisversion ist unveränderlich.
    
Den vollständigen Satz an REST-Verwaltungs-APIs für Geheimnisressourcen finden Sie [hier](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-meshsecrets), den für Geheimnisversionen [hier](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-meshsecretvalues).

### <a name="declare-a-secret-resource"></a>Deklarieren der Geheimnisressource
Sie können eine Geheimnisressource erstellen, indem Sie die REST-API verwenden.

  > [!NOTE] 
  > Wenn der Cluster die Windows-Authentifizierung ohne HttpGateway-Zertifikat verwendet, wird die REST-Anforderung über einen ungesicherten HTTP-Kanal gesendet. Um TLS für diesen Kanal zu aktivieren, sollte die Clusterdefinition zur Angabe eines HTTP-Gatewayserverzertifikats aktualisiert werden.

Um eine `supersecret`-Geheimnisressource mit der Rest-API zu erstellen, nehmen Sie eine PUT-Anforderung an `https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview` vor. Sie müssen sich mit einem Cluster- oder Administratorclientzertifikat authentifizieren, um eine Geheimnisressource zu erstellen.
```powershell
$json = '{"properties": {"kind": "inlinedValue", "contentType": "text/plain", "description": "supersecret"}}'
Invoke-WebRequest  -Uri https://<clusterfqdn>:19080/Resources/Secrets/supersecret?api-version=6.4-preview -Method PUT -CertificateThumbprint <CertThumbprint> -Body $json
```

### <a name="set-the-secret-value"></a>Festlegen des Geheimniswerts
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
Eine Anwendung kann ein Geheimnis aus CSS nutzen, indem sie es als Umgebungsvariable deklariert oder einen Pfad angibt, in dem der Geheimnisklartext serialisiert werden soll. Führen Sie diese Schritte aus, um auf ein CSS-Geheimnis zu verweisen:

1. Fügen Sie in der Datei **settings.xml** einen Abschnitt mit dem folgenden Codeausschnitt hinzu. Beachten Sie dabei, dass der Wert das Format {`secretname:version`} aufweist.
```xml
     <Section Name="testsecrets">
      <Parameter Name="TopSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/
     </Section>
```

2. Importieren Sie den Abschnitt in **ApplicationManifest.xml**.
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
   
Beispiel 1: Bereitstellen der Geheimnisse in einem Container Die einzige Änderung, die Sie vornehmen müssen, um die Geheimnisse im Container verfügbar zu machen, besteht im `specify` eines Bereitstellungspunkts in `<ConfigPackage>`.
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

Beispiel 2: Binden eines Geheimnisses an eine Prozessumgebungsvariable durch das Festlegen von `Type='SecretsStoreRef` Der folgende Codeausschnitt ist ein Beispiel dafür, wie Sie `supersecret` (Version `ver1`) an die Umgebungsvariable `MySuperSecret` in **ServiceManifest.xml** binden.

```xml
   <EnvironmentVariables>
     <EnvironmentVariable Name="MySuperSecret" Type="SecretsStoreRef" Value="supersecret:ver1"/>
   </EnvironmentVariables>
```

Die Umgebungsvariable `SecretPath` verweist auf das Verzeichnis, in dem alle Geheimnisse gespeichert sind. Jeder unter dem Abschnitt `testsecrets` aufgeführte Parameter wird in einer separaten Datei gespeichert. Die Anwendung kann das Geheimnis jetzt wie folgt verwenden:
```C#
secretValue = IO.ReadFile(Path.Join(Environment.GetEnvironmentVariable("SecretPath"),  "TopSecret"))
```
   
## <a name="rotating-the-central-secret-service-encryption-certificate"></a>Rotieren des Central Secret Service-Verschlüsselungszertifikats
Es ist wichtig zu beachten, dass Zertifikate auch nach Ablauf ihrer Gültigkeit zur Entschlüsselung verwendet werden können. Es wird empfohlen, die bisherigen Verschlüsselungszertifikate nach der Rotation weiterhin bereitzustellen, um die Wahrscheinlichkeit einer Sperre zu verringern. Für die Rotation des CSS-Verschlüsselungszertifikats sind die folgenden Schritte erforderlich:

1. Stellen Sie das neue Zertifikat auf jedem Knoten des Clusters bereit. Entfernen Sie zu diesem Zeitpunkt das vorherige Verschlüsselungszertifikat nicht, und stellen Sie es nicht weiterhin bereit.
2. Starten Sie ein Upgrade der Clusterkonfiguration, um den Wert von `EncryptionCertificateThumbprint` in den SHA-1-Fingerabdruck des neuen Zertifikats zu ändern.
Nach Abschluss des Upgrades beginnt CSS mit der erneuten Verschlüsselung des vorhandenen Inhalts im neuen Verschlüsselungszertifikat. Alle Geheimnisse, die CSS nach diesem Vorgang hinzugefügt werden, werden direkt mit dem neuen Verschlüsselungszertifikat verschlüsselt. Da die Umstellung zum Schutz aller Geheimnisse durch das neue Zertifikat asynchron erfolgt, ist es wichtig, dass das vorherige Verschlüsselungszertifikat auf allen Knoten installiert und für CSS verfügbar bleibt.

## <a name="removing-central-secret-service-from-your-cluster"></a>Entfernen von CSS aus Ihrem Cluster
Für das sichere Entfernen von CSS aus einem Cluster sind zwei Upgrades erforderlich. Durch das erste Upgrade wird CSS funktional deaktiviert, während das zweite Upgrade den Dienst aus der Clusterdefinition entfernt. Bei diesem Vorgang wird auch der zugehörige Inhalt dauerhaft gelöscht. Dieser zweistufige Prozess verhindert das versehentliche Löschen des Diensts und stellt sicher, dass bei der Entfernung keine verwaisten Abhängigkeiten von CSS zurückbleiben. Dieses Feature ist ab Service Fabric 8.0 verfügbar.

### <a name="step-1-update-css-deployedstate-to-removing"></a>Schritt 1: Aktualisieren des CSS-Bereitstellungsstatus in „removing“
Aktualisieren Sie die Clusterdefinition von `"IsEnabled" = "true"` oder `"DeployedState" = "enabled"` auf den folgenden Wert:
```json
{
    "name":  "DeployedState",
    "value":  "removing"
}
```
Sobald CSS in den Bereitstellungsstatus `Removing` wechselt, lehnt er alle eingehenden Geheimnis-API-Aufrufe ab. Dies gilt unabhängig davon, ob es sich um direkte REST-Aufrufe handelt oder ob die Aufrufe über Aktivierungen von Diensten erfolgen, die „SecretStoreRefs“ oder „KeyVaultReferences“ umfassen. Alle Anwendungen oder Komponenten im Cluster, die an diesem Punkt noch von CSS abhängig sind, wechseln in einen Warnungsstatus. In diesem Fall sollte ein Rollback für das Upgrade auf den Bereitstellungsstatus `Removing` durchgeführt werden. Wenn das Upgrade bereits erfolgreich war, sollte ein neues Upgrade eingeleitet werden, um CSS wieder auf „DeployedState = `Enabled`“ zurückzusetzen. Wenn CSS im Bereitstellungsstatus `Removing` eine Anforderung empfängt, wird der HTTP-Code 401 (nicht autorisiert) zurückgegeben, und der Dienst versetzt sich selbst in den Integritätszustand „Warnung“.

### <a name="step-2-update-css-deployedstate-to-disabled"></a>Schritt 2: Aktualisieren des CSS-Bereitstellungsstatus in „disabled“
Aktualisieren Sie die Clusterdefinition von `"DeployedState" = "removing"` auf den folgenden Wert:
```json
{
    "name":  "DeployedState",
    "value":  "disabled"
}
```
Central Secret Service sollte nicht mehr im Cluster ausgeführt werden und wird nicht mehr in der Liste der Systemdienste aufgeführt. Der CSS-Inhalt ist unwiderruflich gelöscht. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Anwendungs- und Dienstsicherheit](service-fabric-application-and-service-security.md).
- Lesen Sie eine Einführung zu [verwalteten Identitäten für Service Fabric-Anwendungen](concepts-managed-identity.md).
- Erweitern Sie die CSS-Funktionalität mit [KeyVaultReferences](service-fabric-keyvault-references.md).
