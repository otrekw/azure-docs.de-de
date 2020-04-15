---
title: 'Tutorial: Integration in Power Virtual Agent – QnA Maker'
description: In diesem Tutorial nutzen Sie aktives Lernen, um die Qualität Ihrer Wissensdatenbank zu verbessern. Sie können Einträge überprüfen, annehmen oder ablehnen und hinzufügen, ohne vorhandene Fragen zu entfernen oder zu ändern.
ms.topic: tutorial
ms.date: 03/11/2020
ms.openlocfilehash: 283667c587e395a1d712f82f3385582b4c5c3227
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2020
ms.locfileid: "80398631"
---
# <a name="tutorial-add-knowledge-base-to-power-virtual-agent"></a>Tutorial: Hinzufügen einer Wissensdatenbank zu Power Virtual Agents
Erstellen und erweitern Sie einen [Power Virtual Agent](https://powervirtualagents.microsoft.com/)-Bot, um Antworten aus Ihrer Wissensdatenbank bereitzustellen.

**In diesem Tutorial lernen Sie Folgendes:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Erstellen eines Power Virtual Agents
> * Erstellen eines Systemfallbackthemas
> * Hinzufügen von QnA Maker als Aktion für ein Thema (als Power Automate-Flow)
> * Erstellen einer Power Automate-Lösung
> * Hinzufügen eines Power Automate-Flows zur Lösung
> * Veröffentlichen des Power Virtual Agents
> * Testen des Power Virtual Agents, Empfangen einer Antwort aus der QnA Maker-Wissensdatenbank

## <a name="integrate-a-power-virtual-agent-with-a-knowledge-base"></a>Integrieren eines Power Virtual Agents in eine Wissensdatenbank

Mithilfe von [Power Virtual Agents](https://powervirtualagents.microsoft.com/) können Teams ganz einfach über ein angeleitetes Verfahren auf einer grafischen Benutzeroberfläche leistungsstarke Bots erstellen. Für dieses Verfahren sind weder Code noch die Unterstützung durch Data Scientists oder Entwickler erforderlich.

Ein Power Virtual Agent wird mit einer Reihe von Themen erstellt, um Benutzerfragen zu beantworten, indem verschiedene Aktionen ausgeführt werden. Wenn keine Antwort gefunden wird, kann durch ein Systemfallback eine Antwort zurückgegeben werden.

Konfigurieren Sie den Agent so, dass die Frage als Teil einer Themenaktion oder als Teil des Themenpfads **Systemfallback** an Ihre Wissensdatenbank gesendet wird. In beiden Fällen wird derselbe Mechanismus einer Aktion verwendet, um die Verbindung mit Ihrer Wissensdatenbank herzustellen und eine Antwort zurückzugeben.

## <a name="power-automate-connects-to-generateanswer-action"></a>Power Automate stellt eine Verbindung mit der GenerateAnswer-Aktion her

Erstellen Sie die Aktion mithilfe von Power Automate, um Ihren Agent mit Ihrer Wissensdatenbank zu verbinden. Power Automate bietet einen **Prozessflow**, der eine Verbindung mit der GenerateAnswer-API von QnA Maker herstellt.

Sobald der **Flow** entworfen und gespeichert wurde, ist er in einer Power Automate-**Lösung** verfügbar.  Nachdem der GenerateAnswer-Flow zu einer Lösung hinzugefügt wurde, verwenden Sie diese Lösung als Aktion in Ihrem Agent.

## <a name="process-steps-to-connect-an-agent-to-your-knowledge-base"></a>Erforderliche Schritte, um einen Agent mit Ihrer Wissensdatenbank zu verbinden

Die in der folgenden Übersicht aufgeführten Schritte zeigen, wie ein Power Virtual Agent mit einer QnA Maker-Wissensdatenbank verbunden wird.

Erforderliche Schritte, um einen Power Virtual Agent mit QnA Maker zu verwenden:
* Im [QnA Maker](https://www.qnamaker.ai/)-Portal
    * Erstellen und Veröffentlichen einer Wissensdatenbank
    * Kopieren der Wissensdatenbankdetails, einschließlich Wissensdatenbank-ID sowie Endpunktschlüssel und Endpunkthost der Laufzeit.
* Im [Power Virtual Agent](https://powerva.microsoft.com/)-Portal
    * Erstellen eines Agentthemas
    * Aufruf einer Aktion (an den Power Automate-Flow)
* Im [Power Automate](https://us.flow.microsoft.com/)-Portal
    * Erstellen eines Flows mit Connector zur [GenerateAnswer-API von QnA Maker](https://docs.microsoft.com/connectors/cognitiveservicesqnamaker/)
        * Informationen der veröffentlichten QnA Maker-Wissensdatenbank
            * Wissensdatenbank-ID
            * Endpunkthost der QnA Maker-Ressource
            * Endpunktschlüssel der QnA Maker-Ressource
        * Eingabe – Benutzerabfrage
        * Ausgabe – Antwort aus der Wissensdatenbank
    * Erstellen einer Lösung und Hinzufügen eines Flows
* Zurück im Power Virtual Agent
    * Auswahl der Lösungsausgabe als Nachricht für ein Thema

## <a name="create-and-publish-a-knowledge-base"></a>Erstellen und Veröffentlichen einer Wissensdatenbank

1. Befolgen Sie die [Schnellstartanleitung](../Quickstarts/create-publish-knowledge-base.md), um eine Wissensdatenbank zu erstellen. Führen Sie die Schritte des letzten Abschnitts (Erstellen eines Bots) nicht aus. Dieses Tutorial ersetzt den letzten Abschnitt der Schnellstartanleitung. In diesem Fall wird der Bot nicht durch das Bot Framework, sondern durch den Power Virtual Agent erstellt.

    > [!div class="mx-imgBorder"]
    > ![Geben Sie die Einstellungen Ihrer veröffentlichten Wissensdatenbank ein. Sie finden diese auf der Seite „Einstellungen“ im QnA Maker https://www.qnamaker.ai/) -Portal.](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

    Sie benötigen diese Informationen für den [Power Automate-Schritt](#create-power-automate-flow-to-connect-to-your-knowledge-base), um die Verbindung mit der GenerateAnswer-API von QnA Maker zu konfigurieren.

1. Ermitteln Sie auf der Seite **Einstellungen** im QnA Maker-Portal den Endpunktschlüssel, den Endpunkthost und die Wissensdatenbank-ID.

## <a name="create-power-virtual-agent"></a>Erstellen eines Power Virtual Agents

1. Zunächst müssen Sie sich mit Ihrer Geschäfts-, Schul- oder Uni-E-Mail-Adresse beim Power Virtual Agent [anmelden](https://go.microsoft.com/fwlink/?LinkId=2108000&clcid=0x409).
1. Wenn dies Ihr erster Bot ist, sollte die **Startseite** des Agents geöffnet werden. Wenn dies nicht Ihr erster Power Virtual Agent ist, wählen Sie oben rechts den Bot und dann **+ Neuer Bot** aus.

    > [!div class="mx-imgBorder"]
    > ![Geben Sie die Einstellungen Ihrer veröffentlichten Wissensdatenbank ein. Sie finden diese auf der Seite „Einstellungen“ im QnA Maker https://www.qnamaker.ai/) -Portal.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-home.png)

## <a name="several-topics-are-provided-in-the-bot"></a>Im Bot werden eine Reihe von Themen bereitgestellt

Der Agent verwendet die Themensammlung, um Fragen aus Ihrem Themenbereich zu beantworten. In diesem Tutorial stellt der Agent eine Vielzahl von Themen bereit, die in **Benutzerthemen** und **Systemthemen** untergliedert sind.

> [!div class="mx-imgBorder"]
> ![Der Agent verwendet die Themensammlung, um Fragen aus Ihrem Themenbereich zu beantworten. In diesem Tutorial stellt der Agent eine Vielzahl von Themen bereit, die in „Benutzerthemen“ und „Systemthemen“ untergliedert sind.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topics-provided.png)


## <a name="create-power-virtual-agents-system-fallback-topic"></a>Erstellen des Systemfallbackthemas für den Power Virtual Agent

Der Agent kann über ein beliebiges Thema eine Verbindung mit Ihrer Wissensdatenbank herstellen. In diesem Tutorial wird jedoch das Systemthema **Fallback** verwendet. Das Fallbackthema wird verwendet, wenn der Agent keine Antwort auf eine Frage findet. Der Agent übergibt den Text des Benutzers an die GenerateAnswer-API von QnA Maker, empfängt die Antwort aus Ihrer Wissensdatenbank und zeigt sie dem Benutzer als Nachricht an.

1. Wählen Sie im [Power Virtual Agents](https://powerva.microsoft.com/#/)-Portal oben rechts die Seite **Einstellungen** aus. Das Symbol für diese Seite ist das Zahnrad. Wählen Sie **Systemfallback** aus.

    > [!div class="mx-imgBorder"]
    > ![Power Virtual Agent-Menüelement für Systemfallback](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-system-fallback.png)

1. Wählen Sie im Popupfenster **Einstellungen** die Option **+ Hinzufügen** aus, um ein Systemfallbackthema hinzuzufügen.

    > [!div class="mx-imgBorder"]
    > ![Fügen Sie im Fenster „Einstellungen“ ein Fallbackthema hinzu.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-settings-add-fallback-topic.png)

1. Nachdem das Thema hinzugefügt wurde, wählen Sie **Zum Fallbackthema** aus, um das Fallbackthema im Erstellungsbereich zu erstellen.

    > [!TIP]
    > Wenn Sie erneut zum Fallbackthema wechseln müssen, finden Sie es im Abschnitt **Themen** bei den **Systemthemen**.

## <a name="use-authoring-canvas-to-add-an-action"></a>Hinzufügen einer Aktion im Erstellungsbereich

Verbinden Sie das Fallbackthema im Power Virtual Agents-Erstellungsbereich mit Ihrer Wissensdatenbank. Das Thema beginnt mit **Unbekannter Benutzertext**. Fügen Sie eine Aktion hinzu, die diesen Text an QnA Maker übergibt und anschließend die Antwort als Nachricht anzeigt. Der letzte Schritt beim Anzeigen einer Antwort wird später in diesem Tutorial als [separater Schritt](#add-solutions-flow-to-power-virtual-agent) behandelt.

In diesem Abschnitt wird der Konversationsablauf für das Fallbackthema erstellt.

1. Die neue Fallbackaktion verfügt möglicherweise bereits über Konversationsablaufelemente. Löschen Sie das Element **Eskalieren** über das Menü „Optionen“.

    > [!div class="mx-imgBorder"]
    > ![Starten der Fallbackaktion mit Triggerausdrücken](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-fallback-topic-delete-escalate.png)

1. Wählen Sie das **+** -Verbindungselement aus, das vom Feld **Nachricht** ausgeht, und wählen Sie dann **Aktion aufrufen** aus.

    > [!div class="mx-imgBorder"]
    > ![Aktion aufrufen](../media/how-to-integrate-power-virtual-agent/create-new-item-call-an-action.png)

1. Wählen Sie **Flow erstellen** aus. Durch diesen Vorgang wird **Power Automate** geöffnet, ein separates browserbasiertes Portal.

    > [!div class="mx-imgBorder"]
    > ![Aktion aufrufen](../media/how-to-integrate-power-virtual-agent/create-a-flow.png)

## <a name="create-power-automate-flow-to-connect-to-your-knowledge-base"></a>Erstellen eines Power Automate-Flows für die Verbindung mit Ihrer Wissensdatenbank

Mit dem folgenden Verfahren wird ein **Power Automate**-Flow für folgende Aufgaben erstellt:
* Empfang des eingehenden Benutzertexts
* Senden des Texts an QnA Maker
* Zuweisen der besten QnA Maker-Antwort zu einer Variablen
* Senden der Variablen (beste Antwort) als Antwort an Ihren Agent

1. In **Power Automate** wird die **Flow-Vorlage** für Sie gestartet. Wählen Sie für das Flowelement **Power Virtual Agents** die Option **Bearbeiten**, um die Eingabevariable zu konfigurieren, die vom Agent an Ihre Wissensdatenbank gesendet wird. Die textbasierte Eingabevariable ist die vom Benutzer übermittelte Frage in Textform, die von Ihrem Agent gesendet wird.

    > [!div class="mx-imgBorder"]
    > ![Konfigurieren Ihrer Eingabevariablen als Textzeichenfolge](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable.png)

1. Fügen Sie eine Texteingabe hinzu, und weisen Sie der Variablen den Namen `InputText` und die Beschreibung `IncomingUserQuestion` zu. Diese Benennung ist nützlich, um zwischen dem Eingabetext und dem später erstellten Ausgabetext zu unterscheiden.

    > [!div class="mx-imgBorder"]
    > ![Hinzufügen einer Texteingabe und Festlegen des Variablennamens „InputText“ sowie der Beschreibung „UserQuestion“](../media/how-to-integrate-power-virtual-agent/power-automate-configure-input-variable-name-and-description.png)

1. Wählen Sie das **+** -Verbindungselement aus, das vom Feld **Power Virtual Agents** ausgeht, um einen neuen Schritt im Flow einzufügen (vor **Wert(e) an Power Virtual Agent zurückgeben**), und wählen Sie dann **Aktion hinzufügen** aus.

1. Suchen Sie nach `Qna`, um die **QnA Maker**-Aktionen anzuzeigen, und wählen Sie dann **Antwort generieren** aus.

    > [!div class="mx-imgBorder"]
    > ![Suche nach „Qna“, um die QnA Maker-Aktionen anzuzeigen, und Auswahl von „Antwort generieren“](../media/how-to-integrate-power-virtual-agent/generate-answer-action-selected.png)

    Die drei erforderlichen Verbindungseinstellungen für QnA Maker werden in der Aktion und in den Frageeinstellungen aus dem Power Virtual Agent angezeigt.

    > [!div class="mx-imgBorder"]
    > ![Die Verbindungseinstellungen für QnA Maker in der Aktion.](../media/how-to-integrate-power-virtual-agent/generate-answer-knowledge-base-settings.png)

1. Konfigurieren Sie die Aktion mit Ihrer Wissensdatenbank-ID, dem Endpunkthost und dem Endpunktschlüssel. Sie finden diese Informationen im QnA Maker-Portal auf der Seite **Einstellungen** Ihrer Wissensdatenbank.

    > [!div class="mx-imgBorder"]
    > ![Geben Sie die Einstellungen Ihrer veröffentlichten Wissensdatenbank ein. Sie finden diese auf der Seite „Einstellungen“ im QnA Maker https://www.qnamaker.ai/) -Portal.](../media/how-to-integrate-power-virtual-agent/published-knowledge-base-settings.png)

1. Zum Konfigurieren der **Frage** wählen Sie das Textfeld und dann `InputText` aus der Liste aus.

1. Wenn Sie einen neuen Schritt im Flow einfügen möchten, wählen Sie das **+** -Verbindungselement aus, das vom Aktionsfeld **Antwort generieren** ausgeht, und wählen dann **Aktion hinzufügen** aus.

1. Um eine Variable zum Erfassen des Antworttexts hinzuzufügen, der von GenerateAnswer zurückgegebenen wird, suchen Sie nach der Aktion `Initialize variable` und wählen diese Aktion aus.

    Legen Sie den Namen der Variablen auf `OutgoingQnAAnswer` fest, und wählen Sie als Typ **Zeichenfolge** aus. Legen Sie keinen **Wert** fest.

    > [!div class="mx-imgBorder"]
    > ![Festlegen des Variablennames auf „QnAAnswer“ und Auswahl des Typs „Zeichenfolge“](../media/how-to-integrate-power-virtual-agent/initialize-output-variable-for-qna-answer.png)

1. Wenn Sie einen neuen Schritt im Flow einfügen möchten, wählen Sie das **+** -Verbindungselement aus, das vom Aktionsfeld **Variable initialisieren** ausgeht, und wählen dann **Aktion hinzufügen** aus.

1. Um die gesamte JSON-Antwort der Wissensdatenbank auf die Variable festzulegen, suchen Sie nach der Aktion `Apply to each` und wählen sie aus. Wählen Sie die GenerateAnswer `answers`.

1. Um nur die beste Antwort zurückzugeben, wählen Sie innerhalb desselben Felds **Auf alle anwenden** die Option **Aktion hinzufügen** aus. Suchen Sie nach **Variable festlegen**, und wählen Sie die Aktion aus.

    Wählen Sie im Feld **Variable festlegen** das Textfeld für **Name** und dann aus der Liste **OutgoingQnAAnswer** aus.

    Wählen Sie das Textfeld für **Wert** und dann **Antworten Antwort** aus der Liste aus.

    > [!div class="mx-imgBorder"]
    > ![Festlegen des Namens und des Werts für die Variable](../media/how-to-integrate-power-virtual-agent/power-automate-flow-apply-to-each-set-variable.png)

1. Um die Variable (und ihren Wert) zurückzugeben, wählen Sie das Flowelement **Wert(e) an Power Virtual Agent zurückgeben**, **Bearbeiten** und dann **Ausgabe hinzufügen** aus. Wählen Sie den Ausgabetyp **Text** aus, und geben Sie den **Titel** von `FinalAnswer` ein. Wählen Sie das Textfeld für **Wert** und dann die Variable `OutgoingQnAAnswer` aus.

    > [!div class="mx-imgBorder"]
    > ![Festlegen des Rückgabewerts](../media/how-to-integrate-power-virtual-agent/power-automate-flow-return-value.png)

1. Wählen Sie **Speichern** aus, um den Flow zu speichern.

## <a name="create-solution-and-add-flow"></a>Erstellen einer Lösung und Hinzufügen eines Flows

Damit der Power Virtual Agent den Flow finden und eine Verbindung mit dem Flow herstellen kann, muss der Flow in einer Power Automate-Lösung enthalten sein.

1. Wählen Sie im linken Navigationsbereich des Power Automate-Portals **Lösungen** aus.

1. Wählen Sie **+ Neue Lösung** aus.

1. Geben Sie einen Anzeigenamen ein. Die Liste der Lösungen umfasst alle Lösungen in Ihrer Organisation oder Schule/Hochschule. Wählen Sie eine Namenskonvention aus, mit der Sie die Liste nach Ihren eigenen Lösungen filtern können (verwenden Sie z. B. Ihren E-Mail-Namen als Präfix für Ihren Lösungsnamen): `jondoe-power-virtual-agent-qnamaker-fallback`.

1. Wählen Sie in der Liste der Optionen Ihren Herausgeber aus.

1. Übernehmen Sie die Standardwerte für den Namen und die Version.

1. Wählen Sie **Erstellen** aus, um den Vorgang abzuschließen.

## <a name="add-flow-to-solution"></a>Hinzufügen eines Flows zu einer Lösung

1. Wählen Sie in der Liste der Lösungen die Lösung aus, die Sie soeben erstellt haben. Sie sollte am Anfang der Liste aufgeführt werden. Ist dies nicht der Fall, suchen Sie nach Ihrem E-Mail-Namen, der Teil Ihres Lösungsnamens ist.

1. Wählen Sie in der Lösung **+ Vorhandene hinzufügen** und dann **Flow** aus der Liste aus.

1. Suchen Sie nach Ihrem Flow, und wählen Sie **Hinzufügen** aus, um den Vorgang abzuschließen. Wenn eine Vielzahl von Flows vorhanden sind, ermitteln Sie den neuesten Flow anhand der Spalte **Geändert**.

## <a name="add-solutions-flow-to-power-virtual-agent"></a>Hinzufügen des Flows einer Lösung zu Power Virtual Agents

1. Wechseln Sie mit Ihrem Power Virtual Agent erneut auf die Registerkarte „Browser“. Der Erstellungsbereich sollte noch geöffnet sein.

1. Wählen Sie das **+** -Verbindungselement unterhalb des Aktionsfelds **Nachricht** aus, um einen neuen Schritt im Flow einzufügen. Wählen Sie dann **Aktion aufrufen** aus.

1. Wählen Sie in der neuen Aktion den Eingabewert **UnrecognizedTriggerPhrase** aus. Dadurch wird der Text vom Agent an den Flow übergeben.

    > [!div class="mx-imgBorder"]
    > ![Auswahl des Eingabewerts „UnrecognizedTriggerPhrase“ in der neuen Aktion.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-select-unrecognized-trigger-phrase.png)

1. Wählen Sie das **+** -Verbindungselement unterhalb des Felds **Aktion** aus, um einen neuen Schritt im Flow einzufügen. Wählen Sie dann **Nachricht anzeigen** aus.

1. Geben Sie den Nachrichtentext ein (`Your answer is:`), und wählen Sie in der Symbolleiste `FinalAnswer` als Kontextvariable aus.

    > [!div class="mx-imgBorder"]
    > ![Eingabe des Nachrichtentexts und Auswahl von „FinalAnswer“ im Power Automate-Flow.](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-show-message-final-answer.png)

1. Wählen Sie in der Kontextsymbolleiste **Speichern** aus, um die Details des Erfassungsbereichs für das Thema zu speichern.

Nachfolgend ist der finale Erstellungsbereich gezeigt.

> [!div class="mx-imgBorder"]
> ![Finaler Erstellungsbereich des Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-topic-authoring-canvas-full-flow.png)

## <a name="test-power-virtual-agent"></a>Testen des Power Virtual Agents

1. Aktivieren Sie im Testbereich die Option **Nachverfolgung für Themen**. Mit dieser Option können Sie den Fortschritt zwischen Themen sowie ein einzelnes Thema nachverfolgen.

1. Testen Sie den Agent, indem Sie den Benutzertext in der nachfolgend gezeigten Reihenfolge eingeben. Im Erstellungsbereich werden die erfolgreichen Schritte mit einem grünen Häkchen gezeigt.

|Reihenfolge der Fragen|Testfragen|Zweck|
|--|--|--|
|1|Hallo|Unterhaltung beginnen|
|2|Geschäftszeiten|Beispielthema – dies ist ein vorkonfiguriertes Thema.|
|3|Ja|Als Antwort auf `Did that answer your question?`|
|4|Hervorragend|Als Antwort auf `Please rate your experience.`|
|5|Ja|Als Antwort auf `Can I help with anything else?`|
|6|Was ist eine Wissensdatenbank?|Diese Frage löst die Fallbackaktion aus, die den Text an Ihre Wissensdatenbank sendet. Anschließend wird die Antwort angezeigt. |

> [!div class="mx-imgBorder"]
> ![Finaler Erstellungsbereich des Agents](../media/how-to-integrate-power-virtual-agent/power-virtual-agent-test-tracked.png)

## <a name="publish-your-bot"></a>Veröffentlichen Ihres Bots

Damit der Agent für alle Mitglieder Ihrer Schule/Hochschule oder Organisation verfügbar ist, müssen Sie ihn veröffentlichen.

1. Wählen Sie im linken Navigationsbereich **Veröffentlichen** und dann auf der geöffneten Seite erneut **Veröffentlichen** aus.

1. Testen Sie Ihren Bot auf der Demowebsite, auf die Sie über den Link unterhalb der Schaltfläche **Veröffentlichen** zugreifen können.

    Es wird eine neue Webseite mit Ihrem Bot geöffnet. Stellen Sie dem Bot dieselbe Testfrage: `What is a knowledge base?`

    > [!div class="mx-imgBorder"]
    > ![Finaler Erstellungsbereich des Agents](../media/how-to-integrate-power-virtual-agent/demo-chat-bot.png)

## <a name="share-your-bot"></a>Freigeben Ihres Bots

Um die Demowebsite freizugeben, konfigurieren Sie sie als Kanal.

1. Wählen Sie **Verwalten** und dann im linken Navigationsbereich **Kanäle** aus.

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