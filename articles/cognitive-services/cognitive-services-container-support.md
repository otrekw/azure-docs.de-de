---
title: Containerunterstützung
titleSuffix: Azure Cognitive Services
description: Erfahren Sie, wie Docker-Container Cognitive Services näher an Ihre Daten bringen können.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2020
ms.author: aahi
ms.openlocfilehash: 69984f9dbd94bcdca2e272a5bdebbb7fc1464dae
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104406"
---
# <a name="container-support-in-azure-cognitive-services"></a>Containerunterstützung in Azure Cognitive Services

Dank der Containerunterstützung in Azure Cognitive Services können Entwickler die gleichen umfangreichen APIs verwenden wie in Azure und dabei flexibel entscheiden, wo die mit [Docker-Containern](https://www.docker.com/what-container) verbundenen Dienste bereitgestellt und gehostet werden. Die Containerunterstützung ist derzeit für einige Dienste von Azure Cognitive Services verfügbar, einschließlich:

> [!div class="checklist"]
> * [Anomalieerkennung][ad-containers]
> * [Maschinelles Sehen][cv-containers]
> * [Gesichtserkennung][fa-containers]
> * [Formularerkennung][fr-containers]
> * [Language Understanding (LUIS)][lu-containers]
> * [Spracherkennungsdienst-API][sp-containers]
> * [Textanalyse][ta-containers]

> [!VIDEO https://www.youtube.com/embed/hdfbn4Q8jbo]

Die Verwendung von Containern ist ein Ansatz zur Softwareverteilung, bei dem eine Anwendung oder ein Dienst, einschließlich der Abhängigkeiten und der Konfiguration, als Containerimage zusammengepackt wird. Das Containerimage kann mit geringer oder ganz ohne Bearbeitung auf einem Containerhost bereitgestellt werden. Container sind voneinander und vom zugrunde liegenden Betriebssystem isoliert und nehmen weniger Speicher in Anspruch als ein virtueller Computer. Container können für kurzfristige Aufgaben über Containerimages instanziiert und wieder entfernt werden, wenn sie nicht mehr benötigt werden.

Cognitive Services-Ressourcen sind in [Microsoft Azure](https://azure.microsoft.com) verfügbar. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, um Azure-Ressourcen für diese Dienste zu erstellen und zu erkunden.

## <a name="features-and-benefits"></a>Features und Vorteile

- **Unveränderliche Infrastruktur**: Ermöglichen Sie es den DevOps-Teams, einen konsistenten und zuverlässigen Satz bekannter Systemparameter zu nutzen und sich gleichzeitig an Änderungen anzupassen. Container bieten die Flexibilität, sich innerhalb eines vorhersehbaren Ökosystems zu bewegen und Konfigurationsabweichungen zu vermeiden.
- **Kontrolle über Daten:** Ermöglichen Sie Kunden, auszuwählen, wo Cognitive Services ihre Daten verarbeitet. Dies ist wichtig für Kunden, die keine Daten in die Cloud senden können, aber auf die Cognitive Services-Technologie zugreifen müssen. Einheitliche Unterstützung in Hybridumgebungen – für Daten, Verwaltung, Identität und Sicherheit.
- **Kontrolle über Modellaktualisierungen:** Bietet Kunden Flexibilität bei der Versionsverwaltung und Aktualisierung der in ihren Lösungen eingesetzten Modelle.
- **Portable Architektur:** Ermöglicht die Erstellung einer portablen Anwendungsarchitektur, die in Azure, lokal und am Edge eingesetzt werden kann. Container können direkt für [Azure Kubernetes Service](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml) oder einen [Kubernetes](https://kubernetes.io/)-Cluster mit Bereitstellung in [Azure Stack](/azure-stack/operator) bereitgestellt werden. Weitere Informationen finden Sie unter [Bereitstellen von Kubernetes in Azure Stack](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Hoher Durchsatz/niedrige Latenz:** Bietet Kunden die Möglichkeit, für hohe Durchsatzraten und niedrige Latenzanforderungen zu skalieren, indem Cognitive Services physisch in der Nähe ihrer Anwendungslogik und -daten ausgeführt wird. Container erzwingen keine Obergrenzen für die Transaktionen pro Sekunde (TPS) und können für die zentrale und horizontale Skalierung konfiguriert werden, um bei Bedarf die erforderlichen Hardwareressourcen bereitzustellen.
- **Skalierbarkeit:** Mit der ständig wachsenden Beliebtheit von Software für Containerisierung und Containerorchestrierung wie Kubernetes steht die Skalierbarkeit steht im Vordergrund des technologischen Fortschritts. Aufbauend auf einer skalierbaren Clusterbasis sorgt die Anwendungsentwicklung für eine entsprechende Hochverfügbarkeit.

## <a name="containers-in-azure-cognitive-services"></a>Container in Azure Cognitive Services

Azure Cognitive Services-Container bieten den folgenden Satz von Docker-Containern, von denen jeder eine Teilmenge der Funktionalität von Diensten in Azure Cognitive Services enthält:

| Dienst | Unterstützter Tarif | Container | BESCHREIBUNG |
|--|--|--|--|
| [Anomalieerkennung][ad-containers] | F0, S0 | **Anomalieerkennung** | Die Anomalieerkennungs-API bietet Ihnen die Möglichkeit, Anomalien in Zeitreihendaten durch maschinelles Lernen zu überwachen und zu erkennen.<br>[Zugriff anfordern][request-access] |
| [Maschinelles Sehen][cv-containers] | F0, S1 | **Lesen** | Extrahiert gedruckten Text in Bildern von verschiedensten Objekten mit unterschiedlichen Oberflächen und Hintergründen, wie z.B. Belege, Poster, und Visitenkarten. Der Container für das Lesen erkennt auch *handgeschriebenen Text* in Bildern und bietet Unterstützung für PDF, TIFF oder mehrere Seiten.<br/><br/>**Wichtig:** Der Container für das Lesen funktioniert derzeit nur für Englisch. |
| [Gesichtserkennung][fa-containers] | F0, S0 | **Gesichtserkennung** | Erkennt menschliche Gesichter in Bildern und identifiziert zugehörige Attribute wie Gesichtsmerkmale (z.B. Nasen und Augen), Geschlecht, Alter und andere vom Computer vorhergesagte Gesichtsmerkmale. Zusätzlich zur Erkennung kann die Gesichtserkennung über eine Zuverlässigkeitsbewertung überprüfen, ob zwei Gesichter in einem oder verschiedenen Bildern identisch sind, oder Gesichter mit einer Datenbank vergleichen, um festzustellen, ob ein ähnliches oder identisches Gesicht bereits vorhanden ist. Sie kann auch ähnliche Gesichter in Gruppen mit gemeinsamen Gesichtsmerkmalen organisieren.<br>[Zugriff anfordern][request-access] |
| [Formularerkennung][fr-containers] | F0, S0 | **Formularerkennung** | Die Formularerkennung wendet Technologien des maschinellen Lernens an, um Schlüssel-Wert-Paare und Tabellen in Formularen zu identifizieren und aus diesen zu extrahieren.<br>[Zugriff anfordern][request-access] |
| [LUIS][lu-containers] | F0, S0 | **LUIS** ([Image](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409)) | Lädt Ihr trainiertes oder veröffentlichtes Language Understanding-Modell (auch als LUIS-App bezeichnet) in einen Docker-Container und ermöglicht den Zugriff auf die Abfragevorhersagen von den API-Endpunkten des Containers. Sie können Abfrageprotokolle vom Container erfassen und wieder in das [LUIS-Portal](https://www.luis.ai) hochladen, um die Vorhersagegenauigkeit der App zu verbessern. |
| [Spracherkennungsdienst-API][sp-containers-stt] | F0, S0 | **Spracherkennung** | Wandelt fortlaufende Sprache in Echtzeit in Text um. |
| [Spracherkennungsdienst-API][sp-containers-cstt] | F0, S0 | **Benutzerdefinierte Spracherkennung** | Wandelt fortlaufende Sprache in Echtzeit in Text um und verwendet dazu ein benutzerdefiniertes Modell. |
| [Spracherkennungsdienst-API][sp-containers-tts] | F0, S0 | **Sprachsynthese** | Konvertiert Text in natürlich klingende Sprache. |
| [Spracherkennungsdienst-API][sp-containers-ctts] | F0, S0 | **Benutzerdefinierte Sprachsynthese** | Konvertiert Text in natürlich klingende Sprache und verwendet dazu ein benutzerdefiniertes Modell. |
| [Textanalyse][ta-containers-keyphrase] | F0, S | **Schlüsselbegriffserkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) | Extrahiert die Schlüsselbegriffe, um die wichtigsten Punkte zu ermitteln. Wenn der eingegebene Text beispielsweise „Das Essen war köstlich, und es gab hervorragendes Personal“ lautet, gibt die API die Kernpunkte „Essen“ und „hervorragendes Personal“ zurück. |
| [Textanalyse][ta-containers-language] | F0, S | **Sprachenerkennung** ([Bild](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) | Erkennt die Sprache von Eingabetexten für bis zu 120 Sprachen und meldet einen einzigen Sprachcode für jedes Dokument, das auf Anforderung gesendet wird. Der Sprachcode ist mit einem Wert kombiniert, der die Stärke der Bewertung angibt. |
| [Textanalyse][ta-containers-sentiment] | F0, S | **Standpunktanalyse v3** ([Image](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) | Analysiert unformatierten Text auf Hinweise auf positive oder negative Stimmungen. Von dieser Version der Standpunktanalyse werden für jedes Dokument und jeden darin enthaltenen Satz Stimmungsbezeichnungen (beispielsweise *Positiv* oder *Negativ*) zurückgegeben. |
| [Textanalyse][ta-containers-health] | F0, S | **Text Analytics for Health** | Extraktion und Bezeichnung medizinischer Informationen aus unstrukturiertem klinischem Text. |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

Darüber hinaus werden einige Container für Ressourcenschlüssel im [**All-In-One-Angebot**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) von Cognitive Services unterstützt. Sie können eine einzelne All-In-One-Ressource von Cognitive Services erstellen und denselben Abrechnungsschlüssel für alle unterstützten Dienste verwenden:

* Maschinelles Sehen
* Gesicht
* LUIS
* Textanalyse

## <a name="container-availability-in-azure-cognitive-services"></a>Containerverfügbarkeit in Azure Cognitive Services

Azure Cognitive Services-Container sind über Ihr Azure-Abonnement öffentlich zugänglich, und Docker-Containerimages können entweder aus der Microsoft Container Registry oder dem Docker-Hub abgerufen werden. Verwenden Sie den Befehl [docker pull](https://docs.docker.com/engine/reference/commandline/pull/), um ein Containerimage aus dem entsprechenden Repository herunterzuladen.

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen die folgenden Voraussetzungen erfüllen, bevor Sie die Container der Azure Cognitive Services verwenden können:

**Docker-Engine:** Die Docker-Engine muss lokal installiert sein. Docker stellt Pakete zur Konfiguration der Docker-Umgebung unter [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) und [Windows](https://docs.docker.com/docker-for-windows/) zur Verfügung. Unter Windows muss Docker für die Unterstützung von Linux-Containern konfiguriert werden. Docker-Container können auch direkt für [Azure Kubernetes Service](../aks/index.yml) oder [Azure Container Instances](../container-instances/index.yml) bereitgestellt werden.

Docker muss so konfiguriert werden, dass die Container eine Verbindung mit Azure herstellen und Abrechnungsdaten an Azure senden können.

**Kenntnisse von Microsoft Container Registry und Docker:** Sie sollten über Grundkenntnisse der Konzepte von Microsoft Container Registry und Docker verfügen, einschließlich Registrierungen, Repositorys, Container und Containerimages, und die grundlegenden `docker`-Befehle kennen.

Eine Einführung in Docker und Container finden Sie in der [Docker-Übersicht](https://docs.docker.com/engine/docker-overview/).

Einzelne Container können auch ihre eigenen Anforderungen haben, wie z.B. Anforderungen an die Server- und Speicherzuordnung.

[!INCLUDE [Cognitive Services container security](containers/includes/cognitive-services-container-security.md)]

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zu [Containeranleitungen](containers/container-reuse-recipe.md), die Sie mit Cognitive Services verwenden können.

Installieren und erkunden Sie die Funktionalität der Container in Azure Cognitive Services:

* [Container für die Anomalieerkennung][ad-containers]
* [Container für maschinelles Sehen][cv-containers]
* [Container für die Gesichtserkennung][fa-containers]
* [Container für die Formularerkennung][fr-containers]
* [Container für Language Understanding (LUIS)][lu-containers]
* [Container für die Speech Services-API][sp-containers]
* [Container für die Textanalyse][ta-containers]

<!--* [Personalizer containers](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409)
-->

[ad-containers]: anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: face/face-how-to-install-containers.md
[fr-containers]: form-recognizer/form-recognizer-container-howto.md
[lu-containers]: luis/luis-container-howto.md
[sp-containers]: speech-service/speech-container-howto.md
[sp-containers-stt]: speech-service/speech-container-howto.md?tabs=stt
[sp-containers-cstt]: speech-service/speech-container-howto.md?tabs=cstt
[sp-containers-tts]: speech-service/speech-container-howto.md?tabs=tts
[sp-containers-ctts]: speech-service/speech-container-howto.md?tabs=ctts
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md
[ta-containers-keyphrase]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-containers-language]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-containers-sentiment]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
[ta-containers-health]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=health
[request-access]: https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u