---
title: Einführung in die räumliche Analyse für maschinelles Sehen
titleSuffix: Azure Cognitive Services
description: In diesem Dokument werden die grundlegenden Konzepte und Features eines Containers zur räumlichen Analyse für maschinelles Sehen erläutert.
services: cognitive-services
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: f90e4e5e187977f0ee77a565ff9143902ea3a10d
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736834"
---
# <a name="introduction-to-computer-vision-spatial-analysis"></a>Einführung in die räumliche Analyse für maschinelles Sehen

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

## <a name="example-use-cases-for-spatial-analysis"></a>Beispielanwendungsfälle für räumliche Analysen

Im Folgenden werden einige Beispielanwendungsfälle beschrieben, die wir beim Entwerfen und Testen der räumlichen Analyse berücksichtigt haben.

**Einhaltung der geltenden Abstandsregeln:** In einem Büro sind mehrere Kameras angebracht, die die räumliche Analyse verwenden, um die Einhaltung der geltenden Abstandsregeln zu überwachen, indem der Abstand zwischen den Personen gemessen wird. Das Facilitymanagement kann Wärmebilder verwenden, die Gesamtstatistiken zur Einhaltung der geltenden Abstandsregeln im Laufe der Zeit zeigen, damit der Arbeitsbereich entsprechend angepasst werden kann und das Social Distancing erleichtert wird.

**Kundenanalyse:** Ein Supermarkt verwendet Kameras, die auf die ausgestellten Produkte gerichtet sind, um herauszufinden, welche Auswirkungen neue Produkte auf den Kundenverkehr haben. Mithilfe des Systems kann die Filialleitung ermitteln, welche neuen Produkte die Kunden am stärksten ansprechen.

**Warteschlangenverwaltung:** Kameras, die auf die Warteschlangen an den Kassen ausgerichtet sind, benachrichtigen die Filialleitung, wenn die Wartezeiten zu lang sind. Dann können sie weitere Kassen öffnen. Verlaufsdaten zum Verlassen von Warteschlangen bieten Einblicke in das Verbraucherverhalten.

**Gesichtsmaskenkonformität:** Einzelhandelsfilialen können Kameras verwenden, die Richtung Ladenfront ausgerichtet sind, um zu überprüfen, ob Kunden, die den Laden betreten, eine Gesichtsmaske tragen. So kann die Sicherheitskonformität eingehalten und Aggregatstatistiken zum Gewinnen von Erkenntnissen zu Trends bei der Verwendung von Gesichtsmasken analysiert werden. 

**Gebäudenutzung und -analyse:** In einem Bürogebäude werden Kameras verwendet, die auf die Eingänge von wichtigen Bereichen gerichtet sind, um die Anzahl der eintretenden Personen sowie deren Nutzung des Arbeitsbereichs zu messen. Mithilfe der dadurch gewonnen Erkenntnisse kann das Gebäudemanagement seinen Service und die Anordnung der Arbeitsplätze den Bedürfnissen der Gebäudenutzer entsprechend anpassen.

**Erkennung der mindestens benötigten Mitarbeiter:** In einem Rechenzentrum überwachen Kameras die Aktivitäten rund um die Server. Wenn wichtige physische Geräte repariert werden, müssen aus Sicherheitsgründen immer zwei Mitarbeiter anwesend sein. Kameras werden verwendet, um zu überprüfen, ob diese Richtlinie eingehalten wird.

**Arbeitsplatzoptimierung:** in der Küche eines Fast-Casual-Restaurants werden Kameras eingesetzt, um Informationen zu den Arbeitsabläufen der Mitarbeiter zu sammeln. Die Restaurantleitung verwendet diese Daten, um die Prozesse und die Ausbildung des Teams zu verbessern.

## <a name="considerations-when-choosing-a-use-case"></a>Hinweise zur Auswahl eines Anwendungsfalls

