---
title: Apache Kafka-REST-Proxy – Azure HDInsight
description: Erfahren Sie, wie Sie Apache Kafka-Vorgänge mithilfe eines Kafka-REST-Proxys in Azure HDInsight ausführen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: b53fc3af71ce872c9ca9f513139c8179fd4165ed
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77031391"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>Interagieren mit Apache Kafka-Clustern in Azure HDInsight mithilfe eines REST-Proxys

Mithilfe des Kafka-REST-Proxys können Sie über eine REST-API über HTTP mit Ihrem Kafka-Cluster interagieren. Das bedeutet, dass sich Ihre Kafka-Clients außerhalb des virtuellen Netzwerks befinden können. Darüber hinaus können Clients einfache HTTP-Aufrufe zum Senden und Empfangen von Nachrichten an den Kafka-Cluster vornehmen, statt Kafka-Bibliotheken zu verwenden.  

## <a name="background"></a>Hintergrund

### <a name="architecture"></a>Aufbau

Ohne REST-Proxy müssen sich Kafka-Clients im gleichen VNET wie der Kafka-Cluster oder in einem mittels Peering verbundenen VNET befinden. Durch den REST-Proxy können Sie Datenproduzenten oder Datenconsumer an beliebigen Standorten verbinden. Durch Bereitstellung des REST-Proxys wird ein neuer öffentlicher Endpunkt für Ihren Cluster geschaffen, der in den Portaleinstellungen zu finden ist.

Eine vollständige Spezifikation der Vorgänge, die von der API unterstützt werden, finden Sie unter [Apache Kafka-REST-Proxy-API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

### <a name="security"></a>Sicherheit

Der Zugriff auf den Kafka-REST-Proxy wird mit Azure Active Directory-Sicherheitsgruppen verwaltet. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups).

Wenn Sie den Kafka-Cluster mit aktiviertem REST-Proxy erstellen, geben Sie die AAD-Sicherheitsgruppe an, die Zugriff auf den REST-Endpunkt haben soll. Die Kafka-Clients (Anwendungen), die Zugriff auf den REST-Proxy benötigen, sollten für diese Gruppe vom Gruppenbesitzer registriert werden. Der Gruppenbesitzer kann dies über das Portal oder über PowerShell vornehmen.

