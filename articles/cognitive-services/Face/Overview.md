---
title: Was ist der Azure-Gesichtserkennungsdienst?
titleSuffix: Azure Cognitive Services
description: Der Azure-Gesichtserkennungsdienst verfügt über KI-Algorithmen zum Ermitteln, Erkennen und Analysieren menschlicher Gesichter in Bildern.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 11/23/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: Gesichtserkennung, Gesichtserkennungssoftware, Gesichtsanalyse, Gesichtsabgleich, Gesichtserkennungs-App, Gesichtserkennungssuche nach Bild, Gesichtserkennungssuche
ms.openlocfilehash: 26076289d8c6659abdd55fa805c27b13690feccd
ms.sourcegitcommit: 20f8bf22d621a34df5374ddf0cd324d3a762d46d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107258740"
---
# <a name="what-is-the-azure-face-service"></a>Was ist der Azure-Gesichtserkennungsdienst?

> [!WARNING]
> Am 11. Juni 2020 kündigte Microsoft an, dass keine Technologie zur Gesichtserkennung mehr an die Polizeibehörden in den USA verkauft wird, bis es eine strenge, auf den Menschenrechten basierende Regelung gibt. Daher dürfen Kunden keine Gesichtserkennungsfeatures oder in Azure-Diensten enthaltenen Funktionen (wie Gesichtserkennung oder Video Indexer) verwenden, wenn es sich bei dem Kunden um eine Polizeibehörde in den USA handelt oder wenn der Kunde die Nutzung derartiger Dienste durch oder für eine Polizeibehörde zulässt.

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Der Azure-Gesichtserkennungsdienst verfügt über KI-Algorithmen zum Ermitteln, Erkennen und Analysieren menschlicher Gesichter in Bildern. Gesichtserkennungssoftware ist für viele unterschiedliche Szenarien wichtig, z. B. Sicherheit, natürliche Benutzeroberfläche, Analyse und Verwaltung von Bildinhalten, mobile Apps und Robotik.

Der Gesichtserkennungsdienst verfügt über verschiedene Funktionen für die Gesichtsanalyse, die in den folgenden Abschnitten beschrieben werden.

Diese Dokumentation enthält die folgenden Arten von Artikeln:
* In den [Schnellstarts](./Quickstarts/client-libraries.md) finden Sie Schritt-für-Schritt-Anleitungen, mit denen Sie Aufrufe an den Dienst senden können und in kurzer Zeit Ergebnisse erhalten. 
* Die [Anleitungen](./Face-API-How-to-Topics/HowtoDetectFacesinImage.md) enthalten Anweisungen zur spezifischeren oder individuelleren Verwendung des Diensts.
* Die [konzeptionellen Artikel](./concepts/face-detection.md) enthalten ausführliche Erläuterungen der Funktionen und Features eines Diensts.
* Die [Tutorials](./Tutorials/FaceAPIinCSharpTutorial.md) sind ausführlichere Leitfäden, in denen die Verwendung dieses Diensts als Komponente in umfassenderen Unternehmenslösungen veranschaulicht wird.

## <a name="face-detection"></a>Gesichtserkennung

Mit der Erkennungs-API können menschliche Gesichter in einem Bild erkannt und die Rechteckkoordinaten ihrer Positionen zurückgegeben werden. Optional können bei der Gesichtserkennung verschiedene Gesichtsattribute extrahiert werden, z. B. Kopfhaltung, Geschlecht, Alter, Emotionen, Gesichtsbehaarung und Brille. Bei diesen Attributen handelt es sich um allgemeine Vorhersagen, nicht um tatsächliche Klassifizierungen. 

> [!NOTE]
> Die Funktion zur Gesichtserkennung ist auch über den [Dienst für maschinelles Sehen](../computer-vision/overview.md) verfügbar. Wenn Sie jedoch weitere Gesichtserkennungsvorgänge wie „Identifizieren“, „Überprüfen“, „Ähnliches suchen“ oder „Gruppieren“ verwenden möchten, sollten Sie stattdessen diesen Gesichtserkennungsdienst verwenden.

![Bild von einer Frau und einem Mann mit um die Gesichter gezeichneten Rechtecken und Anzeige von Alter und Geschlecht](./Images/Face.detection.jpg)

