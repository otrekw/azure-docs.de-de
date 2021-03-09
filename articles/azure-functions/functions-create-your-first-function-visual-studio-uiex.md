---
title: 'Schnellstart: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von Visual Studio eine Azure-Funktion mit HTTP-Trigger erstellen und veröffentlichen.
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.topic: quickstart
ms.date: 09/30/2020
ms.custom: devx-track-csharp, mvc, devcenter, vs-azure, 23113853-34f2-4f
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9e3144738bd259ab9be75059af00f125581bb37c
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102050119"
---
# <a name="quickstart-create-your-first-function-in-azure-using-visual-studio"></a>Schnellstart: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio

In diesem Artikel wird mithilfe von Visual Studio eine auf der C#-Klassenbibliothek basierende Funktion erstellt, die auf HTTP-Anforderungen reagiert. Der Code wird lokal getestet und anschließend in der <abbr title="Eine Runtimecomputingumgebung, in der alle Serverdetails für Anwendungsentwickler transparent sind. Dies vereinfacht den Prozess der Bereitstellung sowie die Codeverwaltung.">Serverlos</abbr> Umgebung von <abbr title="Ein Azure-Dienst, der eine kostengünstige Umgebung für serverloses Computing für Anwendungen bietet.">Azure-Funktionen</abbr>.

Im Rahmen dieser Schnellstartanleitung fallen ggf. geringfügige Kosten im Centbereich in Ihrem <abbr title="Das Profil mit Abrechnungsinformationen für die Azure-Nutzung.">Azure-Konto</abbr>an.

## <a name="1-prepare-your-environment"></a>1. Vorbereiten der Umgebung

