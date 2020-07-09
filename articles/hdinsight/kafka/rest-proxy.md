---
title: Apache Kafka-REST-Proxy – Azure HDInsight
description: Erfahren Sie, wie Sie Apache Kafka-Vorgänge mithilfe eines Kafka-REST-Proxys in Azure HDInsight ausführen.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: how-to
ms.custom: has-adal-ref, tracking-python
ms.date: 04/03/2020
ms.openlocfilehash: 3050062a80e253d0e63f6d20a8c8de31e9866ea1
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86082555"
---
# <a name="interact-with-apache-kafka-clusters-in-azure-hdinsight-using-a-rest-proxy"></a>Interagieren mit Apache Kafka-Clustern in Azure HDInsight mithilfe eines REST-Proxys

Mithilfe des Kafka-REST-Proxys können Sie über eine REST-API über HTTP mit Ihrem Kafka-Cluster interagieren. Das bedeutet, dass sich Ihre Kafka-Clients außerhalb des virtuellen Netzwerks befinden können. Clients können einfache HTTP-Aufrufe des Kafka-Clusters vornehmen, statt Kafka-Bibliotheken zu verwenden. In diesem Artikel erfahren Sie, wie Sie einen REST-Proxy-fähigen Kafka-Cluster erstellen. Er enthält auch Beispielcode, der zeigt, wie der REST-Proxy aufgerufen werden kann.

## <a name="rest-api-reference"></a>REST-API-Referenz