Bevor Anforderungen an den REST-Proxyendpunkt übertragen werden, sollte die Clientanwendung ein OAuth-Token zum Überprüfen der Mitgliedschaft in der richtigen Sicherheitsgruppe erhalten. Weitere Informationen zur Funktionsweise von OAuth-Token finden Sie unter [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](../../active-directory/develop/v1-protocols-oauth-code.md). Ein Beispiel für das Abrufen eines OAuth-Tokens in Python finden Sie unter [Beispiel für eine Clientanwendung](#client-application-sample).

Sobald die Clientanwendung über das OAuth-Token verfügt, muss dieses Token in der HTTP-Anforderung an den REST-Proxy übergeben werden.

## <a name="prerequisites"></a>Voraussetzungen

1. Registrieren Sie eine Anwendung in Azure AD. Die Clientanwendungen, die Sie für die Interaktion mit dem Kafka-REST-Proxy schreiben, verwenden die ID und den geheimen Schlüssel dieser Anwendung zur Authentifizierung bei Azure.
1. Erstellen Sie eine Azure AD-Sicherheitsgruppe, und fügen Sie die Anwendung, die Sie in Azure AD registriert haben, der Sicherheitsgruppe hinzu. Mit dieser Sicherheitsgruppe wird gesteuert, welche Anwendungen mit dem REST-Proxy interagieren dürfen. Weitere Informationen zum Erstellen von Azure AD-Gruppen finden Sie unter [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern mithilfe von Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>Erstellen eines Kafka-Clusters mit aktiviertem REST-Proxy

1. Aktivieren Sie während des Workflows zum Erstellen eines Kafka-Clusters auf der Registerkarte „Sicherheit + Netzwerkbetrieb“ die Option „Kafka-REST-Proxy aktivieren“.

     ![Aktivieren des Kafka-REST-Proxys und Auswählen der Sicherheitsgruppe](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. Klicken Sie auf **Sicherheitsgruppe auswählen**. Wählen Sie in der Liste der Sicherheitsgruppen die Sicherheitsgruppe aus, die Zugriff auf den REST-Proxy haben soll. Sie können das Suchfeld verwenden, um nach der entsprechenden Sicherheitsgruppe zu suchen. Klicken Sie unten auf die Schaltfläche **Auswählen**.

     ![Aktivieren des Kafka-REST-Proxys und Auswählen der Sicherheitsgruppe](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. Führen Sie die restlichen Schritte zum Erstellen Ihres Clusters aus, wie es unter [Erstellen eines Apache Kafka-Clusters in Azure HDInsight im Azure-Portal](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started) beschrieben ist.

1. Nachdem der Cluster erstellt wurde, wechseln Sie zu den Clustereigenschaften, um die URL für den Kafka-REST-Proxy zu erfassen.

     ![Anzeigen der REST-Proxy-URL](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>Beispiel für eine Clientanwendung

Sie können den unten gezeigten Python-Code verwenden, um mit dem REST-Proxy in Ihrem Kafka-Cluster zu interagieren. Im Code werden folgende Schritte ausgeführt:

1. Abrufen eines OAuth-Tokens von Azure AD
1. Erstellen des angegebenen Themas
1. Senden von Nachrichten für diese Thema
1. Verarbeiten von Nachrichten von diesem Thema

Weitere Informationen zum Abrufen von OAuth-Token in Python finden Sie unter der [Klasse „AuthenticationContext“ in Python](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Es tritt möglicherweise eine Verzögerung auf, während Themen, die nicht über den Kafka-REST-Proxy erstellt oder gelöscht werden, dort widergespiegelt werden. Die Ursache für diese Verzögerung ist eine Cacheaktualisierung.

```python
#Required python packages
#pip3 install adal
#pip install msrestazure

import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
import requests

#--------------------------Configure these properties-------------------------------#
# Tenant ID for your Azure Subscription
tenant_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Application Id
client_id = 'XYZABCDE-1234-1234-1234-ABCDEFGHIJKL'
# Your Client Credentials
client_secret = 'password'
# kafka rest proxy -endpoint
kafkarest_endpoint = "https://<clustername>-kafkarest.azurehdinsight.net"
#--------------------------Configure these properties-------------------------------#

#getting token
login_endpoint = AZURE_PUBLIC_CLOUD.endpoints.active_directory
resource = "https://hib.azurehdinsight.net"
context = adal.AuthenticationContext(login_endpoint + '/' + tenant_id)

token = context.acquire_token_with_client_credentials(
    resource,
    client_id,
    client_secret)

accessToken = 'Bearer ' + token['accessToken']

print(accessToken)

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Gehen Sie zum Verwenden des Beispielcodes folgendermaßen vor:

1. Speichern Sie den Beispielcode auf einem Computer mit installiertem Python.
1. Installieren Sie erforderliche Python-Abhängigkeiten, indem Sie `pip3 install adal` und `pip install msrestazure`ausführen.
1. Ändern Sie den Code, und aktualisieren Sie die folgenden Eigenschaften entsprechend Ihrer Umgebung:
    1.  *Tenant_id*: Der Azure-Mandant, in dem sich Ihr Abonnement befindet.
    1.  *Client_id*: Die ID für die Anwendung, die Sie in der Sicherheitsgruppe registriert haben.
    1.  *Client_secret*: Der geheime Schlüssel für die Anwendung, die Sie in der Sicherheitsgruppe registriert haben.
    1.  *Kafkarest_endpoint*: Entnehmen Sie diesen Wert der Registerkarte „Eigenschaften“ in der Clusterübersicht, wie es im [Abschnitt zur Bereitstellung](#create-a-kafka-cluster-with-rest-proxy-enabled) beschrieben ist. Er sollte das folgende Format aufweisen: `https://<clustername>-kafkarest.azurehdinsight.net`
3. Führen Sie die Python-Datei über die Befehlszeile aus, indem Sie `python <filename.py>` ausführen.

## <a name="next-steps"></a>Nächste Schritte

* [Referenzdokumente zur Kafka-REST-Proxy-API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
* [Tutorial: Verwenden der Apache Kafka Producer- und Consumer-APIs](apache-kafka-producer-consumer-api.md).