---
title: 'Erstellen einer C#-Funktion mit Visual Studio Code: Azure Functions'
description: Erfahren Sie, wie Sie eine C#-Funktion erstellen und dann das lokale Projekt für serverloses Hosting in Azure Functions unter Verwendung der Azure Functions-Erweiterung in Visual Studio Code veröffentlichen.
ms.topic: quickstart
ms.date: 11/03/2020
ms.custom: devx-track-csharp
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2848ce6214d59ba2732dcfc148ccaf9936497f17
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121030"
---
# <a name="quickstart-create-a-c-function-in-azure-using-visual-studio-code"></a>Schnellstart: Erstellen einer C#-Funktion in Azure mit Visual Studio Code

> [!div class="op_single_selector" title1="Wählen Sie Ihre Funktionssprache aus: "]
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [Python](create-first-function-vs-code-python.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Sonstiges (Go/Rust)](create-first-function-vs-code-other.md)

In diesem Artikel wird mithilfe von Visual Studio Code eine auf der C#-Klassenbibliothek basierende Funktion erstellt, die auf HTTP-Anforderungen reagiert. Stellen Sie den Code nach lokalen Tests in folgender Umgebung bereit: <abbr title="Hierbei handelt es sich um eine Runtimecomputingumgebung, in der alle Serverdetails für Anwendungsentwickler transparent sind. Dies vereinfacht den Prozess der Bereitstellung sowie die Codeverwaltung.">Serverlos</abbr> Umgebung in <abbr title="Hierbei handelt es sich um den Dienst von Azure, der eine kostengünstige Umgebung für serverloses Computing für Anwendungen bietet.">Azure-Funktionen</abbr>.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

Es gibt auch eine [CLI-basierte Version](create-first-function-cli-csharp.md) dieses Artikels.
    
## <a name="1-configure-your-environment"></a>1. Konfigurieren Ihrer Umgebung

Vergewissern Sie sich zunähst, dass Folgendes vorhanden ist:

+ Ein Azure- <abbr title="Das Profil mit Abrechnungsinformationen für die Azure-Nutzung.">Azure-Konto</abbr> mit einem aktiven <abbr title="Die grundlegende Organisationsstruktur, in der Sie Ressourcen in Azure verwalten. Diese wird in der Regel einer Einzelperson oder Abteilung innerhalb einer Organisation zugeordnet.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools), Version 3.x.

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

