---
title: Was ist räumliche Analyse?
titleSuffix: Azure Cognitive Services
description: In diesem Dokument werden die grundlegenden Konzepte und Features eines Containers zur räumlichen Analyse für maschinelles Sehen erläutert.
services: cognitive-services
author: nitinme
manager: nitinme
ms.author: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 03/29/2021
ms.openlocfilehash: fc0d0b0a589ff5b8354a56a2f8c95062f4d2d9a9
ms.sourcegitcommit: 2e123f00b9bbfebe1a3f6e42196f328b50233fc5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2021
ms.locfileid: "108076147"
---
# <a name="what-is-spatial-analysis"></a>Was ist räumliche Analyse?

Der Dienst für räumliche Analysen unterstützt Unternehmen dabei, den Wert ihrer physischen Plätze zu maximieren, indem man die Bewegungen und Präsenz der Menschen in einem bestimmten Bereich versteht. Das Feature bietet Ihnen die Möglichkeit, Videos von Überwachungskameras zu erfassen, KI-Vorgänge durchzuführen, um Erkenntnisse aus Videostreams zu gewinnen, und Ereignisse zu generieren, die von anderen Systemen verwendet werden können. Mithilfe von Videos von Kamerastreams können KI-Vorgänge beispielsweise die Personen zählen, die einen bestimmten Bereich betreten, oder messen, ob eine Mund-Nasen-Bedeckung getragen wird und die geltenden Abstandsregeln eingehalten werden.

<!--This documentation contains the following types of articles:
* The [quickstarts](./quickstarts-sdk/analyze-image-client-library.md) are step-by-step instructions that let you make calls to the service and get results in a short period of time. 
* The [how-to guides](./Vision-API-How-to-Topics/HowToCallVisionAPI.md) contain instructions for using the service in more specific or customized ways.
* The [conceptual articles](tbd) provide in-depth explanations of the service's functionality and features.
* The [tutorials](./tutorials/storage-lab-tutorial.md) are longer guides that show you how to use this service as a component in broader business solutions.-->

## <a name="what-it-does"></a>Funktionsbeschreibung

Heute basieren die grundlegenden Vorgänge der räumlichen Analyse auf einer Pipeline, die Videos erfasst, Personen im Video erkennt, diese verfolgt, während sie sich bewegen, und Ereignisse generiert, wenn Personen mit relevanten Bereichen interagieren.

## <a name="spatial-analysis-features"></a>Funktionen für räumliche Analysen

