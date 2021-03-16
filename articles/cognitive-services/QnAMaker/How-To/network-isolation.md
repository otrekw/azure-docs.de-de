---
title: Netzwerkisolation
description: Benutzer können den öffentlichen Zugriff auf QnA Maker-Ressourcen einschränken.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 8fe8c07866b23e5d990b71bfc9cd556c338634d3
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102203367"
---
# <a name="recommended-settings-for-network-isolation"></a>Empfohlene Einstellungen für die Netzwerkisolation

Gehen Sie wie hier beschrieben vor, um den öffentlichen Zugriff auf QnA Maker-Ressourcen einzuschränken. Schützen Sie eine Cognitive Services-Ressource durch [Konfigurieren des virtuellen Netzwerks](../../cognitive-services-virtual-networks.md?tabs=portal) vor öffentlichem Zugriff.

## <a name="restrict-access-to-cognitive-search-resource"></a>Einschränken des Zugriffs auf Cognitive Search-Ressourcen

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker, allgemeine Verfügbarkeit (stabile Version)](#tab/v1)

Konfigurieren von Cognitive Search als privater Endpunkt innerhalb eines VNET Wenn eine Suchinstanz während der Erstellung einer QnA Maker-Ressource erstellt wird, können Sie Cognitive Search zwingen, eine private Endpunktkonfiguration zu unterstützen, die vollständig innerhalb des virtuellen Netzwerks (VNet) eines Kunden erstellt wurde.

Alle Ressourcen müssen in derselben Region erstellt werden, um einen privaten Endpunkt nutzen zu können.

* QnA Maker-Ressource
* Neue Cognitive Search-Ressource
* Neue Virtual Network-Ressource

Führen Sie die folgenden Schritte im [Azure-Portal](https://portal.azure.com) aus:

1. Erstellen Sie eine [QnA Maker-Ressource](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).
2. Erstellen Sie eine neue Cognitive Search-Ressource mit Endpunktkonnektivität (Daten), die auf _Privat_ festgelegt ist. Erstellen Sie die Ressource in derselben Region wie die in Schritt 1 erstellte QnA Maker-Ressource. Erfahren Sie mehr über die [Erstellung einer Ressource für Cognitive Search](../../../search/search-create-service-portal.md), und wechseln Sie dann über diesen Link direkt zur [Erstellungsseite der Ressource](https://ms.portal.azure.com/#create/Microsoft.Search).
3. Erstellen Sie eine neue [Virtual Network-Ressource](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM).
4. Konfigurieren Sie das VNET auf der in Schritt 1 dieses Verfahrens erstellten App-Dienstressource. Erstellen Sie einen neuen DNS-Eintrag im VNET für die in Schritt 2 erstellte neue Cognitive Search-Ressource. für die IP-Adresse von Cognitive Search.
5. [Ordnen Sie den App-Dienst](../how-to/set-up-qnamaker-service-azure.md) mit der in Schritt 2 erstellten neuen Cognitive Search-Ressource zu. Dann können Sie die in Schritt 1 erstellte ursprüngliche Cognitive Search-Ressource löschen.
    
Erstellen Sie Ihre erste Wissensdatenbank im [QnA Maker-Portal](https://www.qnamaker.ai/).

#  <a name="qna-maker-managed-preview-release"></a>[QnA Maker verwaltet (Vorschauversion)](#tab/v2)

[Erstellen Sie private Endpunkte](../reference-private-endpoint.md) für die Azure Search-Ressource.

---

## <a name="restrict-access-to-app-service-qna-runtime"></a>Einschränken des Zugriffs auf App Service (QnA-Runtime)

Sie können der App Service-Zulassungsliste IP-Adressen hinzufügen, um den Zugriff einzuschränken, oder die App Service-Umgebung zum Hosten von QnA Maker App Service konfigurieren.

#### <a name="add-ips-to-app-service-allowlist"></a>Hinzufügen von IP-Adressen zur Positivliste von App Service

1. Lassen Sie nur Datenverkehr von Cognitive Services-IP-Adressen zu. Diese sind bereits im Diensttag `CognitiveServicesManagement` enthalten. Dies ist für die Erstellung von APIs (KB erstellen/aktualisieren) erforderlich, um die App Service-Instanz aufzurufen und den Azure Search-Dienst entsprechend zu aktualisieren. Lesen Sie die [weiteren Informationen zu Diensttags](../../../virtual-network/service-tags-overview.md).
2. Lassen Sie außerdem auch andere Einstiegspunkte (beispielsweise Azure Bot Service oder das QnA Maker-Portal) für den Zugriff auf die Vorhersage-API „GenerateAnswer“ zu.
3. Führen Sie die folgenden Schritte aus, um die IP-Adressbereiche einer Zulassungsliste hinzuzufügen:

   1. Laden Sie [IP-Adressbereiche für alle Diensttags](https://www.microsoft.com/download/details.aspx?id=56519) herunter.
   2. Wählen Sie die IPs von „CognitiveServicesManagement“ aus.
   3. Navigieren Sie zum Abschnitt „Netzwerk“ Ihrer App Service-Ressource, und klicken Sie auf die Option „Zugriffseinschränkung konfigurieren“, um die IP-Adressen einer Zulassungsliste hinzuzufügen.

    ![Ausnahmen für eingehende Ports](../media/inbound-ports.png)

Wir haben außerdem ein automatisiertes Skript, um dasselbe für Ihren App Service zu erledigen. Sie finden das [PowerShell-Skript, um eine Zulassungsliste zu konfigurieren](https://github.com/pchoudhari/QnAMakerBackupRestore/blob/master/AddRestrictedIPAzureAppService.ps1), auf GitHub. Sie müssen die Abonnement-ID, die Ressourcengruppe und den tatsächlichen App Service-Namen als Skriptparameter eingeben. Beim Ausführen des Skripts werden die IP-Adressen automatisch der Zulassungsliste von App Service hinzugefügt.

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
4.  Erstellen Sie mit Azure Resource Manager eine Cognitive Services-Instanz für QnA Maker (Microsoft.CognitiveServices/accounts), und legen Sie den QnA Maker-Endpunkt hierfür auf den oben erstellten App Service-Endpunkt (https://mywebsite.myase.p.azurewebsite.net) fest.
    
---
