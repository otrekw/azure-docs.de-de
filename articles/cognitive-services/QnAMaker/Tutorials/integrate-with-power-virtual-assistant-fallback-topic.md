---
title: 'Tutorial: Integrieren in Power Virtual Agents: QnA Maker'
titleSuffix: Azure Cognitive Services
description: In diesem Tutorial nutzen Sie aktives Lernen, um die Qualität Ihrer Wissensdatenbank zu verbessern. Sie können Einträge überprüfen, annehmen/ablehnen oder hinzufügen, ohne vorhandene Fragen zu entfernen oder zu ändern.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 03/11/2020
ms.author: diberry
ms.openlocfilehash: 4557dee995c8a01067f7e6ad0e79bb7115b6ecdb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402811"
---
# <a name="tutorial-add-your-knowledge-base-to-power-virtual-agents"></a>Tutorial: Hinzufügen Ihrer Wissensdatenbank zu Power Virtual Agents
Erstellen und erweitern Sie einen [Power Virtual Agents](https://powervirtualagents.microsoft.com/)-Bot, um Antworten aus Ihrer Wissensdatenbank bereitzustellen.

In diesem Tutorial lernen Sie Folgendes:

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Power Virtual Agents-Bots
> * Erstellen eines Systemfallbackthemas
> * Hinzufügen von QnA Maker als Aktion für ein Thema als Power Automate-Flow
> * Erstellen einer Power Automate-Lösung
> * Hinzufügen eines Power Automate-Flows zu Ihrer Lösung
> * Veröffentlichen von Power Virtual Agents
> * Testen von Power Virtual Agents und Empfangen einer Antwort von Ihrer QnA Maker-Wissensdatenbank

## <a name="integrate-an-agent-with-a-knowledge-base"></a>Integrieren eines Agents in eine Wissensdatenbank

Mithilfe von [Power Virtual Agents](https://powervirtualagents.microsoft.com/) können Teams über eine intuitive grafische Benutzeroberfläche leistungsstarke Bots ohne Programmieraufwand erstellen. Sie benötigen keine Data Scientists oder Entwickler.

In Power Virtual Agents wird ein Agent mit einer Reihe von Themenbereichen erstellt, um Benutzerfragen durch Ausführen von Aktionen zu beantworten. Wenn keine Antwort gefunden wird, kann durch ein Systemfallback eine Antwort zurückgegeben werden.

Konfigurieren Sie den Agent so, dass die Frage als Teil einer Themenaktion oder als Teil des Themenpfads *Systemfallback* an Ihre Wissensdatenbank gesendet wird. In beiden Fällen wird eine Aktion verwendet, um eine Verbindung mit Ihrer Wissensdatenbank herzustellen und eine Antwort zurückzugeben.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Automate-Verbindung mit der Aktion `GenerateAnswer`

Erstellen Sie die Aktion mithilfe von Power Automate, um Ihren Agent mit Ihrer Wissensdatenbank zu verbinden. Power Automate bietet einen Prozessflow, der eine Verbindung mit der QnA Maker-API `GenerateAnswer` herstellt.

Nachdem Sie den Flow entworfen und gespeichert haben, steht er in einer Power Automate-Lösung zur Verfügung. Verwenden Sie diese Lösung als Aktion in Ihrem Agent.

## <a name="connect-an-agent-to-your-knowledge-base"></a>Verbinden eines Agents mit Ihrer Wissensdatenbank

Im Anschluss finden Sie eine Übersicht über die Schritte, mit denen ein Agent in Power Virtual Agents mit einer Wissensdatenbank in QnA Maker verbunden wird.

* Vorgehensweise im [QnA Maker](https://www.qnamaker.ai/)-Portal:
    * Erstellen und veröffentlichen Sie Ihre Wissensdatenbank.
    * Kopieren Sie die Details Ihrer Wissensdatenbank, einschließlich der ID sowie des Endpunktschlüssels und -hosts für die Laufzeit.
* Vorgehensweise im [Power Virtual Agents](https://powerva.microsoft.com/)-Portal:
    * Erstellen Sie ein Agent-Thema.
    * Rufen Sie eine Aktion (für den Power Automate-Flow) auf.
* Vorgehensweise im [Power Automate](https://us.flow.microsoft.com/)-Portal:
    * Erstellen Sie einen Flow mit einer Verbindung zur [GenerateAnswer-API von QnA Maker](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/).
        * Informationen der veröffentlichten QnA Maker-Wissensdatenbank:
            * Wissensdatenbank-ID
            * Endpunkthost der QnA Maker-Ressource
            * Endpunktschlüssel der QnA Maker-Ressource
        * Eingabe – Benutzerabfrage
        * Ausgabe – Antwort aus der Wissensdatenbank
    * Erstellen Sie eine Lösung, und fügen Sie den Flow hinzu.
* Kehren Sie zu Power Virtual Agents zurück:
    * Wählen Sie die Ausgabe der Lösung als Nachricht für ein Thema aus.

## <a name="create-and-publish-a-knowledge-base"></a>Erstellen und Veröffentlichen einer Wissensdatenbank

1. Befolgen Sie die [Schnellstartanleitung](../Quickstarts/create-publish-knowledge-base.md), um eine Wissensdatenbank zu erstellen. Schließen Sie den letzten Abschnitt zur Boterstellung nicht ab. Erstellen Sie stattdessen anhand dieses Tutorials einen Bot mit Power Virtual Agents.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Einstellungen der veröffentlichen Wissensdatenbank](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    Geben Sie die Einstellungen Ihrer veröffentlichten Wissensdatenbank ein. Diese Einstellungen finden Sie im [QnA Maker](https://www.qnamaker.ai/)-Portal auf der Seite **Einstellungen**. Die Angaben werden für den [Power Automate-Schritt](#create-a-power-automate-flow-to-connect-to-your-knowledge-base) benötigt, um Ihre QnA Maker-Verbindung vom Typ `GenerateAnswer` zu konfigurieren.

1. Ermitteln Sie im QnA Maker-Portal auf der Seite **Einstellungen** den Endpunktschlüssel, den Endpunkthost und die Wissensdatenbank-ID.

## <a name="create-an-agent-in-power-virtual-agents"></a>Erstellen eines Agents in Power Virtual Agents

1. [Melden Sie sich bei Power Virtual Agents an.](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409) Verwenden Sie die E-Mail-Adresse Ihres Geschäfts-, Schul- oder Unikontos.
1. Wenn dies Ihr erster Bot ist, wird die **Startseite** des Agents angezeigt. Sollte dies nicht Ihr erster Bot sein, wählen Sie rechts oben auf der Seite den Bot und anschließend **+ New Bot** (+ Neuer Bot) aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Startseite von Power Virtual Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

1. Geben Sie die Einstellungen Ihrer veröffentlichten Wissensdatenbank ein. Diese Einstellungen finden Sie im [QnA Maker](https://www.qnamaker.ai/)-Portal auf der Seite **Einstellungen**.

## <a name="topics-provided-in-the-bot"></a>Im Bot bereitgestellte Themen

Der Agent verwendet die Themensammlung, um Fragen aus Ihrem Themenbereich zu beantworten. In diesem Tutorial stellt der Agent eine Vielzahl von Themen bereit, die in Benutzerthemen und Systemthemen unterteilt sind.

> [!div class="mx-imgBorder"]
> ![Screenshot: Im Agent bereitgestellte Themen](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-the-system-fallback-topic"></a>Erstellen des Systemfallbackthemas

Der Agent kann zwar von jedem Thema aus eine Verbindung mit Ihrer Wissensdatenbank herstellen, in diesem Tutorial wird jedoch das *Systemfallbackthema* verwendet. Das Fallbackthema wird verwendet, wenn der Agent keine Antwort auf eine Frage findet. Der Agent übergibt den Text des Benutzers an die `GenerateAnswer`-API von QnA Maker, empfängt die Antwort aus Ihrer Wissensdatenbank und zeigt sie dem Benutzer als Nachricht an.

1. Wählen Sie rechts oben im [Power Virtual Agents](https://powerva.microsoft.com/#/)-Portal die Option **Einstellungen** (Zahnradsymbol) aus. Wählen Sie anschließend **System Fallback** (Systemfallback) aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Power Virtual Agents-Menüelement für Systemfallback](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Wählen Sie **+ Hinzufügen** aus, um ein Systemfallbackthema hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Hinzufügen eines Fallbackthemas](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Wählen Sie nach dem Hinzufügen des Themas die Option **Go to Fallback topic** (Zum Fallbackthema) aus, um das Fallbackthema im Erstellungsbereich zu erstellen.

    > [!TIP]
    > Wenn Sie später noch einmal zum Fallbackthema zurückkehren möchten, finden Sie es im Abschnitt **Themen** bei den **Systemthemen**.

## <a name="use-the-authoring-canvas-to-add-an-action"></a>Hinzufügen einer Aktion mithilfe des Erstellungsbereichs

Verbinden Sie das Fallbackthema im Power Virtual Agents-Erstellungsbereich mit Ihrer Wissensdatenbank. Das Thema beginnt mit dem unbekannten Benutzertext. Fügen Sie eine Aktion hinzu, die diesen Text an QnA Maker übergibt und anschließend die Antwort als Nachricht anzeigt. Der letzte Schritt beim Anzeigen einer Antwort wird später in diesem Tutorial als [separater Schritt](#add-your-solutions-flow-to-power-virtual-agents) behandelt.

In diesem Abschnitt wird der Konversationsablauf für das Fallbackthema erstellt.

1. Die neue Fallbackaktion verfügt möglicherweise bereits über Konversationsablaufelemente. Löschen Sie das Element **Eskalieren** über das Menü **Optionen**.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Fallbackthema in Power Virtual Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. Wählen Sie das vom Feld **Nachricht** ausgehende **+** -Verbindungselement und anschließend **Aktion aufrufen** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: „Aktion aufrufen“](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png)

1. Wählen Sie **Flow erstellen** aus. Der Prozess führt Sie zum Power Automate-Portal.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: „Flow erstellen“](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)

## <a name="create-a-power-automate-flow-to-connect-to-your-knowledge-base"></a>Erstellen eines Power Automate-Flows für die Verbindung mit Ihrer Wissensdatenbank

Mit dem folgenden Verfahren wird ein Power Automate-Flow für folgende Aufgaben erstellt:
* Empfangen des eingehenden Benutzertexts und Senden des Texts an QnA Maker
* Zuweisen der besten QnA Maker-Antwort zu einer Variablen und Senden der Variablen (beste Antwort) als Antwort an Ihren Agent

1. In **Power Automate** wird die **Flow-Vorlage** für Sie gestartet. Wählen Sie für das Flowelement **Power Virtual Agents** die Option **Bearbeiten**, um die Eingabevariable zu konfigurieren, die vom Agent an Ihre Wissensdatenbank gesendet wird. Die textbasierte Eingabevariable ist die vom Benutzer übermittelte Frage in Textform, die von Ihrem Agent gesendet wird.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Power Automate-Option zum Konfigurieren Ihrer Eingabevariablen als Textzeichenfolge](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Fügen Sie eine Texteingabe hinzu, und weisen Sie der Variablen den Namen `InputText` und die Beschreibung `IncomingUserQuestion` zu. Diese Benennung ist nützlich, um zwischen dem Eingabetext und dem später erstellten Ausgabetext zu unterscheiden.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Power Automate-Option zum Konfigurieren des Namens und der Beschreibung für die Eingabevariable](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png)

1. Wählen Sie das vom Feld **Power Virtual Agents** ausgehende **+** -Verbindungselement aus, um einen neuen Schritt in den Flow einzufügen (vor **Wert(e) an Power Virtual Agent zurückgeben**). Wählen Sie dann **Aktion hinzufügen** aus.

1. Suchen Sie nach `Qna`, um die **QnA Maker**-Aktionen anzuzeigen, und wählen Sie **Antwort generieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: „Antwort generieren“](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png)

    Die erforderlichen Verbindungseinstellungen für QnA Maker werden in der Aktion und in den Frageeinstellungen aus dem Agent angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Erforderliche Verbindungseinstellungen](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. Konfigurieren Sie die Aktion mit Ihrer Wissensdatenbank-ID, Ihrem Endpunkthost und Ihrem Endpunktschlüssel. Sie finden diese Informationen im QnA Maker-Portal auf der Seite **Einstellungen** Ihrer Wissensdatenbank.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Einstellungen der veröffentlichen Wissensdatenbank](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. Wählen Sie zum Konfigurieren der **Frage** das Textfeld und anschließend `InputText` aus der Liste aus.

1. Wählen Sie das vom Aktionsfeld **Antwort generieren** ausgehende **+** -Verbindungselement aus, um einen neuen Schritt in den Flow einzufügen. Wählen Sie dann **Aktion hinzufügen** aus.

1. Um eine Variable zum Erfassen des Antworttexts hinzuzufügen, der von `GenerateAnswer` zurückgegeben wird, suchen Sie nach der Aktion `Initialize variable`, und wählen Sie sie aus.

    Legen Sie den Namen der Variablen auf `OutgoingQnAAnswer` fest, und wählen Sie als Typ **Zeichenfolge** aus. Legen Sie keinen **Wert** fest.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Initialisieren der Ausgabevariablen](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Wählen Sie das vom Aktionsfeld **Variable initialisieren** ausgehende **+** -Verbindungselement aus, um einen neuen Schritt in den Flow einzufügen. Wählen Sie dann **Aktion hinzufügen** aus.

1. Um die gesamte JSON-Antwort der Wissensdatenbank auf die Variable festzulegen, suchen Sie nach der Aktion `Apply to each` und wählen sie aus. Wählen Sie `answers` für `GenerateAnswer` aus.

1. Um nur die beste Antwort zurückzugeben, wählen Sie innerhalb desselben Felds **Auf alle anwenden** die Option **Aktion hinzufügen** aus. Suchen Sie nach **Variable festlegen**, und wählen Sie die Aktion aus.

    Wählen Sie im Feld **Variable festlegen** das Textfeld für **Name** und anschließen in der Liste die Option **OutgoingQnAAnswer** aus.

    Wählen Sie das Textfeld für **Wert** und anschließend in der Liste die Option **Answers Answer** aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Festlegen des Namens und des Werts für die Variable](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Um die Variable (und ihren Wert) zurückzugeben, wählen Sie das Flowelement **Wert(e) an Power Virtual Agent zurückgeben** aus. Wählen Sie anschließend **Bearbeiten** > **Ausgabe hinzufügen** aus. Wählen Sie den Ausgabetyp **Text** aus, und geben Sie unter **Titel** den Titel `FinalAnswer` ein. Wählen Sie das Textfeld für **Wert** und dann die Variable `OutgoingQnAAnswer` aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Festlegen des Rückgabewerts](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Wählen Sie **Speichern** aus, um den Flow zu speichern.

## <a name="create-a-solution-and-add-the-flow"></a>Erstellen einer Lösung und Hinzufügen des Flows

Damit der Agent den Flow finden und eine Verbindung damit herstellen kann, muss der Flow in einer Power Automate-Lösung enthalten sein.

1. Wählen Sie im linken Navigationsbereich des Power Automate-Portals **Lösungen** aus.

1. Wählen Sie **+ Neue Lösung** aus.

1. Geben Sie einen Anzeigenamen ein. Die Liste der Lösungen umfasst alle Lösungen in Ihrer Organisation oder Schule/Hochschule. Verwenden Sie eine Namenskonvention, die das Filtern nach Ihren Lösungen erleichtert. Eine Möglichkeit wäre beispielsweise, dem Namen Ihrer Lösung Ihre E-Mail-Adresse voranzustellen: `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Wählen Sie in der Liste der Optionen Ihren Herausgeber aus.

1. Übernehmen Sie die Standardwerte für den Namen und die Version.

1. Wählen Sie **Erstellen** aus, um den Vorgang abzuschließen.

## <a name="add-your-flow-to-the-solution"></a>Hinzufügen Ihres Flows zur Lösung

1. Wählen Sie in der Liste der Lösungen die Lösung aus, die Sie soeben erstellt haben. Sie sollte am Anfang der Liste aufgeführt werden. Ist dies nicht der Fall, suchen Sie nach Ihrem E-Mail-Namen, der Teil Ihres Lösungsnamens ist.

1. Wählen Sie in der Lösung **+ Vorhandene hinzufügen** und dann **Flow** aus der Liste aus.

1. Suchen Sie nach Ihrem Flow, und wählen Sie **Hinzufügen** aus, um den Vorgang abzuschließen. Wenn eine Vielzahl von Flows vorhanden sind, ermitteln Sie den neuesten Flow anhand der Spalte **Geändert**.

## <a name="add-your-solutions-flow-to-power-virtual-agents"></a>Hinzufügen des Flows Ihrer Lösung zu Power Virtual Agents

1. Kehren Sie zum Browsertab mit Ihrem Agent in Power Virtual Agents zurück. Der Erstellungsbereich sollte noch geöffnet sein.

1. Wählen Sie das **+** -Verbindungselement unterhalb des Aktionsfelds **Nachricht** aus, um einen neuen Schritt in den Flow einzufügen. Wählen Sie dann **Aktion aufrufen** aus.

1. Wählen Sie in der neuen Aktion den Eingabewert **UnrecognizedTriggerPhrase** aus. Dadurch wird der Text vom Agent an den Flow übergeben.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Power Virtual Agents-Option zum Auswählen von „UnrecognizedTriggerPhrase“](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Wählen Sie das **+** -Verbindungselement unterhalb des Felds **Aktion** aus, um einen neuen Schritt in den Flow einzufügen. Wählen Sie dann **Nachricht anzeigen** aus.

1. Geben Sie den folgenden Nachrichtentext ein: `Your answer is:`. Wählen Sie auf der bereitgestellten Symbolleiste `FinalAnswer` als Kontextvariable aus.

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Power Virtual Agents-Option zum Eingeben des Nachrichtentexts](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. Wählen Sie auf der Kontextsymbolleiste **Speichern** aus, um die Details des Erfassungsbereichs für das Thema zu speichern.

Der fertige Agent-Erstellungsbereich sollte wie folgt aussehen:

> [!div class="mx-imgBorder"]
> ![Screenshot: Fertiger Agent-Erstellungsbereich](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-the-agent"></a>Testen des Agents

1. Aktivieren Sie im Testbereich die Option **Nachverfolgung für Themen**. Mit dieser Option können Sie den Fortschritt zwischen Themen sowie innerhalb eines einzelnen Themas nachverfolgen.

1. Testen Sie den Agent, indem Sie den Benutzertext in der folgenden Reihenfolge eingeben. Im Erstellungsbereich werden die erfolgreichen Schritte mit einem grünen Häkchen gezeigt.

    |Reihenfolge der Fragen|Testfragen|Zweck|
    |--|--|--|
    |1|Hallo|Unterhaltung beginnen|
    |2|Geschäftszeiten|Beispielthema. Ein für Sie vorkonfiguriertes Thema.|
    |3|Ja|Als Antwort auf `Did that answer your question?`|
    |4|Hervorragend|Als Antwort auf `Please rate your experience.`|
    |5|Ja|Als Antwort auf `Can I help with anything else?`|
    |6|Was ist eine Wissensdatenbank?|Diese Frage löst die Fallbackaktion aus, die den Text an Ihre Wissensdatenbank sendet. Anschließend wird die Antwort angezeigt. |

> [!div class="mx-imgBorder"]
> ![Screenshot: Fertiger Agent-Erstellungsbereich](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Veröffentlichen Ihres Bots

Damit der Agent für alle Mitglieder Ihrer Schule/Hochschule oder Organisation verfügbar ist, müssen Sie ihn veröffentlichen.

1. Wählen Sie im Navigationsbereich auf der linken Seite die Option **Veröffentlichen** aus. Wählen Sie anschließend auf der Seite die Option **Veröffentlichen** aus.

1. Testen Sie Ihren Bot auf der Demowebsite. (Den Link finden Sie unter **Veröffentlichen**.)

    Es wird eine neue Webseite mit Ihrem Bot geöffnet. Stellen Sie dem Bot dieselbe Testfrage: `What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Screenshot: Fertiger Agent-Erstellungsbereich](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Freigeben Ihres Bots

Konfigurieren Sie die Demowebsite als Kanal, um sie freizugeben.

1. Wählen Sie im Navigationsbereich auf der linken Seite **Verwalten** > **Kanäle** aus.

1. Wählen Sie aus der Liste der Kanäle **Demowebsite** aus.

1. Kopieren Sie den Link, und wählen Sie **Speichern** aus. Fügen Sie den Link zu Ihrer Demowebsite in eine E-Mail an die Mitglieder Ihrer Schule/Hochschule oder Organisation ein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit der Wissensdatenbank fertig sind, entfernen Sie die QnA Maker-Ressourcen im Azure-Portal.

## <a name="next-step"></a>Nächster Schritt

[Abrufen von Analysen zu Ihrer Wissensdatenbank](../How-To/get-analytics-knowledge-base.md)

Weitere Informationen:
* [Power Virtual Agents](https://docs.microsoft.com/power-virtual-agents/)
* [Power Automate](https://docs.microsoft.com/power-automate/)
* [QnA Maker-Connector](https://us.flow.microsoft.com/connectors/shared_cognitiveservicesqnamaker/qna-maker/) und [Einstellungen für den Connector](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)