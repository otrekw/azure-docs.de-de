---
title: Verwenden des Identitätsbrokers (Vorschauversion) für die Verwaltung von Anmeldeinformationen – Azure HDInsight
description: Erfahren Sie etwas über den HDInsight-Identitätsbroker zur Vereinfachung der Authentifizierung für in Domänen eingebundene Apache Hadoop-Cluster.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: 1e7eaf49fb8b62259b8c619c89edffd629dfde7f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81685513"
---
# <a name="use-id-broker-preview-for-credential-management"></a>Verwenden des Identitätsbrokers (Vorschauversion) für die Verwaltung von Anmeldeinformationen

In diesem Artikel wird beschrieben, wie Sie die Funktion „Identitätsbroker“ in Azure HDInsight einrichten und verwenden. Mit dieser Funktion können Sie sich über Azure Multi-Factor Authentication bei Apache Ambari anmelden und die erforderlichen Kerberos-Tickets abrufen, ohne dass Kennworthashes in Azure Active Directory Domain Services (Azure AD DS) erforderlich sind.

## <a name="overview"></a>Übersicht

Der Identitätsbroker vereinfacht die komplexe Einrichtung der Authentifizierung in den folgenden Szenarien:

* In Ihrer Organisation werden Benutzer basierend auf einem Verbund für den Zugriff auf Cloudressourcen authentifiziert. Bisher mussten Sie zur Verwendung von HDInsight-Clustern mit Enterprise-Sicherheitspaket (ESP) in der lokalen Umgebung die Kennworthashsynchronisierung für Azure Active Directory aktivieren. Diese Anforderung kann in einigen Organisationen schwierig oder unerwünscht sein.

* Sie entwickeln Lösungen, in denen Technologien auf der Basis unterschiedlicher Authentifizierungsmechanismen verwendet werden. Apache Hadoop und Apache Ranger basieren beispielsweise auf Kerberos, Azure Data Lake Storage hingegen auf OAuth.

Der Identitätsbroker bietet eine einheitliche Authentifizierungsinfrastruktur, sodass die Notwendigkeit entfällt, Kennworthashes mit Azure AD DS zu synchronisieren. Der Identitätsbroker umfasst Komponenten, die auf einem virtuellen Windows Server-Computer (Identitätsbrokerknoten) sowie auf Clustergatewayknoten ausgeführt werden. 

In der folgenden Abbildung ist der Authentifizierungsablauf für alle Benutzer, einschließlich Verbundbenutzer, nach der Aktivierung des Identitätsbrokers dargestellt:

![Authentifizierungsablauf mit Identitätsbroker](./media/identity-broker/identity-broker-architecture.png)

Mit dem Identitätsbroker können Sie sich über Multi-Factor Authentication bei ESP-Clustern anmelden, ohne Kennwörter angeben zu müssen. Wenn Sie bereits bei anderen Azure-Diensten angemeldet sind, z. B. im Azure-Portal, können Sie sich mittels einmaligem Anmelden (SSO) bei Ihrem HDInsight-Cluster anmelden.

## <a name="enable-hdinsight-id-broker"></a>Aktivieren des HDInsight-Identitätsbrokers

Führen Sie die folgenden Schritte aus, um einen ESP-Cluster mit aktiviertem Identitätsbroker zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Führen Sie die allgemeinen Schritte zum Erstellen eines ESP-Clusters aus. Weitere Informationen finden Sie unter [Erstellen eines HDInsight-Clusters mit ESP](apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp).
1. Wählen Sie **HDInsight-Identitätsbroker aktivieren (Vorschau)** aus.

Mit der Funktion „Identitätsbroker“ wird im Cluster ein zusätzlicher virtueller Computer hinzugefügt. Dieser virtuelle Computer ist der Identitätsbrokerknoten, der Serverkomponenten zur Unterstützung der Authentifizierung enthält. Der Identitätsbrokerknoten ist in die Azure AD DS-Domäne eingebunden.