**Vermeiden Sie kritische Sicherheitswarnungen:** Die räumliche Analyse wurde nicht für Echtzeitwarnungen vor kritischen Sicherheitsproblemen konzipiert. Sie sollte nicht für Szenarios verwendet werden, in denen Echtzeitwarnungen benötigt werden, damit eingegriffen werden kann, um Verletzungen zu verhindern, z. B. wenn Schwermaschinen ausgeschaltet werden müssen, sobald jemand den Raum betritt. Die räumliche Analyse kann zur Reduzierung von Risiken verwendet werden. Dabei werden Statistiken und bestimmte Maßnahmen eingesetzt, um riskantes Verhalten zu reduzieren, z. B. das Betreten eines gesperrten/verbotenen Bereichs.

**Vermeiden Sie die Verwendung für Entscheidungen zu einem Beschäftigungsverhältnis:** Die räumliche Analyse stellt probabilistische Metriken zum Aufenthaltsort und zu den Wegen der Personen bereit, die sich in einem bestimmten Bereich aufhalten. Zwar können diese Daten für die Verbesserung der allgemeinen Prozesse nützlich sein, aber sie liefern keine eindeutigen Hinweise auf die Leistung einzelner Mitarbeiter und sollten nicht verwendet werden, um Entscheidungen zu einem Beschäftigungsverhältnis zu treffen.

**Vermeiden Sie die Verwendung für Entscheidungen im Gesundheitswesen:** Die räumliche Analyse stellt probabilistische und partielle Daten zu den Wegen von Personen bereit. Die Daten eignen sich nicht dafür, Entscheidungen im Gesundheitswesen zu treffen.

**Vermeiden Sie die Verwendung für geschützte Räume:** Schützen Sie die Privatsphäre der Menschen, indem Sie die Standorte und Positionen der Kamera bewerten, die Winkel und relevante Bereiche anpassen, damit geschützte Bereiche wie Toiletten nicht übersehen werden.

**Seien Sie vorsichtig bei der Verwendung in Schulen oder Altenheimen:** Die räumliche Analyse wurde noch nicht umfassend mit Daten zu Minderjährigen oder Erwachsenen über 65 Jahren getestet. Unsere Kunden sollten Fehlerraten für ihren Anwendungsfall in Umgebungen gründlich prüfen, in denen sich vornehmlich diese Altersgruppen aufhalten.

**Seien Sie vorsichtig bei der Verwendung in öffentlichen Bereichen:** Prüfen Sie die Standorte und Positionen der Kameras, und passen Sie die Winkel und relevanten Bereiche an, um die Anzahl der Aufnahmen in öffentlichen Bereichen möglichst gering zu halten. Das Licht und das Wetter in öffentlichen Bereichen wie Straßen oder Parks haben großen Einfluss auf die Leistung des Systems für die räumliche Analyse. Es ist sehr schwierig, öffentliche Räume effizient zu schützen.

## <a name="spatial-analysis-gating-for-public-preview"></a>Beschränkung der räumlichen Analyse für die öffentliche Vorschau

Damit die räumliche Analyse nur in Szenarios verwendet wird, für die sie entwickelt wurde, stellen wir unseren Kunden diese Technologie nur auf Antrag zur Verfügung. Wenn Sie Zugriff auf die räumliche Analyse erhalten möchten, füllen Sie zunächst unser Onlineeingabeformular aus. [Über diesen Link gelangen Sie zum Antrag.](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u)

Der Zugriff auf die öffentliche Vorschauversion der räumlichen Analyse unterliegt dem alleinigen Ermessen von Microsoft basierend auf den Kriterien für die Qualifikation, dem Überprüfungsprozess und der Verfügbarkeit, um eine begrenzte Anzahl von Kunden während dieser eingeschränkten Vorschau zu unterstützen. Für die öffentliche Vorschauversion suchen wir Kunden, die bereits mit Microsoft zusammenarbeiten, Interesse an einer Zusammenarbeit im Rahmen der empfohlenen Anwendungsfälle und weiterer Szenarios haben, die im Einklang mit unserer Verpflichtung zur verantwortungsvollen Verwendung von KI stehen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Merkmale und Einschränkungen der räumlichen Analyse](/legal/cognitive-services/computer-vision/accuracy-and-limitations?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext)