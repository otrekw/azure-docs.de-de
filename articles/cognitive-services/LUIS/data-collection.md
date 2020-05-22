---
title: Datensammlung
description: Es wird beschrieben, welche Beispieldaten bei der Entwicklung Ihrer App erfasst werden sollten.
ms.topic: conceptual
ms.date: 05/06/2020
ms.openlocfilehash: 7abb3736eb9d7c73465ffa646b79e8e7dd7ae88b
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599326"
---
# <a name="data-collection-for-your-app"></a>Datensammlung für Ihre App

Für eine LUIS-App (Language Understanding) werden bei der App-Entwicklung Daten benötigt.

## <a name="data-used-in-luis"></a>Für LUIS verwendete Daten

Für LUIS wird Text als Datenelement zum Trainieren und Testen Ihrer LUIS-App genutzt, um die Klassifizierung für [Absichten](luis-concept-intent.md) und die Extraktion von [Entitäten](luis-concept-entity-types.md) zu ermöglichen. Sie benötigen ein ausreichend großes Dataset, damit genügend Daten zum Erstellen von separaten Datasets für das Training und für Tests vorhanden sind. Unten sind spezielle Informationen zur Vielfältigkeit und Verteilung angegeben.  Die Daten in diesen Datasets sollten sich nicht überlappen.

## <a name="training-data-selection-for-example-utterances"></a>Auswahl von Trainingsdaten für Beispieläußerungen

Wählen Sie Äußerungen für Ihren Trainingssatz basierend auf den folgenden Kriterien aus:

* **Reale Daten sind am besten geeignet**:
    * **Reale Daten aus der Clientanwendung**: Wählen Sie Äußerungen aus, bei denen es sich um reale Daten aus Ihrer Clientanwendung handelt.  Falls der Kunde heute ein Webformular mit seiner Anfrage sendet und Sie einen Bot entwickeln, können Sie mit den Daten des Webformulars beginnen.
    * **Per Crowdsourcing erworbene Daten**: Falls Sie nicht über vorhandene Daten verfügen, können Sie erwägen, Äußerungen per Crowdsourcing zu beschaffen.  Versuchen Sie, für Ihr Szenario Äußerungen per Crowdsourcing von Ihrer bestehenden Benutzerpopulation zu beschaffen, um für Ihre Anwendung eine möglichst gute Annäherung an die realen Daten zu erzielen. Menschliche Äußerungen, die per Crowdsourcing beschafft werden, sind besser als von Computern generierte Äußerungen geeignet.  Wenn Sie ein Dataset mit synthetischen Äußerungen erstellen, die anhand von bestimmten Mustern generiert werden, fehlen viele natürliche Varianten, die Sie bei menschlichen Äußerungen erhalten. In der Produktion ist dann keine gute Generalisierung möglich.
* **Vielfältigkeit der Daten**:
    * **Vielfältigkeit der Regionen**: Stellen Sie sicher, dass die Daten für jede Absicht so vielfältig wie möglich sind – auch in Bezug auf die _Formulierung_ (Wortwahl) und _Grammatik_.  Achten Sie beim Trainieren einer Absicht im Hinblick auf Human Resources-Richtlinien zu Urlaubstagen darauf, dass Sie über Äußerungen verfügen, die für die Ausdrücke aller von Ihnen verwalteten Regionen gelten.  In Europa wird beispielsweise `taking a holiday` verwendet, während die Aussage in den USA `taking vacation days` lautet.
    * **Sprachvielfalt**: Bei Benutzern mit unterschiedlichen Muttersprachen, die nicht in ihrer Muttersprache kommunizieren, sollten Sie über Äußerungen verfügen, die auf diesen Personenkreis zugeschnitten sind.
    * **Vielfältigkeit der Eingaben**: Prüfen Sie Ihren Dateneingabepfad. Wenn Sie Daten nur von einer Person, einer Abteilung oder einem Eingabegerät (Mikrofon) erfassen, mangelt es vermutlich an Vielfältigkeit, die wichtig ist, damit Ihre App Informationen über alle Eingabepfade erhält.
    * **Vielfältigkeit bei der Interpunktion**: Berücksichtigen Sie, dass Benutzer in Textanwendungen unterschiedliche Interpunktionsgrade verwenden, und achten Sie darauf, dass die Vielfältigkeit in Bezug auf die genutzte Interpunktion abgedeckt ist. Falls Sie Daten nutzen, die auf Sprache basieren, ist keine Interpunktion vorhanden, und dies sollte dann auch für Ihre Daten gelten.