![Option zum Aktivieren des Identitätsbrokers](./media/identity-broker/identity-broker-enable.png)

### <a name="using-azure-resource-manager-templates"></a>Verwenden von Azure-Ressourcen-Manager-Vorlagen
Wenn Sie dem Computeprofil Ihrer Vorlage eine neue Rolle namens `idbrokernode` mit den folgenden Attributen hinzufügen, wird der Cluster mit aktiviertem Identitätsbrokerknoten erstellt:

```json
.
.
.
"computeProfile": {
    "roles": [
        {
            "autoscale": null,
            "name": "headnode",
           ....
        },
        {
            "autoscale": null,
            "name": "workernode",
            ....
        },
        {
            "autoscale": null,
            "name": "idbrokernode",
            "targetInstanceCount": 1,
            "hardwareProfile": {
                "vmSize": "Standard_A2_V2"
            },
            "virtualNetworkProfile": {
                "id": "string",
                "subnet": "string"
            },
            "scriptActions": [],
            "dataDisksGroups": null
        }
    ]
}
.
.
.
```

## <a name="tool-integration"></a>Toolintegration

Das HDInsight [IntelliJ-Plug-In](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-intellij-tool-plugin#integrate-with-hdinsight-identity-broker-hib) wird für die Unterstützung von OAuth aktualisiert. Sie können dieses Plug-In verwenden, um eine Verbindung mit dem Cluster herzustellen und Aufträge zu übermitteln.

## <a name="ssh-access-without-a-password-hash-in-azure-ad-ds"></a>SSH-Zugriff ohne Kennworthash in Azure AD DS

Nach dem Aktivieren des Identitätsbrokers müssen Sie dennoch einen Kennworthash in Azure AD DS für SSH-Szenarien mit Domänenkonten speichern. Sie müssen ein Kennwort angeben, um eine SSH-Verbindung mit einem in eine Domäne eingebundenen virtuellen Computer herzustellen oder um den Befehl `kinit` auszuführen. 

Für die SSH-Authentifizierung muss der Hash in Azure AD DS verfügbar sein. Wenn Sie SSH nur für administrative Szenarien verwenden möchten, können Sie ein ausschließliches Cloudkonto erstellen und für die SSH-Verbindung mit dem Cluster verwenden. Andere Benutzer können weiterhin Ambari- oder HDInsight-Tools (z. B. das IntelliJ-Plug-In) verwenden, ohne dass der Kennworthash in Azure AD DS verfügbar ist.

## <a name="clients-using-oauth-to-connect-to-hdinsight-gateway-with-id-broker-setup"></a>Clients, die OAuth zum Herstellen einer Verbindung mit dem HDInsight-Gateway mithilfe des Identitätsbrokersetups verwenden

Im Identitätsbrokersetup können benutzerdefinierte Apps und Clients aktualisiert werden, die eine Verbindung mit dem Gateway herstellen, um zuerst das erforderliche OAuth-Token abzurufen. Sie können die Schritte in diesem [Dokument](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-app) ausführen, um das Token mit den folgenden Informationen abzurufen:

*   OAuth-Ressourcen-URI: https://hib.azurehdinsight.net 
* AppId: 7865c1d2-f040-46cc-875f-831a1ef6a28a
*   Berechtigung: (Name: Cluster.ReadWrite, id: 8f89faa0-ffef-4007-974d-4989b39ad77d)

## <a name="next-steps"></a>Nächste Schritte

* [Konfigurieren eines HDInsight-Clusters mit Enterprise-Sicherheitspaket (Enterprise Security Package, ESP) mithilfe von Azure Active Directory Domain Services](apache-domain-joined-configure-using-azure-adds.md)
* [Synchronisieren von Azure Active Directory-Benutzern in einen HDInsight-Cluster](../hdinsight-sync-aad-users-to-cluster.md)
* [Überwachen der Clusterleistung](../hdinsight-key-scenarios-to-monitor.md)
