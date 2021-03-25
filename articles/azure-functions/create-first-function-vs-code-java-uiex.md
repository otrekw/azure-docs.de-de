---
title: 'Erstellen einer Java-Funktion mit Visual Studio Code: Azure Functions'
description: Erfahren Sie, wie Sie eine Java-Funktion erstellen und dann das lokale Projekt für serverloses Hosting in Azure Functions unter Verwendung der Azure Functions-Erweiterung in Visual Studio Code veröffentlichen.
ms.topic: quickstart
ms.date: 11/03/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 6c84bf4ebc73857fa0280ffbcbf46b68c2da630f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102031717"
---
# <a name="quickstart-create-a-java-function-in-azure-using-visual-studio-code"></a>Schnellstart: Erstellen einer Java-Funktion in Azure mit Visual Studio Code

[!INCLUDE [functions-language-selector-quickstart-vs-code](../../includes/functions-language-selector-quickstart-vs-code.md)]

Verwenden Sie Visual Studio Code zum Erstellen einer Java-Funktion, die auf HTTP-Anforderungen reagiert. Testen Sie den Code lokal, und stellen Sie ihn anschließend in der serverlosen Umgebung von Azure Functions bereit.

Im Rahmen dieser Schnellstartanleitung fallen ggf. geringfügige Kosten im Centbereich in Ihrem <abbr title="Das Profil mit Abrechnungsinformationen für die Azure-Nutzung.">Azure-Konto</abbr>.

Sollte Visual Studio Code nicht Ihr bevorzugtes Entwicklungstool sein, stehen ähnliche Tutorials mit [Maven](create-first-function-cli-java.md), [Gradle](./functions-create-first-java-gradle.md) und [IntelliJ IDEA](/azure/developer/java/toolkit-for-intellij/quickstart-functions) für Java-Entwickler zur Verfügung.

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

Vergewissern Sie sich zunähst, dass Folgendes vorhanden ist:

+ Ein Azure-Konto mit einem aktiven <abbr title="Die grundlegende Organisationsstruktur, in der Sie Ressourcen in Azure verwalten. Diese wird in der Regel einer Einzelperson oder Abteilung innerhalb einer Organisation zugeordnet.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Java Developer Kit](/azure/developer/java/fundamentals/java-jdk-long-term-support), Version 8 oder 11.