Informationen zu den Vorgängen, die von der Kafka-REST-API unterstützt werden, finden Sie in der [Referenz zur HDInsight Kafka-REST-Proxy-API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

## <a name="background"></a>Hintergrund

![Design für Kafka-REST-Proxy](./media/rest-proxy/rest-proxy-architecture.png)

Eine vollständige Spezifikation der Vorgänge, die von der API unterstützt werden, finden Sie unter [Apache Kafka-REST-Proxy-API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy).

### <a name="rest-proxy-endpoint"></a>REST-Proxyendpunkt

Beim Erstellen eines HDInsight Kafka-Clusters mit REST-Proxy wird ein neuer öffentlicher Endpunkt für Ihren Cluster erstellt, den Sie unter den **Eigenschaften** für Ihren HDInsight-Cluster im Azure-Portal finden können.

### <a name="security"></a>Sicherheit

Der Zugriff auf den Kafka-REST-Proxy wird mit Azure Active Directory-Sicherheitsgruppen verwaltet. Stellen Sie beim Erstellen des Kafka-Clusters die Azure AD-Sicherheitsgruppe mit Zugriff auf den REST-Endpunkt bereit. Kafka-Clients, die Zugriff auf den REST-Proxy benötigen, sollten für diese Gruppe vom Gruppenbesitzer registriert werden. Der Gruppenbesitzer kann die Registrierung über das Portal oder über PowerShell vornehmen.

Für REST-Proxy-Endpunktanforderungen sollten Clientanwendungen ein OAuth-Token abrufen. Das Token wird verwendet, um die Mitgliedschaft in Sicherheitsgruppen zu überprüfen. Weiter unten finden Sie ein [Beispiel für eine Clientanwendung](#client-application-sample), in dem das Abrufen eines OAuth-Tokens gezeigt wird. Die Clientanwendung übergibt das OAuth-Token in der HTTP-Anforderung an den REST-Proxy.

> [!NOTE]
> Weitere Informationen zu AAD-Sicherheitsgruppen finden Sie unter [Verwalten des Zugriffs auf Apps und Ressourcen mithilfe von Azure Active Directory-Gruppen](../../active-directory/fundamentals/active-directory-manage-groups.md). Weitere Informationen zur Funktionsweise von OAuth-Token finden Sie unter [Autorisieren des Zugriffs auf Azure Active Directory-Webanwendungen mit dem Flow zum Erteilen des OAuth 2.0-Codes](../../active-directory/develop/v1-protocols-oauth-code.md).

## <a name="kafka-rest-proxy-with-network-security-groups"></a>Kafka REST-Proxy mit Netzwerksicherheitsgruppen
Wenn Sie Ihr eigenes virtuelles Netzwerk bereitstellen und den Netzwerkdatenverkehr mit Netzwerksicherheitsgruppen steuern, lassen Sie **eingehenden** Datenverkehr zusätzlich zu Port 443 auch an Port **9400** zu. Dadurch wird sichergestellt, dass der Kafka REST-Proxyserver erreichbar ist.

## <a name="prerequisites"></a>Voraussetzungen

1. Registrieren Sie eine Anwendung in Azure AD. Die Clientanwendungen, die Sie für die Interaktion mit dem Kafka-REST-Proxy schreiben, verwenden die ID und den geheimen Schlüssel dieser Anwendung zur Authentifizierung bei Azure.

1. Erstellen Sie eine Azure AD-Sicherheitsgruppe. Fügen Sie die Anwendung, die Sie in Azure AD registriert haben, der Sicherheitsgruppe als **Mitglied** der Gruppe hinzu. Mit dieser Sicherheitsgruppe wird gesteuert, welche Anwendungen mit dem REST-Proxy interagieren dürfen. Weitere Informationen zum Erstellen von Azure AD-Gruppen finden Sie unter [Erstellen einer einfachen Gruppe und Hinzufügen von Mitgliedern mithilfe von Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

    Überprüfen Sie, ob die Gruppe vom Typ **Sicherheit** ist.
    ![Sicherheitsgruppe](./media/rest-proxy/rest-proxy-group.png)

    Überprüfen Sie, ob die Anwendung Mitglied der Gruppe ist.
    ![Überprüfen der Mitgliedschaft](./media/rest-proxy/rest-proxy-membergroup.png)

## <a name="create-a-kafka-cluster-with-rest-proxy-enabled"></a>Erstellen eines Kafka-Clusters mit aktiviertem REST-Proxy

Die folgenden Schritte verwenden das Azure-Portal. Ein Beispiel für die Verwendung der Azure-Befehlszeilenschnittstelle finden Sie unter [Erstellen eines Apache Kafka REST-Proxycluster mithilfe der Azure-Befehlszeilenschnittstelle](tutorial-cli-rest-proxy.md).

1. Aktivieren Sie während des Workflows zum Erstellen eines Kafka-Clusters auf der Registerkarte **Sicherheit + Netzwerkbetrieb**die Option **Kafka-REST-Proxy aktivieren**.

     ![Aktivieren des Kafka-REST-Proxys und Auswählen der Sicherheitsgruppe](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest.png)

1. Klicken Sie auf **Sicherheitsgruppe auswählen**. Wählen Sie in der Liste der Sicherheitsgruppen die Sicherheitsgruppe aus, die Zugriff auf den REST-Proxy haben soll. Sie können das Suchfeld verwenden, um nach der entsprechenden Sicherheitsgruppe zu suchen. Klicken Sie unten auf die Schaltfläche **Auswählen**.

     ![Aktivieren des Kafka-REST-Proxys und Auswählen der Sicherheitsgruppe](./media/rest-proxy/azure-portal-cluster-security-networking-kafka-rest2.png)

1. Führen Sie die restlichen Schritte zum Erstellen Ihres Clusters aus, wie es unter [Erstellen eines Apache Kafka-Clusters in Azure HDInsight im Azure-Portal](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-get-started) beschrieben ist.

1. Nachdem der Cluster erstellt wurde, wechseln Sie zu den Clustereigenschaften, um die URL für den Kafka-REST-Proxy zu erfassen.

     ![Anzeigen der REST-Proxy-URL](./media/rest-proxy/apache-kafka-rest-proxy-view-proxy-url.png)

## <a name="client-application-sample"></a>Beispiel für eine Clientanwendung

Sie können den unten gezeigten Python-Code verwenden, um mit dem REST-Proxy in Ihrem Kafka-Cluster zu interagieren. Gehen Sie zum Verwenden des Beispielcodes folgendermaßen vor:

1. Speichern Sie den Beispielcode auf einem Computer mit installiertem Python.
1. Installieren Sie erforderliche Python-Abhängigkeiten, indem Sie `pip3 install msal` ausführen.
1. Ändern Sie den Codeabschnitt **Configure these properties**, und aktualisieren Sie die folgenden Eigenschaften entsprechend Ihrer Umgebung:

    |Eigenschaft |BESCHREIBUNG |
    |---|---|
    |Mandanten-ID|Der Azure-Mandant, in dem sich Ihr Abonnement befindet.|
    |Client-ID|Die ID für die Anwendung, die Sie in der Sicherheitsgruppe registriert haben.|
    |Geheimer Clientschlüssel|Der geheime Schlüssel für die Anwendung, die Sie in der Sicherheitsgruppe registriert haben.|
    |Kafkarest_endpoint|Entnehmen Sie diesen Wert der Registerkarte **Eigenschaften** in der Clusterübersicht, wie im [Abschnitt zur Bereitstellung](#create-a-kafka-cluster-with-rest-proxy-enabled) beschrieben. Er sollte das folgende Format aufweisen: `https://<clustername>-kafkarest.azurehdinsight.net`|

1. Führen Sie die Python-Datei über die Befehlszeile aus, indem Sie `sudo python3 <filename.py>` ausführen.

Im Code wird Folgendes ausgeführt:

1. Abrufen eines OAuth-Tokens von Azure AD
1. Senden einer Anforderung an den Kafka-REST-Proxy

Weitere Informationen zum Abrufen von OAuth-Token in Python finden Sie unter der [Klasse „AuthenticationContext“ in Python](https://docs.microsoft.com/python/api/adal/adal.authentication_context.authenticationcontext?view=azure-python). Es tritt möglicherweise eine Verzögerung auf, während `topics`, die nicht über den Kafka-REST-Proxy erstellt oder gelöscht werden, dort widergespiegelt werden. Die Ursache für diese Verzögerung ist eine Cacheaktualisierung.

```python
#Required python packages
#pip3 install msal

import msal

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

# Scope
scope = 'https://hib.azurehdinsight.net/.default'
#Authority
authority = 'https://login.microsoftonline.com/' + tenant_id

# Create a preferably long-lived app instance which maintains a token cache.
app = msal.ConfidentialClientApplication(
    client_id , client_secret, authority,
    #cache - For details on how look at this example: https://github.com/Azure-Samples/ms-identity-python-webapp/blob/master/app.py
    )

# The pattern to acquire a token looks like this.
result = None

result = app.acquire_token_for_client(scopes=[scope])

print(result)
accessToken = result['access_token']

# relative url
getstatus = "/v1/metadata/topics"
request_url = kafkarest_endpoint + getstatus

# sending get request and saving the response as response object
response = requests.get(request_url, headers={'Authorization': accessToken})
print(response.content)
```

Weiter unten finden Sie ein weiteres Beispiel zum Abrufen eines Tokens von Azure für den REST-Proxy mithilfe eines curl-Befehls. **Beachten Sie, dass beim Abrufen des Tokens `scope=https://hib.azurehdinsight.net/.default` angegeben werden muss.**

```cmd
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=<clientid>&client_secret=<clientsecret>&grant_type=client_credentials&scope=https://hib.azurehdinsight.net/.default' 'https://login.microsoftonline.com/<tenantid>/oauth2/v2.0/token'
```

## <a name="next-steps"></a>Nächste Schritte

* [Referenzdokumente zur Kafka-REST-Proxy-API](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/)