Weitere Informationen zur Gesichtserkennung finden Sie im Artikel zu den Konzepten der [Gesichtserkennungs-API](concepts/face-detection.md). Lesen Sie auch die Referenzdokumentation zur [Erkennungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Gesichtsüberprüfung

Die Überprüfungs-API basiert auf der Erkennung und geht der folgenden Frage nach: „Handelt es sich bei diesen beiden Bildern um die gleiche Person?“. Die Überprüfung wird auch als 1:1-Abgleich bezeichnet, da das Testbild mit nur einer registrierten Vorlage verglichen wird. Die Überprüfung kann in Identitätsüberprüfungs- oder Zugriffssteuerungsszenarien verwendet werden, um zu überprüfen, ob ein Bild mit einem zuvor aufgenommenen Bild übereinstimmt (z. B. ein Foto von einem von einer Behörde ausgegebenen Ausweis). Weitere Informationen finden Sie im Konzeptleitfaden zur [Gesichtserkennung](concepts/face-recognition.md) oder in der Referenzdokumentation zur [Überprüfungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="face-identification"></a>Gesichtsidentifizierung

Die Identifizierungs-API beginnt ebenfalls mit der Erkennung und beantwortet die Frage „Stimmt dieses erkannte Gesicht mit einem registrierten Gesicht in einer Datenbank überein?“. Der Vorgang ähnelt der Gesichtserkennungssuche und wird daher auch als 1:n-Abgleich bezeichnet. Übereinstimmungskandidaten werden basierend darauf zurückgegeben, wie genau die Testvorlage mit dem erkannten Gesicht den einzelnen registrierten Vorlagen entspricht.

Die folgende Abbildung enthält ein Beispiel für eine Datenbank mit dem Namen `"myfriends"`. Jede Gruppe kann bis zu 1 Million unterschiedliche Personenobjekte enthalten. Für jedes Personenobjekt können bis zu 248 Gesichter registriert sein.

![Raster mit drei Spalten für unterschiedliche Personen, jeweils mit drei Zeilen mit Bildern von Gesichtern](./Images/person.group.clare.jpg)

Nachdem Sie eine Datenbank erstellt und trainiert haben, können Sie die Identifizierung eines neuen erkannten Gesichts für die Gruppe durchführen. Wenn das Gesicht als eine Person aus der Gruppe erkannt wird, wird das Personenobjekt zurückgegeben.

Weitere Informationen zur Personenidentifizierung finden Sie im Konzeptleitfaden zur [Gesichtserkennung](concepts/face-recognition.md) oder in der Referenzdokumentation zur [Identify-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="find-similar-faces"></a>Suchen ähnlicher Gesichter

Die „Find Similar API“ (API „Ähnliches suchen“) ermöglicht einen Abgleich zwischen einem Zielgesicht und mehreren Kandidatengesichtern, um eine Untermenge von Gesichtern zu ermitteln, die dem Zielgesicht ähnlich sind. Dies ist hilfreich, wenn eine Gesichtserkennungssuche anhand von Bildern durchgeführt werden soll. 

Es werden zwei Arbeitsmodi unterstützt: **matchPerson** und **matchFace**. Im Modus **matchPerson** werden ähnliche Gesichter zurückgegeben, nachdem mit der [Überprüfungs-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) nach derselben Person gefiltert wurde. Im Modus **matchFace** wird der Filter für dieselbe Person ignoriert. Er gibt eine Liste mit ähnlichen Kandidatengesichtern zurück, die nicht unbedingt zu derselben Person gehören müssen.

Das folgende Beispiel zeigt das Zielgesicht:

![Lächelnde Frau](./Images/FaceFindSimilar.QueryFace.jpg)

Und diese Bilder sind die Kandidatengesichter:

![Fünf Bilder mit lächelnden Personen. Die Bilder a und b gehören zu derselben Person.](./Images/FaceFindSimilar.Candidates.jpg)

Für die Suche nach vier ähnlichen Gesichtern gibt der Modus **matchPerson** a und b zurück. Diese zeigen dieselbe Person wie das Zielgesicht. Im Modus **matchFace** werden a, b, c und d zurückgegeben, also genau vier Kandidaten – auch wenn es sich teilweise um andere Personen handelt oder die Ähnlichkeit nur gering ist. Weitere Informationen finden Sie im Konzeptleitfaden zur [Gesichtserkennung](concepts/face-recognition.md) oder in der Referenzdokumentation zur [Find Similar-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Gesichtsgruppierung

Mit der API für die Gruppierung wird eine Gruppe von unbekannten Gesichtern anhand der Ähnlichkeit in mehrere Untergruppen aufgeteilt. Jede Gruppe ist eine zusammenhanglose korrekte Teilmenge der ursprünglichen Gesichtergruppe. Alle Gesichter in einer Gruppe gehören wahrscheinlich zur selben Person. Es kann mehrere verschiedene Gruppen für eine einzelne Person geben. Die Gruppen werden durch einen anderen Faktor unterschieden, z. B. den Ausdruck. Weitere Informationen finden Sie im Konzeptleitfaden zur [Gesichtserkennung](concepts/face-recognition.md) oder in der Referenzdokumentation zur [Group-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).


## <a name="sample-apps"></a>Beispiel-Apps

Die folgenden Beispielanwendungen zeigen einige Verwendungsmöglichkeiten des Gesichtserkennungsdiensts:

- [Gesichtserkennungs-API: Windows Client Library and sample](https://github.com/Microsoft/Cognitive-Face-Windows) (Microsoft-Gesichtserkennungs-API: Windows-Clientbibliothek und -Beispiel): Eine WPF-App, mit der verschiedene Szenarien der Gesichtserkennung, -analyse und -identifizierung veranschaulicht werden.
- [FamilyNotes UWP app](https://github.com/Microsoft/Windows-appsample-familynotes) (FamilyNotes-UWP-App): Eine UWP-App (Universelle Windows-Plattform), bei der die Gesichtsidentifizierung zusammen mit Spracheingabe, Cortana, Freihand und Kamera in einem Szenario zum Austauschen von Familiennotizen genutzt wird.

## <a name="data-privacy-and-security"></a>Datenschutz und Sicherheit

Wie bei allen Cognitive Services-Ressourcen müssen Entwickler, die den Gesichtserkennungsdienst nutzen, die Microsoft-Richtlinien zu Kundendaten beachten. Weitere Informationen finden Sie im Microsoft Trust Center auf der [Seite zu Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices).

## <a name="next-steps"></a>Nächste Schritte

Befolgen Sie eine Schnellstartanleitung zum Codieren der Basiskomponenten einer Gesichtserkennungs-App in der Sprache Ihrer Wahl.

- [Schnellstart: Clientbibliothek](quickstarts/client-libraries.md)