+ Erstellen Sie ein <abbr title="Das Profil mit Abrechnungsinformationen für die Azure-Nutzung.">Azure-Konto</abbr> mit einem aktiven <abbr title="Die grundlegende Organisationsstruktur, in der Sie Ressourcen in Azure verwalten. Diese wird in der Regel einer Einzelperson oder Abteilung innerhalb einer Organisation zugeordnet.">Abonnement</abbr>. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ Installieren Sie [Visual Studio 2019](https://azure.microsoft.com/downloads/), und wählen Sie bei der Installation die Workload **Azure-Entwicklung** aus. 

![Installieren von Visual Studio mit der Workload „Azure-Entwicklung“](media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<br/>
<details>
<summary><strong>Alternative: Verwenden eines Azure Functions-Projekts</strong></summary>
Wenn Sie stattdessen ein <abbr title="Ein logischer Container für eine oder mehrere einzelne Funktionen, die zusammen bereitgestellt und verwaltet werden können.">Azure Functions-Projekt</abbr> mit Visual Studio 2017 verwenden möchten, müssen Sie zuerst die [neuesten Azure Functions-Tools](functions-develop-vs.md#check-your-tools-version) installieren.
</details>

## <a name="2-create-a-function-app-project"></a>2. Erstellen eines Funktionen-App-Projekts

1. Wählen Sie im Visual Studio-Menü **Datei** > **Neu** > **Projekt** aus.

1. Geben Sie unter **Neues Projekt erstellen** den Suchbegriff *functions* in das Suchfeld ein, und wählen Sie die Vorlage **Azure Functions** und anschließend **Weiter** aus.

1. Geben Sie unter **Neues Projekt konfigurieren**, einen **<abbr title="Der Name der Funktions-App muss als C#-Namespace gültig sein, verwenden Sie daher keine Unterstriche, Bindestriche oder andere nicht alphanumerische Zeichen.">Projektnamen</abbr>** für Ihr Projekt ein, und wählen Sie anschließend **Erstellen** aus. 

1. Geben Sie für die Einstellungen unter **Neue Azure Functions-Anwendung erstellen** die folgenden Informationen an:

    + Wählen Sie **<abbr title="Durch diesen Wert wird ein Funktionsprojekt mit Version 3.x der Runtime von Azure Functions erstellt, die .NET Core 3.x unterstützt. Von Azure Functions 1.x wird das .NET Framework unterstützt.">Azure Functions v3 (.NET Core)</abbr>** aus der Dropdownliste für die Functions-Runtime aus. (Weitere Informationen finden Sie in der [Übersicht über die Runtimeversionen von Azure Functions](functions-versions.md).)
    
    + Wählen Sie **<abbr title="Mit diesem Wert wird eine Funktion erstellt, die durch eine HTTP-Anforderung ausgelöst wird.">HTTP-Trigger</abbr>** als Funktionsvorlage aus.
    
    + Wählen Sie **<abbr title="Da für eine Azure-Funktion ein Speicherkonto erforderlich ist, wird ein Speicherkonto zugewiesen oder erstellt, wenn Sie Ihr Projekt in Azure veröffentlichen. Von HTTP-Triggern wird keine Azure Storage-Kontoverbindungszeichenfolge verwendet. Für alle anderen Triggertypen ist dagegen eine gültige Azure Storage-Kontoverbindungszeichenfolge erforderlich.">Speicheremulator</abbr>** aus der Speicherkonto-Dropdownliste aus.
        
    + Wählen Sie **Anonym** aus der Dropdownliste für die <abbr title="Die erstellte Funktion kann von jedem Client ausgelöst werden, ohne dass ein Schlüssel angegeben werden muss. Diese Autorisierungseinstellung erleichtert Ihnen das Testen Ihrer neuen Funktion.">Autorisierungsstufe</abbr> aus. (Weitere Informationen zu Schlüsseln und zur Autorisierung finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys) sowie in der [Übersicht über HTTP-Trigger und -Bindungen in Azure Functions](functions-bindings-http-webhook.md).)

    + Klicken Sie auf **Erstellen**.
        
## <a name="3-rename-the-function"></a>3. Umbenennen der Funktion

Das Methodenattribut `FunctionName` dient zum Festlegen des Namens der Funktion, der standardmäßig als `Function1` generiert wird. Da der Standardfunktionsname bei der Projekterstellung mit den Tools nicht überschrieben werden kann, nehmen Sie sich eine Minute Zeit, um einen besseren Namen für die Funktionsklasse, die Datei und die Metadaten zu erstellen.

1. Klicken Sie im **Datei-Explorer** mit der rechten Maustaste auf die Datei „Function1.cs“, und benennen Sie sie in *HttpExample.cs* um.

1. Benennen Sie die Klasse „Function1“ im Code in ‚HttpExample‘ um.

1. Benennen Sie in der `HttpTrigger`-Methode namens `Run` das Methodenattribut `FunctionName` in `HttpExample` um.


## <a name="4-run-the-function-locally"></a>4. Lokales Ausführen der Funktion

1. Drücken Sie in Visual Studio <kbd>F5</kbd>, um Ihre Funktion auszuführen.

1. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.

    ![Lokale Azure-Laufzeit](../../includes/media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

1. Fügen Sie die URL zu der HTTP-Anforderung in die Adressleiste Ihres Browsers ein. Fügen Sie anschließend die Abfragezeichenfolge **?name=<IHR_NAME>** an diese URL an, und führen Sie die Anforderung aus. 

    ![localhost-Antwort der Funktion im Browser](../../includes/media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

1. Drücken Sie in Visual Studio <kbd>UMSCHALTTASTE</kbd>+<kbd>F5</kbd>, um das Debuggen zu beenden.

<br/>
<details>
<summary><strong>Problembehandlung</strong></summary>
 Sie müssen unter Umständen eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können. Autorisierungsebenen werden niemals erzwungen, wenn Sie eine Funktion lokal ausführen.
</details>

## <a name="5-publish-the-project-to-azure"></a>5. Veröffentlichen des Projekts in Azure

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen** aus.

1. Wählen Sie unter **Ziel** die Option **Azure** aus.

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-1.png" alt-text="Auswählen des Ziels „Azure“":::

1. Wählen Sie unter **Spezifisches Ziel** die Option **Azure-Funktions-App (Windows)** aus.

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-2.png" alt-text="Auswählen von „Azure-Funktions-App“":::

1. Wählen Sie unter **Funktionsinstanz** die Option **Neue Azure-Funktion erstellen...** aus, und verwenden Sie dann die im Anschluss angegebenen Werte:

    + Geben Sie unter **Name** Folgendes an: <abbr title="Verwenden Sie einen Namen, der Ihre neue Funktions-App eindeutig identifiziert. Übernehmen Sie diesen Namen, oder geben Sie einen neuen Namen ein. Gültige Zeichen sind `a-z`, `0-9` und `-`.">Global eindeutiger Name</abbr>
    
    + **Wählen** Sie ein Abonnement aus der Dropdownliste aus.
    
    + **Wählen** Sie eine vorhandene <abbr title="Ein logischer Container für verwandte Azure-Ressourcen, die Sie als Einheit verwalten können.">Ressourcengruppe</abbr> aus der Dropdownliste aus, oder wählen Sie **Neu** aus, um eine neue Ressourcengruppe zu erstellen.
    
    + **Wählen** Sie <abbr title="Wenn Sie Ihr Projekt in einer Funktions-App veröffentlichen, die in einem Verbrauchsplan ausgeführt wird, bezahlen Sie nur für die Ausführungen Ihrer Funktions-App. Für andere Hostingpläne fallen höhere Kosten an.">Nutzung</abbr> in der Dropdownliste für den Wiedergabetyp aus. (Weitere Informationen finden Sie unter [Verbrauchsplan](consumption-plan.md).)
    
    + **Wählen** Sie einen  <abbr title="Ein geografischer Verweis auf ein bestimmtes Azure-Rechenzentrum, in dem Ressourcen zugeordnet werden. Eine Liste mit den verfügbaren Regionen finden Sie unter [Regionen](https://azure.microsoft.com/regions/).">Standort</abbr> aus der Dropdownliste aus.
    
    + **Wählen** Sie ein <abbr="Für die Functions-Runtime wird ein Azure Storage-Konto benötigt. Wählen Sie Neu aus, um ein universelles Speicherkonto zu konfigurieren. Sie können auch ein vorhandenes Konto auswählen, das die Speicherkontoanforderungen erfüllt.">Azure Storage-Konto</abbr> aus der Dropdownliste aus.

    ![Dialogfeld „App Service erstellen“](../../includes/media/functions-vstools-publish/functions-visual-studio-publish.png)

1. Klicken Sie auf **Erstellen**. 

1. Vergewissern Sie sich unter **Funktionsinstanz**, dass die Option **Aus Paketdatei ausführen** aktiviert ist. 

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-4.png" alt-text="Abschließen der Profilerstellung":::

    <br/>
    <details>
    <summary><strong>Wozu dient diese Einstellung?</strong></summary>
    Bei Verwendung von **Aus Paketdatei ausführen** wird Ihre Funktions-App unter Verwendung der [ZIP-Bereitstellung](functions-deployment-technologies.md#zip-deploy) mit aktiviertem Modus [Run-From-Package](run-functions-from-deployment-package.md) bereitgestellt. Dies ist die empfohlene Bereitstellungsmethode für Ihr Funktionsprojekt, da damit eine bessere Leistung erzielt wird.    
    </details>   

1. Wählen Sie **Fertig stellen** aus.

1. Klicken Sie auf der Seite Veröffentlichen auf **Veröffentlichen**.

1. Überprüfen Sie auf der Seite „Veröffentlichen“ die Stamm-URL der Funktions-App.

1. Wählen Sie auf der Registerkarte „Veröffentlichen“ die Option **Im <abbr title="Im Cloud-Explorer können Sie Visual Studio verwenden, um die Inhalte der Website anzuzeigen, die Funktions-App zu starten und zu beenden und direkt zu Funktions-App-Ressourcen in Azure und im Azure-Portal zu navigieren.">Cloud-Explorer</abbr> verwalten** aus.
    
    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-complete.png" alt-text="Veröffentlichen einer Erfolgsmeldung":::
    

## <a name="6-test-your-function-in-azure"></a>6. Testen der Funktion in Azure

1. In Cloud-Explorer sollte Ihre neue Funktions-App ausgewählt sein. Erweitern Sie andernfalls Ihr Abonnement, erweitern Sie **App Services**, und wählen Sie Ihre neue Funktions-App aus.

1. Klicken Sie mit der rechten Maustaste auf die Funktions-App, und wählen Sie **In Browser öffnen** aus. Dadurch wird der Stamm ihrer Funktions-App in Ihrem Standardwebbrowser geöffnet und die Seite angezeigt, die anzeigt, dass ihre Funktions-App ausgeführt wird. 

    :::image type="content" source="media/functions-create-your-first-function-visual-studio/function-app-running-azure.png" alt-text="Ausgeführte Funktions-App":::

1. Fügen Sie in der Adressleiste des Browsers die Zeichenfolge **/api/HttpExample?name=Functions** an die Basis-URL an, und führen Sie die Anforderung aus.

    Die URL, über die Ihre Funktion mit HTTP-Trigger aufgerufen wird, hat das folgende Format:

    `http://<APP_NAME>.azurewebsites.net/api/HttpExample?name=Functions`

2. Wechseln Sie zu dieser URL, und im Browser wird eine von der App auf die GET-Remoteanforderung zurückgegebene Antwort angezeigt, die wie im folgenden Beispiel aussieht:

    :::image type="content" source="media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png" alt-text="Funktionsantwort im Browser":::

## <a name="7-clean-up-resources"></a>7. Bereinigen von Ressourcen

Löschen Sie die Funktions-App und die zugehörigen Ressourcen, um weitere Kosten zu vermeiden.

1. Erweitern Sie im Cloud-Explorer Ihr Abonnement, erweitern Sie **App Services**, klicken Sie mit der rechten Maustaste auf Ihre Funktions-App, und wählen Sie **Im Portal öffnen** aus. 

1. Wählen Sie auf der Seite „Funktions-App“ die Registerkarte **Übersicht** und anschließend unter **Ressourcengruppe** den Link aus.

   :::image type="content" source="media/functions-create-your-first-function-visual-studio/functions-app-delete-resource-group.png" alt-text="Auswählen der zu löschenden Ressourcengruppe auf der Seite „Funktions-App“":::

1. Prüfen Sie auf der Seite **Ressourcengruppe** die Liste mit den enthaltenen Ressourcen, und vergewissern Sie sich, dass es sich dabei um die Ressourcen handelt, die Sie löschen möchten.
 
1. Klicken Sie auf **Ressourcengruppe löschen**, und folgen Sie den Anweisungen.

    Der Löschvorgang kann einige Minuten dauern. Nach Abschluss des Vorgangs wird kurz eine Benachrichtigung angezeigt. Sie können auch am oberen Seitenrand auf das Glockensymbol klicken, um die Benachrichtigung anzuzeigen.

## <a name="next-steps"></a>Nächste Schritte

Im nächsten Artikel erfahren Sie, wie Sie eine <abbr title="Eine Möglichkeit, eine Funktion einer Speicherwarteschlange zuzuordnen, damit Nachrichten in der Warteschlange erstellt werden können. Bindungen sind deklarative Verbindungen zwischen einer Funktion und anderen Ressourcen. Eine Eingabebindung stellt Daten für die Funktion bereit. Eine Ausgabebindung stellt Daten aus der Funktion für andere Ressourcen bereit.">Azure Storage-Warteschlangen-Ausgabebindung</abbr> zu Ihrer Funktion hinzufügen:

> [!div class="nextstepaction"]
> [Hinzufügen einer Azure Storage-Warteschlangenbindung zu Ihrer Funktion](functions-add-output-binding-storage-queue-vs.md)
