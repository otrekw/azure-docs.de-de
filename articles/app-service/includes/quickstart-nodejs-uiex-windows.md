---
title: 'Schnellstart: Erstellen einer Node.js-Web-App – Windows'
description: Stellen Sie in wenigen Minuten Ihre erste Node.js-App vom Typ „Hallo Welt“ in Azure App Service für die Windows-Plattform bereit.
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 08/01/2020
ms.custom: mvc, devcenter, seodec18
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 324919c1d085302e675e0bc9a370ce35f52a0f1d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102109071"
---
<!-- advanced for windows -->

## <a name="3-deploy-to-azure-app-service-from-visual-studio-code"></a>3. Bereitstellen in Azure App Service mit Visual Studio Code

1. Öffnen Sie den Anwendungsordner in Visual Studio Code.

    ```bash
    code .
    ```

1. Wählen Sie im Explorer **AZURE APP SERVICE** die Option **Bei Azure anmelden...** aus, und befolgen Sie die Anweisungen. Nach der Anmeldung sollte im Explorer der Name Ihres Azure-Abonnements angezeigt werden.

    ![Anmelden bei Azure](../media/quickstart-nodejs/sign-in.png)

    <details>
    <summary>Behandeln von Problemen bei der Azure-Anmeldung</summary>
    
    Sollte bei der Azure-Anmeldung der Fehler **Abonnement mit dem Namen [Abonnement-ID] nicht gefunden** angezeigt werden, befinden Sie sich möglicherweise hinter einem Proxy und können die Azure-API nicht erreichen. Konfigurieren Sie die Umgebungsvariablen `HTTP_PROXY` und `HTTPS_PROXY` mithilfe von `export` in Ihrem Terminal mit Ihren Proxyinformationen.
    
    ```bash
    export HTTPS_PROXY=https://username:password@proxy:8080
    export HTTP_PROXY=http://username:password@proxy:8080
    ```

    [Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)


1. Wählen Sie im **AZURE APP SERVICE-Explorer** den blauen, nach oben weisenden Pfeil aus, um Ihre App in Azure bereitzustellen. 

    :::image type="content" source="../media/quickstart-nodejs/deploy.png" alt-text="Screenshot: Azure-App-Dienst in VS Code mit ausgewähltem blauem Pfeil":::

1. Wählen Sie das Verzeichnis aus, das Sie zurzeit geöffnet haben: `nodejs-docs-hello-world`.

1. Wählen Sie **Neue Web-App erstellen... Erweitert** für die Bereitstellung in App Service unter Windows.

1. Geben Sie einen global eindeutigen <abbr title="Gültige Zeichen für den Namen einer App sind die Buchstaben a-z, die Ziffern 0-9 und der Bindestrich (-).">name</abbr> für Ihre Web-App ein, und drücken Sie die **EINGABETASTE**. 
1. Wählen Sie **Neue Ressourcengruppe erstellen** aus, und geben Sie einen Namen für die Ressourcengruppe ein (beispielsweise `AppServiceQS-rg`).
1. Wählen Sie Ihre **Node.js-Version** aus. Empfohlen wird LTS.

    Der Benachrichtigungskanal zeigt die Azure-Ressourcen an, die für Ihre App erstellt werden.
1. Wählen Sie als Betriebssystem **Windows** aus.
1. Wählen Sie **Neuen App Services-Plan erstellen** aus, geben Sie einen Namen für den Plan ein (beispielsweise `AppServiceQS-plan`), und wählen Sie anschließend den Tarif **F1 Free** aus.
1. Wählen Sie **Vorerst überspringen** aus, wenn eine Eingabeaufforderung zu Application Insights sehen.
1. Wählen Sie eine Region in Ihrer Nähe oder in der Nähe von Ressourcen aus, auf die Sie zugreifen möchten.

1. Wählen Sie **Ja** aus, wenn Sie zum Aktualisieren Ihres Arbeitsbereichs aufgefordert werden, sodass spätere Bereitstellungen automatisch die gleiche App Service-Web-App als Ziel verwenden. 

    :::image type="content" source="../media/quickstart-nodejs/save-configuration.png" alt-text="Screenshot: Aufforderung zum Aktualisieren des Arbeitsbereichs mit ausgewählter Schaltfläche „Ja“":::

1. Klicken Sie noch einmal mit der rechten Maustaste auf den Knoten für den App-Dienst, und wählen Sie **In Web-App bereitstellen** aus.

1. Klicken Sie noch einmal mit der rechten Maustaste auf den Knoten für den App-Dienst, und wählen Sie **Website durchsuchen** aus.

    [Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&step=deploy-app)

1. Wählen Sie nach Abschluss der Bereitstellung an der Eingabeaufforderung **Website durchsuchen** aus, um Ihre neu bereitgestellte Web-App anzuzeigen.

<br/>
<details>
<summary><strong>Problembehandlung</strong></summary>

Wenn Sie diese Schritte nicht ausführen konnten, überprüfen Sie Folgendes:

* Stellen Sie sicher, dass Ihre Anwendung an dem Port lauscht, der von der PORT-Umgebungsvariable `process.env.PORT` bereitgestellt wird.

* Wenn der Fehler **Sie sind nicht berechtigt, dieses Verzeichnis oder diese Seite anzuzeigen.** angezeigt wird, wurde die Anwendung wahrscheinlich nicht ordnungsgemäß gestartet. Sehen Sie sich die Protokollausgabe an, um den Fehler zu suchen und zu beheben. 

</details>

<br>

[Problem melden](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azure-app-service&prepare-your-environment)


<br/>
<hr/>
