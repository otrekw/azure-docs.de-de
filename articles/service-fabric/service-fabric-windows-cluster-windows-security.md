---
title: Schützen eines Windows-Clusters mithilfe von Windows-Sicherheit
description: Es wird beschrieben, wie Sie die Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit in einem eigenständigen Cluster unter Windows mit Windows-Sicherheit konfigurieren.
ms.topic: conceptual
ms.date: 08/24/2017
ms.openlocfilehash: a34c7084a9faaf0d676d4f6c68da53b2bc84f01b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "103574610"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Schützen eines eigenständigen Windows-Clusters mit Windows-Sicherheit
Um nicht autorisierten Zugriff auf einen Service Fabric-Cluster zu verhindern, müssen Sie den Cluster schützen. Sicherheit ist besonders wichtig, wenn der Cluster Produktionsworkloads ausführt. In diesem Artikel wird beschrieben, wie Knoten-zu-Knoten- und Client-zu-Knoten-Sicherheit mit Windows-Sicherheit in der *ClusterConfig.JSON*-Datei konfiguriert wird.  Der Prozess entspricht dem Sicherheitskonfigurationsschritt von [Erstellen eines eigenständigen Clusters unter Windows Server](service-fabric-cluster-creation-for-windows-server.md). Weitere Informationen zur Verwendung von Windows-Sicherheit durch Service Fabric finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

