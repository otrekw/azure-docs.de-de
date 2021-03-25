---
title: 'Erstellen einer Python-Funktion mit Visual Studio Code: Azure Functions'
description: Erfahren Sie, wie Sie eine Python-Funktion erstellen und dann das lokale Projekt für serverloses Hosting in Azure Functions unter Verwendung der Azure Functions-Erweiterung in Visual Studio Code veröffentlichen.
ms.topic: quickstart
ms.date: 11/04/2020
ms.custom: devx-track-python
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 42f07b76cefed38aad53caba9ba35c74238540fe
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102031734"
---
# <a name="quickstart-create-a-function-in-azure-with-python-using-visual-studio-code"></a>Schnellstart: Erstellen einer Funktion in Azure mit Python mithilfe von Visual Studio Code

> [!div class="op_single_selector" title1="Wählen Sie Ihre Funktionssprache aus: "]
> - [Python](create-first-function-vs-code-python.md)
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Sonstiges (Go/Rust)](create-first-function-vs-code-other.md)

In diesem Artikel wird mithilfe von Visual Studio Code eine Python-Funktion erstellt, die auf HTTP-Anforderungen reagiert. Stellen Sie den Code nach lokalen Tests in folgender Umgebung bereit: <abbr title="Eine Runtimecomputingumgebung, in der alle Serverdetails für Anwendungsentwickler transparent sind. Dies vereinfacht den Prozess der Bereitstellung sowie die Codeverwaltung.">Serverlos</abbr> Umgebung von <abbr title="Ein Azure-Dienst, der eine kostengünstige Umgebung für serverloses Computing für Anwendungen bietet.">Azure-Funktionen</abbr>.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

Es gibt auch eine [CLI-basierte Version](create-first-function-cli-python.md) dieses Artikels.

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

Vergewissern Sie sich zunähst, dass Folgendes vorhanden ist:

+ Ein Azure- <abbr title="Das Profil mit Abrechnungsinformationen für die Azure-Nutzung.">Azure-Konto</abbr> mit einem aktiven <abbr title="Die grundlegende Organisationsstruktur, in der Sie Ressourcen in Azure verwalten. Diese wird in der Regel einer Einzelperson oder Abteilung innerhalb einer Organisation zugeordnet.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools), Version 3.x.

