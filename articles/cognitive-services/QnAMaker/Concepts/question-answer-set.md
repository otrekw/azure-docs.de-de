---
title: 'Entwerfen einer Wissensdatenbank: QnA Maker'
description: Eine QnA Maker-Wissensdatenbank besteht aus einer Reihe von Frage-Antwort-Sätzen (QnA) und optionalen Metadaten, die jedem QnA-Paar zugeordnet sind.
ms.topic: conceptual
ms.date: 01/27/2020
ms.openlocfilehash: cb71de0f6a3e372d8c1c1fede67ba0c0354532ca
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76844316"
---
# <a name="question-and-answer-set-concepts"></a>Konzepte von Frage-Antwort-Sätzen

Eine Wissensdatenbank besteht aus Frage-Antwort-Sätzen (Question and Answer – QnA).  Jeder Satz hat eine Antwort und enthält alle Informationen, die mit dieser _Antwort_ verknüpft sind. Eine Antwort ähnelt einer Datenbankzeile oder einer Datenstrukturinstanz.

## <a name="question-and-answer-sets"></a>Frage-Antwort-Sätze

Die **erforderlichen** Einstellungen in einem Frage-Antwort-Satz (QnA) lauten wie folgt:

* Eine **Frage**: Text der Benutzerabfrage für das maschinelle Lernen von QnA Maker, mit dem der Text der Benutzerfrage mit unterschiedlichen Formulierungen, aber derselben Antwort abgeglichen wird.
* Die **Antwort**: wird zurückgegeben, wenn eine Benutzerabfrage mit der zugeordneten Frage übereinstimmt.

Jeder Satz wird durch eine **ID** dargestellt.

Die **optionalen** Einstellungen für einen Satz umfassen Folgendes:

* **Alternative Formen der Frage:** helfen QnA Maker, die richtige Antwort für eine größere Vielfalt von Frageformulierungen zurückzugeben.
* **Metadaten**: Metadaten sind Tags in Form von Schlüssel-Wert-Paaren, die einem QnA-Paar zugeordnet sind. Metadatentags werden verwendet, um QnA-Paare zu filtern und die Datenmenge einzuschränken, für die ein Abfrageabgleich vorgenommen wird.
* **Mehrfachdurchlauf-Eingabeaufforderungen:** werden verwendet, um eine mehrteilige Unterhaltung fortzusetzen.

![QnA Maker-Wissensdatenbanken](../media/qnamaker-concepts-knowledgebase/knowledgebase.png)

## <a name="editorially-add-to-knowledge-base"></a>Redaktionelles Hinzufügen zur Wissensdatenbank

Wenn Sie nicht über bereits vorhandene Inhalte zum Füllen der Wissensdatenbank verfügen, können QnA-Sätze auch redaktionell im QnA Maker-Portal hinzufügen. Informationen zum Aktualisieren Ihrer Wissensdatenbank finden Sie [hier](../How-To/edit-knowledge-base.md).

## <a name="editing-your-knowledge-base-locally"></a>Lokales Bearbeiten Ihrer Wissensdatenbank

Nachdem eine Wissensdatenbank erstellt wurde, wird empfohlen, den Text der Wissensdatenbank im [QnA Maker-Portal](https://qnamaker.ai) zu bearbeiten, anstatt diesen in lokale Dateien zu exportieren und wieder zu importieren. Es kann jedoch vorkommen, dass Sie eine Wissensdatenbank lokal bearbeiten müssen.

Exportieren Sie die Wissensdatenbank auf der Seite **Einstellungen**, und bearbeiten Sie die Wissensdatenbank dann mit Microsoft Excel. Wenn Sie eine andere Anwendung verwenden, um die exportierte Datei zu bearbeiten, fügt die Anwendung eventuell Syntaxfehler ein, da sie nicht vollständig TSV-konform ist. TSV-Dateien von Microsoft Excel fügen in der Regel keine Formatierungsfehler ein.

Wenn Sie mit der Bearbeitung fertig sind, importieren Sie die TSV-Datei auf der Seite **Einstellungen**. Die aktuelle Wissensdatenbank wird dabei vollständig durch die importierte Wissensdatenbank ersetzt.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Lebenszyklus einer Wissensdatenbank in QnA Maker](./development-lifecycle-knowledge-base.md)