> [!NOTE]
> Sie sollten sich die Auswahl für die Knoten-zu-Knoten-Sicherheit sorgfältig überlegen, da eine Clusteraktualisierung von einer Sicherheitsauswahl zur anderen nicht möglich ist. Um die Sicherheitsauswahl zu ändern, müssen Sie den vollständigen Cluster neu erstellen.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Konfigurieren der Windows-Sicherheit mithilfe von gMSA  
gMSA (gruppenverwaltetes Dienstkonto) ist das bevorzugte Sicherheitsmodell. Die Beispielkonfigurationsdatei *ClusterConfig.gMSA.Windows.MultiMachine.JSON*, die mit dem Paket [Microsoft.Azure.ServiceFabric.WindowsServer\<version>.zip](https://go.microsoft.com/fwlink/?LinkId=730690) für den eigenständigen Cluster heruntergeladen wurde, enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit mithilfe eines [gruppenverwalteten Dienstkontos (Group Managed Service Account, gMSA)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11)):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Konfigurationseinstellung** | **Beschreibung** |
| --- | --- |
| ClusterCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit für Knoten-zu-Knoten-Kommunikation zu aktivieren.  | 
| ServerCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit für Client-zu-Knoten-Kommunikation zu aktivieren. |
| WindowsIdentities |Enthält die Cluster und Clientidentitäten. |
| ClustergMSAIdentity |Konfiguriert die Knoten-zu-Knoten-Sicherheit. Ein von der Gruppe verwaltetes Dienstkonto |
| ClusterSPN |Registrierter SPN für gMSA-Konto|
| ClientIdentities |Konfiguriert die Client-zu-Knoten-Sicherheit. Ein Array mit Clientbenutzerkonten. |
| Identity |Fügen Sie den Domänenbenutzer – „Domäne\Benutzername“ – für die Identität des Clients hinzu. |
| IsAdmin |Legen Sie den Wert „true“ fest, um anzugeben, dass der Domänenbenutzer über Administratorclientzugriff verfügt, oder „false“ für Benutzerclientzugriff. |

> [!NOTE]
> Der ClustergMSAIdentity-Wert muss das Format „mysfgmsa@mydomain“ aufweisen.

Die [Knoten-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird durch Festlegen von **ClustergMSAIdentity** konfiguriert, wenn Service Fabric in einem gMSA ausgeführt werden muss. Um Vertrauensstellungen zwischen Knoten erstellen zu können, müssen die Knoten über das Vorhandensein des jeweils anderen Knotens informiert sein. Dies kann auf zwei Arten erreicht werden: Geben Sie das gruppenverwaltete Dienstkonto an, das alle Knoten im Cluster enthält, oder geben Sie die Domänencomputergruppe an, die alle Knoten im Cluster enthält. Wir empfehlen Ihnen dringend, den Ansatz der [gruppenverwalteten Dienstkonten](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11)) zu verwenden. Dies gilt besonders für größere Cluster (mehr als zehn Knoten) oder für Cluster, die sich voraussichtlich vergrößern oder verkleinern.  
Für diesen Ansatz ist keine Erstellung einer Domänengruppe erforderlich, für die Clusteradministratoren die Zugriffsrechte zum Hinzufügen und Entfernen von Mitgliedern gewährt wurden. Diese Konten sind auch nützlich für die automatische Kennwortverwaltung. Weitere Informationen finden Sie unter [Erste Schritte mit gruppenverwalteten Dienstkonten](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431(v=ws.11)).  
 
Die [Client-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mit **ClientIdentities** konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Dies ist auf zwei Arten möglich: Geben Sie die Domänengruppenbenutzer oder die Domänenknotenbenutzer an, die eine Verbindung herstellen können. Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Die Zugriffssteuerung ermöglicht es dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.  Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen. Weitere Informationen zur Zugriffssteuerung finden Sie unter [Rollenbasierte Zugriffssteuerung für Service Fabric-Clients](service-fabric-cluster-security-roles.md).  
 
Im folgenden Beispielabschnitt zur **Sicherheit** wird die Windows-Sicherheit mithilfe eines gruppenverwalteten Dienstkontos konfiguriert und angegeben, dass die Computer im Dienstkonto *ServiceFabric.clusterA.contoso.com* Teil des Clusters sind und dass *CONTOSO\usera* über Administratorclientzugriff verfügt:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Konfigurieren der Windows-Sicherheit mithilfe einer Computergruppe  
Wie oben erläutert, wird gMSA bevorzugt, aber auch dieses Sicherheitsmodells wird unterstützt. Die Beispielkonfigurationsdatei *ClusterConfig.Windows.MultiMachine.JSON*, die mit dem Paket [Microsoft.Azure.ServiceFabric.WindowsServer\<version>.zip](https://go.microsoft.com/fwlink/?LinkId=730690) für den eigenständigen Cluster heruntergeladen wurde, enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit.  Die Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert: 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Konfigurationseinstellung** | **Beschreibung** |
| --- | --- |
| ClusterCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit für Knoten-zu-Knoten-Kommunikation zu aktivieren.  |
| ServerCredentialType |Legen Sie den Wert *Windows* fest, um Windows-Sicherheit für Client-zu-Knoten-Kommunikation zu aktivieren. |
| WindowsIdentities |Enthält die Cluster und Clientidentitäten. |
| ClusterIdentity |Verwenden Sie einen Computergruppennamen – „Domäne\Computergruppe“ – um Knoten-zu-Knoten-Sicherheit zu konfigurieren. |
| ClientIdentities |Konfiguriert die Client-zu-Knoten-Sicherheit. Ein Array mit Clientbenutzerkonten. |  
| Identity |Fügen Sie den Domänenbenutzer – „Domäne\Benutzername“ – für die Identität des Clients hinzu. |  
| IsAdmin |Legen Sie den Wert „true“ fest, um anzugeben, dass der Domänenbenutzer über Administratorclientzugriff verfügt, oder „false“ für Benutzerclientzugriff. |  

Die [Knoten-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird durch Festlegen von **ClusterIdentity** konfiguriert, wenn Sie eine Computergruppe in einer Active Directory-Domäne verwenden möchten. Weitere Informationen finden Sie unter [Erstellen einer Computergruppe in Active Directory](/previous-versions/commerce-server/aa545347(v=cs.70)).

[Client-zu-Knoten-Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mit **ClientIdentities** konfiguriert. Um die Vertrauensstellung zwischen einem Client und dem Cluster herzustellen, müssen Sie den Cluster so konfigurieren, dass ermittelt werden kann, welche Clientidentitäten vertrauenswürdig sind. Sie können Vertrauensstellungen auf zwei verschiedene Arten erstellen:

- Geben Sie die Domänengruppenbenutzer an, die eine Verbindung herstellen können.
- Geben Sie die Domänenknotenbenutzer an, die eine Verbindung herstellen können.

Service Fabric unterstützt zwei unterschiedliche Zugriffsberechtigungstypen für Clients, die mit einem Service Fabric-Cluster verbunden sind: Administrator und Benutzer. Die Zugriffssteuerung ermöglicht dem Clusteradministrator, den Zugriff auf bestimmte Arten von Clustervorgängen für verschiedene Gruppen von Benutzern einzuschränken, wodurch die Sicherheit des Clusters erhöht wird.  Administratoren haben vollständigen Zugriff auf Verwaltungsfunktionen (einschließlich Lese-/Schreibzugriff). Benutzer haben standardmäßig nur Lesezugriff auf Verwaltungsfunktionen (z. B. Abfragefunktionen) sowie die Möglichkeit, Anwendungen und Dienste aufzulösen.  

Im folgenden Beispielabschnitt zur **Sicherheit** wird die Windows-Sicherheit konfiguriert und angegeben, dass die Computer in *ServiceFabric/clusterA.contoso.com* Teil des Clusters sind, und dass *CONTOSO\usera* über Administratorclientzugriff verfügt:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric sollte nicht auf einem Domänencontroller bereitgestellt werden. Stellen Sie sicher, dass „ClusterConfig.json“ nicht die IP-Adresse des Domänencontrollers einschließt, wenn Sie eine Computergruppe oder ein gruppenverwaltetes Dienstkonto (group Managed Service Account, gMSA) verwenden.
>
>

## <a name="next-steps"></a>Nächste Schritte
Setzen Sie den Vorgang der Clustererstellung unter *Erstellen eines eigenständigen Clusters unter Windows* fort, nachdem Sie die Windows-Sicherheit in der Datei [ClusterConfig.JSON](service-fabric-cluster-creation-for-windows-server.md)konfiguriert haben.

Weitere Informationen zur Knoten-zu-Knoten-Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierten Zugriffssteuerung finden Sie unter [Szenarien für die Clustersicherheit](service-fabric-cluster-security.md).

Beispiele für die Herstellung der Verbindung mit PowerShell oder FabricClient finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md) .
