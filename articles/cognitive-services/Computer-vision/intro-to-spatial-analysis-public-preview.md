---
title: Übersicht über die räumliche Analyse
titleSuffix: Azure Cognitive Services
description: In diesem Dokument werden die grundlegenden Konzepte und Features eines Containers zur räumlichen Analyse für maschinelles Sehen erläutert.
services: cognitive-services
author: nitinme
manager: nitinme
ms.author: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/01/2021
ms.openlocfilehash: ad05dd59c925242baf5c2b0e36c1f51bc4fec5d4
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100575370"
---
# <a name="overview-of-computer-vision-spatial-analysis"></a>Übersicht über die räumliche Analyse für maschinelles Sehen

Die räumliche Analyse für maschinelles Sehen ist ein neues Feature der Funktion für maschinelles Sehen von Azure Cognitive Services, mit dem Organisationen den ihnen zur Verfügung stehenden Platz in vollem Umfang nutzen können, indem sie Informationen zu den Wegen der Menschen und ihrem Aufenthalt in bestimmten Bereichen erhalten. Das Feature bietet Ihnen die Möglichkeit, Videos von Überwachungskameras zu erfassen, KI-Vorgänge durchzuführen, um Erkenntnisse aus Videostreams zu gewinnen, und Ereignisse zu generieren, die von anderen Systemen verwendet werden können. Mithilfe von Videos von Kamerastreams können KI-Vorgänge beispielsweise die Personen zählen, die einen bestimmten Bereich betreten, oder messen, ob eine Mund-Nasen-Bedeckung getragen wird und die geltenden Abstandsregeln eingehalten werden.

## <a name="the-basics-of-spatial-analysis"></a>Grundlagen der räumlichen Analyse

Heute basieren die grundlegenden Vorgänge der räumlichen Analyse auf einer Pipeline, die Videos erfasst, Personen im Video erkennt, diese verfolgt, während sie sich bewegen, und Ereignisse generiert, wenn Personen mit relevanten Bereichen interagieren.

## <a name="spatial-analysis-terms"></a>Begriffe im Zusammenhang mit der räumlichen Analyse

| Begriff | Definition |
|------|------------|
| Personenerkennung | Diese Komponente beantwortet die Frage, wo sich die Personen auf einem Bild befinden. Sie findet Personen in einem Bild und übergibt einen Begrenzungsrahmen, der die Aufenthaltsorte der einzelnen Personen markiert, an die Komponente zur Personenverfolgung. |
| Personenverfolgung | Diese Komponente verbindet die erkannten Personen im Laufe der Zeit, während sich diese vor der Kamera bewegen. Dafür verwendet sie temporale Logik zu den üblichen Wegen der Menschen und grundlegende Informationen zu ihrem äußeren Erscheinungsbild. Personen werden nicht über mehrere Kameras hinweg beobachtet. Wenn eine Person aus dem Sichtfeld einer Kamera für etwas länger als eine Minute hinaustritt und dann wieder in das Sichtfeld läuft, wird diese vom System als neue Person wahrgenommen. Durch das Tracking von Personen können keine Personen kameraübergreifend eindeutig identifiziert werden. Die Gesichtserkennung oder die Nachverfolgung der Gangart werden nicht genutzt. |
| Gesichtsmaskenerkennung | Diese Komponente erkennt die Position des Gesichts einer Person im Kamerasichtfeld und identifiziert das Vorhandensein einer Gesichtsmaske. Zu diesem Zweck scannt der KI-Vorgang Bilder aus einem Video. Immer dann, wenn ein Gesicht erkannt wird, stellt der Dienst einen Begrenzungsrahmen um das Gesicht bereit. Mithilfe der Objekterkennungsfunktionen kann nun im Begrenzungsrahmen identifiziert werden, ob die Person eine Gesichtsmaske trägt. Die Gesichtsmaskenerkennung kann nicht zwischen Gesichtern unterscheiden oder Gesichtsmerkmale vorhersagen oder erkennen sowie eine Gesichtserkennung durchführen. |
| Relevanter Bereich | Hierbei handelt es sich um eine Zone oder Linie, die im Eingabevideo als Teil der Konfiguration definiert ist. Wenn eine Person mit dem relevanten Bereich des Videos interagiert, generiert das System ein Ereignis. Beispielsweise ist für den Vorgang „PersonCrossingLine“ eine Linie im Video definiert. Wenn eine Person diese Linie überquert, wird ein Ereignis generiert. |
| Ereignis | Ereignisse stellen die primäre Ausgabe der räumlichen Analyse dar. Jeder Vorgang gibt entweder in regelmäßigen Abständen (z. B. einmal pro Minute) oder wenn ein Trigger ausgelöst wird ein bestimmtes Ereignis aus. Das Ereignis umfasst Informationen darüber, was im Eingabevideo passiert ist, enthält aber keine Bilder oder Videos. Beispielsweise kann der PeopleCount-Vorgang jedes Mal, wenn sich die Anzahl der Personen ändert (Trigger) oder einmal pro Minute (regelmäßig) ein Ereignis ausgeben, das die aktuelle Anzahl enthält. |

## <a name="responsible-use-of-spatial-analysis-technology"></a>Verantwortungsvolle Verwendung der Technologie für räumliche Analysen

Informationen zur verantwortungsvollen Verwendung der Technologie für räumliche Analysen finden Sie im [Transparenzhinweis](/legal/cognitive-services/computer-vision/transparency-note-spatial-analysis?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext). Die Transparenzhinweise von Microsoft sollen Ihnen helfen zu verstehen, wie unsere KI-Technologie funktioniert, welche Entscheidungen Systembesitzer treffen können, die die Systemleistung und das Systemverhalten beeinflussen, und wie wichtig es ist, das gesamte System zu betrachten, einschließlich der Technologie, der Menschen und der Umgebung.

## <a name="spatial-analysis-gating-for-public-preview"></a>Beschränkung der räumlichen Analyse für die öffentliche Vorschau

Damit die räumliche Analyse nur in Szenarios verwendet wird, für die sie entwickelt wurde, stellen wir unseren Kunden diese Technologie nur auf Antrag zur Verfügung. Wenn Sie Zugriff auf die räumliche Analyse erhalten möchten, füllen Sie zunächst unser Onlineeingabeformular aus. [Über diesen Link gelangen Sie zum Antrag.](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u)

Der Zugriff auf die öffentliche Vorschauversion der räumlichen Analyse unterliegt dem alleinigen Ermessen von Microsoft basierend auf den Kriterien für die Qualifikation, dem Überprüfungsprozess und der Verfügbarkeit, um eine begrenzte Anzahl von Kunden während dieser eingeschränkten Vorschau zu unterstützen. Für die öffentliche Vorschauversion suchen wir Kunden, die bereits mit Microsoft zusammenarbeiten, Interesse an einer Zusammenarbeit im Rahmen der empfohlenen Anwendungsfälle und weiterer Szenarios haben, die im Einklang mit unserer Verpflichtung zur verantwortungsvollen Verwendung von KI stehen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erste Schritte mit dem Container für die räumliche Analyse](spatial-analysis-container.md)