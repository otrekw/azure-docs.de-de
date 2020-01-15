---
title: Einrichten gruppenverwalteter Dienstkonten für Azure Service Fabric-Containerdienste
description: Hier erfahren Sie, wie Sie gruppenverwaltete Dienstkonten (group Managed Service Accounts, gMSA) für einen in Azure Service Fabric ausgeführten Container einrichten.
ms.topic: conceptual
ms.date: 03/20/2019
ms.openlocfilehash: 9873e2d7672412b0e1e22c6c2a774cf629fd728a
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639206"
---
# <a name="set-up-gmsa-for-windows-containers-running-on-service-fabric"></a>Einrichten gruppenverwalteter Dienstkonten für in Service Fabric ausgeführte Windows-Container

Zur Einrichtung gruppenverwalteter Dienstkonten (group Managed Service Accounts, gMSAs) wird eine Datei mit Anmeldeinformationen (`credspec`) auf allen Knoten im Cluster platziert. Die Datei kann mithilfe einer VM-Erweiterung auf alle Knoten kopiert werden.  Die Datei vom Typ `credspec` muss die gMSA-Kontoinformationen enthalten. Weitere Informationen zur `credspec`-Datei finden Sie unter [Erstellen einer Anmeldeinformationen-Spezifikation](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts#create-a-credential-spec) (Dienstkonten). Die Angabe von Anmeldeinformationen und das Tag `Hostname` werden im Anwendungsmanifest angegeben. Das Tag `Hostname` muss dem gMSA-Kontonamen entsprechen, unter dem der Container ausgeführt wird.  Durch das Tag `Hostname` kann sich der Container gegenüber anderen Diensten in der Domäne mittels Kerberos-Authentifizierung authentifizieren.  Der folgende Codeausschnitt zeigt ein Beispiel für die Angabe von `Hostname` und `credspec` im Anwendungsmanifest:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
Lesen Sie als Nächstes die folgenden Artikel:

* [Bereitstellen eines Windows-Containers in Service Fabric unter Windows Server 2016](service-fabric-get-started-containers.md)
* [Bereitstellen eines Docker-Containers in Service Fabric unter Linux](service-fabric-get-started-containers-linux.md)