+ [Python 3.8](https://www.python.org/downloads/release/python-381/), [Python 3.7](https://www.python.org/downloads/release/python-375/) und [Python 3.6](https://www.python.org/downloads/release/python-368/) werden von Azure Functions (x64) unterstützt.

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

+ [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für Visual Studio Code  

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code

<hr/>
<br/>

## <a name="2-create-your-local-project"></a>2. <a name="create-an-azure-functions-project"></a>Erstellen Ihres lokalen Projekts

1. Wählen Sie das Azure-Symbol auf der <abbr title="Die vertikale Gruppe von Symbolen auf der linken Seite des Visual Studio Code Fensters.">Aktivitätsleiste</abbr>und dann im Bereich **Azure: Funktionen** das Symbol für **Neues Projekt erstellen...** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Select a language for your function project** (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie die Option `Python`.

    + **Select a Python alias to create a virtual environment** (Wählen Sie einen Python-Alias zum Erstellen einer virtuellen Umgebung aus.): Wählen Sie den Speicherort Ihres Python-Interpreters aus. Sollte der Speicherort nicht angezeigt werden, geben Sie den vollständigen Pfad Ihrer Python-Binärdatei ein.  

    + **Select a template for your project's first function** (Wählen Sie die Vorlage für die erste Funktion Ihres Projekts aus.): Wählen Sie die Option `HTTP trigger`.

    + **Provide a function name** (Geben Sie einen Funktionsnamen an.): Geben Sie `HttpExample`ein.

    + **Autorisierungsstufe:** Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann. Weitere Informationen zu Autorisierungsstufen finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Select how you would like to open your project** (Wählen Sie aus, wie Sie Ihr Projekt öffnen möchten.): Wählen Sie die Option `Add to workspace`.

<br/>
<details>
<summary><strong>Sie können kein Funktionsprojekt erstellen?</strong></summary>

Dies sind die häufigsten Probleme, die beim Erstellen eines lokalen Functions-Projekts gelöst werden müssen:
* Die Azure Functions-Erweiterung ist nicht installiert. 
</details>

<hr/>
<br/>

## <a name="run-the-function-locally"></a>Lokales Ausführen der Funktion

1. Drücken Sie <kbd>F5</kbd>, um das Funktions-App-Projekt zu starten.

1. Im Bereich **Terminal** wird der URL-Endpunkt Ihrer lokal ausgeführten Funktion angezeigt.

    ![VS Code-Ausgabe der lokalen Funktion](../../includes/media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)


1. Navigieren Sie bei ausgeführten Core Tools zum Bereich **Azure: Functions**. Erweitern Sie unter **Functions** die Option **Lokales Projekt** > **Functions**. Klicken Sie unter Windows mit der rechten Maustaste (unter macOS <kbd>STRG</kbd>+Mausklick) auf die Funktion `HttpExample`, und wählen Sie **Funktion jetzt ausführen...** aus.

    :::image type="content" source="../../includes/media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="„Funktion jetzt ausführen“ in Visual Studio Code":::

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt. Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.  

1. Wenn die Funktion lokal ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst. Informationen zur Funktionsausführung werden im **Terminal**-Bereich angezeigt.

1. Drücken Sie <kbd>STRG+C</kbd>, um die Core Tools zu beenden und die Verbindung mit dem Debugger zu trennen.

<br/>
<details>
<summary><strong>Die Funktion kann nicht lokal ausgeführt werden?</strong></summary>

Dies sind die häufigsten Probleme, die beim Ausführen eines lokalen Functions-Projekts gelöst werden müssen:
* Azure Functions Core Tools sind nicht installiert. 
*  Sollten bei der Ausführung unter Windows Probleme auftreten, vergewissern Sie sich, dass die Standardterminalshell für Visual Studio Code nicht auf **WSL Bash** festgelegt ist. 
</details>

<hr/>
<br/>

## <a name="4-sign-in-to-azure"></a>4. Anmelden bei Azure

Melden Sie sich zum Veröffentlichen Ihrer App bei Azure an. Sollten Sie bereits angemeldet sein, fahren Sie mit dem nächsten Abschnitt fort.

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Option **Bei Azure anmelden...** aus.

    ![Anmelden bei Azure in VS Code](../../includes/media/functions-sign-in-vs-code/functions-sign-into-azure.png)

1. **Wählen Sie Ihr Azure-Konto aus**, und **melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen** an, wenn Sie im Browser dazu aufgefordert werden.

1. Nachdem Sie sich erfolgreich angemeldet haben, schließen Sie das neue Browserfenster, und kehren Sie zu Visual Studio Code zurück. 

<hr/>
<br/>

## <a name="5-publish-the-project-to-azure"></a>5. Veröffentlichen des Projekts in Azure

Die erste Bereitstellung Ihres Codes umfasst das Erstellen einer Funktionsressource in Ihrem Azure-Abonnement.

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    ![Veröffentlichen Ihres Projekts in Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Ordner auswählen:** Wählen Sie den Ordner aus, der Ihre Funktions-App enthält.

    + **Wählen Sie das Abonnement aus:** Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.

    + **Select Function App in Azure:** (Wählen Sie die Funktions-App in Azure aus:) Wählen Sie die Option `+ Create new Function App`.

    + **Enter a globally unique name for the function app:** (Geben Sie einen global eindeutigen Namen für die Funktions-App ein:) Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist. Der von Ihnen eingegebene Name wird überprüft, um sicherzustellen, dass er <abbr title="Der Name muss für alle Azure-Kunden global eindeutig sein. Sie können beispielsweise eine Kombination aus Ihrem persönlichen Namen oder Organisationsnamen, dem Anwendungsnamen und einem numerischen Bezeichner verwenden, etwa „contoso-bizapp-func-20“.">in Azure eindeutig ist</abbr>. 

    + **Select a runtime:** (Wählen Sie eine Runtime aus:) Wählen Sie die lokal ausgeführte Python-Version aus. Sie können den Befehl `python --version` ausführen, um Ihre Version zu überprüfen.

    + **Wählen Sie einen Speicherort für neue Ressourcen aus:** Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen.

    Die Erweiterung zeigt den Status einzelner Ressourcen an, während diese in Azure im Benachrichtigungsbereich erstellt werden.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Benachrichtigung über die Erstellung von Azure-Ressourcen":::

1. Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. Wählen Sie **Ausgabe anzeigen** aus, um die Ergebnisse der Erstellung und Bereitstellung anzuzeigen. 

    ![Benachrichtigung nach Abschluss der Erstellung](./media/functions-create-first-function-vs-code/function-create-notifications.png)

<br/>
<details>
<summary><strong>Kann die Funktion nicht veröffentlicht werden?</strong></summary>

In diesem Abschnitt wurden die Azure-Ressourcen erstellt, und der lokale Code wurde in der Funktions-App bereitgestellt. War dies nicht erfolgreich:

* Überprüfen Sie die Ausgabe auf Fehlerinformationen. Das Glockensymbol in der unteren rechten Ecke ist eine weitere Möglichkeit zum Anzeigen der Ausgabe. 
* Haben Sie die Veröffentlichung in einer vorhandenen Funktions-App durchgeführt? Diese Aktion überschreibt den Inhalt dieser App in Azure.
</details>


<br/>
<details>
<summary><strong>Welche Ressourcen wurden erstellt?</strong></summary>

Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt, deren Namen auf dem Namen Ihrer Funktions-App basieren: 
* **Ressourcengruppe:** Eine Ressourcengruppe ist ein logischer Container für verwandte Ressourcen in der gleichen Region.
* **Azure Storage-Konto:** Mit einer Storage-Ressource werden Statusinformationen und andere Informationen zu Ihren Projekten verwaltet.
* **Verbrauchsplan:** Ein Verbrauchsplan definiert den zugrunde liegenden Host für Ihre serverlose Funktions-App.
* **Funktions-App:** Eine Funktions-App stellt die Umgebung zum Ausführen des Funktionscodes und der Gruppenfunktionen als logische Einheit zur Verfügung.
* **Application Insights:** Application Insights verfolgt die Verwendung Ihrer serverlosen Funktion nach.

</details>

<hr/>
<br/>

## <a name="6-run-the-function-in-azure"></a>6. Ausführen der Funktion in Azure

1. Navigieren Sie zurück zur Seitenleiste **Azure: Funktionen**, und erweitern Sie die neue Funktions-App.
1. Erweitern Sie **Funktionen**, klicken Sie unter Windows mit der rechten Maustaste (unter macOS <kbd>STRG</kbd>+Mausklick) auf die Funktion `HttpExample`, und wählen Sie **Funktion jetzt ausführen...** aus.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="„Funktion jetzt ausführen“ in Azure über Visual Studio Code":::

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt.

    Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.  

1. Wenn die Funktion in Azure ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst.

## <a name="7-clean-up-resources"></a>7. Bereinigen von Ressourcen

Wenn Sie mit dem [nächsten Schritt](#next-steps) fortfahren und eine <abbr title="Eine Möglichkeit, eine Funktion einer Speicherwarteschlange zuzuordnen, damit Nachrichten in der Warteschlange erstellt werden können."> Azure Storage-Warteschlangen-Ausgabebindung</abbr> zu Ihrer Funktion hinzufügen, müssen alle Ihre Ressourcen erhalten bleiben, um darauf aufbauen zu können.

Andernfalls können Sie die Funktions-App und die zugehörigen Ressourcen wie im Anschluss beschrieben löschen, um weitere Kosten zu vermeiden.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Weitere Informationen zu den Kosten von Functions finden Sie unter [Abschätzen der Kosten des Verbrauchstarifs](functions-consumption-costs.md).

## <a name="next-steps"></a>Nächste Schritte

Erweitern dieser Funktion durch Hinzufügen einer Ausgabe <abbr title="Eine deklarative Verbindung zwischen einer Funktion und anderen Ressourcen. Eine Eingabebindung stellt Daten für die Funktion bereit. Eine Ausgabebindung stellt Daten aus der Funktion für andere Ressourcen bereit.">binding</abbr>. Diese Bindung schreibt die Zeichenfolge aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange. 

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Azure Storage-Warteschlange](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-python)

[Treten Probleme auf? Informieren Sie uns darüber.](https://aka.ms/python-functions-qs-survey)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