* **Datenverteilung**: Stellen Sie sicher, dass die auf die Absichten verteilten Daten der Verteilung der Daten entsprechen, die von Ihrer Clientanwendung empfangen werden. Wenn von Ihrer LUIS-App Äußerungen klassifiziert werden, bei denen es sich um Anforderungen zur Urlaubsplanung handelt (50 %), aber auch Äußerungen mit Nachfragen zu den verbliebenen Urlaubstagen (20 %), Urlaubsgenehmigungen (20 %) und einigen anderen Bereichen bzw. allgemeinen Themen (10 %) vorkommen, sollte Ihr Dataset entsprechende Prozentsätze in Bezug auf die einzelnen Äußerungen aufweisen.
* **Verwendung aller Datenformen**: Wenn bei Ihrer LUIS-App Daten in unterschiedlichen Formen eingehen, sollten Sie sicherstellen, dass diese Formen in Ihren Äußerungen für das Training enthalten sind. Wenn für Ihre Clientanwendung sowohl Sprache als auch getippter Text als Eingabe vorkommt, müssen Sie sowohl über per Spracherkennung generierte Äußerungen als auch über getippte Äußerungen verfügen.  Sie können so unterschiedliche Varianten der Sprache und Eingabe von Benutzern und außerdem verschiedene Fehler bei der Spracherkennung sowie Tippfehler feststellen.  Alle diese Varianten sollten in Ihren Trainingsdaten widergespiegelt werden.
* **Positive und negative Beispiele**: Beim Trainieren einer LUIS-App muss ihr vermittelt werden, was jeweils die Absicht ist (positiv) bzw. nicht ist (negativ). Bei LUIS können Äußerungen nur für jeweils eine Absicht positiv sein. Wenn einer Absicht eine Äußerung hinzugefügt wird, macht LUIS diese Beispieläußerung für alle anderen Absichten automatisch zu einem negativen Beispiel.
* **Daten außerhalb des Anwendungsbereichs**: Falls für Ihre Anwendung Äußerungen vorkommen, die sich außerhalb der von Ihnen definierten Absichten bewegen, sollten Sie dafür sorgen, dass dies abgedeckt ist. Die Beispiele, die nicht einer bestimmten definierten Absicht zugewiesen sind, werden mit der Absichtsbezeichnung **None** (Keine) versehen.  Es ist wichtig, dass Sie realistische Beispiele für die Absicht **None** verwenden, um Äußerungen, die außerhalb des Bereichs mit den definierten Absichten liegen, richtig vorhersagen zu können.

    Es kann beispielsweise sein, dass Sie einen HR-Bot für die Urlaubsverwaltung erstellen und drei Absichten verwenden:
    * Urlaub planen oder bearbeiten
    * Verfügbare Urlaubstage abfragen
    * Urlaub genehmigen/ablehnen

    Stellen Sie sicher, dass Sie über Äußerungen für beide Absichten verfügen. Sorgen Sie außerdem dafür, dass potenzielle Äußerungen außerhalb dieses Bereichs abgedeckt sind, die von der Anwendung wie folgt bereitgestellt werden sollten:
    * `What are my medical benefits?`
    * `Who is my HR rep?`
    * `tell me a joke`
* **Seltene Beispiele**: Ihre App muss sowohl seltene als auch häufig vorkommende Beispiele aufweisen.  Wenn Ihre App niemals mit seltenen Beispielen in Berührung gekommen ist, kann sie diese in der Produktion auch nicht identifizieren. Bei Nutzung von realen Daten können Sie genauer vorhersagen, wie Ihre LUIS-App in der Produktion funktioniert.

### <a name="quality-instead-of-quantity"></a>Qualität anstelle von Quantität

Überprüfen Sie die Qualität Ihrer vorhandenen Daten, bevor Sie weitere Daten hinzufügen.  Bei LUIS nutzen Sie das so genannte „Machine Teaching“.  Von Ihrer LUIS-App wird eine Kombination aus Ihren Bezeichnungen und den von Ihnen definierten Machine Learning-Features verwendet.  Es wird nicht einfach nur die Quantität der Bezeichnungen berücksichtigt, um die bestmögliche Vorhersage zu treffen.  Der wichtigste Teil ist die Vielfältigkeit der Beispiele und deren Darstellung gemäß den Daten, die für Ihre LUIS-App in der Produktion anfallen.

### <a name="preprocessing-data"></a>Vorverarbeiten von Daten

Die folgenden Vorverarbeitungsschritte führen zu einer besseren LUIS-App:

