---
title: Netzwerkisolation
description: Benutzer können den öffentlichen Zugriff auf QnA Maker-Ressourcen einschränken.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: c2fad19bd84418d41aca1b2e0770eaa3cde488b0
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105043358"
---
# <a name="recommended-settings-for-network-isolation"></a>Empfohlene Einstellungen für die Netzwerkisolation

Gehen Sie wie hier beschrieben vor, um den öffentlichen Zugriff auf QnA Maker-Ressourcen einzuschränken. Schützen Sie eine Cognitive Services-Ressource durch [Konfigurieren des virtuellen Netzwerks](../../cognitive-services-virtual-networks.md?tabs=portal) vor öffentlichem Zugriff.

## <a name="restrict-access-to-app-service-qna-runtime"></a>Einschränken des Zugriffs auf App Service (QnA-Runtime)

Sie können der App Service-Zulassungsliste IP-Adressen hinzufügen, um den Zugriff einzuschränken, oder die App Service-Umgebung zum Hosten von QnA Maker App Service konfigurieren.

#### <a name="add-ips-to-app-service-allow-list"></a>Hinzufügen von IP-Adressen zur Zulassungsliste von App Service

1. Lassen Sie nur Datenverkehr von Cognitive Services-IP-Adressen zu. Diese sind bereits im Diensttag `CognitiveServicesManagement` enthalten. Dies ist für die Erstellung von APIs (KB erstellen/aktualisieren) erforderlich, um die App Service-Instanz aufzurufen und den Azure Search-Dienst entsprechend zu aktualisieren. Lesen Sie die [weiteren Informationen zu Diensttags](../../../virtual-network/service-tags-overview.md).
2. Lassen Sie außerdem auch andere Einstiegspunkte (beispielsweise Azure Bot Service oder das QnA Maker-Portal) für den Zugriff auf die Vorhersage-API „GenerateAnswer“ zu.
3. Führen Sie die folgenden Schritte aus, um die IP-Adressbereiche in einer Zulassungsliste hinzuzufügen:

   1. Laden Sie [IP-Adressbereiche für alle Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) herunter.
   2. Wählen Sie die IPs von „CognitiveServicesManagement“ aus.
   3. Navigieren Sie zum Abschnitt „Netzwerk“ Ihrer App Service-Ressource, und klicken Sie auf die Option „Zugriffseinschränkung konfigurieren“, um die IP-Adressen in einer Zulassungsliste hinzuzufügen.

Wir haben außerdem ein automatisiertes Skript, um dasselbe für Ihren App Service zu erledigen. Das [PowerShell-Skript zum Konfigurieren einer Zulassungsliste](https://github.com/pchoudhari/QnAMakerBackupRestore/blob/master/AddRestrictedIPAzureAppService.ps1) finden Sie auf GitHub. Sie müssen die Abonnement-ID, die Ressourcengruppe und den tatsächlichen App Service-Namen als Skriptparameter eingeben. Beim Ausführen des Skripts werden die IP-Adressen automatisch der Zulassungsliste von App Service hinzugefügt.

#### <a name="configure-app-service-environment-to-host-qna-maker-app-service"></a>Konfigurieren der App Service-Umgebung zum Hosten von QnA Maker App Service
    
Die App Service-Umgebung kann verwendet werden, um QnA Maker App Service zu hosten. Führen Sie die folgenden Schritte aus:

1. Erstellen Sie eine App Service-Umgebung, und kennzeichnen Sie sie als „extern“. Eine Anleitung finden Sie in [diesem Tutorial](../../../app-service/environment/create-external-ase.md).
2.  Erstellen Sie eine App Service-Instanz innerhalb der App Service-Umgebung.
    1. Überprüfen Sie die Konfiguration für die App Service-Instanz, und fügen Sie 'PrimaryEndpointKey' als Anwendungseinstellung hinzu. Der Wert für 'PrimaryEndpointKey' sollte auf „\<app-name\>-PrimaryEndpointKey“ festgelegt werden. Der App-Name wird in der App Service-URL definiert. Wenn die App Service-URL beispielsweise „mywebsite.myase.p.azurewebsite.net“ lautet, ist „mywebsite“ der App-Name. In diesem Fall sollte der Wert für 'PrimaryEndpointKey' auf „mywebsite-PrimaryEndpointKey“ festgelegt werden.
    2. Erstellen Sie eine Instanz des Azure Search-Diensts.
    3. Vergewissern Sie sich, dass die Azure Search- und App-Einstellungen ordnungsgemäß konfiguriert sind. 
          Befolgen Sie die Schritte in [diesem Tutorial](../reference-app-service.md?tabs=v1#app-service).
3.  Aktualisieren Sie die der App Service-Umgebung zugeordneten Netzwerksicherheitsgruppe.
    1. Aktualisieren Sie die vorab erstellten Sicherheitsregeln für eingehenden Datenverkehr entsprechend Ihrer Anforderungen.
    2. Fügen Sie eine neue Sicherheitsregel für eingehenden Datenverkehr mit 'Service Tag' als Quelle und 'CognitiveServicesManagement' als Diensttag für die Quelle hinzu.
       
    ![Ausnahmen für eingehende Ports](../media/inbound-ports.png)

4.  Erstellen Sie mit Azure Resource Manager eine Cognitive Services-Instanz für QnA Maker (Microsoft.CognitiveServices/accounts), und legen Sie den QnA Maker-Endpunkt hierfür auf den oben erstellten App Service-Endpunkt (https://mywebsite.myase.p.azurewebsite.net) fest.
    
---

## <a name="restrict-access-to-cognitive-search-resource"></a>Einschränken des Zugriffs auf Cognitive Search-Ressourcen

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/v1)

Die Cognitive Search-Instanz kann über einen privaten Endpunkt isoliert werden, nachdem die QnA Maker-Ressourcen erstellt wurden. Bei Verbindungen mit privaten Endpunkten ist ein VNet erforderlich, über das auf die Suchdienstinstanz zugegriffen werden kann. 

Wenn QnA Maker App Service über eine App Service-Umgebung eingeschränkt ist, verwenden Sie das gleiche VNet, um eine Verbindung mit privatem Endpunkt mit der Cognitive Search-Instanz herzustellen. Erstellen Sie einen neuen DNS-Eintrag im VNet, um den Cognitive Search-Endpunkt der IP-Adresse des privaten Cognitive Search-Endpunkts zuzuordnen. 

Wenn eine App Service-Umgebung nicht für QnA Maker App Service verwendet wird, erstellen Sie zunächst eine neue VNet-Ressource und dann die Verbindung des privaten Endpunkts mit der Cognitive Search-Instanz. In diesem Fall muss QnA Maker App Service in [das VNet integriert werden](../../../app-service/web-sites-integrate-with-vnet.md), um eine Verbindung mit der Cognitive Search-Instanz herzustellen. 

#  <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/v2)

[Erstellen Sie private Endpunkte](../reference-private-endpoint.md) für die Azure Search-Ressource.

---