+ [Apache Maven](https://maven.apache.org), Version 3.0 oder höher

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

+ [Java-Erweiterungspaket](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)  

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code

<br/>
<hr/>

## <a name="2-create-your-local-functions-project"></a>2. <a name="create-an-azure-functions-project"></a>Erstellen eines lokalen Funktionsprojekts

1. Wählen Sie auf der **Aktivitätsleiste** das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** das Symbol **Neues Projekt erstellen...** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. **Wählen Sie einen Verzeichnisspeicherort** für Ihren Projektarbeitsbereich und dann **Auswählen** aus.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Select a language for your function project** (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie die Option `Java`.

    + **Select a version of Java** (Wählen Sie eine Java-Version aus): Wählen Sie `Java 8` oder `Java 11` aus, die Java-Version, mit der Ihre Funktionen in Azure ausgeführt werden. Wählen Sie eine Java-Version aus, die Sie lokal überprüft haben.

    + **Provide a group ID** (Geben Sie eine Gruppen-ID an.): Wählen Sie die Option `com.function`.

    + **Provide an artifact ID** (Geben Sie eine Artefakt-ID an.): Wählen Sie die Option `myFunction`.

    + **Provide a version** (Geben Sie eine Version an.): Wählen Sie die Option `1.0-SNAPSHOT`.

    + **Provide a package name** (Geben Sie einen Paketnamen an.): Wählen Sie die Option `com.function`.

    + **Provide an app name** (Geben Sie einen App-Namen an.): Wählen Sie die Option `myFunction-12345`.

    + **Autorisierungsstufe:** Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann.

    + **Select how you would like to open your project** (Wählen Sie aus, wie Sie Ihr Projekt öffnen möchten.): Wählen Sie die Option `Add to workspace`.

<br/>

<details>
<summary><strong>Sie können kein Funktionsprojekt erstellen?</strong></summary>

Dies sind die häufigsten Probleme, die beim Erstellen eines lokalen Functions-Projekts gelöst werden müssen:
* Die Azure Functions-Erweiterung ist nicht installiert. 
</details>

<br/>
<hr/>

## <a name="3-run-the-function-locally"></a>3. Lokales Ausführen der Funktion

1. Drücken Sie <kbd>F5</kbd>, um das Funktions-App-Projekt zu starten.

1. Im **Terminal** wird der URL-Endpunkt Ihrer lokal ausgeführten Funktion angezeigt.

    ![VS Code-Ausgabe der lokalen Funktion](media/functions-create-first-function-vs-code/functions-vscode-f5.png)

1. Navigieren Sie bei ausgeführten Core Tools zum Bereich **Azure: Functions**. Erweitern Sie unter **Functions** die Option **Lokales Projekt** > **Functions**. Klicken Sie unter Windows mit der rechten Maustaste (unter macOS <kbd>STRG</kbd>+Mausklick) auf die Funktion `HttpExample`, und wählen Sie **Funktion jetzt ausführen...** aus.

    :::image type="content" source="../../includes/media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="„Funktion jetzt ausführen“ in Visual Studio Code":::

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt. Drücken Sie die <kbd>EINGABETASTE</kbd>, um diese Anforderungsmeldung an Ihre Funktion zu senden.  

1. Wenn die Funktion lokal ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst. Informationen zur Funktionsausführung werden im **Terminal**-Bereich angezeigt.

1. Drücken Sie <kbd>STRG+C</kbd>, um die Core Tools zu beenden und die Verbindung mit dem Debugger zu trennen.

<br/>

<details>
<summary><strong>Die Funktion kann nicht lokal ausgeführt werden?</strong></summary>

Dies sind die häufigsten Probleme, die beim Ausführen eines lokalen Functions-Projekts gelöst werden müssen:
* Azure Functions Core Tools sind nicht installiert. 
*  Sollten bei der Ausführung unter Windows Probleme auftreten, vergewissern Sie sich, dass die Standardterminalshell für Visual Studio Code nicht auf „WSL Bash“ festgelegt ist. 
</details>

<br/>
<hr/>

## <a name="4-sign-in-to-azure"></a>4. Anmelden bei Azure

Melden Sie sich zum Veröffentlichen Ihrer App bei Azure an. Sollten Sie bereits angemeldet sein, fahren Sie mit dem nächsten Abschnitt fort.

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Option **Bei Azure anmelden...** aus.

    ![Anmelden bei Azure in VS Code](../../includes/media/functions-sign-in-vs-code/functions-sign-into-azure.png)

1. **Wählen Sie Ihr Azure-Konto aus**, und **melden Sie sich mit Ihren Azure-Kontoanmeldeinformationen** an, wenn Sie im Browser dazu aufgefordert werden.

1. Nachdem Sie sich erfolgreich angemeldet haben, schließen Sie das neue Browserfenster, und kehren Sie zu Visual Studio Code zurück.

<br/>
<hr/>

## <a name="5-publish-the-project-to-azure"></a>5. Veröffentlichen des Projekts in Azure

Die erste Bereitstellung Ihres Codes umfasst das Erstellen einer Funktionsressource in Ihrem Azure-Abonnement.

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    ![Veröffentlichen Ihres Projekts in Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Ordner auswählen:** Wählen Sie den Ordner aus, der Ihre Funktions-App enthält. 

    + **Wählen Sie das Abonnement aus:** Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.

    + **Select Function App in Azure:** (Wählen Sie die Funktions-App in Azure aus:) Wählen Sie die Option `Create new Function App`.

    + **Enter a globally unique name for the function app:** (Geben Sie einen global eindeutigen Namen für die Funktions-App ein:) Geben Sie einen Namen ein, der innerhalb von Azure in einem URL-Pfad eindeutig ist. Der von Ihnen eingegebene Name wird überprüft, um sicherzustellen, dass er global eindeutig ist.

    - **Wählen Sie einen Speicherort für neue Ressourcen aus:**  Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen.

1. Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. Wählen Sie **Ausgabe anzeigen** aus, um die Ergebnisse der Erstellung und Bereitstellung anzuzeigen.

    ![Benachrichtigung nach Abschluss der Erstellung](../../includes/media/functions-publish-project-vscode/function-create-notifications.png)

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

<br/>
<hr/>

## <a name="6-run-the-function-in-azure"></a>6. Ausführen der Funktion in Azure

1. Erweitern Sie im Bereich **Azure: Funktionen** auf der Seitenleiste Ihr Abonnement, die neue Funktions-App und **Funktionen**. Klicken Sie unter Windows mit der rechten Maustaste (unter macOS <kbd>STRG</kbd>+Mausklick) auf die Funktion `HttpExample`, und wählen Sie **Funktion jetzt ausführen...** aus.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="„Funktion jetzt ausführen“ in Azure über Visual Studio Code":::

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt. Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.  

1. Wenn die Funktion in Azure ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst.

<br/>
<hr/>

## <a name="7-clean-up-resources"></a>7. Bereinigen von Ressourcen

Wenn Sie nicht beabsichtigen, mit dem [nächsten Schritt](#next-steps) fortzufahren, löschen Sie die Funktions-App und die zugehörigen Ressourcen, um weitere Kosten zu vermeiden.

1. Wählen Sie in Visual Studio Code auf der Aktivitätsleiste das Azure-Symbol und dann auf der Seitenleiste den Bereich „Funktionen“ aus.
1. Wählen Sie die Funktions-App aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Funktions-App löschen...** aus.

<br/>
<hr/>

## <a name="next-steps"></a>Nächste Schritte

Erweitern Sie die Funktion durch Hinzufügen einer <abbr title="Eine Möglichkeit in Azure Storage, eine Funktion einer Speicherwarteschlange zuzuordnen, damit Nachrichten in der Warteschlange erstellt werden können">Ausgabebindung</abbr>. Diese Bindung schreibt die Zeichenfolge aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange.

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Azure Storage-Warteschlange](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-java)