* **Entfernen von Duplikaten**: Doppelte Äußerungen richten keinen Schaden an, aber sie sind auch keine Hilfe. Sie können also Aufwand in Bezug auf die Bezeichnungen sparen, indem Sie sie entfernen.
* **Anwenden der gleichen Vorverarbeitung für die Client-App**: Wenn Ihre Clientanwendung, von der der LUIS-Vorhersageendpunkt aufgerufen wird, die Datenverarbeitung vor dem Senden des Texts an LUIS zur Laufzeit anwendet, sollten Sie die LUIS-App anhand von Daten trainieren, die auf die gleiche Weise verarbeitet werden. Wenn für Ihre Clientanwendung beispielsweise die [Bing-Rechtschreibprüfung](../bing-spell-check/overview.md) genutzt wird, um für LUIS-Eingaben die Rechtschreibung zu korrigieren, sollten Sie Ihre Trainings- und Testäußerungen vor dem Hinzufügen zu LUIS entsprechend korrigieren.
* **Kein Anwenden von neuen Bereinigungsprozessen, die von der Client-App nicht verwendet werden**: Wenn Ihre Client-App per Sprache generierten Text direkt ohne Bereinigung (z. B. in Bezug auf Grammatik oder Interpunktion) akzeptiert, müssen Ihre Äußerungen dies auch so widerspiegeln – einschließlich der gesamten fehlenden Interpunktion und anderer relevanter Erkennungsfehler.
* **Kein Bereinigen von Daten**: Vermeiden Sie das Entfernen von fehlerhaften Eingaben, die aufgrund von Spracherkennungsfehlern, versehentlichen Tastenanschlägen oder Rechtschreibfehlern entstehen. Wenn für Ihre App Eingaben dieser Art auftreten können, ist es wichtig, dass sie entsprechend trainiert und getestet wird. Fügen Sie eine Absicht mit einer _fehlerhaften Eingabe_ hinzu, wenn Sie der Meinung sind, dass Ihre App Fehler nicht versteht. Versehen Sie diese Daten mit einer Bezeichnung, damit Ihre LUIS-App zur Laufzeit die richtige Antwort vorhersagen kann. Ihre Clientanwendung kann eine entsprechende Antwort für unverständliche Äußerungen auswählen. Beispiel: `Please try again`.

### <a name="labeling-data"></a>Bezeichnen von Daten

* **Bezeichnen von Text, als ob er fehlerfrei wäre**: Bei Beispieläußerungen sollten alle Formen einer Entität bezeichnet sein. Dies gilt für Text, der falsch geschrieben, falsch eingegeben oder falsch übersetzt wurde.

### <a name="data-review-after-luis-app-is-in-production"></a>Datenüberprüfung bei Nutzung der LUIS-App in der Produktion

[Überprüfen Sie die Endpunktäußerungen](luis-concept-review-endpoint-utterances.md), um den Datenverkehr für reale Äußerungen zu überwachen, nachdem Sie eine App in der Produktion bereitgestellt haben.  So können Sie Ihre Trainingsäußerungen mit realen Daten aktualisieren, um Ihre App zu verbessern. Alle Apps, die mit Crowdsourcing-Daten oder nicht realen Daten entwickelt werden, müssen basierend auf der realen Nutzung verbessert werden.

## <a name="test-data-selection-for-batch-testing"></a>Testdatenauswahl für Batchtests

Alle oben aufgeführten Prinzipien für das Trainieren von Äußerungen gelten für Äußerungen, die Sie für Ihre [Testsätze](luis-concept-batch-test.md) verwenden sollten. Stellen Sie sicher, dass die Verteilung auf Absichten und Entitäten die tatsächliche Verteilung möglichst genau widerspiegelt.

Verwenden Sie keine Äußerungen aus Ihrem Trainingssatz in Ihrem Testsatz. Dies führt zu einer schädlichen Beeinflussung Ihrer Ergebnisse, und Sie erhalten keine richtigen Hinweise dazu, wie sich Ihre LUIS-App in der Produktion verhält.

Nach der Veröffentlichung der ersten Version Ihrer App sollten Sie Ihren Testsatz mit Äußerungen aus realem Datenverkehr aktualisieren. So können Sie sicherstellen, dass Ihr Testsatz Ihre Verteilung in der Produktion widerspiegelt, und dann überwachen, ob eine realistische Leistung erzielt wird.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> Informieren Sie sich darüber, [wie LUIS Ihre Daten vor dem Erstellen von Vorhersagen ändert](luis-concept-data-alteration.md).
