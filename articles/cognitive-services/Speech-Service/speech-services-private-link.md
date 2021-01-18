---
title: Verwenden privater Endpunkte mit dem Speech-Dienst
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Sie den Speech-Dienst mit privaten Endpunkten verwenden, die durch einen Azure Private Link bereitgestellt werden
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: alexeyo
ms.openlocfilehash: d5822b6eeecfc61a5092519618ddfcaf88a625ae
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018529"
---
# <a name="use-speech-service-through-a-private-endpoint"></a>Verwenden des Speech-Diensts mithilfe eines privaten Endpunkts

Mithilfe von [Azure Private Link](../../private-link/private-link-overview.md) können Sie Verbindungen mit Diensten in Azure über einen [privaten Endpunkt](../../private-link/private-endpoint-overview.md) herstellen.
Ein privater Endpunkt ist eine private IP-Adresse, die nur in einem bestimmten [virtuellen Netzwerk](../../virtual-network/virtual-networks-overview.md) und Subnetz zugänglich ist.

In diesem Artikel wird erläutert, wie Sie Private Link und private Endpunkte mit Azure Cognitive Speech Services einrichten und verwenden.

> [!NOTE]
> In diesem Artikel werden die Besonderheiten bei der Einrichtung und Verwendung von Private Link mit Azure Cognitive Speech Services erläutert. Bevor Sie fortfahren, informieren Sie sich über das [Verwenden von virtuellen Netzwerken mit Cognitive Services](../cognitive-services-virtual-networks.md).

Führen Sie die folgenden Aufgaben aus, um einen Speech-Dienst über einen privaten Endpunkt zu verwenden:

1. [Erstellen eines benutzerdefinierten Domänennamens für die Speech-Ressource](#create-a-custom-domain-name)
2. [Erstellen und Konfigurieren privater Endpunkte](#enable-private-endpoints)
3. [Anpassen vorhandener Anwendungen und Lösungen](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled)

Wenn Sie private Endpunkte zu einem späteren Zeitpunkt entfernen, die Speech-Ressource jedoch trotzdem weiterhin verwenden möchten, führen Sie die Aufgaben in [diesem Abschnitt](#use-speech-resource-with-custom-domain-name-without-private-endpoints) aus.

## <a name="create-a-custom-domain-name"></a>Erstellen eines benutzerdefinierten Domänennamens

Für private Endpunkte ist ein [benutzerdefinierter Unterdomänenname von Cognitive Services](../cognitive-services-custom-subdomains.md) erforderlich. Befolgen Sie die Anweisungen unten, um einen für Ihre Speech-Ressource zu erstellen.

> [!WARNING]
> Eine Speech-Ressource mit aktiviertem benutzerdefiniertem Domänennamen interagiert auf andere Weise mit Speech Services.
> Wahrscheinlich müssen Sie Ihren Anwendungscode sowohl für Szenarien [mit aktivierten privaten Endpunkten](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) als auch [**ohne** aktivierte private Endpunkte](#use-speech-resource-with-custom-domain-name-without-private-endpoints) anpassen.
>
> Wenn Sie einen benutzerdefinierten Domänennamen aktivieren, wird der Vorgang [**unumkehrbar**](../cognitive-services-custom-subdomains.md#can-i-change-a-custom-domain-name). Sie können den [regionalen Namen](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) nur wiederherstellen, indem Sie eine neue Speech-Ressource erstellen.
>
> Wenn Ihrer Speech-Ressource viele benutzerdefinierte Modelle und Projekte zugeordnet sind, die über [Speech Studio](https://speech.microsoft.com/) erstellt wurden, wird **dringend** empfohlen, die Konfiguration mit einer Testressource auszuprobieren und Produktionsressourcen erst im Anschluss zu bearbeiten.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Führen Sie die folgenden Schritte aus, um einen benutzerdefinierten Domänennamen im Azure-Portal zu erstellen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com/), und melden Sie sich bei Ihrem Azure-Konto an.
1. Wählen Sie die erforderliche Speech-Ressource aus.
1. Klicken Sie im linken Navigationsbereich in der Gruppe **Ressourcenverwaltung** auf **Netzwerk**.
1. Klicken Sie auf der Registerkarte **Firewalls und virtuelle Netzwerke** auf **Benutzerdefinierten Domänennamen generieren**. Dadurch wird rechts ein Panel mit Anweisungen zum Erstellen einer eindeutigen benutzerdefinierten Unterdomäne für Ihre Ressource geöffnet.
1. Geben Sie im Bereich „Benutzerdefinierten Domänennamen generieren“ einen benutzerdefinierten Domänennamensteil ein. Ihre vollständige benutzerdefinierte Domäne sieht wie `https://{your custom name}.cognitiveservices.azure.com` aus. 
    **Nachdem Sie einen benutzerdefinierten Domänennamen erstellt haben, kann dieser _nicht mehr geändert werden_! Lesen Sie die Warnung oben noch einmal.** Nachdem Sie Ihren benutzerdefinierten Domänennamen eingegeben haben, klicken Sie auf **Speichern**.
1. Nachdem der Vorgang abgeschlossen ist, klicken Sie in der Gruppe **Ressourcenverwaltung** auf **Schlüssel und Endpunkt**. Vergewissern Sie sich, dass der neue Endpunktname Ihrer Ressource so beginnt:

    `https://{your custom name}.cognitiveservices.azure.com`

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Um einen benutzerdefinierten Domänennamen mithilfe von PowerShell zu erstellen, vergewissern Sie sich, dass auf Ihrem Computer PowerShell, Version 7.x oder höher, mit dem Azure PowerShell Modul, Version 5.1.0 oder höher, verfügbar ist. Führen Sie die folgenden Schritte aus, um die Versionen dieser Tools anzuzeigen:

1. Geben Sie in einem PowerShell-Fenster Folgendes ein:

    `$PSVersionTable`

    Vergewissern Sie sich, dass der Wert von PSVersion größer als 7.x ist. Zum Ausführen eines Upgrades von PowerShell befolgen Sie die Anweisungen unter [Installieren verschiedener Versionen von PowerShell](/powershell/scripting/install/installing-powershell).

1. Geben Sie in einem PowerShell-Fenster Folgendes ein:

    `Get-Module -ListAvailable Az`

    Wenn nichts angezeigt wird oder die Version des Azure PowerShell-Moduls niedriger als 5.1.0 ist, befolgen Sie die Anweisungen unter [installieren des Azure PowerShell-Moduls](/powershell/azure/install-Az-ps), um das Upgrade durchzuführen.

Bevor Sie fortfahren, führen Sie `Connect-AzAccount` aus, um eine Verbindung mit Azure herzustellen.

## <a name="verify-custom-domain-name-is-available"></a>Vergewissern Sie sich, dass der benutzerdefinierte Domänenname verfügbar ist

Überprüfen Sie, ob die benutzerdefinierte Domäne, die Sie verwenden möchten, verfügbar ist. Führen Sie mithilfe des Vorgangs [Check Domain Availability](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) in der Cognitive Services REST-API diese Schritte aus, um zu bestätigen, dass die Domäne verfügbar ist.

> [!TIP]
> Der folgende Code funktioniert **nicht** in Azure Cloud Shell.

```azurepowershell
$subId = "Your Azure subscription Id"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
Set-AzContext -SubscriptionId $subId

# Prepare OAuth token to use in request to Cognitive Services REST API.
$Context = Get-AzContext
$AccessToken = (Get-AzAccessToken -TenantId $Context.Tenant.Id).Token
$token = ConvertTo-SecureString -String $AccessToken -AsPlainText -Force

# Prepare and send the request to Cognitive Services REST API.
$uri = "https://management.azure.com/subscriptions/" + $subId + `
    "/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18"
$body = @{
subdomainName = $subdomainName
type = "Microsoft.CognitiveServices/accounts"
}
$jsonBody = $body | ConvertTo-Json
Invoke-RestMethod -Method Post -Uri $uri -ContentType "application/json" -Authentication Bearer `
    -Token $token -Body $jsonBody | Format-List
```
Wenn der gewünschte Name verfügbar ist, sehen Sie eine Antwort wie diese:
```azurepowershell
isSubdomainAvailable : True
reason               :
type                 :
subdomainName        : my-custom-name
```
Wenn der Name bereits verwendet wird, erhalten Sie die folgende Antwort:
```azurepowershell
isSubdomainAvailable : False
reason               : Sub domain name 'my-custom-name' is already used. Please pick a different name.
type                 :
subdomainName        : my-custom-name
```
## <a name="create-your-custom-domain-name"></a>Erstellen Ihres benutzerdefinierten Domänennamens

Verwenden Sie das Cmdlet [Set-AzCognitiveServicesAccount](/powershell/module/az.cognitiveservices/set-azcognitiveservicesaccount), um den benutzerdefinierten Domänennamen für die ausgewählte Speech-Ressource zu aktivieren.

> [!WARNING]
> Nach erfolgreicher Ausführung des unten abgebildeten Codes können Sie einen benutzerdefinierten Domänennamen für Ihre Speech-Ressource erstellen.
> Dieser Name kann **nicht** geändert werden. Weitere Informationen hierzu finden Sie in der obigen **Warnung**.

```azurepowershell
$resourceGroup = "Resource group name where Speech resource is located"
$speechResourceName = "Your Speech resource name"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
$subId = "Your Azure subscription Id"
Set-AzContext -SubscriptionId $subId

# Set the custom domain name to the selected resource.
# WARNING: THIS CANNOT BE CHANGED OR UNDONE!
Set-AzCognitiveServicesAccount -ResourceGroupName $resourceGroup `
    -Name $speechResourceName -CustomSubdomainName $subdomainName
```

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Für diesen Abschnitt ist die aktuelle Version der Azure CLI erforderlich. Bei Verwendung von Azure Cloud Shell ist die aktuelle Version bereits installiert.

## <a name="verify-the-custom-domain-name-is-available"></a>Vergewissern Sie sich, dass der benutzerdefinierte Domänenname verfügbar ist

Überprüfen Sie, ob die benutzerdefinierte Domäne, die Sie verwenden möchten, verfügbar ist. Verwenden Sie hierfür die [checkDomainAvailability](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability)-Methode der Cognitive Services-REST-API.

Kopieren Sie den folgenden Codeblock, fügen Sie Ihren bevorzugten benutzerdefinierten Domänennamen ein, und speichern Sie ihn in der Datei `subdomain.json`.

```json
{
    "subdomainName": "custom domain name",
    "type": "Microsoft.CognitiveServices/accounts"
}
```

Kopieren Sie die Datei in den aktuellen Ordner, oder laden Sie sie in Azure Cloud Shell hoch, und führen Sie den folgenden Befehl aus. Ersetzen Sie `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` durch Ihre Azure-Abonnement-ID.

```azurecli-interactive
az rest --method post --url "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18" --body @subdomain.json
```
Wenn der gewünschte Name verfügbar ist, sehen Sie eine Antwort wie diese:
```azurecli
{
  "isSubdomainAvailable": true,
  "reason": null,
  "subdomainName": "my-custom-name",
  "type": null
}
```

Wenn der Name bereits verwendet wird, erhalten Sie die folgende Antwort:
```azurecli
{
  "isSubdomainAvailable": false,
  "reason": "Sub domain name 'my-custom-name' is already used. Please pick a different name.",
  "subdomainName": "my-custom-name",
  "type": null
}
```
## <a name="enable-custom-domain-name"></a>Aktivieren eines benutzerdefinierten Domänennamens

Verwenden Sie den Befehl [az cognitiveservices account update](/cli/azure/cognitiveservices/account#az_cognitiveservices_account_update), um den benutzerdefinierten Domänennamen für die ausgewählte Speech-Ressource zu aktivieren.

Wählen Sie das Azure-Abonnement mit der Speech-Ressource aus. Wenn nur ein aktives Abonnement in Ihrem Azure-Konto vorhanden ist, können Sie diesen Schritt überspringen. Ersetzen Sie `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` durch Ihre Azure-Abonnement-ID.
```azurecli-interactive
az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
Legen Sie den benutzerdefinierten Domänennamen auf die ausgewählte Ressource fest. Ersetzen Sie die Beispielparameterwerte durch die tatsächlichen Parameterwerte, und führen Sie den folgenden Befehl aus.

> [!WARNING]
> Nach erfolgreicher Ausführung des folgenden Befehls können Sie einen benutzerdefinierten Domänennamen für Ihre Speech-Ressource erstellen. Dieser Name kann **nicht** geändert werden. Weitere Informationen finden Sie in der Mahnung zur Vorsicht oben.

```azurecli
az cognitiveservices account update --name my-speech-resource-name --resource-group my-resource-group-name --custom-domain my-custom-name
```

**_

## <a name="enable-private-endpoints"></a>Aktivieren privater Endpunkte

Es wird empfohlen, die an das virtuelle Netzwerk mit den erforderlichen Updates für die privaten Endpunkte angefügte [private DNS-Zone](../../dns/private-dns-overview.md) zu verwenden. Diese wird beim Bereitstellungsprozess standardmäßig erstellt. Wenn Sie jedoch Ihren eigenen DNS-Server verwenden, müssen Sie möglicherweise auch Ihre DNS-Konfiguration ändern. Dies wird weiter unten in diesem Artikel unter _DNS für private Endpunkte_ erläutert. Entscheiden Sie sich für eine DNS-Strategie, _ *bevor** Sie private Endpunkte für eine Speech-Produktionsressource bereitstellen, und testen Sie die DNS-Änderungen, insbesondere dann, wenn Sie Ihren eigenen DNS-Server verwenden.

Erstellen Sie mithilfe eines der folgenden Artikel einen oder mehrere private Endpunkte. In diesen Artikeln wird eine Web-App als Beispielressource verwendet, die mit privaten Endpunkten aktiviert werden soll. Verwenden Sie anstelle der Parameter im jeweiligen Artikel die folgenden:

| Einstellung             | Wert                                    |
|---------------------|------------------------------------------|
| Ressourcentyp       | **Microsoft.CognitiveServices/accounts** |
| Resource            | **\<your-speech-resource-name>**         |
| Zielunterressource | **Konto**                              |

- [Schnellstart: Erstellen eines privaten Endpunkts mit dem Azure-Portal](../../private-link/create-private-endpoint-portal.md)
- [Erstellen eines privaten Endpunkts mit Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)
- [Erstellen eines privaten Endpunkts mit der Azure CLI](../../private-link/create-private-endpoint-cli.md)

**DNS für private Endpunkte:** Sehen Sie sich die Grundprinzipien für [DNS für private Endpunkte in Cognitive Services-Ressourcen](../cognitive-services-virtual-networks.md#dns-changes-for-private-endpoints) an. Stellen Sie dann mithilfe der folgenden Überprüfungen sicher, dass Ihre DNS-Konfiguration ordnungsgemäß funktioniert:

### <a name="resolve-dns-from-the-virtual-network"></a>DNS-Auflösung über das virtuelle Netzwerk

Diese Überprüfung ist **erforderlich**.

Führen Sie die folgenden Schritte aus, um den benutzerdefinierten DNS-Eintrag über Ihr virtuelles Netzwerk zu testen.

1. Melden Sie sich bei einem virtuellen Computer an, der sich in dem virtuellen Netzwerk befindet, an das Sie Ihren privaten Endpunkt angefügt haben. 
1. Öffnen Sie die Windows-Eingabeaufforderung oder die Bash-Shell, führen Sie `nslookup` aus, und vergewissern Sie sich, dass der benutzerdefinierte Domänenname Ihrer Ressource erfolgreich aufgelöst wird.

```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  168.63.129.16

Non-authoritative answer:
Name:    my-private-link-speech.privatelink.cognitiveservices.azure.com
Address:  172.28.0.10
Aliases:  my-private-link-speech.cognitiveservices.azure.com
```

3. Überprüfen Sie, ob die IP-Adresse mit der IP-Adresse Ihres privaten Endpunkts übereinstimmt.

### <a name="resolve-dns-from-other-networks"></a>DNS-Auflösung über andere Netzwerke

Führen Sie diese Überprüfung nur durch, wenn Sie die Speech-Ressource, für die private Endpunkte aktiviert wurden, im Hybridmodus verwenden möchten und im Abschnitt **Netzwerke** Ihrer Ressource entweder die Zugriffsoption **Alle Netzwerke** oder die Zugriffsoption **Selected Networks and Private Endpoints** (Ausgewählte Netzwerke und private Endpunkte) aktiviert ist. Wenn Sie nur über private Endpunkte auf die Ressource zugreifen, können Sie diesen Abschnitt überspringen.

1. Melden Sie sich bei einem Computer an, der an ein Netzwerk angefügt ist, das auf die Ressource zugreifen kann.
2. Öffnen Sie die Windows-Eingabeaufforderung oder die Bash-Shell, führen Sie `nslookup` aus, und vergewissern Sie sich, dass der benutzerdefinierte Domänenname Ihrer Ressource erfolgreich aufgelöst wird.

```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    vnetproxyv1-weu-prod.westeurope.cloudapp.azure.com
Address:  13.69.67.71
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          my-private-link-speech.privatelink.cognitiveservices.azure.com
          westeurope.prod.vnet.cog.trafficmanager.net
```

3. Überprüfen Sie, ob die IP-Adresse mit der IP-Adresse Ihres privaten Endpunkts übereinstimmt.

> [!NOTE]
> Die aufgelöste IP-Adresse verweist auf einen Proxyendpunkt eines virtuellen Netzwerks, der den Netzwerkdatenverkehr an den privaten Endpunkt für die Cognitive Services-Ressource sendet. Das Verhalten unterscheidet sich bei einer Ressource mit einem benutzerdefinierter Domänennamen aber *ohne* private Endpunkte. Details dazu finden Sie in [diesem Abschnitt](#dns-configuration).

## <a name="adjust-existing-applications-and-solutions"></a>Anpassen vorhandener Anwendungen und Lösungen

Eine Speech-Ressource mit aktivierter benutzerdefinierten Domäne interagiert auf andere Weise mit Speech Services. Das gilt für Speech-Ressourcen mit aktivierter benutzerdefinierter Domäne mit und ohne private Endpunkte. Die Informationen in diesem Abschnitt beziehen sich auf beide Szenarios.

### <a name="use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen und aktivierten privaten Endpunkten

Eine Speech-Ressource, für die benutzerdefinierte Domänennamen und private Endpunkte aktiviert sind, interagiert auf andere Weise mit Speech Services. In diesem Abschnitt wird erläutert, wie eine solche Ressource mit Speech Services-REST-API und dem [Speech-SDK](speech-sdk.md) verwendet wird.

> [!NOTE]
> Beachten Sie, dass eine Speech-Ressource ohne private Endpunkte, aber mit einem **benutzerdefinierten Domänennamen** auf andere Weise als eine Speech-Ressource mit aktivierten privaten Endpunkten mit Speech Services interagiert. Wenn Sie eine solche Ressource besitzen (also falls Sie beispielsweise eine Ressource mit privaten Endpunkten besaßen, diese aber entfernt haben), sollten Sie den [Abschnitt zu diesem Thema](#use-speech-resource-with-custom-domain-name-without-private-endpoints) lesen.

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-rest-api"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen und privaten Endpunkten mit der REST-API

In diesem Abschnitt wird `my-private-link-speech.cognitiveservices.azure.com` als DNS-Beispielname (benutzerdefinierte Domäne) für die Speech-Ressource verwendet.

##### <a name="note-on-speech-services-rest-api"></a>Anmerkung zur Speech Services-REST-API

Speech Services beinhaltet eine REST-API für die [Spracherkennung](rest-speech-to-text.md) und die [Sprachsynthese](rest-text-to-speech.md). Bei Szenarios mit aktivierten privaten Endpunkten muss Folgendes berücksichtigt werden:

Für die Spracherkennung gibt es zwei unterschiedliche REST-APIs. Jede API dient einem anderen Zweck, verwendet unterschiedliche Endpunkte und erfordert einen anderen Ansatz, wenn sie in einem Szenario mit aktivierten privaten Endpunkten verwendet wird.

Es gibt folgende REST-APIs für die Spracherkennung:
- Die [Spracherkennungs-REST-API 3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30) wird für die [Batchtranskription](batch-transcription.md) und [Custom Speech](custom-speech-overview.md) eingesetzt. Version 3.0 ist die [Nachfolgeversion von Version 2.0](/azure/cognitive-services/speech-service/migrate-v2-to-v3).
- Die [Spracherkennungs-REST-API für kurze Audiodaten](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) wird für die Onlinetranskription eingesetzt. 

Die Verwendung der Spracherkennungs-REST-API für kurze Audiodaten und der Sprachsynthese-REST-API ist in Szenarios mit privaten Endpunkten identisch und entspricht dem [Speech-SDK-Ansatz](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk), der im Verlauf dieses Artikels erläutert wird. 

Die Spracherkennungs-REST-API 3.0 verwendet andere Endpunkte und erfordert daher einen anderen Ansatz für Szenarios mit aktivierten privaten Endpunkten.

Beide Fälle werden in den nächsten Unterabschnitten beschrieben.


##### <a name="speech-to-text-rest-api-v30"></a>Spracherkennungs-REST-API 3.0

Speech-Ressourcen verwenden üblicherweise [regionale Cognitive Services-Endpunkte](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) für die Kommunikation mit der [Spracherkennungs-REST-API 3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30). Für diese Ressourcen gilt das folgende Benennungsformat: <p/>`{region}.api.cognitive.microsoft.com`

Hier sehen Sie ein Beispiel für eine Anforderungs-URL:

```http
https://westeurope.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions
```
Nachdem Sie benutzerdefinierte Domänen für eine Speech-Ressource aktiviert haben (für private Endpunkte erforderlich), verwendet diese Ressource das folgende DNS-Namensmuster für den grundlegenden REST-API-Endpunkt: <p/>`{your custom name}.cognitiveservices.azure.com`

In diesem Beispiel lautet der Name des REST-API-Endpunkts dann: <p/>`my-private-link-speech.cognitiveservices.azure.com`

Die Anforderungs-URL aus dem Beispiel muss in folgendes Format konvertiert werden:
```http
https://my-private-link-speech.cognitiveservices.azure.com/speechtotext/v3.0/transcriptions
```
Diese URL sollte (bei [korrekter DNS-Auflösung](#resolve-dns-from-the virtual-network)) von dem virtuellen Netzwerk aus erreichbar sein, dem der private Endpunkt angefügt ist.

Nach dem Aktivieren des benutzerdefinierten Domänennamens für eine Speech-Ressource ersetzen Sie in der Regel den Hostnamen in allen Anforderungs-URLs durch den neuen Hostnamen der benutzerdefinierten Domäne. Die restlichen Bestandteile der Anforderung (wie der Pfad `/speechtotext/v3.0/transcriptions` im obigen Beispiel) bleiben unverändert.

> [!TIP]
> Einige Kunden haben Anwendungen entwickelt, die die Region des DNS-Namens des regionalen Endpunkts verwenden, um beispielsweise eine Anforderung an die Speech-Ressource zu senden, die in der jeweiligen Azure-Region bereitgestellt wurde.
>
> Der benutzerdefinierte Domänenname von Speech-Ressourcen enthält **keine** Informationen zu der Region, in der die Ressource bereitgestellt wird. Die oben beschriebene Anwendungslogik funktioniert also **nicht** und muss abgeändert werden.

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>Spracherkennungs-REST-API für kurze Audiodaten und Sprachsynthese-REST-API

Die [Spracherkennungs-REST-API für kurze Audiodaten](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) und die [Sprachsynthese-REST-API](rest-text-to-speech.md) verwenden zwei Endpunktarten:
- [Regionale Cognitive Services-Endpunkte](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) für die Kommunikation mit der Cognitive Services-REST-API, um ein Autorisierungstoken abzurufen
- Spezielle Endpunkte für alle sonstigen Vorgänge

Eine ausführliche Beschreibung dieser speziellen Endpunkte und Informationen zur Transformation der URL für eine Speech-Ressource mit aktivierten privaten Endpunkten finden Sie in [diesem Abschnitt](#general-principle) unter „Verwendung mit dem Speech-SDK“. Das Prinzip für das SDK gilt auch für die Spracherkennungs-API 1.0 und die Sprachsynthese-REST-API.

Lesen Sie das Material im entsprechenden Unterabschnitt im vorherigen Absatz, und sehen Sie sich dann das folgende Beispiel an. Im Beispiel wird die Sprachsynthese-REST-API beschrieben. Die Verwendung der Spracherkennungs-REST-API für kurze Audiodaten ist jedoch identisch.

> [!NOTE]
> Wenn Sie die **Spracherkennungs-REST-API für kurze Audiodaten** in Szenarios mit privaten Endpunkten nutzen, verwenden Sie ein Autorisierungstoken, das [im](rest-speech-to-text.md#request-headers) `Authorization`-[Header](rest-speech-to-text.md#request-headers) übergeben wird. Das Übergeben des Speech-Abonnementschlüssels an den speziellen Endpunkt über den `Ocp-Apim-Subscription-Key`-Header funktioniert **nicht** und führt dazu, dass der Fehler 401 auftritt.

**Verwendungsbeispiel für die Sprachsynthese-REST-API**

In diesem Beispiel wird „Europa, Westen“ als Azure-Region und `my-private-link-speech.cognitiveservices.azure.com` als DNS-Name der Speech-Ressource (benutzerdefinierte Domäne) verwendet. Der benutzerdefinierte Domänenname `my-private-link-speech.cognitiveservices.azure.com` in diesem Beispiel gehört zu der Ressource, die in der Region „Europa, Westen“ erstellt wurde.

Die folgenden beiden Vorgänge sind erforderlich, um die Liste der unterstützten Stimmen für die Region abzurufen:

- Rufen Sie das Autorisierungstoken ab:
```http
https://westeurope.api.cognitive.microsoft.com/sts/v1.0/issuetoken
```
- Verwenden Sie das Token, um die Liste der Stimmen abzurufen:
```http
https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list
```
Eine ausführlichere Erläuterung der obigen Schritte finden Sie in der [Dokumentation zur Sprachsynthese-REST-API](rest-text-to-speech.md).

Für Speech-Ressourcen mit aktivierten privaten Endpunkten müssen die Endpunkt-URLs geändert werden, damit diese Abfolge funktioniert. Die Abfolge sollte folgendermaßen aussehen:
- Rufen Sie das Autorisierungstoken ab:
```http
https://my-private-link-speech.cognitiveservices.azure.com/v1.0/issuetoken
```
Eine ausführlichere Erläuterung finden Sie im Unterabschnitt [Spracherkennungs-REST-API 3.0](#speech-to-text-rest-api-v30).
- Verwenden Sie das Authentifizierungstoken, um die Liste der Stimmen abzurufen:
```http
https://my-private-link-speech.cognitiveservices.azure.com/tts/cognitiveservices/voices/list
```
Eine ausführlichere Erläuterung finden Sie im Abschnitt [Allgemeines Prinzip](#general-principle) unter „Verwendung mit dem Speech-SDK“.

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen und privaten Endpunkten Verwendung mit dem Speech-SDK

Für die Verwendung des Speech-SDK mit Speech-Ressourcen, für die benutzerdefinierte Domänennamen und private Endpunkte aktiviert sind, sind höchstwahrscheinlich ein Review und Änderungen an Ihrem Anwendungscode notwendig.

In diesem Abschnitt wird `my-private-link-speech.cognitiveservices.azure.com` als DNS-Beispielname (benutzerdefinierte Domäne) für die Speech-Ressource verwendet.

##### <a name="general-principle"></a>Allgemeines Prinzip

In SDK-Szenarios (und Szenarios mit der Sprachsynthese-REST-API) verwenden Speech-Ressourcen in der Regel die dedizierten regionalen Endpunkte für verschiedene Dienstangebote. Das DNS-Namensformat für diese Endpunkte lautet: </p>`{region}.{speech service offering}.speech.microsoft.com`

Beispiel: </p>`westeurope.stt.speech.microsoft.com`

Die möglichen Werte für die Region (erstes Element des DNS-Namens) werden [hier](regions.md) aufgeführt. Die folgende Tabelle enthält die möglichen Werte für das Speech Services-Angebot (zweites Element des DNS-Namens):

| DNS-Namenswert | Speech Services-Angebot                                    |
|----------------|-------------------------------------------------------------|
| `commands`     | [Benutzerdefinierte Befehle](custom-commands.md)                       |
| `convai`       | [Unterhaltungstranskription](conversation-transcription.md) |
| `s2s`          | [Sprachübersetzung](speech-translation.md)                 |
| `stt`          | [Spracherkennung](speech-to-text.md)                         |
| `tts`          | [Sprachsynthese](text-to-speech.md)                         |
| `voice`        | [Custom Voice](how-to-custom-voice.md)                      |

Das obige Beispiel (`westeurope.stt.speech.microsoft.com`) steht also für den Spracherkennungs-Endpunkt in der Region „Europa, Westen“.

Für private Endpunkte aktivierte Endpunkte kommunizieren mit Speech Services über einen speziellen Proxy. Daher **müssen Sie die Endpunktverbindungs-URLs ändern**. 

Eine URL für einen „Standardendpunkt“ sieht wie folgt aus: <p/>`{region}.{speech service offering}.speech.microsoft.com/{URL path}`

Eine URL für einen privaten Endpunkt sieht wie folgt aus: <p/>`{your custom name}.cognitiveservices.azure.com/{speech service offering}/{URL path}`

**1. Beispiel:** Die Anwendung kommuniziert mithilfe der folgenden URL (Spracherkennung mit Basismodell für Englisch (USA) in „Europa, Westen“):

```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Damit Sie die URL in einem Szenario mit aktivierten privaten Endpunkten verwenden können, in dem der benutzerdefinierte Domänenname der Speech-Ressource `my-private-link-speech.cognitiveservices.azure.com` lautet, müssen Sie die URL folgendermaßen anpassen:

```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Beachten Sie die folgenden Details:

- Der Hostname (`westeurope.stt.speech.microsoft.com`) wird durch den Hostnamen der benutzerdefinierten Domäne (`my-private-link-speech.cognitiveservices.azure.com`) ersetzt.
- Das zweite Element des ursprünglichen DNS-Namens (`stt`) wird zum ersten Element des URL-Pfads und steht vor dem ursprünglichen Pfad. Die ursprüngliche URL `/speech/recognition/conversation/cognitiveservices/v1?language=en-US` wird folglich zu `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

**2. Beispiel:** Die Anwendung verwendet die folgende URL für die Sprachsynthese mit einem Custom Voice-Modell in der Region „Europa, Westen“:
```http
https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Im Folgenden ist eine entsprechende URL mit einem privaten Endpunkt aufgeführt, bei der der benutzerdefinierte Domänenname der Speech-Ressource `my-private-link-speech.cognitiveservices.azure.com` ist:

```http
https://my-private-link-speech.cognitiveservices.azure.com/voice/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Es wird das gleiche Prinzip wie im ersten Beispiel angewendet, doch das zentrale Element ist dieses Mal `voice`.

##### <a name="modify-applications"></a>Ändern von Anwendungen

Führen Sie die folgenden Schritte aus, um Ihren Code zu ändern:

**1. Bestimmen Sie die Anwendungsendpunkt-URL.**

- [Aktivieren Sie die Protokollierung für Ihre Anwendung](how-to-use-logging.md), und führen Sie die Anwendung aus, um Aktivitäten zu protokollieren.
- Suchen Sie in der Protokolldatei nach `SPEECH-ConnectionUrl`. Der `value`-Parameter in den entsprechenden Zeilen enthält die vollständige URL, die Ihre Anwendung verwendet hat, um den Speech-Dienst zu erreichen.

Beispiel:

```
(114917): 41ms SPX_DBG_TRACE_VERBOSE:  property_bag_impl.cpp:138 ISpxPropertyBagImpl::LogPropertyAndValue: this=0x0000028FE4809D78; name='SPEECH-ConnectionUrl'; value='wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?traffictype=spx&language=en-US'
```

Die Anwendung hat in diesem Beispiel also die folgende URL verwendet:

```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

**2. Erstellen Sie mithilfe der vollständigen Endpunkt-URL eine `SpeechConfig`-Instanz.**

Ändern Sie den Endpunkt, den Sie im vorherigen Abschnitt festgelegt haben, wie unter [Allgemeines Prinzip](#general-principle) beschrieben.

Passen Sie nun die Erstellung der `SpeechConfig`-Instanz an. Ihre Anwendung enthält wahrscheinlich eine Zeile, die der folgenden ähnelt:
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Diese funktioniert für Speech-Ressourcen mit aktivierten privaten Endpunkten nicht, da der Hostname und die URL sich wie im vorherigen Abschnitt beschrieben ändern. Wenn Sie Ihre vorhandene Anwendung ohne Änderungen mit dem Schlüssel der Ressource mit aktivierten privaten Endpunkten verwenden, tritt der Authentifizierungsfehler 401 auf.

Damit dies funktioniert, müssen Sie die Instanziierung der `SpeechConfig`-Klasse ändern und die Initialisierung mit FromEndpoint oder WithEndpoint verwenden. Nehmen Sie an, dass die folgenden beiden Variablen definiert sind:
- `subscriptionKey` enthält den Schlüssel der Speech-Ressource, für die private Endpunkte aktiviert sind.
- `endPoint` enthält die vollständige, **geänderte** Endpunkt-URL (mit dem Typ, der in der entsprechenden Programmiersprache erforderlich ist). In diesem Beispiel sollte die Variable den folgenden Inhalt aufweisen:
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Erstellen Sie als Nächstes eine `SpeechConfig`-Instanz:
```csharp
var config = SpeechConfig.FromEndpoint(endPoint, subscriptionKey);
```
```cpp
auto config = SpeechConfig::FromEndpoint(endPoint, subscriptionKey);
```
```java
SpeechConfig config = SpeechConfig.fromEndpoint(endPoint, subscriptionKey);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(endpoint=endPoint, subscription=subscriptionKey)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithEndpoint:endPoint subscription:subscriptionKey];
```

> [!TIP]
> Die Abfrageparameter im Endpunkt-URI werden nicht geändert – auch dann nicht, wenn sie von anderen APIs festgelegt wurden. Wenn die Erkennungssprache z. B. im URI als Abfrageparameter „language=en-US“ definiert ist, über die entsprechende Eigenschaft aber auf „ru-RU“ festgelegt ist, wird die Spracheinstellung im URI verwendet. Die tatsächlich verwendete Sprache ist also „en-US“. Im Endpunkt-URI festgelegte Parameter haben immer Vorrang. Nur Parameter, die nicht im Endpunkt-URI angegeben sind, können von anderen APIs überschrieben werden.

Nachdem Sie diese Änderung vorgenommen haben, sollte Ihre Anwendung mit Speech-Ressourcen mit aktivierten privaten Endpunkten kompatibel sein. Eine nahtlosere Unterstützung des Szenarios mit privaten Endpunkten ist in Arbeit.

### <a name="use-speech-resource-with-custom-domain-name-without-private-endpoints"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen ohne private Endpunkte

In diesem Artikel wurde mehrmals betont, dass die Aktivierung der benutzerdefinierten Domäne für eine Speech-Ressource **nicht rückgängig gemacht werden kann** und eine solche Ressource nicht auf die herkömmliche Weise (also nicht wie bei der Verwendung von [regionalen Endpunktnamen](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)) mit Speech Services kommuniziert.

In diesem Abschnitt wird erläutert, wie Sie eine Speech-Ressource mit aktiviertem benutzerdefinierten Domänennamen, aber **ohne** private Endpunkte, mit der Speech Services-REST-API und dem [Speech-SDK](speech-sdk.md) verwenden. Dabei kann es sich um eine Ressource handeln, die in einem Szenario mit privaten Endpunkten eingesetzt wurde, deren private Endpunkte jedoch gelöscht wurden.

#### <a name="dns-configuration"></a>DNS-Konfiguration

Rufen Sie sich ins Gedächtnis, wie der DNS-Name einer benutzerdefinierten Domäne der Speech-Ressource mit aktivierten privaten Endpunkten [über öffentliche Netzwerke aufgelöst wird](#resolve-dns-from-other-networks). In diesem Fall verweist die aufgelöste IP-Adresse auf einen VNET-Proxyendpunkt. Dieser wird zum Weiterleiten des Netzwerkdatenverkehrs an die Cognitive Services-Ressource verwendet, für die private Endpunkte aktiviert sind.

Wenn **alle** privaten Endpunkte einer Ressource entfernt werden (bzw. direkt nach der Aktivierung des benutzerdefinierten Domänennamens), wird der CNAME-Eintrag der Speech-Ressource erneut bereitgestellt. Dieser verweist nun auf die IP-Adresse des zugehörigen [regionalen Cognitive Services-Endpunkts](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints).

Die Ausgabe des Befehls `nslookup` sieht also wie folgt aus:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    apimgmthskquihpkz6d90kmhvnabrx3ms3pdubscpdfk1tsx3a.cloudapp.net
Address:  13.93.122.1
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          westeurope.api.cognitive.microsoft.com
          cognitiveweprod.trafficmanager.net
          cognitiveweprod.azure-api.net
          apimgmttmdjylckcx6clmh2isu2wr38uqzm63s8n4ub2y3e6xs.trafficmanager.net
          cognitiveweprod-westeurope-01.regional.azure-api.net
```
Vergleichen Sie diese mit der Ausgabe in [diesem Abschnitt](#resolve-dns-from-other-networks).

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-rest-api"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen und ohne private Endpunkte mit der REST-API

##### <a name="speech-to-text-rest-api-v30"></a>Spracherkennungs-REST-API 3.0

Die Verwendung der Spracherkennungs-REST-API 3.0 entspricht dem Szenario für [Speech-Ressourcen mit aktivierten privaten Endpunkten](#speech-to-text-rest-api-v30).

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>Spracherkennungs-REST-API für kurze Audiodaten und Sprachsynthese-REST-API

In diesem Fall besteht nur ein Unterschied zwischen der Verwendung der Spracherkennungs-REST-API für kurze Audiodaten und der Sprachsynthese-REST-API. Dieser wird im folgenden Hinweis erläutert. Beide APIs sollten wie in den Dokumentationen [Spracherkennungs-REST-API für kurze Audiodaten](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) und [Sprachsynthese-REST-API](rest-text-to-speech.md) beschrieben eingesetzt werden.

> [!NOTE]
> Wenn Sie die **Spracherkennungs-REST-API für kurze Audiodaten** in Szenarios mit benutzerdefinierten Domänen nutzen, verwenden Sie ein Autorisierungstoken, das [im](rest-speech-to-text.md#request-headers) `Authorization`-[Header](rest-speech-to-text.md#request-headers) übergeben wird. Das Übergeben des Speech-Abonnementschlüssels an den speziellen Endpunkt über den `Ocp-Apim-Subscription-Key`-Header funktioniert **nicht** und führt dazu, dass der Fehler 401 auftritt.

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-speech-sdk"></a>Verwenden von Speech-Ressourcen mit benutzerdefiniertem Domänennamen und ohne private Endpunkte Verwendung mit dem Speech-SDK

Für die Verwendung des Speech-SDK mit Speech-Ressourcen, für die benutzerdefinierte Domänennamen, aber **keine** privaten Endpunkte aktiviert sind, sind höchstwahrscheinlich ein Review und Änderungen an Ihrem Anwendungscode notwendig. Beachten Sie, dass diese Änderungen sich vom Szenario einer [Speech-Ressource mit aktivierten privaten Endpunkten](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) **unterscheidet**. Eine nahtlosere Unterstützung des Szenarios mit privaten Endpunkten und benutzerdefinierten Domänen ist in Arbeit.

In diesem Abschnitt wird `my-private-link-speech.cognitiveservices.azure.com` als DNS-Beispielname (benutzerdefinierte Domäne) für die Speech-Ressource verwendet.

Im Abschnitt zu [Speech-Ressourcen mit aktivierten privaten Endpunkten](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) wurde erläutert, wie die verwendete URL bestimmt, geändert und über die Initialisierungen FromEndpoint und WithEndpoint der Klasseninstanz `SpeechConfig` funktionsfähig gemacht werden kann.

Wenn Sie diese Anwendung jedoch ausführen, nachdem alle privaten Endpunkte entfernt wurden (mit etwas Zeit für die erneute Bereitstellung des entsprechenden DNS-Eintrags), tritt ein interner Dienstfehler (404) auf. Das liegt daran, dass der [DNS-Eintrag](#dns-configuration) jetzt auf den regionalen Cognitive Services-Endpunkt anstatt auf den VNET-Proxy verweist. Dort werden URL-Pfade wie `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US` nicht gefunden, sodass der Fehler „Nicht gefunden“ (404) angezeigt wird.

Ändern Sie Ihre Anwendung beispielsweise in die Standardinstanziierung von `SpeechConfig` zurück:
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Dann wird die Anwendung mit einem Authentifizierungsfehler (401) beendet.

##### <a name="modifying-applications"></a>Ändern von Anwendungen

Führen Sie die folgenden Schritte aus, damit Ihre Anwendung eine Speech-Ressource mit einem benutzerdefinierten Domänennamen und ohne private Endpunkte verwendet:

**1. Fordern Sie das Autorisierungstoken über die Cognitive Services-REST-API an.**

In [diesem Artikel](../authentication.md#authenticate-with-an-authentication-token) erfahren Sie, wie Sie das Token über die Cognitive Services-REST-API abrufen.

Verwenden Sie den benutzerdefinierten Domänennamen in der Endpunkt-URL. In diesem Beispiel lautet die URL:
```http
https://my-private-link-speech.cognitiveservices.azure.com/sts/v1.0/issueToken
```
> [!TIP]
> Diese URL finden Sie im Azure-Portal. Klicken Sie auf der Seite Ihrer Speech-Ressource in der Gruppe **Ressourcenverwaltung** auf **Keys and Endpoint** (Schlüssel und Endpunkt).

**2. Erstellen Sie mithilfe der FromAuthorizationToken- oder der WithAuthorizationToken-Methode eine `SpeechConfig`-Instanz.**

Erstellen Sie mit dem im vorherigen Abschnitt abgerufenen Autorisierungstoken eine `SpeechConfig`-Instanz. Nehmen Sie an, dass die folgenden Variablen definiert sind:

- `token` enthält das im vorherigen Abschnitt abgerufene Autorisierungstoken.
- `azureRegion` enthält den Namen der [Region](regions.md) der Speech-Ressource (z. B. `westeurope`).
- `outError` (nur für [Objective-C](/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithauthorizationtokenregionerror)-Szenarios)

Erstellen Sie als Nächstes eine `SpeechConfig`-Instanz:

```csharp
var config = SpeechConfig.FromAuthorizationToken(token, azureRegion);
```
```cpp
auto config = SpeechConfig::FromAuthorizationToken(token, azureRegion);
```
```java
SpeechConfig config = SpeechConfig.fromAuthorizationToken(token, azureRegion);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(auth_token=token, region=azureRegion)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithAuthorizationToken:token region:azureRegion error:outError];
```
> [!NOTE]
> Der Aufrufer muss sicherstellen, dass das Autorisierungstoken gültig ist.
> Bevor das Autorisierungstoken abläuft, muss der Aufrufer es aktualisieren, indem er diesen Setter mit einem neuen gültigen Token aufruft.
> Da Konfigurationswerte bei der Erstellung eines neuen Erkennungs- oder Synthesemoduls kopiert werden, gilt der neue Tokenwert nicht für bereits erstellte Erkennungs- oder Synthesemodule.
> Legen Sie für diese das Autorisierungstoken des entsprechenden Erkennungs- oder Synthesemoduls fest, um das Token zu aktualisieren.
> Wenn Sie das Token nicht aktualisieren, treten bei der Verwendung des Erkennungs- oder Synthesemoduls Fehler auf.

Nach dieser Änderung sollte Ihre Anwendung mit Speech-Ressourcen funktionieren, die einen benutzerdefinierten Domänennamen ohne private Endpunkte verwenden.

## <a name="pricing"></a>Preise

Ausführliche Preisinformationen finden Sie unter [Azure Private Link – Preise](https://azure.microsoft.com/pricing/details/private-link).

## <a name="learn-more"></a>Weitere Informationen

* [Azure Private Link](../../private-link/private-link-overview.md)
* [Speech SDK](speech-sdk.md)
* [Spracherkennungs-REST-API](rest-speech-to-text.md)
* [Text-to-Speech-REST-API](rest-text-to-speech.md)