| Funktion | Definition |
|------|------------|
| **Personenerkennung** | Diese Komponente beantwortet die Frage, wo sich die Personen auf einem Bild befinden. Sie findet Personen in einem Bild und übergibt einen Begrenzungsrahmen, der die Aufenthaltsorte der einzelnen Personen markiert, an die Komponente zur Personenverfolgung. |
| **Personenverfolgung** | Diese Komponente verbindet die erkannten Personen im Laufe der Zeit, während sich diese vor der Kamera bewegen. Dafür verwendet sie temporale Logik zu den üblichen Wegen der Menschen und grundlegende Informationen zu ihrem äußeren Erscheinungsbild. Personen werden nicht über mehrere Kameras hinweg beobachtet. Wenn eine Person aus dem Sichtfeld für etwas länger als eine Minute hinaustritt und dann wieder in das Sichtfeld läuft, wird diese vom System als neue Person wahrgenommen. Durch das Tracking von Personen können keine Personen kameraübergreifend eindeutig identifiziert werden. Die Gesichtserkennung oder die Nachverfolgung der Gangart werden nicht genutzt. |
| **Gesichtsmaskenerkennung** | Diese Komponente erkennt die Position des Gesichts einer Person im Kamerasichtfeld und identifiziert das Vorhandensein einer Gesichtsmaske. Zu diesem Zweck scannt der KI-Vorgang Bilder aus einem Video. Immer dann, wenn ein Gesicht erkannt wird, stellt der Dienst einen Begrenzungsrahmen um das Gesicht bereit. Mithilfe der Objekterkennungsfunktionen kann nun im Begrenzungsrahmen identifiziert werden, ob die Person eine Gesichtsmaske trägt. Die Gesichtsmaskenerkennung kann nicht zwischen Gesichtern unterscheiden oder Gesichtsmerkmale vorhersagen oder erkennen sowie eine Gesichtserkennung durchführen. |
| **Relevanter Bereich** | Dies ist eine benutzerdefinierte Zone oder Zeile im Eingangs-Videobild. Wenn eine Person mit diesem Bereich auf dem Video interagiert, erzeugt das System ein Ereignis. Beispielsweise ist für den Vorgang „PersonCrossingLine“ eine Linie im Video definiert. Wenn eine Person diese Linie überquert, wird ein Ereignis generiert. |
| **Event** | Ein Ereigniss stellen die primäre Ausgabe der räumlichen Analyse dar. Jeder Vorgang gibt ein bestimmtes Ereignis entweder regelmäßig (z. b. einmal pro Minute) oder bei jedem Auftreten eines bestimmten Auslösers aus. Das Ereignis umfasst Informationen darüber, was im Eingabevideo passiert ist, enthält aber keine Bilder oder Videos. Beispielsweise kann der PeopleCount-Vorgang jedes Mal, wenn sich die Anzahl der Personen ändert (Trigger) oder einmal pro Minute (regelmäßig) ein Ereignis ausgeben, das die aktuelle Anzahl enthält. |

## <a name="get-started"></a>Erste Schritte

### <a name="public-preview-gating"></a>Öffentliches Vorschau-Gating

Damit die räumliche Analyse nur in Szenarios verwendet wird, für die sie entwickelt wurde, stellen wir unseren Kunden diese Technologie nur auf Antrag zur Verfügung. Wenn Sie Zugriff auf die räumliche Analyse erhalten möchten, füllen Sie zunächst unser Onlineeingabeformular aus. [Über diesen Link gelangen Sie zum Antrag.](https://aka.ms/csgate)

Der Zugriff auf die öffentliche Vorschauversion der räumlichen Analyse unterliegt dem alleinigen Ermessen von Microsoft basierend auf den Kriterien für die Qualifikation, dem Überprüfungsprozess und der Verfügbarkeit, um eine begrenzte Anzahl von Kunden während dieser eingeschränkten Vorschau zu unterstützen. Für die öffentliche Vorschauversion suchen wir Kunden, die bereits mit Microsoft zusammenarbeiten, Interesse an einer Zusammenarbeit im Rahmen der empfohlenen Anwendungsfälle und weiterer Szenarios haben, die im Einklang mit unserer Verpflichtung zur verantwortungsvollen Verwendung von KI stehen.

### <a name="follow-a-quickstart"></a>Gehen Sie wie unter Schnellstart vor

Nachdem Sie Zugriff auf die räumliche Analyse erhalten haben, befolgen Sie den [Schnellstart](spatial-analysis-container.md) , um den Container einzurichten und mit der Analyse von Videos zu beginnen.

## <a name="responsible-use-of-spatial-analysis-technology"></a>Verantwortungsvolle Verwendung der Technologie für räumliche Analysen

Informationen zur verantwortungsvollen Verwendung der Technologie für räumliche Analysen finden Sie im [Transparenzhinweis](/legal/cognitive-services/computer-vision/transparency-note-spatial-analysis?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext). Die Transparenzhinweise von Microsoft sollen Ihnen helfen zu verstehen, wie unsere KI-Technologie funktioniert, welche Entscheidungen Systembesitzer treffen können, die die Systemleistung und das Systemverhalten beeinflussen, und wie wichtig es ist, das gesamte System zu betrachten, einschließlich der Technologie, der Menschen und der Umgebung.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Schnellstart: Container für die räumliche Analyse](spatial-analysis-container.md)