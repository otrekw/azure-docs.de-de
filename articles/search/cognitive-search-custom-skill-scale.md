---
title: Skalieren und Verwalten eines benutzerdefinierten Skills
titleSuffix: Azure Cognitive Search
description: Hier erfahren Sie mehr über die Tools und Techniken zum effizienten Aufskalieren eines benutzerdefinierten Skills, um einen möglichst hohen Durchsatz zu erzielen. Benutzerdefinierte Skills rufen benutzerdefinierte KI-Modelle oder -Logik auf, die Sie einer um KI erweiterten Indizierungspipeline in Azure Cognitive Search hinzufügen können.
manager: luisca
author: vkurpad
ms.author: vikurpad
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/28/2021
ms.openlocfilehash: f80e78da5c7d226c704c8b13fc496e416f5d0f0d
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108125587"
---
# <a name="efficiently-scale-out-a-custom-skill"></a>Effizientes Aufskalieren eines benutzerdefinierten Skills

Benutzerdefinierte Skills sind Web-APIs, die eine bestimmte Schnittstelle implementieren. Ein benutzerdefinierter Skill kann für jede öffentlich adressierbare Ressource implementiert werden. Die gängigsten Implementierungen für benutzerdefinierte Skills:
* Azure Functions für benutzerdefinierte Logikskills
* Azure-Web-Apps für einfache Container-KI-Skills
* Azure Kubernetes-Dienst für komplexere oder umfassendere Skills

## <a name="prerequisites"></a>Voraussetzungen

+ Unter [Schnittstelle für benutzerdefinierte Qualifikationen](cognitive-search-custom-skill-interface.md) finden Sie eine Einführung in die Eingabe/Ausgabe-Schnittstelle, die eine benutzerdefinierte Qualifikation implementieren sollte.

+ Einrichten Ihrer Umgebung. Zunächst können Sie [dieses Tutorial von Anfang bis Ende](../azure-functions/create-first-function-vs-code-python.md) durcharbeiten, um die serverlose Azure Function unter Verwendung von Visual Studio Code und Python-Erweiterungen einzurichten.

## <a name="skillset-configuration"></a>Skillsetkonfiguration

Voraussetzung für das Konfigurieren eines benutzerdefinierten Skills zum Maximieren des Durchsatzes des Indizierungsprozesses ist ein Verständnis des Skills, der Indexerkonfigurationen und der Beziehung des Skills zu den einzelnen Dokumenten. Dies umfasst beispielsweise die Anzahl der Aufrufe eines Skills pro Dokument und die erwartete Dauer pro Aufruf.

### <a name="skill-settings"></a>Skilleinstellungen

Legen Sie für den [benutzerdefinierten Skill](cognitive-search-custom-skill-web-api.md) die folgenden Parameter fest.

1. Legen Sie `batchSize` des benutzerdefinierten Skills fest, um die Anzahl der Datensätze zu konfigurieren, die in einem einzelnen Aufruf des Skills an den Skill gesendet werden.

2. Legen Sie `degreeOfParallelism` fest, um die Anzahl der parallel ausgeführten Anforderungen einzustellen, die der Indexer an den Skill ausgibt.

3. Legen Sie `timeout` auf einen Wert fest, der groß genug ist, um eine gültige Antwort des Skills zu erhalten.

