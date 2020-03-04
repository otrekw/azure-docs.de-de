---
title: Terminologie – Personalisierung
description: Die Personalisierung verwendet Terminologie aus dem vertiefenden Lernen. Diese Begriffe werden im Azure-Portal und in den APIs verwendet.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: f75437c5afd5d3fd7f7570079be410d3db1ca8db
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624200"
---
# <a name="terminology"></a>Begriff

Die Personalisierung verwendet Terminologie aus dem vertiefenden Lernen. Diese Begriffe werden im Azure-Portal und in den APIs verwendet.

## <a name="conceptual-terminology"></a>Konzeptionelle Terminologie

* **Lernschleife**: Sie erstellen eine Personalisierungsressource, eine sogenannte _Lernschleife_, für jeden Teil Ihrer Anwendung, der von der Personalisierung profitieren kann. Wenn Sie mehr als eine zu personalisierende Erfahrung haben, erstellen Sie für jede eine Schleife.

* **Modell**: Ein Personalisierungsmodell erfasst alle Daten, die über Benutzerverhalten gelernt wurden, erhält Trainingsdaten aus der Kombination der Argumente, die Sie an die Rangfolge- und Relevanzaufrufe senden, und besitzt ein Trainingsverhalten, das von der Lernrichtlinie bestimmt wird.

## <a name="personalizer-configuration"></a>Personalisierungskonfiguration

Die Personalisierung wird über das [Azure-Portal](https://portal.azure.com) konfiguriert.

* **Relevanzen**: Konfigurieren Sie die Standardwerte für die Wartezeit für Relevanz, die Standardrelevanz und die Relevanzaggregationsrichtlinie.

* **Erkundung**: Konfigurieren Sie den Prozentsatz der Rangaufrufe, die für die Erkundung verwendet werden sollen

* **Häufigkeit der Modellaktualisierung**: Wie oft das Modell neu trainiert wird.

* **Datenaufbewahrung**: Wie viele Tage an Daten zu speichern sind. Dies kann sich auf Offlineauswertungen auswirken, die zur Verbesserung Ihrer Lernschleife verwendet werden.

## <a name="use-rank-and-reward-apis"></a>Verwenden von Rang- und Relevanz-APIs

* **Rang**: In Anbetracht der Aktionen mit Features und der Kontextfeatures verwenden Sie „explore“ (erkunden) oder „exploit“ (nutzen), um die oberste Aktion (Inhaltselement) zurückzugeben.

    * **Aktionen**: Aktionen sind die Inhaltselemente, z. B. Produkte oder Werbeaktionen, unter denen ausgewählt wird. Die Personalisierung wählt die Top-Aktion (zurückgegebene Relevanzaktions-ID) aus, die Ihren Benutzern über die Rang-API angezeigt werden soll.

    * **Context**: Um eine genauere Rangfolge bereitzustellen, geben Sie Informationen zu Ihrem Kontext an, beispielsweise:
        * Ihre Benutzer.
        * Das Gerät, das sie verwenden.
        * Die aktuelle Zeit.
        * Andere Daten über die aktuelle Situation.
        * Vergangene Daten über den Benutzer oder Kontext.

        Ihre spezifische Anwendung kann abweichende Kontextinformationen haben.

    * **[Merkmale](concepts-features.md)** : Eine Einheit von Informationen über ein Inhaltselement oder einen Benutzerkontext. Stellen Sie sicher, dass Sie nur aggregierte Features verwenden. Verwenden Sie keine bestimmten Zeiten, Benutzer-IDs oder andere nicht aggregierte Daten als Features.

        * Ein _Aktionsfeature_ sind Metadaten über den Inhalt.
        * Ein _Kontextfeature_ sind Metadaten über den Kontext, in dem der Inhalt angezeigt wird.

* **Exploration** (Erkunden): Der Personalisierungsdienst erkundet, wenn er, anstatt die beste Aktion zurückzugeben, eine andere Aktion für den Benutzer auswählt. Der Personalisierungsdienst kann durch Erkunden Verschiebungen und Stagnation vermeiden sowie sich an laufendes Benutzerverhalten anpassen.

* **Exploitation** (Nutzen): Der Personalisierungsdienst verwendet das aktuelle Modell, um über die beste Aktion auf Grundlage zurückliegender Daten zu entscheiden.

* **Experimentdauer**: Die Zeitspanne, die der Personalisierungsdienst auf eine Relevanz wartet, beginnend ab dem Zeitpunkt, zu dem der Rangfolgeaufruf für dieses Ereignis erfolgte.

* **Inaktive Ereignisse**: Ein inaktives Ereignis ist eines, für das Sie die Rangfolge aufgerufen haben, sich aber nicht sicher sind, ob der Benutzer jemals das Ergebnis sehen wird, weil dies Entscheidungen der Clientanwendung unterliegt. Inaktive Ereignisse ermöglichen es Ihnen, Personalisierungsergebnisse zu erstellen und zu speichern und dann zu entscheiden, sie später zu verwerfen, ohne dass dies das Machine Learning-Modell beeinträchtigt.


* **Relevanz**: Ein Maß dafür, wie der Benutzer auf die von der Rang-API zurückgegebene Relevanzaktions-ID reagiert hat, als eine Bewertung zwischen 0 und 1. Der Wert von 0 bis 1 wird von Ihrer Geschäftslogik festgelegt, basierend darauf, wie die Auswahl dabei geholfen hat, Ihre geschäftlichen Ziele der Personalisierung zu erreichen. Die Lernschleife speichert diese Relevanz nicht als individuellen Benutzerverlauf.

## <a name="offline-evaluations"></a>Offlineauswertungen

* **Auswertung**: Eine Offlineauswertung ermittelt die beste Lernrichtlinie für Ihre Schleife auf der Grundlage der Daten Ihrer Schleife.

* **Lernrichtlinie**: Die Art und Weise, wie die Personalisierung ein Modell bei jedem Ereignis trainiert, wird durch einige Parameter bestimmt, die die Funktionsweise des Machine Learning-Algorithmus beeinflussen. Eine neue Lernschleife beginnt mit einer standardmäßigen **Lernrichtlinie**, die eine moderate Leistung erzielen kann. Im Zuge von [Auswertungen](concepts-offline-evaluation.md) erstellt die Personalisierung neue Lernrichtlinien, die speziell für die Anwendungsfälle Ihrer Schleife optimiert sind. Mit Richtlinien, die im Zuge der Auswertung generiert und für die einzelnen spezifischen Schleifen optimiert wurden, verbessert sich die Leistung der Personalisierung erheblich. Die Lernrichtlinie wird im Bereich der **Modell- und Lerneinstellungen** für die Personalisierungsressource im Azure-Portal als _Lerneinstellungen_ bezeichnet.