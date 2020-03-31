---
title: Importieren von Zertifikaten in einen Container
description: Erfahren Sie, wie Sie Zertifikatdateien in einen Service Fabric-Containerdienst importieren.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: da4babd8f9d1a25a8514d0c6f1526b43a9723854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614110"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Importieren einer Zertifikatdatei in einen Container, der unter Service Fabric ausgeführt wird

Containerdienste können durch Angabe eines Zertifikats geschützt werden. Service Fabric bietet einen Mechanismus, über den Dienste innerhalb eines Containers auf ein Zertifikat zugreifen können, das auf den Knoten eines Windows- oder Linux-Clusters (ab Version 5.7) installiert ist. Das Zertifikat muss in einem Zertifikatspeicher unter LocalMachine auf allen Knoten des Clusters installiert werden. Der mit dem Zertifikat übereinstimmende private Schlüssel muss verfügbar, zugänglich und – unter Windows – exportierbar sein. Die Zertifikatinformationen werden im Anwendungsmanifest unter dem Tag `ContainerHostPolicies` angegeben, wie im folgenden Codeausschnitt zu sehen:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Für Windows-Cluster exportiert die Runtime beim Start der Anwendung alle referenzierten Zertifikate und die zugehörigen entsprechenden privaten Schlüssel in eine PFX-Datei und schützt sie jeweils durch ein zufällig generiertes Kennwort. Auf die PFX- und Kennwortdateien kann jeweils innerhalb des Containers über folgende Umgebungsvariablen zugegriffen werden: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Für Linux-Cluster werden die Zertifikate (PEM) aus dem durch X509StoreName angegebenen Speicher in den Container kopiert. Die entsprechenden Linux-Umgebungsvariablen sind:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Wenn Sie bereits über die Zertifikate im erforderlichen Format verfügen und innerhalb des Containers darauf zugreifen möchten, können Sie alternativ ein Datenpaket innerhalb des App-Pakets erstellen und Folgendes in Ihrem Anwendungsmanifest angeben:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Die Zertifikatdateien werden vom Containerdienst oder -prozess in den Container importiert. Für den Zertifikatimport können Sie Skripts vom Typ `setupentrypoint.sh` verwenden oder benutzerdefinierten Code innerhalb des Containers ausführen. C#-Beispielcode zum Importieren der PFX-Datei:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Dieses PFX-Zertifikat kann verwendet werden, um die Anwendung oder den Dienst zu authentifizieren oder die Kommunikation mit anderen Diensten zu schützen. Standardmäßig ist der Zugriff auf die Dateien nur über das Konto SYSTEM möglich. Wenn der Dienst es erfordert, können Sie den Zugriff über andere Konten einrichten.

Lesen Sie als Nächstes die folgenden Artikel:

* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)