4. Legen Sie in der Definition des `indexer` [`batchSize`](/rest/api/searchservice/create-indexer#indexer-parameters) auf die Anzahl der Dokumente fest, die aus der Datenquelle gelesen und gleichzeitig angereichert werden sollen.

### <a name="considerations"></a>Überlegungen

Um die Indexerleistung durch Festlegen dieser Variablen zu optimieren, müssen Sie ermitteln, ob Ihre Skills bei vielen parallel ausgeführten kleinen Anforderungen oder bei wenigen großen Anforderungen eine bessere Leistung zeigen. Orientieren Sie sich an den folgenden Fragen:

* Welche Kardinalität weist der Skillaufruf auf? Wird der Skill für jedes Dokument einmal ausgeführt, z. B. bei einem Skill zur Dokumentklassifizierung, oder kann der Skill pro Dokument mehrfach ausgeführt werden wie bei einem Skill zur Absatzklassifizierung?

* Wie viele Dokumente werden durchschnittlich aus der Datenquelle gelesen, um eine Skillanforderung auf Grundlage der Skillbatchgröße zu erfüllen? Im Idealfall sollte dieser Wert kleiner als die Batchgröße des Indexers sein. Bei Batchgrößen von mehr als 1, kann es vorkommen, dass der Skill Datensätze von mehreren Quelldokumenten empfängt. Wenn die Indexerbatchgröße beispielsweise 5 beträgt und die die Skillbatchanzahl 50 lautet und jedes Dokument nur fünf Datensätze generiert, benötigt der Indexer mehrere Indexerbatches, um die Anforderung eines benutzerdefinierten Skills zu erfüllen.

* Die durchschnittliche Anzahl von Anforderungen, die ein Indexerbatch generieren kann, sollte eine optimale Einstellung für den Grad an Parallelität ergeben. Wenn Ihre Infrastruktur, in der der Skill gehostet wird, diesen Grad an Parallelität nicht unterstützen kann, sollten Sie den Grad an Parallelität verringern. Als bewährte Methode sollten Sie Ihre Konfiguration mit einigen wenigen Dokumenten testen, um die Auswahl der Parameter zu überprüfen.

* Messen Sie beim Testen mit einer geringeren Anzahl von Dokumenten die Ausführungszeit des Skills im Verhältnis zur Gesamtzeit, die für die Verarbeitung der Teilmenge von Dokumenten benötigt wird. Verbringt der Indexer mehr Zeit mit der Erstellung eines Batches als mit dem Warten auf eine Antwort vom Skill? 

* Berücksichtigen Sie die Auswirkungen der Parallelität auf den Upstream. Wenn die Eingabe für einen benutzerdefinierten Skill die Ausgabe aus einem zuvor ausgeführten Skill ist, werden alle Skills im Skillset effektiv aufskaliert, um eine möglichst geringe Latenz zu erzielen?

## <a name="error-handling-in-the-custom-skill"></a>Fehlerbehandlung im benutzerdefinierten Skill

Benutzerdefinierte Skills sollten den Erfolgsstatuscode „HTTP 200“ zurückgeben, wenn der Skill erfolgreich ausgeführt wurde. Wenn mindestens ein Datensatz in einem Batch einen Fehler verursacht, sollten Sie den Multistatuscode 207 zurückgeben. Die Liste der Fehler oder Warnungen für den Datensatz sollten eine entsprechende Meldung enthalten.

Alle Elemente in einem Batch, die Fehler verursachen, führen zu einem Fehler bei dem entsprechenden Dokument. Wenn das Dokument erfolgreich verarbeitet werden muss, geben Sie eine Warnung zurück.

Jeder Statuscode über 299 wird als Fehler ausgewertet, und alle Anreicherungen sind fehlgeschlagen und führen zu einem Dokumentfehler. 

### <a name="common-error-messages"></a>Häufige Fehlermeldungen

* `Could not execute skill because it did not execute within the time limit '00:00:30'. This is likely transient. Please try again later. For custom skills, consider increasing the 'timeout' parameter on your skill in the skillset.` Legen Sie den Timeoutparameter für den Skill für eine längere Ausführungsdauer auf „Zulassen“ fest.

* `Could not execute skill because Web Api skill response is invalid.` Ein Hinweis darauf, dass der Skill keine Meldung im Antwortformat des benutzerdefinierten Skills zurückgibt. Dies kann auf eine nicht abgefangene Ausnahme im Skill zurückzuführen sein.

* `Could not execute skill because the Web Api request failed.` Wahrscheinlich durch Autorisierungsfehler oder -ausnahmen verursacht.

* `Could not execute skill.` In der Regel eine Folge der Zuordnung der Skillantwort zu einer vorhandenen Eigenschaft in der Dokumenthierarchie.

## <a name="testing-custom-skills"></a>Testen von benutzerdefinierten Skills

Testen Sie Ihre benutzerdefinierten Skills zunächst mit einem REST-API-Client, um Folgendes zu überprüfen:

* Der Skill implementiert die Schnittstelle des benutzerdefinierten Skills für Anforderungen und Antworten.

* Der Skill gibt gültigen JSON-Code mit dem MIME-Typ `application/JSON` zurück.

* Er gibt einen gültigen HTTP-Statuscode zurück.

Erstellen Sie eine [Debugsitzung](cognitive-search-debug-session.md), um den Skill Ihrem Skillset hinzuzufügen, und stellen Sie sicher, dass er eine gültige Anreicherung ausgibt. Obwohl eine Debugsitzung keine Optimierung der Skillleistung ermöglicht, können Sie so sicherstellen, dass der Skill mit gültigen Werten konfiguriert ist und die erwarteten angereicherten Objekte zurückgibt.

## <a name="best-practices"></a>Bewährte Methoden

* Obwohl Skills größere Nutzlasten akzeptieren und zurückgeben können, sollten Sie die Antwort auf 150 MB oder weniger einschränken, wenn sie JSON-Code zurückgeben.

* Legen Sie die Batchgröße für den Indexer und den Skill fest, um sicherzustellen, dass jeder Datenquellenbatch eine vollständige Nutzlast für den Skill generiert.

* Legen Sie für zeitintensive Aufgaben den Timeout auf einen ausreichend hohen Wert fest, um sicherzustellen, dass der Indexer bei der gleichzeitigen Verarbeitung von Dokumenten keine Fehler verursacht.

* Optimieren Sie die Batchgröße des Indexers und des Skills sowie den Grad an Parallelität, um das Auslastungsmuster zu generieren, das der Skill erwartet, d. h. wenige große Anforderungen oder viele kleine Anforderungen.

* Überwachen Sie benutzerdefinierte Skills mit ausführlichen Protokollen für Fehler, da Szenarien auftreten können, in denen bestimmte Anforderungen infolge der Variabilität der Daten durchgängig Fehler verursachen.


## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Ihren benutzerdefinierten Skill jetzt so skaliert, dass der Durchsatz im Indexer möglichst hoch ist. 

+ [Power Skills: ein Repository benutzerdefinierter Skills](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Hinzufügen einer benutzerdefinierten Qualifikation zu einer KI-Anreicherungspipeline](cognitive-search-custom-skill-interface.md)
+ [Hinzufügen eines Azure Machine Learning-Skills](./cognitive-search-aml-skill.md)
+ [Verwenden von Debugsitzungen zum Testen von Änderungen](./cognitive-search-debug-session.md)