---
title: LUIS-Glossar
titleSuffix: Azure Cognitive Services
description: In diesem Glossar werden Begriffe erläutert, auf die Sie möglicherweise beim Arbeiten mit der LUIS-API stoßen.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: f764b0d42e08e68b45d49a5aae8542f05707ccde
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82099358"
---
# <a name="language-understanding-glossary-of-common-vocabulary-and-concepts"></a>Language Understanding-Glossar mit allgemeinem Vokabular und Konzepten
In diesem Glossar für Language Understanding (LUIS) werden Begriffe erläutert, auf die Sie möglicherweise beim Arbeiten mit der LUIS-API stoßen.

## <a name="active-version"></a><a name="active-version"></a>Aktive Version

Die aktive LUIS-Version ist die Version, in der alle Änderungen am Modell vorgenommen werden. Wenn Sie im [LUIS](luis-reference-regions.md)-Portal Änderungen an einer Version vornehmen möchten, bei der es sich nicht um die aktive Version handelt, müssen Sie diese Version zunächst als aktive Version festlegen.

## <a name="authoring"></a><a name="authoring"></a>Erstellung

Bei der Erstellung handelt es sich um die Fähigkeit zum Erstellen, Verwalten und Bereitstellen einer [LUIS-App](#luis-app) – entweder über das [LUIS](luis-reference-regions.md)-Portal oder über die [Erstellungs-APIs](https://go.microsoft.com/fwlink/?linkid=2092087).

## <a name="authoring-key"></a><a name="authoring-key"></a>Erstellungsschlüssel

Früherer Name: „Programmierschlüssel“. Er wird zum Erstellen der App verwendet. Er sollte nicht für Endpunktabfragen auf Produktionsebene verwendet werden. Weitere Informationen finden Sie unter [Schlüsselgrenzwerte](luis-limits.md#key-limits).

## <a name="batch-test-json-file"></a><a name="batch-test-json-file"></a>JSON-Datei für Batchtests

Batchtests ermöglichen die Überprüfung des Modells einer aktuellen LUIS-App mit einem konsistenten und bekannten Testsatz von Benutzeräußerungen. Der Batchtest ist in einer [Datei im JSON-Format](luis-concept-batch-test.md#batch-file-format) definiert.

Weitere Informationen:
* [Konzepte](luis-concept-batch-test.md)
* [Anleitung](luis-how-to-batch-test.md)
* [Tutorial](luis-tutorial-batch-testing.md)

## <a name="collaborator"></a><a name="collaborator"></a>Projektmitarbeiter

Ein Mitarbeiter/Mitwirkender ist nicht der [Besitzer](#owner) der App, er verfügt aber über dieselben Berechtigungen zum Hinzufügen, Bearbeiten und Löschen der Absichten, Entitäten und Äußerungen.

## <a name="contributor"></a><a name="contributor"></a>Mitwirkender

Ein Mitwirkender ist das gleiche wie ein [Mitarbeiter](#collaborator).

## <a name="descriptor"></a><a name="descriptor"></a>Deskriptor

Ein Deskriptor ist ein [Feature](#features), das zur Trainingszeit auf ein Modell angewendet wird, einschließlich [Ausdruckslisten](#phrase-list) und [Entitäten](#entity).

## <a name="domain"></a><a name="domain"></a>Domäne

Im LUIS-Kontext ist eine **Domäne** ein Themenbereich. Die Domäne bezieht sich auf die inhaltlichen Themen Ihrer App. Sie kann ein allgemeines Gebiet umfassen, wie z.B. bei einer Reise-App. Eine Reise-App kann sich auch nur auf die Informationsbereiche beziehen, die für Ihr Unternehmen wichtig sind, z.B. bestimmte geografische Regionen, Sprachen und Dienste.

## <a name="endpoint"></a><a name="endpoint"></a>Endpunkt

In der [LUIS-Endpunkt](https://go.microsoft.com/fwlink/?linkid=2092356)-URL übermitteln Sie LUIS Abfragen, nachdem die [LUIS-App](#luis-app) erstellt und veröffentlicht wurde. Die Endpunkt-URL enthält die Region der veröffentlichten App und die App-ID. Sie finden den Endpunkt auf der Seite **[Schlüssel und Endpunkte](luis-how-to-azure-subscription.md)** Ihrer App, oder Sie können die Endpunkt-URL bei der [Get App Info](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c37)-API abrufen.

## <a name="entity"></a><a name="entity"></a>Entität

[Entitäten](luis-concept-entity-types.md) sind wichtige Wörter in [Äußerungen](luis-concept-utterance.md). Sie beschreiben Informationen mit Bezug zur [Absicht](luis-concept-intent.md), die manchmal auch für diese wichtig sind. Eine Entität ist im Wesentlichen einen Datentyp in LUIS.

## <a name="f-measure"></a><a name="f-measure"></a>F-Measure

Bei [Batchtests](luis-interactive-test.md#batch-testing) ein Maß für die Testgenauigkeit.

## <a name="false-negative-fn"></a><a name="false-negative"></a>Falsch negatives Ergebnis (FN)

Bei [Batchtests](luis-interactive-test.md#batch-testing) stellen die Datenpunkte Äußerungen dar, in denen Ihre App das Fehlen der Zielabsicht/-entität falsch vorhergesagt hat.

## <a name="false-positive-fp"></a><a name="false-positive"></a>Falsch positives Ergebnis (FP)

Bei [Batchtests](luis-interactive-test.md#batch-testing) stellen die Datenpunkte Äußerungen dar, in denen Ihre App das Vorhandensein der Zielabsicht/-entität falsch vorhergesagt hat.

## <a name="features"></a><a name="features"></a>Feature

Beim maschinellen Lernen ist ein [Feature](luis-concept-feature.md) ein eindeutiges Merkmal oder Attribut der Daten, die Ihr System untersucht.

## <a name="intent"></a><a name="intent"></a>Absicht

Eine [Absicht](luis-concept-intent.md) stellt eine Aufgabe oder Aktion dar, die der Benutzer ausführen möchte. Sie ist ein vom Benutzer in seiner Eingabe genannter Zweck oder ein Ziel, wie das Buchen eines Flugs, das Bezahlen einer Rechnung oder das Suchen nach einem Nachrichtenartikel. In LUIS basiert die Vorhersage von Absichten auf der gesamte Äußerung. Entitäten sind im Vergleich dazu Teile einer Äußerung.

## <a name="labeling"></a><a name="labeling"></a>Bezeichnung

Beim Bezeichnen (oder Markieren) wird ein Wort oder Ausdruck in der [Äußerung](#utterance) einer Absicht einer [Entität](#entity) (Datentyp) zugeordnet.

## <a name="luis-app"></a><a name="luis-app"></a>LUIS-App

Eine LUIS-App ist eine Sammlung von Sprachmodellen für die Verarbeitung natürlicher Sprache, einschließlich [Absichten](#intent), [Entitäten](#entity) und bezeichneten [Äußerungen](#utterance).

## <a name="owner"></a><a name="owner"></a>Besitzer

Jede App hat einen Besitzer. Dies ist die Person, die die App erstellt hat. Der Besitzer kann [Projektmitarbeiter](#collaborator) hinzufügen.

## <a name="patterns"></a><a name="pattern"></a>Muster
Das frühere Muster-Feature wurde durch [Muster](luis-concept-patterns.md) ersetzt. Verwenden Sie Muster, um die Vorhersagegenauigkeit zu verbessern, während Sie weniger Trainingsbeispiele bereitstellen müssen.

## <a name="phrase-list"></a><a name="phrase-list"></a>Ausdrucksliste

Eine [Ausdrucksliste](luis-concept-feature.md) enthält eine Gruppe von Werten (Wörter oder Ausdrücke), die derselben Klasse angehören und auf ähnliche Weise behandelt werden sollen (z.B. Städte- oder Produktnamen). Die Elemente einer austauschbaren Liste werden als Synonyme behandelt.

## <a name="prebuilt-domain"></a><a name="prebuilt-domains"></a>Vordefinierte Domäne

Ein [vordefinierte Domäne](luis-how-to-use-prebuilt-domains.md) ist eine LUIS-App, die für einen bestimmten Themenbereich wie z.B. die Heimautomatisierung (HomeAutomation) oder Restaurantreservierungen (RestaurantReservation) konfiguriert wurde. Die Absichten, Äußerungen und Entitäten werden für diese Domäne konfiguriert.

## <a name="prebuilt-entity"></a><a name="prebuilt-entity"></a>Vordefinierte Entität

Eine [vordefinierte Entität](luis-prebuilt-entities.md) ist eine Entität, die LUIS für allgemeine Arten von Informationen bereitstellt, z.B. „number“, „URL“ und „email“. Sie können Ihrer Anwendung vordefinierte Entitäten hinzufügen.

## <a name="precision"></a><a name="precision"></a>Genauigkeit
Bei [Batchtests](luis-interactive-test.md#batch-testing) stellt die Genauigkeit (auch als positiver Vorhersagewert bezeichnet) den Anteil der relevanten Äußerungen unter den abgerufenen Äußerungen dar.

## <a name="programmatic-key"></a><a name="programmatic-key"></a>Programmierschlüssel

Umbenannt in [Erstellungsschlüssel](#authoring-key).

## <a name="publish"></a><a name="publish"></a>Veröffentlichen

Veröffentlichen bedeutet, dass die aktive Version einer LUIS-App auf einem [Endpunkt](#endpoint) für Staging oder Produktion verfügbar gemacht wird.

## <a name="quota"></a><a name="quota"></a>Kontingent

Das LUIS-Kontingent ist die Einschränkung durch den [Azure-Abonnementtarif](https://aka.ms/luis-price-tier). Das LUIS-Kontingent kann sowohl im Hinblick auf die Anforderungen pro Sekunde (HTTP-Status 429) als auch auf die Summe der Anforderungen in einem Monat (HTTP-Status 403) eingeschränkt werden.

## <a name="recall"></a><a name="recall"></a>Recall (Trefferquote)
Bei [Batchtests](luis-interactive-test.md#batch-testing) stellt der Recall (auch als Empfindlichkeit bezeichnet) die Fähigkeit von LUIS zu generalisieren dar.

## <a name="semantic-dictionary"></a><a name="semantic-dictionary"></a>Semantisches Wörterbuch
Ein semantisches Wörterbuch wird auf der Seite für Listenentitäten und auch auf der für Ausdruckslisten bereitgestellt. Das semantische Wörterbuch enthält Vorschläge für Wörter basierend auf dem aktuellen Bereich.

## <a name="sentiment-analysis"></a><a name="sentiment-analysis"></a>Standpunktanalyse
Die Standpunktanalyse (Stimmungsanalyse) liefert durch eine [Textanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) positive oder negative Werte zu Äußerungen.

## <a name="speech-priming"></a><a name="speech-priming"></a>Sprachvorbereitung

Mithilfe der Sprachvorbereitung können Sie Ihren Sprachdienst mit Ihrem LUIS-Modell vorbereiten.

## <a name="spelling-correction"></a><a name="spelling-correction"></a>Rechtschreibkorrektur

Aktivieren Sie die Bing-Rechtschreibprüfung, damit falsch geschriebene Wörter in Äußerungen vor der Vorhersage korrigiert werden.

## <a name="starter-key"></a><a name="starter-key"></a>Startschlüssel

Ein öffentlicher Schlüssel für die ersten Gehversuche in LUIS.

## <a name="structure"></a><a name="structure"></a>Struktur

Fügen Sie einer maschinell gelernten Entität eine Struktur hinzu, um Unterkomponenten mit Deskriptoren (Features) und Einschränkungen (regulären Ausdrücken oder Listenentitäten) bereitzustellen.

## <a name="subscription-key"></a><a name="subscription-key"></a>Abonnementschlüssel

Der Abonnementschlüssel ist der dem LUIS-Dienst zugewiesene **Vorhersageendpunktschlüssel**, [den Sie in Azure erstellt haben](luis-how-to-azure-subscription.md). Dieser Schlüssel ist nicht der [Erstellungsschlüssel](#programmatic-key). Wenn Sie einen Endpunktschlüssel besitzen, sollte dieser anstelle des Erstellungsschlüssels für alle Endpunktanforderungen verwendet werden. Sie finden Ihren aktuellen Endpunktschlüssel in der Endpunkt-URL im unteren Bereich der Seite [**Schlüssel und Endpunkte**](luis-how-to-azure-subscription.md) auf der [LUIS](luis-reference-regions.md)-Website. Es ist der Wert des Name-Wert-Paars **subscription-key**.

## <a name="test"></a><a name="test"></a>Testen

Das [Testen](luis-interactive-test.md#test-your-app) einer LUIS-App bedeutet das Übergeben einer Äußerung an LUIS und das Anzeigen der JSON-Ergebnisse.

## <a name="timezone-offset"></a><a name="timezoneoffset"></a>Zeitzonenunterschied

Der Endpunkt enthält einen Zeitzonenunterschied (timezoneOffset). Dies ist die Anzahl Minuten, die Sie in der vordefinierten Entität datetimeV2 hinzufügen oder entfernen sollten. Wenn die Äußerung z.B. „Wie spät ist es?“ lautet, wird als datetimeV2-Wert die aktuelle Uhrzeit bei der Clientanforderung zurückgegeben. Wenn Ihre Clientanforderung von einem Bot oder einer anderen Anwendung stammt, die nicht mit Ihrem Botbenutzer übereinstimmt, sollten Sie den Zeitversatz zwischen dem Bot und dem Benutzer übergeben.

Weitere Informationen finden Sie unter [Ändern der Zeitzone von vordefinierten datetimeV2-Entitäten](luis-concept-data-alteration.md?#change-time-zone-of-prebuilt-datetimev2-entity).

## <a name="token"></a><a name="token"></a>Token
Ein Token ist die kleinste Einheit, die in einer Entität bezeichnet werden kann. Die Tokenisierung erfolgt auf Grundlage der [Kultur](luis-language-support.md#tokenization) der Anwendung.

## <a name="train"></a><a name="train"></a>Trainieren

Unter Training versteht man, LUIS alle Änderungen an der aktiven Version seit dem letzten Training beizubringen.

## <a name="true-negative-tn"></a><a name="true-negative"></a>Richtig negatives Ergebnis (TN)

Bei [Batchtests](luis-interactive-test.md#batch-testing) stellen die Datenpunkte Äußerungen dar, in denen Ihre App das Fehlen der Zielabsicht/-entität richtig vorhergesagt hat.

## <a name="true-positive-tp"></a><a name="true-positive"></a>Richtig positives Ergebnis (TP)

Bei [Batchtests](luis-interactive-test.md#batch-testing) stellen die Datenpunkte Äußerungen dar, in denen Ihre App das Vorhandensein der Zielabsicht/-entität richtig vorhergesagt hat.

## <a name="utterance"></a><a name="utterance"></a>Äußerung

Eine Äußerung ist eine Aussage in natürlicher Sprache, z.B. „Buche 2 Tickets nach Seattle am nächsten Dienstag“. Sie fügen der Absicht Beispieläußerungen hinzu.

## <a name="version"></a><a name="version"></a>Version

Eine LUIS-[Version](luis-how-to-manage-versions.md) ist ein spezifisches Datenmodell, das einer LUIS-App-ID und dem veröffentlichten Endpunkt zugeordnet ist. Jede LUIS-App hat mindestens eine Version.
