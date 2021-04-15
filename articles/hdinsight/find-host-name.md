---
title: So erhalten Sie die Hostnamen von Azure HDInsight-Clusterknoten
description: Erfahren Sie, wie Sie Hostnamen und den FQDN-Namen von Azure HDInsight-Clusterknoten erhalten.
ms.service: hdinsight
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.date: 03/23/2021
ms.openlocfilehash: 676b4ccd52e8a6f1f378756a255ad55b74f18ebe
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105111405"
---
# <a name="find-the-host-names-of-cluster-nodes"></a>Suchen der Hostnamen von Clusterknoten

HDInsight-Cluster wird mit öffentlichem DNS „clustername.azurehdinsight.NET“ erstellt. Wenn Sie eine SSH-Verbindung mit einzelnen Knoten herstellen oder die Verbindung mit Clusterknoten mit im gleichen benutzerdefinierten virtuellen Netzwerk festlegen, müssen Sie den Hostnamen oder vollständig qualifizierte Domänennamen (FQDN) von Clusterknoten verwenden.

In diesem Artikel erfahren Sie, wie Sie die Hostnamen von Clusterknoten erhalten. Sie können Sie manuell über die Ambari Web-Benutzeroberfläche oder automatisch über die Ambari-REST-API erhalten.

> [!WARNING]
> Verwenden Sie die folgenden empfohlenen Vorgehensweisen zum Abrufen von Hostnamen von Clusterknoten. Die Zahlen im Hostnamen sind nicht in der Reihenfolge garantiert, und HDInsight kann das Format des Hostnamens so ändern, dass er auf VMS mit Releaseaktualisierung ausgerichtet ist. Verwenden Sie nicht die Abhängigkeit von einer bestimmten Benennungskonvention, die heute vorhanden ist. 
>

Sie können die Hostnamen über die Ambari-Benutzeroberfläche oder die Ambari-Rest-API erhalten. 

## <a name="get-the-host-names-from-ambari-web-ui"></a>Hostnamen aus Ambari Web-Benutzeroberfläche
Sie können die Ambari Web-Benutzeroberfläche verwenden, um die Hostnamen zu erhalten, wenn Sie SSH mit dem Knoten herstellen. Die Ambari Web-Benutzeroberfläche ist in Ihrem HDInsight-Cluster unter `https://CLUSTERNAME.azurehdinsight.net/#/main/hosts` verfügbar, wobei `CLUSTERNAME` dem Namen Ihres Clusters entspricht.

![Get-Host-Names-In-Ambari-UI](.\media\find-host-name\find-host-name-in-ambari-ui.png)

## <a name="get-the-host-names-from-ambari-rest-api"></a>Hostnamen in Ambari Web-Benutzeroberfläche finden
Beim Erstellen von Automatisierungsskripts können Sie die Ambari-REST-API verwenden, um die Hostnamen zu erhalten, bevor Sie Verbindungen mit Hosts herstellen. Die Zahlen im Hostnamen sind nicht in der Reihenfolge garantiert, und HDInsight kann das Format des Hostnamens so ändern, dass er auf VMs mit Releaseaktualisierung ausgerichtet ist. Verwenden Sie nicht die Abhängigkeit von einer bestimmten Benennungskonvention, die heute vorhanden ist. 

Im folgenden finden Sie einige Beispiele für das Abrufen des FQDN für die Knoten im Cluster. Weitere Informationen finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API](.\hdinsight-hadoop-manage-ambari-rest-api.md).

Im folgenden Beispiel wird [jq](https://stedolan.github.io/jq/) oder [ConvertFrom-Json](/powershell/module/microsoft.powershell.utility/convertfrom-json) verwendet, um das JSON-Antwortdokument zu analysieren und nur die Hostnamen anzuzeigen.

```bash
export password=''
export clusterName=''
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq -r '.items[].Hosts.host_name'
```  

```powershell
$clusterName=''
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```