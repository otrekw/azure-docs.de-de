---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 10/08/2019
ms.openlocfilehash: 5089af4a4e1714d49b844a1b6823487a3f6a8dcf
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483033"
---
Beginnen Sie mit der Verwendung des Anomalieerkennungsdiensts, indem Sie eine der unten aufgeführten Azure-Ressourcen erstellen.

* <a href="https://azure.microsoft.com/try/cognitive-services/#decision" target="_blank" rel="noopener">Erstellen einer Testressource (wird auf neuer Registerkarte geöffnet)</a>
    * Kein Azure-Abonnement erforderlich: 
    * Kostenlos und sieben Tage lang gültig. Nach der Registrierung stehen ein Testschlüssel und ein Endpunkt auf der [Azure-Website](https://azure.microsoft.com/try/cognitive-services/my-apis/) zur Verfügung. 
    * Diese Option eignet sich perfekt, wenn Sie die Anomalieerkennung testen möchten, aber über kein Azure-Abonnement verfügen.

* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank" rel="noopener">Erstellen einer Anomalieerkennungsressource (wird auf neuer Registerkarte geöffnet):</a>
    * Verfügbar über das Azure-Portal, bis Sie die Ressource löschen.
    * Verwenden Sie den Tarif „Free“, um den Dienst zu testen, und führen Sie später für die Produktion ein Upgrade auf einen kostenpflichtigen Tarif durch.



### <a name="create-an-environment-variable"></a>Erstellen einer Umgebungsvariablen

>[!NOTE]
> Nach dem 1. Juli 2019 erstellte Endpunkte für Ressourcen, bei denen es sich nicht um Testressourcen handelt, verwenden das unten gezeigte benutzerdefinierte Format für Subdomänen. Weitere Informationen und eine vollständige Liste mit regionalen Endpunkten finden Sie unter [Benutzerdefinierte Unterdomänennamen für Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Erstellen Sie unter Verwendung des Schlüssels und des Endpunkts der von Ihnen erstellten Ressource zwei Umgebungsvariablen für die Authentifizierung:

* `ANOMALY_DETECTOR_KEY`: Der Ressourcenschlüssel zum Authentifizieren Ihrer Anforderungen.
* `ANOMALY_DETECTOR_ENDPOINT`: Der Ressourcenendpunkt zum Senden von API-Anforderungen. Er sieht wie folgt aus: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

Führen Sie die Schritte für Ihr Betriebssystem aus:

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Starten Sie das Konsolenfenster neu, nachdem Sie die Umgebungsvariable hinzugefügt haben.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source ~/.bashrc` aus, damit die Änderungen wirksam werden.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Bearbeiten Sie `.bash_profile`, und fügen Sie die Umgebungsvariable hinzu:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Führen Sie nach dem Hinzufügen der Umgebungsvariablen im Konsolenfenster `source .bash_profile` aus, damit die Änderungen wirksam werden.

***