+ [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code  

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code

## <a name="2-create-your-local-project"></a><a name="create-an-azure-functions-project"></a>2. Erstellen Ihres lokalen Projekts

In diesem Abschnitt verwenden Sie Visual Studio Code, um ein lokales <abbr title="Ein logischer Container für eine oder mehrere einzelne Funktionen, die zusammen bereitgestellt und verwaltet werden können">Azure Functions-Projekt</abbr> in C# zu erstellen. Weiter unten in diesem Artikel wird der Funktionscode in Azure veröffentlicht.

1. Klicken Sie auf das Azure-Symbol in der <abbr title="Die vertikale Gruppe von Symbolen auf der linken Seite des Visual Studio Code Fensters.">Aktivitätsleiste</abbr>und dann im Bereich **Azure: Funktionen** das Symbol für **Neues Projekt erstellen...** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Select a language for your function project** (Wählen Sie eine Sprache für Ihr Funktionsprojekt aus.): Wählen Sie die Option `C#`.

    + **Select a template for your project's first function** (Wählen Sie die Vorlage für die erste Funktion Ihres Projekts aus.): Wählen Sie die Option `HTTP trigger`.

    + **Provide a function name** (Geben Sie einen Funktionsnamen an.): Geben Sie `HttpExample`ein.

    + **Provide a namespace** (Geben Sie einen Namespace an.): Geben Sie `My.Functions`ein.

    + **Autorisierungsstufe:** Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann. Weitere Informationen zu Autorisierungsstufen finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Select how you would like to open your project** (Wählen Sie aus, wie Sie Ihr Projekt öffnen möchten.): Wählen Sie die Option `Add to workspace`.

1. Auf der Grundlage dieser Informationen generiert Visual Studio Code ein Azure Functions-Projekt mit einem HTTP- <abbr title="Der Typ des Ereignisses, das den Funktionscode aufruft, etwa eine HTTP-Anforderung, eine Warteschlangennachricht oder eine bestimmte Uhrzeit">Trigger</abbr>. Die lokalen Projektdateien können im Explorer angezeigt werden. Weitere Informationen zu den erstellten Dateien finden Sie unter [Generierte Projektdateien](functions-develop-vs-code.md#generated-project-files).

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Nachdem Sie sich vergewissert haben, dass die Funktion auf Ihrem lokalen Computer korrekt ausgeführt wird, können Sie das Projekt mithilfe von Visual Studio Code direkt in Azure veröffentlichen.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="5-publish-the-project-to-azure"></a>5. Veröffentlichen des Projekts in Azure

In diesem Abschnitt erstellen Sie eine Funktions-App sowie zugehörige Ressourcen in Ihrem Azure-Abonnement und stellen anschließend Ihren Code bereit. 

> [!IMPORTANT]
> Beim Veröffentlichen in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben. 

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    + **Wählen Sie einen Ordner aus:** Wählen Sie einen Ordner in Ihrem Arbeitsbereich aus, oder navigieren Sie zu einem Ordner, der Ihre Funktions-App enthält. Diese Option wird nicht angezeigt, wenn Sie bereits eine gültige Funktions-App geöffnet haben.

    + **Wählen Sie das Abonnement aus:** Wählen Sie das zu verwendende Abonnement aus. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.

    + **Select Function App in Azure:** (Wählen Sie die Funktions-App in Azure aus:) Wählen Sie die Option `+ Create new Function App`. (Wählen Sie nicht die Option `Advanced` aus, diese wird im vorliegenden Artikel nicht behandelt.)

    + **Enter a globally unique name for the function app:** (Geben Sie einen global eindeutigen Namen für die Funktions-App ein:) Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist. Der Name, den Sie eingeben, wird überprüft, um sicherzustellen, dass er <abbr title="Der Name muss für alle Functions-Projekte eindeutig sein, die von Azure-Kunden global genutzt werden. In der Regel verwenden Sie eine Kombination aus Ihrem Namen oder Unternehmensnamen, dem Anwendungsname und einem numerischen Bezeichner, z. B. contoso-bizapp-func-20.">für Azure Functions eindeutig ist</abbr>. 

    + **Wählen Sie einen Speicherort für neue Ressourcen aus:**  Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen.

    Die Erweiterung zeigt den Status einzelner Ressourcen an, während diese in Azure im Benachrichtigungsbereich erstellt werden.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Benachrichtigung über die Erstellung von Azure-Ressourcen":::

1. Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt, deren Namen auf dem Namen Ihrer Funktions-App basieren:

    + Eine **Ressourcengruppe**, bei der es sich um einen logischen Container für verwandte Ressourcen handelt.
    + Ein **Azure Storage-Standardkonto**, unter dem Status- und andere Informationen zu Ihren Projekten verwaltet werden.
    + Ein **Verbrauchsplan**, der den zugrunde liegenden Host für Ihre serverlose Funktions-App definiert 
    + Eine **Funktions-App**, die als Umgebung zum Ausführen Ihres Funktionscodes dient Mit einer Funktions-App können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen im selben Hostingplan.
    + Eine mit der Funktions-App verbundene **Application Insights-Instanz**, die die Nutzung Ihrer serverlosen Funktion nachverfolgt

    Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. 

    > [!TIP]
    > Standardmäßig werden die von Ihrer Funktions-App benötigten Azure-Ressourcen basierend auf dem von Ihnen angegebenen Namen der Funktions-App erstellt. Darüber hinaus werden sie auch in derselben neuen Ressourcengruppe mit der Funktions-App erstellt. Falls Sie entweder die Namen dieser Ressourcen anpassen oder vorhandene Ressourcen wiederverwenden möchten, müssen Sie [das Projekt stattdessen mit den erweiterten Erstellungsoptionen veröffentlichen](functions-develop-vs-code.md#enable-publishing-with-advanced-create-options).


1. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen. Wenn Sie die Benachrichtigung übersehen haben, wählen Sie das Glockensymbol in der unteren rechten Ecke aus, um sie erneut anzuzeigen.

    ![Benachrichtigung nach Abschluss der Erstellung](./media/functions-create-first-function-vs-code/function-create-notifications.png)

## <a name="6-run-the-function-in-azure"></a>6. Ausführen der Funktion in Azure

1. Erweitern Sie im Bereich **Azure: Funktionen** auf der Seitenleiste Ihr Abonnement, die neue Funktions-App und **Funktionen**. Klicken Sie unter Windows mit der rechten Maustaste (unter macOS <kbd>STRG</kbd>+Mausklick) auf die Funktion `HttpExample`, und wählen Sie **Funktion jetzt ausführen...** aus.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="„Funktion jetzt ausführen“ in Azure über Visual Studio Code":::

1. Unter **Enter request body** (Anforderungstext eingeben) wird `{ "name": "Azure" }` als Wert für den Text der Anforderungsnachricht angezeigt.

    Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.  

1. Wenn die Funktion in Azure ausgeführt wird und eine Antwort zurückgibt, wird in Visual Studio Code eine Benachrichtigung ausgelöst.

## <a name="5-clean-up-resources"></a>5. Bereinigen von Ressourcen

Wenn Sie mit dem [nächsten Schritt fortfahren](#next-steps) und eine <abbr title="Eine Möglichkeit, eine Funktion einer Speicherwarteschlange zuzuordnen, damit Nachrichten in der Warteschlange erstellt werden können."> Azure Storage-Warteschlangen-Ausgabebindung</abbr> zu Ihrer Funktion hinzufügen, müssen alle Ihre Ressourcen erhalten bleiben, um darauf aufbauen zu können.

Andernfalls können Sie die Funktions-App und die zugehörigen Ressourcen wie im Anschluss beschrieben löschen, um weitere Kosten zu vermeiden.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Weitere Informationen zu den Kosten von Functions finden Sie unter [Abschätzen der Kosten des Verbrauchstarifs](functions-consumption-costs.md).

## <a name="next-steps"></a>Nächste Schritte

Sie haben Visual Studio Code genutzt, um eine Funktions-App mit einer einfachen Funktion zu erstellen, die über HTTP ausgelöst wird. Im nächsten Artikel erweitern Sie diese Funktion durch Hinzufügen einer Ausgabe <abbr title="Eine deklarative Verbindung zwischen einer Funktion und anderen Ressourcen Eine Eingabebindung stellt Daten für die Funktion bereit. Eine Ausgabebindung stellt Daten aus der Funktion für andere Ressourcen bereit.">binding</abbr>. Diese Bindung schreibt die Zeichenfolge aus der HTTP-Anforderung in eine Nachricht in einer Azure Queue Storage-Warteschlange. 

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Azure Storage-Warteschlange](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-csharp)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
