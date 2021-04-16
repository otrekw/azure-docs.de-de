---
title: Erstellen von Logic Apps-Workflows (Vorschau) in Visual Studio Code
description: Erstellen Sie Workflows für Automatisierungs- und Integrationsszenarien in Visual Studio Code mit der Erweiterung „Azure Logic Apps (Vorschau)“, und führen Sie sie aus.
services: logic-apps
ms.suite: integration
ms.reviewer: estfan, logicappspm, az-logic-apps-dev
ms.topic: conceptual
ms.date: 03/30/2021
ms.openlocfilehash: 491d5f14cc8f456d228a5bc6efaa6686575979c1
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/31/2021
ms.locfileid: "106078739"
---
# <a name="create-stateful-and-stateless-workflows-in-visual-studio-code-with-the-azure-logic-apps-preview-extension"></a>Erstellen zustandsbehafteter und zustandsloser Workflows in Visual Studio Code mit der Erweiterung „Azure Logic Apps (Vorschau)“

> [!IMPORTANT]
> Diese Funktion befindet sich in der öffentlichen Vorschauphase, wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mit [Azure Logic Apps (Vorschau)](logic-apps-overview-preview.md) können Sie Automatisierungs- und Integrationslösungen für Apps, Daten, Clouddienste und Systeme erstellen, indem Sie Logik-Apps erstellen und ausführen, die [*zustandsbehaftete* und *zustandslose* Workflows](logic-apps-overview-preview.md#stateful-stateless) mit der neuen Erweiterung „Azure Logic Apps (Vorschau)“ in Visual Studio Code integrieren. Mit diesem neuen Logik-App-Typ können Sie mehrere Workflows erstellen, die von der neu gestalteten Azure Logic Apps-Runtime (Vorschau) unterstützt werden. Diese Runtime bietet Portabilität, eine bessere Leistung und mehr Flexibilität bei der Bereitstellung und Ausführung in verschiedenen Hostingumgebungen – nicht nur in Azure, sondern auch in Docker-Containern. Weitere Informationen zum neuen Logik-App-Typ finden Sie unter [Übersicht über die Vorschauversion von Azure Logic Apps](logic-apps-overview-preview.md).

![Screenshot, der Visual Studio Code, Logik-App-Projekt und Workflow zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-logic-apps-overview.png)

In Visual Studio Code können Sie zunächst ein Projekt erstellen, bei dem Sie die Workflows Ihrer Logik-App *lokal* in Ihrer Entwicklungsumgebung erstellen und ausführen können, indem Sie die Erweiterung „Azure Logic Apps (Vorschau)“ verwenden. Sie können zwar auch mit dem [Erstellen einer neuen **Logic Apps (Vorschau)** -Ressource im Azure-Portal](create-stateful-stateless-workflows-azure-portal.md) beginnen, aber beide Ansätze bieten Ihnen die Möglichkeit, ihre Logik-App in denselben Hostingumgebungen bereitzustellen und auszuführen.

In der Zwischenzeit können Sie weiterhin den ursprünglichen Logik-App-Typ erstellen. Trotz der Unterschiede bei der Entwicklung in Visual Studio Code zwischen dem ursprünglichen und dem neuen Logik-App-Typ kann Ihr Azure-Abonnement beide Typen enthalten. Sie können alle bereitgestellten Logik-Apps in Ihrem Azure-Abonnement anzeigen und darauf zugreifen, aber die Apps werden getrennt in ihren eigenen Kategorien und Abschnitten angeordnet.

In diesem Artikel erfahren Sie, wie Sie Ihre Logik-App und einen Workflow in Visual Studio Code erstellen, indem Sie die Erweiterung „Azure Logic Apps (Vorschau)“ verwenden und diese Aufgaben auf hoher Ebene ausführen:

* Erstellen eines Projekts für Ihre Logik-App und eines Workflows.

* Hinzufügen eines Triggers und einer Aktion.

* Lokales Ausführen, Testen, Debuggen und Überprüfen des Ausführungsverlaufs.

* Suchen von Domänennamensdetails für den Firewallzugriff.

* Bereitstellen in Azure, was die optionale Aktivierung von Application Insights beinhaltet.

* Verwalten Ihrer bereitgestellten Logik-App in Visual Studio Code und im Azure-Portal.

* Aktivieren des Ausführungsverlaufs für zustandslose Workflows.

* Aktivieren oder Öffnen von Application Insights nach der Bereitstellung.

* Bereitstellen in einem Docker-Container, den Sie überall ausführen können.

> [!NOTE]
> Informationen zu aktuellen bekannten Problemen finden Sie in GitHub unter [Logic Apps Public Preview Known Issues](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md) (Bekannte Probleme der öffentlichen Vorschauversion von Logic Apps).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="access-and-connectivity"></a>Zugriff und Konnektivität

* Zugriff auf das Internet, damit Sie die Anforderungen herunterladen, von Visual Studio Code aus eine Verbindung mit Ihrem Azure-Konto herstellen und aus Visual Studio Code in Azure, einem Docker-Container oder einer anderen Umgebung veröffentlichen können.

* Ein Azure-Konto und ein Azure-Abonnement. Falls Sie kein Abonnement besitzen, können Sie sich [für ein kostenloses Azure-Konto registrieren](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

* Um dieselbe Beispiel-Logik-App in diesem Artikel zu erstellen, benötigen Sie ein Office 365 Outlook-E-Mail-Konto, das ein Geschäfts-, Schul- oder Unikonto von Microsoft für die Anmeldung verwendet.

  Wenn Sie sich entschließen, einen anderen [E-Mail-Connector zu verwenden, der von Azure Logic Apps unterstützt wird](/connectors/), z. B. Outlook.com oder [Gmail](../connectors/connectors-google-data-security-privacy-policy.md), können Sie dennoch das Beispiel durcharbeiten, und die allgemeinen übergeordneten Schritte sind identisch, doch Ihre Benutzeroberfläche und deren Optionen können sich in gewisser Weise unterscheiden. Wenn Sie beispielsweise den Outlook.com-Connector verwenden, verwenden Sie stattdessen Ihr persönliches Microsoft-Konto, um sich anzumelden.

<a name="storage-requirements"></a>

### <a name="storage-requirements"></a>Speicheranforderungen

#### <a name="windows"></a>Windows

Führen Sie die folgenden Schritte zum Einrichten des Azure-Speicheremulators aus, um Ihr Logik-App-Projekt unter Windows in Visual Studio Code lokal zu erstellen und auszuführen:

1. Laden Sie den [Azure Storage-Emulator 5.10](https://go.microsoft.com/fwlink/p/?linkid=717179) herunter, und installieren Sie ihn.

1. Sofern noch nicht vorhanden, benötigen Sie eine lokale SQL-Datenbank-Installation, z. B. die kostenlose [SQL Server 2019 Express Edition](https://go.microsoft.com/fwlink/p/?linkid=866658), damit der Emulator ausgeführt werden kann.

   Weitere Informationen finden Sie unter [Verwenden des Azure Storage-Emulators für Entwicklung und Tests](../storage/common/storage-use-emulator.md).

1. Bevor Sie Ihr Projekt ausführen können, müssen Sie den Emulator starten.

   ![Screenshot, der den ausgeführten Azure Storage-Emulator zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/start-storage-emulator.png)

#### <a name="macos-and-linux"></a>macOS und Linux

Führen Sie die hier angegebenen Schritte zum Erstellen und Einrichten eines Azure Storage-Kontos aus, um Ihr Logik-App-Projekt unter macOS oder Linux in Visual Studio Code lokal zu erstellen und auszuführen.

> [!NOTE]
> Derzeit funktioniert der Designer in Visual Studio Code nicht mit einem Linux-Betriebssystem. Sie können aber weiterhin Logik-Apps ausführen, erstellen und bereitstellen, für die die Vorschauversion der Logic Apps-Runtime für Linux-VMs verwendet wird. Zum jetzigen Zeitpunkt können Sie Ihre Logik-Apps in Visual Studio Code unter Windows oder macOS erstellen und dann auf einer Linux-basierten VM bereitstellen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an, und [erstellen Sie ein Azure Storage Konto](../storage/common/storage-account-create.md?tabs=azure-portal), das eine [Voraussetzung für Azure Functions](../azure-functions/storage-considerations.md) ist.

1. Wählen Sie im Menü Ihres Speicherkontos unter **Einstellungen** die Option **Zugriffsschlüssel** aus.

1. Suchen Sie im Bereich **Zugriffsschlüssel** nach der Verbindungszeichenfolge für das Speicherkonto, und kopieren Sie sie. Sie sieht in etwa wie im folgenden Beispiel aus:

   `DefaultEndpointsProtocol=https;AccountName=fabrikamstorageacct;AccountKey=<access-key>;EndpointSuffix=core.windows.net`

   ![Screenshot, der das Azure-Portal mit Speicherkonto-Zugriffsschlüsseln und kopierter Verbindungszeichenfolge anzeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/find-storage-account-connection-string.png)

   Weitere Informationen finden Sie unter [Anzeigen von Kontozugriffsschlüsseln](../storage/common/storage-account-keys-manage.md?tabs=azure-portal#view-account-access-keys).

1. Speichern Sie die Verbindungszeichenfolge an einem sicheren Ort. Nachdem Sie Ihr Logik-App-Projekt in Visual Studio Code erstellt haben, müssen Sie die Zeichenfolge der Datei **local.settings.json** im Stammebenenordner Ihres Projekts hinzufügen.

   > [!IMPORTANT]
   > Falls Sie die Bereitstellung in einem Docker-Container planen, müssen Sie diese Verbindungszeichenfolge mit der Docker-Datei nutzen, die Sie für die Bereitstellung nutzen. Achten Sie bei Produktionsszenarien darauf, dass Sie solche Geheimnisse und sensiblen Informationen schützen und sichern, z. B. durch die Verwendung eines Schlüsseltresors.
  
### <a name="tools"></a>Tools

* [Visual Studio Code 1.30.1 (Januar 2019) oder höher](https://code.visualstudio.com/) (kostenlos). Laden Sie darüber hinaus diese Tools für Visual Studio Code herunter, und installieren Sie sie, falls sie noch nicht vorhanden sind:

  * [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account), die eine einzelne allgemeine Azure-Anmeldungs- und -Abonnementfilterungserfahrung für alle anderen Azure-Erweiterungen in Visual Studio Code bereitstellt.

  * [C# für Visual Studio Code-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp), mit der die F5-Funktionalität in die Lage versetzt wird, Ihre Logik-App auszuführen.

  * [Azure Functions Core Tools 3.0.3245 oder höher](https://github.com/Azure/azure-functions-core-tools/releases/tag/3.0.3245) über die MSI-Version (Microsoft Installer), die `func-cli-3.0.3245-x*.msi` lautet.

    Diese Tools enthalten eine Version derselben Runtime, auf der die Azure Functions-Runtime basiert, die die Erweiterung in der Vorschau in Visual Studio Code verwendet.

    > [!IMPORTANT]
    > Wenn Sie über eine Installation verfügen, die älter als diese Versionen ist, deinstallieren Sie diese Version zuerst, oder stellen Sie sicher, dass die PATH-Umgebungsvariable auf die Version verweist, die Sie herunterladen und installieren.

  * [Azure Logic Apps (Vorschau)-Erweiterung für Visual Studio Code](https://go.microsoft.com/fwlink/p/?linkid=2143167). Diese Erweiterung bietet Ihnen die Möglichkeit, Logik-Apps zu erstellen, in denen Sie zustandsbehaftete und zustandslose Workflows erstellen können, die lokal in Visual Studio Code ausgeführt werden, und diese Logik-Apps dann direkt in Azure oder Docker-Containern bereitzustellen.

    Zurzeit können Sie die ursprüngliche Azure Logic Apps-Erweiterung und die Public Preview-Erweiterung in Visual Studio Code parallel installieren. Obwohl sich die Entwicklungsumgebungen in gewisser Weise zwischen den Erweiterungen unterscheiden, kann Ihr Azure-Abonnement beide Logik-App-Typen enthalten, die Sie mit den Erweiterungen erstellen. Visual Studio Code zeigt alle bereitgestellten Logik-Apps in Ihrem Azure-Abonnement an, organisiert sie aber in verschiedenen Abschnitten nach Erweiterungsnamen, **Logik-Apps** und **Azure Logic Apps (Vorschau)** .

    > [!IMPORTANT]
    > Wenn Sie Logik-App-Projekte mit der älteren Erweiterung in der privaten Vorschau erstellt haben, funktionieren diese Projekte nicht mit der Public Preview-Erweiterung. Sie können die Projekte aber migrieren, nachdem Sie die Erweiterung in der privaten Vorschau deinstalliert, die zugehörigen Dateien gelöscht und die Public Preview-Erweiterung installiert haben. Anschließend erstellen Sie ein neues Projekt in Visual Studio Code und kopieren Ihre zuvor erstellte **workflow.definition**-Datei der Logik-App in Ihr neues Projekt. Weitere Informationen finden Sie unter [Migration von der Erweiterung in der privaten Vorschau](#migrate-private-preview).
    > 
    > Wenn Sie Logik-App-Projekte mit der Public Preview-Erweiterung erstellt haben, können Sie diese Projekte weiter nutzen, ohne dass Migrationsschritte ausgeführt werden müssen.

    **Führen Sie zum Installieren der Erweiterung **Azure Logic Apps (Vorschau)** die folgenden Schritte aus:**

    1. Wählen Sie in Visual Studio Code in der linken Symbolleiste **Erweiterungen** aus.

    1. Geben Sie in das Suchfeld für Erweiterungen `azure logic apps preview` ein. Wählen Sie in der Ergebnisliste **Azure Logic Apps (Vorschau)** **>** **Installieren** aus.

       Nach Abschluss der Installation wird die Erweiterung in der Vorschau in der Liste **Erweiterungen: Installiert** angezeigt.

       ![Screenshot, der die Liste der installierten Erweiterungen in Visual Studio Code mit unterstrichener Erweiterung „Azure Logic Apps (Vorschau)“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-extension-installed.png)

       > [!TIP]
       > Versuchen Sie, Visual Studio Code neu zu starten, falls die Erweiterung nicht in der Installationsliste angezeigt wird.

* Installieren Sie die [Node.js-Versionen 10.x.x, 11.x.x oder 12.x.x](https://nodejs.org/en/download/releases/), um die [„Inlinecodevorgänge“-Aktion](../logic-apps/logic-apps-add-run-inline-code.md) mit Ausführung von JavaScript zu verwenden.

  > [!TIP] 
  > Laden Sie für Windows die MSI-Version herunter. Falls Sie stattdessen die ZIP-Version nutzen, müssen Sie Node.js manuell verfügbar machen, indem Sie eine PATH-Umgebungsvariable für Ihr Betriebssystem verwenden.

* Zum lokalen Ausführen von webhookbasierten Triggern und Aktionen wie des [integrierten HTTP-Webhook-Triggers](../connectors/connectors-native-webhook.md) in Visual Studio Code müssen Sie die [Weiterleitung für die Rückruf-URL einrichten](#webhook-setup).

* Zum Testen der Beispiel-Logik-App, die Sie in diesem Artikel erstellen, benötigen Sie ein Tool, das Aufrufe an den Anforderungstrigger senden kann, wobei es sich um den ersten Schritt in der Beispiel-Logik-App handelt. Falls Sie nicht über ein solches Tool verfügen, können Sie [Postman](https://www.postman.com/downloads/) herunterladen, installieren und verwenden.

* Wenn Sie Ihre Logik-App erstellen und mit Einstellungen bereitstellen, die die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren. Dies können Sie entweder tun, wenn Sie Ihre Logik-App über Visual Studio Code bereitstellen, oder nach der Bereitstellung. Sie benötigen eine Application Insights-Instanz, können aber diese Ressource entweder [im Voraus](../azure-monitor/app/create-workspace-resource.md), beim Bereitstellen Ihrer Logik-App oder nach der Bereitstellung erstellen.

<a name="migrate-private-preview"></a>

## <a name="migrate-from-private-preview-extension"></a>Migration von der Erweiterung in der privaten Vorschau

Logik-App-Projekte, die Sie mithilfe der **Azure Logic Apps-Erweiterung (Private Vorschau)** erstellt haben, funktionieren nicht mit der Public Preview-Erweiterung. Sie können diese Projekte aber zu neuen Projekten migrieren, indem Sie die folgenden Schritte ausführen:

1. Deinstallieren Sie die Erweiterung in der privaten Vorschau.

1. Löschen Sie alle zugehörigen Erweiterungspakete und NuGet-Paketordner an den folgenden Speicherorten:

   * Den Ordner **Microsoft.Azure.Functions.ExtensionBundle.Workflows**, der vorherige Erweiterungsbündel enthält und sich in einem dieser beiden Pfade befindet:

     * `C:\Users\{userName}\AppData\Local\Temp\Functions\ExtensionBundles`

     * `C:\Users\{userName}\.azure-functions-core-tools\Functions\ExtensionBundles`

   * Den Ordner **microsoft.azure.workflows.webjobs.extension**, der der [NuGet](/nuget/what-is-nuget)-Cache für die private Vorschauerweiterung ist und sich in diesem Pfad befindet:

     `C:\Users\{userName}\.nuget\packages`

1. Installieren Sie die Erweiterung **Azure Logic Apps (Vorschau)** .

1. Erstellen Sie ein neues Projekt in Visual Studio Code.

1. Kopieren Sie die Datei **workflow.definition** der zuvor erstellten Logik-App in Ihr neues Projekt.

<a name="set-up"></a>

## <a name="set-up-visual-studio-code"></a>Einrichten von Visual Studio Code

1. Um sicherzustellen, dass alle Erweiterungen ordnungsgemäß installiert sind, laden Sie Visual Studio Code neu, oder starten Sie es erneut.

1. Vergewissern Sie sich, dass Visual Studio Code Erweiterungsupdates automatisch sucht und installiert, damit Ihre Vorschauerweiterung die neuesten Updates erhält. Andernfalls müssen Sie die veraltete Version manuell deinstallieren und die neueste Version installieren.

   1. Navigieren Sie im Menü **Datei** zu **Voreinstellungen** **>** **Einstellungen**.

   1. Wechseln Sie auf der Registerkarte **Benutzer** zu **Features** **>** **Erweiterungen**.

   1. Vergewissern Sie sich, dass **Updates automatisch suchen** und **Automatische Aktualisierung** ausgewählt sind.

Darüber hinaus sind standardmäßig die folgenden Einstellungen aktiviert und für die Logic Apps-Vorschauerweiterung festgelegt:

* **Azure Logic Apps V2: Projektlaufzeit**, festgelegt auf Version **~3**

  > [!NOTE]
  > Diese Version ist erforderlich, um die [Aktionen in Inlinecodevorgängen](../logic-apps/logic-apps-add-run-inline-code.md) verwenden zu können.

* **Azure Logic Apps V2: Experimental View Manager**: Hiermit wird der aktuelle Designer in Visual Studio Code aktiviert. Deaktivieren Sie diese Einstellung, falls für den Designer Probleme auftreten, z. B. beim Ziehen und Ablegen von Elementen.

Führen Sie die folgenden Schritte aus, um nach diesen Einstellungen zu suchen und sie zu bestätigen:

1. Navigieren Sie im Menü **Datei** zu **Voreinstellungen** **>** **Einstellungen**.

1. Wechseln Sie auf der Registerkarte **Benutzer** zu **>** **Erweiterungen** **>** **Azure Logic Apps (Vorschau)** .

   Beispielsweise finden Sie hier die Einstellung **Azure Logic Apps V2: Projektlaufzeit** und können über das Suchfeld nach anderen Einstellungen suchen:

   ![Screenshot, der die Visual Studio Code-Einstellungen für die Erweiterung „Azure Logic Apps (Vorschau)“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/azure-logic-apps-preview-settings.png)

<a name="connect-azure-account"></a>

## <a name="connect-to-your-azure-account"></a>Herstellen einer Verbindung mit Ihrem Azure-Konto

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus.

   ![Screenshot, der die Visual Studio Code-Aktivitätsleiste mit ausgewähltem Azure-Symbol zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-azure-icon.png)

1. Wählen Sie im Azure-Bereich unter **Azure: Logic Apps (Vorschau)** die Option **Bei Azure anmelden** aus. Wenn die Authentifizierungsseite für Visual Studio Code angezeigt wird, melden Sie sich mit Ihrem Azure-Konto an.

   ![Screenshot, der den Azure-Bereich und den ausgewählten Link für die Azure-Anmeldung zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/sign-in-azure-subscription.png)

   Nachdem Sie sich angemeldet haben, werden im Azure-Bereich die Abonnements in Ihrem Azure-Konto angezeigt. Wenn Sie auch über die öffentlich herausgegebene Erweiterung verfügen, finden Sie alle Logik-Apps, die Sie mit dieser Erweiterung erstellt haben, im Abschnitt **Logic Apps** und nicht im Abschnitt **Logic Apps (Vorschau)** .
   
   Wenn die erwarteten Abonnements nicht angezeigt werden oder der Bereich nur bestimmte Abonnements anzeigen soll, führen Sie die folgenden Schritte aus:

   1. Bewegen Sie in der Liste „Abonnements“ den Zeiger neben das erste Abonnement, bis die Schaltfläche (Filtersymbol) **Abonnements auswählen** angezeigt wird. Wählen Sie das Filtersymbol aus.

      ![Screenshot, der den Azure-Bereich und das ausgewählte Filtersymbol zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/filter-subscription-list.png)

      Oder wählen Sie in der Statusleiste von Visual Studio Code Ihr Azure-Konto aus. 

   1. Wenn eine weitere Abonnement liste angezeigt wird, wählen Sie die gewünschten Abonnements aus, und stellen Sie dann sicher, dass Sie **OK** auswählen.

<a name="create-project"></a>

## <a name="create-a-local-project"></a>Erstellen eines lokalen Projekts

Bevor Sie Ihre Logik-App erstellen können, erstellen Sie ein lokales Projekt, damit Sie Ihre Logik-App über Visual Studio Code verwalten, ausführen und bereitstellen können. Das zugrunde liegende Projekt ähnelt stark einem Azure Functions-Projekt, das auch als Funktions-App-Projekt bezeichnet wird. Diese Projekttypen sind jedoch voneinander getrennt, sodass Logik-Apps und Funktions-App nicht im selben Projekt vorhanden sein können.

1. Erstellen Sie auf Ihrem Computer einen *leeren* lokalen Ordner, der für das Projekt verwendet werden soll, das Sie später in Visual Studio Code erstellen werden.

1. Schließen Sie in Visual Studio Code alle offenen Ordner.

1. Wählen Sie im Azure-Bereich neben **Azure: Logic Apps (Vorschau)** die Option **Neues Projekt erstellen** (Symbol mit Ordner und Blitz) aus.

   ![Screenshot, der die Symbolleiste im Azure-Bereich mit ausgewähltem „Neues Projekt erstellen“ anzeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/create-new-project-folder.png)

1. Wenn Sie von der Windows Defender Firewall aufgefordert werden, für `Code.exe`, wobei es sich um Visual Studio Code handelt, und für `func.exe`, wobei es sich um Azure Functions Core Tools handelt, Netzwerkzugriff zu gewähren, wählen Sie **Private Netzwerke, z. B. Heim- oder Firmennetzwerk** **>** **Zugriff zulassen** aus.

1. Navigieren Sie zu dem Speicherort, an dem Sie Ihren Projektordner erstellt haben, wählen Sie diesen Ordner aus, und fahren Sie fort.

   ![Screenshot, der das Dialogfeld „Ordner auswählen“ mit einem neu erstellten Projektordner und der ausgewählten Schaltfläche „Auswählen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-project-folder.png)

1. Wählen Sie in der angezeigten Vorlagenliste entweder **Zustandsbehafteter Workflow** oder **Zustandsloser Workflow** aus. In diesem Beispiel wird **Zustandsbehafteter Workflow** ausgewählt.

   ![Screenshot, der die Liste der Workflowvorlagen mit ausgewähltem „Zustandsbehafteter Workflow“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-stateful-stateless-workflow.png)

1. Geben Sie einen Namen für Ihren Workflow ein, und drücken Sie die EINGABETASTE. In diesem Beispiel wird `Fabrikam-Stateful-Workflow` als Name verwendet.

   ![Screenshot: Feld „Neuen zustandsbehafteten Workflow erstellen (3/4)“ und „Fabrikam-Stateful-Workflow“ als Workflowname](./media/create-stateful-stateless-workflows-visual-studio-code/name-your-workflow.png)

   Visual Studio Code schließt die Erstellung des Projekts ab und öffnet die Datei **workflow.json** für Ihren Workflow im Code-Editor.

   > [!NOTE]
   > Wählen Sie in der Eingabeaufforderung zum Öffnen des Projekts die Option **In aktuellem Fenster öffnen** aus, wenn Sie Ihr Projekt im aktuellen Visual Studio Code-Fenster öffnen möchten. Wählen Sie **In neuem Fenster öffnen** aus, um eine neue Instanz für Visual Studio Code zu öffnen.

1. Öffnen Sie in der Visual Studio-Symbolleiste den Explorer-Bereich, falls er nicht bereits geöffnet ist.

   Im Explorer-Bereich wird Ihr Projekt angezeigt, das nun automatisch generierte Projektdateien enthält. Beispielsweise verfügt das Projekt über einen Ordner, der den Namen Ihres Workflows trägt. In diesem Ordner enthält die **workflow.json**-Datei die zugrunde liegende JSON-Definition Ihres Workflows.

   ![Screenshot, der den Explorer-Bereich mit Projektordner, Workflowordner und der Datei „workflow.json“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/local-project-created.png)

1. Richten Sie bei Verwendung von macOS oder Linux den Zugriff auf Ihr Speicherkonto ein, indem Sie die folgenden Schritte ausführen, die für die lokale Ausführung Ihres Projekts erforderlich sind:

   1. Öffnen Sie im Stammordner Ihres Projekts die Datei **local.settings.json**.

      ![Screenshot: Explorer-Bereich und Datei „local.settings.json“ in Ihrem Projekt](./media/create-stateful-stateless-workflows-visual-studio-code/local-settings-json-files.png)

   1. Ersetzen Sie den Eigenschaftswert `AzureWebJobsStorage` durch die Verbindungszeichenfolge des Speicherkontos, die Sie zuvor gespeichert haben. Beispiel:

      Vorher:

      ```json
      {
         "IsEncrypted": false,
         "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "FUNCTIONS_WORKER_RUNTIME": "dotnet"
          }
      }
      ```

      Nachher:

      ```json
      {
         "IsEncrypted": false,
         "Values": {
            "AzureWebJobsStorage": "DefaultEndpointsProtocol=https;AccountName=fabrikamstorageacct;AccountKey=<access-key>;EndpointSuffix=core.windows.net",
           "FUNCTIONS_WORKER_RUNTIME": "dotnet"
         }
      }
      ```

      > [!IMPORTANT]
      > Achten Sie bei Produktionsszenarien darauf, dass Sie solche Geheimnisse und sensiblen Informationen schützen und sichern, z. B. durch die Verwendung eines Schlüsseltresors.

   1. Achten Sie darauf, dass Sie nach Abschluss des Vorgangs Ihre Änderungen speichern.

<a name="enable-built-in-connector-authoring"></a>

## <a name="enable-built-in-connector-authoring"></a>Aktivieren der Erstellung integrierter Connectors

Sie können Ihre eigenen integrierten Connectors für jeden benötigten Dienst erstellen, indem Sie das [Erweiterbarkeitsframework der Vorschauversion](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272) verwenden. Ähnlich wie integrierte Connectors wie Azure Service Bus und SQL Server bieten diese Connectors einen höheren Durchsatz, niedrige Wartezeiten, lokale Konnektivität und werden nativ im selben Prozess wie die Vorschau-Runtime ausgeführt.

Die Erstellungsfunktion ist derzeit nur in Visual Studio Code verfügbar, aber nicht standardmäßig aktiviert. Um diese Connectors zu erstellen, müssen Sie zuerst Ihr Projekt von erweiterungspaketbasiert (Node.js) in NuGet-Paketbasiert (.NET) konvertieren.

> [!IMPORTANT]
> Diese Aktion ist ein unidirektionaler Vorgang, der nicht rückgängig gemacht werden kann.

1. Bewegen Sie im Bereich „Explorer“ im Stammverzeichnis Ihres Projekts den Mauszeiger über einen beliebigen leeren Bereich unterhalb aller anderen Dateien und Ordner, öffnen Sie das Kontextmenü, und wählen Sie **In Nuget-basiertes Logik-App-Projekt konvertieren** aus.

   ![Screenshot, der den Bereich „Explorer“ mit dem aus einem leeren Bereich heraus geöffneten Kontextmenü des Projekts im Projektfenster zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/convert-logic-app-project.png)

1. Wenn die Eingabeaufforderung angezeigt wird, bestätigen Sie die Projektkonvertierung.

1. Um den Vorgang fortzusetzen, überprüfen und befolgen Sie die Schritte im Artikel [Azure Logic Apps läuft überall: Erweiterbarkeit integrierter Connectors](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-built-in-connector/ba-p/1921272).

<a name="open-workflow-definition-designer"></a>

## <a name="open-the-workflow-definition-file-in-the-designer"></a>Öffnen der Workflowdefinitionsdatei im Designer

1. Überprüfen Sie die auf Ihrem Computer installierten Versionen mit folgendem Befehl:

   `..\Users\{yourUserName}\dotnet --list-sdks`

   Wenn Sie über .NET Core SDK 5.x verfügen, kann diese Version Sie möglicherweise daran hindern, die zugrunde liegende Workflowdefinition der Logik-App im Designer zu öffnen. Anstatt diese Version zu deinstallieren, erstellen Sie im Stammordner Ihres Projekts die Datei **global.json**, mit der auf Ihre .NET Core Runtime 3.x-Version verwiesen wird, die höher als 3.1.201 ist. Beispiel:

   ```json
   {
      "sdk": {
         "version": "3.1.8",
         "rollForward": "disable"
      }
   }
   ```

   > [!IMPORTANT]
   > Stellen Sie sicher, dass Sie die Datei **global.json** in Visual Studio Code im Stammordner Ihres Projekts explizit hinzufügen. Andernfalls wird der Designer nicht geöffnet.

1. Erweitern Sie den Projektordner für Ihren Workflow. Öffnen Sie das Kontextmenü der Datei **workflow.json**, und wählen Sie **Im Designer öffnen** aus.

   ![Screenshot, der den Explorer-Bereich und das Kontextmenüfenster für die Datei „workflow.json“ mit ausgewähltem „In Designer öffnen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/open-definition-file-in-designer.png)

1. Wählen Sie in der Liste **Enable connectors in Azure** (Connectors in Azure aktivieren) die Option **Use connectors from Azure** (Connectors aus Azure verwenden) aus, die für alle verwalteten Connectors gilt, die im Azure-Portal verfügbar sind, nicht nur für Connectors für Azure-Dienste.

   ![Screenshot, der den Explorer-Bereich mit der geöffneten Liste „Connectors in Azure aktivieren“ und ausgewähltem „Connectors von Azure verwenden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/use-connectors-from-azure.png)

   > [!NOTE]
   > Zustandslose Workflows unterstützen derzeit nur *Aktionen* für in Azure bereitgestellte [verwaltete Connectors](../connectors/apis-list.md#managed-api-connectors), keine Aktionen für Trigger. Obwohl Sie die Möglichkeit haben, Connectors in Azure für zustandslose Workflows zu aktivieren, zeigt der Designer keine verwalteten Connectortrigger an, die Sie auswählen können.

1. Wählen Sie in der Liste **Abonnement auswählen** das Azure-Abonnement aus, das für Ihr Logik-App-Projekt verwendet werden soll.

   ![Screenshot, der den Explorer-Bereich mit dem Feld „Abonnement auswählen“ und dem ausgewählten Abonnement zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-azure-subscription.png)

1. Wählen Sie in der Liste der Ressourcengruppen **Neue Ressourcengruppe erstellen** aus.

   ![Screenshot, der den Explorer-Bereich mit der Liste der Ressourcengruppen und ausgewähltem „Neue Ressourcengruppe erstellen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/create-select-resource-group.png)

1. Geben Sie einen Namen für die Ressourcengruppe an, und drücken Sie die EINGABETASTE. In diesem Beispiel wird `Fabrikam-Workflows-RG` verwendet.

   ![Screenshot, der den Explorer-Bereich und das Feld für den Namen der Ressourcengruppe zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-name-for-resource-group.png)

1. Suchen Sie in der Liste der Speicherorte die Azure-Region, die für die Erstellung Ihrer Ressourcengruppe und Ressourcen verwendet werden soll, und wählen Sie diese aus. In diesem Beispiel wird **USA, Westen-Mitte** verwendet.

   ![Screenshot, der den Explorer-Bereich mit der Liste der Standorte und ausgewähltem „USA, Westen-Mitte“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-azure-region.png)

   Nachdem Sie diesen Schritt ausgeführt haben, öffnet Visual Studio Code den Workflow-Designer.

   > [!NOTE]
   > Wenn Visual Studio Code die Workflow-Entwurfszeit-API startet, wird ggf. eine Meldung angezeigt, dass der Startvorgang einige Sekunden dauern kann. Sie können diese Meldung ignorieren oder **OK** auswählen.
   >
   > Falls der Designer nicht geöffnet wird, helfen Ihnen die Informationen im Abschnitt [Designer wird nicht geöffnet](#designer-fails-to-open) bei der Problembehandlung weiter.

   Sobald der Designer angezeigt wird, wird die Eingabeaufforderung **Vorgang auswählen** im Designer standardmäßig ausgewählt angezeigt, wodurch der Bereich **Aktion hinzufügen** angezeigt wird.

   ![Screenshot, der den Workflow-Designer zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-designer.png)

1. Als Nächstes [fügen Sie Ihrem Workflow einen Trigger und Aktionen hinzu](#add-trigger-actions).

<a name="add-trigger-actions"></a>

## <a name="add-a-trigger-and-actions"></a>Hinzufügen eines Triggers und von Aktionen

Nachdem Sie den Designer geöffnet haben, wird die Eingabeaufforderung **Vorgang auswählen** im Designer standardmäßig ausgewählt angezeigt. Sie können nun mit dem Erstellen Ihres Workflows beginnen, indem Sie einen Trigger und Aktionen hinzufügen.

Der Workflow in diesem Beispiel verwendet diesen Triggern und die folgenden Aktionen:

* Den integrierten [Anforderungstrigger](../connectors/connectors-native-reqres.md), **Beim Empfang einer HTTP-Anforderung**, der eingehende Aufrufe oder Anforderungen empfängt und einen Endpunkt erstellt, der von anderen Diensten oder Logik-Apps aufgerufen werden kann.

* Die [Office 365 Outlook-Aktion](../connectors/connectors-create-api-office365-outlook.md): **E-Mail senden**.

* Die integrierte [Antwortaktion](../connectors/connectors-native-reqres.md), mit der Sie eine Antwort gesendet und Daten an den Aufrufer zurückgegeben haben.

### <a name="add-the-request-trigger"></a>Hinzufügen des Anforderungstriggers

1. Vergewissern Sie sich neben dem Designer im Bereich **Trigger hinzufügen** unter dem Suchfeld **Vorgang auswählen**, dass **Integriert** ausgewählt ist, damit Sie einen Trigger auswählen können, der nativ ausgeführt wird.

1. Geben Sie in das Suchfeld **Vorgang auswählen** die Zeichenfolge `when a http request` ein, und wählen Sie den integrierten Anforderungstrigger namens **Beim Empfang einer HTTP-Anforderung** aus.

   ![Screenshot, der den Workflow-Designer mit dem Bereich **Trigger hinzufügen** mit ausgewähltem Trigger „Beim Empfang einer HTTP-Anforderung“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/add-request-trigger.png)

   Wenn der Trigger im Designer angezeigt wird, wird der Detailbereich des Triggers geöffnet, um die Eigenschaften, Einstellungen und anderen Aktionen des Triggers anzuzeigen.

   ![Screenshot, der den Workflow-Designer mit dem ausgewählten Trigger „Beim Empfang einer HTTP-Anforderung“ und geöffnetem Triggerdetailbereich zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/request-trigger-added-to-designer.png)

   > [!TIP]
   > Wenn der Detailbereich nicht angezeigt wird, stellen Sie sicher, dass der Trigger im Designer ausgewählt ist.

1. Wenn Sie ein Element aus dem Designer löschen müssen, [führen Sie diese Schritte aus](#delete-from-designer).

### <a name="add-the-office-365-outlook-action"></a>Hinzufügen der Office 365 Outlook-Aktion

1. Wählen Sie im Logik-App-Designer unter dem von Ihnen hinzugefügten Trigger **Neuer Schritt** aus.

   Die Eingabeaufforderung **Vorgang auswählen** wird im Designer angezeigt, und der Bereich **Aktion hinzufügen** wird erneut geöffnet, sodass Sie die nächste Aktion auswählen können.

1. Wählen Sie im Bereich **Aktion hinzufügen** unter dem Suchfeld **Vorgang auswählen** die Zeichenfolge **Azure** aus, damit Sie eine Aktion für einen verwalteten Connector suchen und auswählen können, der in Azure bereitgestellt ist.

   In diesem Beispiel wird die Office 365 Outlook-Aktion **E-Mail senden (V2)** ausgewählt und verwendet.

   ![Screenshot, der den Workflow-Designer und den Bereich **Aktion hinzufügen** mit ausgewählter Office 365 Outlook-Aktion „E-Mail senden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/add-send-email-action.png)

1. Wählen Sie im Detailbereich der Aktion **Anmelden** aus, damit Sie eine Verbindung mit Ihrem E-Mail-Konto herstellen können.

   ![Screenshot, der den Workflow-Designer und den Bereich **E-Mail senden (V2)** mit ausgewähltem „Anmelden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-sign-in.png)

1. Wenn Visual Studio Code Sie zur Zustimmung für den Zugriff auf Ihr E-Mail-Konto auffordert, wählen Sie **Öffnen** aus.

   ![Screenshot, der die Aufforderung von Visual Studio Code zur Zulassung des Zugriffs zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/visual-studio-code-open-external-website.png)

   > [!TIP]
   > Um zukünftige Aufforderungen zu vermeiden, wählen Sie **Vertrauenswürdige Domänen konfigurieren** aus, damit Sie die Authentifizierungsseite als vertrauenswürdige Domäne hinzufügen können.

1. Befolgen Sie die nachfolgenden Aufforderungen, um sich anzumelden, den Zugriff zuzulassen und die Rückkehr zu Visual Studio Code zuzulassen.

   > [!NOTE]
   > Wenn zu viel Zeit verstrichen ist, bevor Sie die Eingabeaufforderungen erfüllt haben, kommt es beim Authentifizierungsprozess zum Timeout, und er schlägt fehl. Kehren Sie in diesem Fall zum Designer zurück, und wiederholen Sie die Anmeldung, um die Verbindung zu erstellen.

1. Wenn die Erweiterung „Azure Logic Apps (Vorschau)“ Sie zur Zustimmung für den Zugriff auf Ihr E-Mail-Konto auffordert, wählen Sie **Öffnen** aus. Befolgen Sie die nachfolgende Aufforderung, um den Zugriff zuzulassen.

   ![Screenshot, der die Aufforderung der Erweiterung in der Vorschau zur Zulassung des Zugriffs zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/allow-preview-extension-open-uri.png)

   > [!TIP]
   > Um zukünftige Eingabeaufforderungen zu vermeiden, wählen Sie **Für diese Erweiterung nicht mehr nachfragen** aus.

   Nachdem Visual Studio Code die Verbindung erstellt hat, zeigen einige Connectors die Meldung `The connection will be valid for {n} days only` an. Dieses Zeitlimit gilt nur für die Dauer der Erstellung Ihrer Logik-App in Visual Studio Code. Nach der Bereitstellung gilt dieses Limit nicht mehr, da sich Ihre Logik-App zur Laufzeit mittels der automatisch aktivierten [vom System zugewiesenen verwalteten Identität](../logic-apps/create-managed-service-identity.md) authentifizieren kann. Diese verwaltete Identität unterscheidet sich von den Anmeldeinformationen für die Authentifizierung oder der Verbindungszeichenfolge, die Sie verwenden, wenn Sie eine Verbindung herstellen. Wenn Sie diese vom System zugewiesene verwaltete Identität deaktivieren, funktionieren Verbindungen zur Laufzeit nicht.

1. Wenn die Aktion **E-Mail senden** im Designer nicht ausgewählt angezeigt wird, wählen Sie diese Aktion aus.

1. Geben Sie im Detailbereich der Aktion auf der Registerkarte **Parameter** die erforderlichen Informationen für die Aktion an, z. B.:

   ![Screenshot, der den Workflow-Designer mit Details für die Office 365 Outlook-Aktion „E-Mail senden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/send-email-action-details.png)

   | Eigenschaft | Erforderlich | Wert | BESCHREIBUNG |
   |----------|----------|-------|-------------|
   | **An** | Ja | <*Ihre E-Mail-Adresse*> | Der E-Mail-Empfänger, bei dem es sich um Ihre E-Mail-Adresse zu Testzwecken handeln kann. In diesem Beispiel wird die fiktive E-Mail-Adresse `sophiaowen@fabrikam.com` verwendet. |
   | **Subject** | Ja | `An email from your example workflow` | der E-Mail-Betreff |
   | **Text** | Ja | `Hello from your example workflow!` | Der Textinhalt der E-Mail |
   ||||

   > [!NOTE]
   > Wenn Sie Änderungen im Detailbereich auf der Registerkarte **Einstellungen**, **Statisches Ergebnis** oder **Ausführen nach** vornehmen möchten, stellen Sie sicher, dass Sie **Fertig** ausgewählt haben, um die Änderungen zu übernehmen, bevor Sie zwischen Registerkarten wechseln oder den Fokus innerhalb des Designers verschieben. Andernfalls behält Visual Studio Code Ihre Änderungen nicht. Weitere Informationen finden Sie in GitHub unter [Logic Apps Public Preview Known Issues](https://github.com/Azure/logicapps/blob/master/articles/logic-apps-public-preview-known-issues.md) (Bekannte Probleme der öffentlichen Vorschauversion von Logic Apps).

1. Wählen Sie im Designer **Speichern** aus.

> [!IMPORTANT]
> Zum lokalen Ausführen eines Workflows, der einen webhookbasierten Trigger oder Aktionen verwendet, wie z. B. den [integrierten Trigger oder eine Aktion des HTTP-Webhooks](../connectors/connectors-native-webhook.md), müssen Sie diese Funktion aktivieren, indem Sie die [Weiterleitung für die Rückruf-URL des Webhooks einrichten](#webhook-setup).

<a name="webhook-setup"></a>

## <a name="enable-locally-running-webhooks"></a>Aktivieren von lokal ausgeführten Webhooks

Wenn Sie einen webhookbasierten Trigger oder eine Aktion wie z. B. **HTTP-Webhook** verwenden und eine Logik-App in Azure ausgeführt wird, abonniert die Logic Apps-Runtime den Dienstendpunkt, indem eine Rückruf-URL mit diesem Endpunkt erzeugt und registriert wird. Der Trigger oder die Aktion wartet dann darauf, dass der Dienstendpunkt die URL aufruft. Wenn Sie jedoch in Visual Studio Code arbeiten, beginnt die generierte Rückruf-URL mit `http://localhost:7071/...`. Diese URL gilt für den Localhost-Server, der privat ist, sodass der Dienstendpunkt diese URL nicht aufrufen kann.

Zum lokalen Ausführen von webhookbasierten Triggern und Aktionen in Visual Studio Code müssen Sie eine öffentliche URL einrichten, die den Localhost-Server verfügbar macht und Aufrufe vom Dienstendpunkt sicher an die Webhookrückruf-URL weiterleitet. Sie können einen Weiterleitungsdienst und ein Tool wie [**ngrok**](https://ngrok.com/) verwenden, mit dem ein HTTP-Tunnel für den Localhost-Port geöffnet wird, oder Ihr eigenes Tool verwenden.

#### <a name="set-up-call-forwarding-using-ngrok"></a>Einrichten der Aufrufweiterleitung mit **ngrok**

1. [Registrieren Sie sich für ein **ngrok**-Konto](https://dashboard.ngrok.com/signup), wenn Sie keines besitzen. Andernfalls [melden Sie sich bei Ihrem Konto an](https://dashboard.ngrok.com/login).

1. Rufen Sie Ihr persönliches Authentifizierungstoken ab, das Ihr **ngrok**-Client benötigt, um eine Verbindung mit Ihrem Konto herzustellen und den Zugriff zu authentifizieren.

   1. Um Ihre [Authentifizierungstokenseite](https://dashboard.ngrok.com/auth/your-authtoken) zu finden, erweitern Sie in Ihrem Kontodashboardmenü die Option **Authentifizierung**, und wählen Sie **Ihr Authentifizierungstoken** aus.

   1. Kopieren Sie das Token aus dem Feld **Ihr Authentifizierungstoken** an einen sicheren Speicherort.

1. Laden Sie von der [**ngrok**-Downloadseite](https://ngrok.com/download) oder [Ihrem Kontodashboard](https://dashboard.ngrok.com/get-started/setup) die gewünschte **ngrok**-Version herunter, und extrahieren Sie die ZIP-Datei. Weitere Informationen finden Sie unter [Schritt 1: Unzip to install](https://ngrok.com/download) (Zum Installieren entzippen).

1. Öffnen Sie auf Ihrem Computer das Eingabeaufforderungstool. Wechseln Sie zu dem Speicherort, wo sich die Datei **ngrok.exe** befindet.

1. Stellen Sie eine Verbindung des **ngrok**-Clients mit Ihrem **ngrok**-Konto her, indem Sie den folgenden Befehl ausführen. Weitere Informationen finden Sie unter [Schritt 2: Connect your account](https://ngrok.com/download) (Stellen Sie eine Verbindung mit Ihrem Konto her).

   `ngrok authtoken <your_auth_token>`

1. Öffnen Sie den HTTP-Tunnel zum Localhost-Port 7071, indem Sie den folgenden Befehl ausführen. Weitere Informationen finden Sie unter [Schritt 3: Fire it up](https://ngrok.com/download) (Starten Sie es).

   `ngrok http 7071`

1. In der Ausgabe finden Sie folgende Zeile:

   `http://<domain>.ngrok.io -> http://localhost:7071`

1. Kopieren und speichern Sie die URL mit dem folgenden Format: `http://<domain>.ngrok.io`

#### <a name="set-up-the-forwarding-url-in-your-app-settings"></a>Einrichten der Weiterleitungs-URL in ihren App-Einstellungen

1. Fügen Sie in Visual Studio Code im Designer als Trigger bzw. Aktion **HTTP + Webhook** hinzu.

1. Geben Sie die zuvor erstellte Weiterleitungs-URL (Umleitung) ein, wenn die Eingabeaufforderung für den Hostendpunkt-Speicherort angezeigt wird.

   > [!NOTE]
   > Wenn Sie die Eingabeaufforderung ignorieren, wird eine Warnung mit dem Hinweis angezeigt, dass Sie die Weiterleitungs-URL angeben müssen. Wählen Sie daher die Option **Konfigurieren** aus, und geben Sie die URL ein. Nachdem Sie diesen Schritt ausgeführt haben, wird die Eingabeaufforderung für nachfolgende Webhook-Trigger oder -Aktionen, die Sie ggf. hinzufügen, nicht mehr angezeigt.
   >
   > Gehen Sie wie folgt vor, wenn die Eingabeaufforderung auf der Stammebene Ihres Projekts wieder angezeigt werden soll: Öffnen Sie das Kontextmenü der Datei **local.settings.json**, und wählen Sie die Option **Configure Webhook Redirect Endpoint** (Endpunkt für Webhookumleitung konfigurieren) aus. Die Eingabeaufforderung wird angezeigt, damit Sie die Weiterleitungs-URL angeben können.

   In Visual Studio Code wird die Weiterleitungs-URL im Stammordner Ihres Projekts der Datei **local.settings.json** hinzugefügt. Im Objekt `Values` wird jetzt die Eigenschaft mit dem Namen `Workflows.WebhookRedirectHostUri` angezeigt und auf die Weiterleitungs-URL festgelegt. Beispiel:
   
   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "UseDevelopmentStorage=true",
         "FUNCTIONS_WORKER_RUNTIME": "node",
         "FUNCTIONS_V2_COMPATIBILITY_MODE": "true",
         <...>
         "Workflows.WebhookRedirectHostUri": "http://xxxXXXXxxxXXX.ngrok.io",
         <...>
      }
   }
   ```

Wenn Sie zum ersten Mal eine lokale Debugsitzung starten oder den Workflow ohne Debuggen ausführen, registriert die Logic Apps-Runtime den Workflow beim Dienstendpunkt und abonniert diesen Endpunkt, um die Benachrichtigung über Webhookvorgänge zu ermöglichen. Wenn Sie den Workflow das nächste Mal ausführen, wird die Runtime nicht registriert oder erneut abonniert, weil die Abonnementregistrierung bereits im lokalen Speicher vorhanden ist.

Wenn Sie die Debugsitzung für eine Workflowausführung beenden, die lokal ausgeführte webhookbasierte Trigger oder Aktionen verwendet, werden die vorhandenen Abonnementregistrierungen nicht gelöscht. Wenn Sie die Registrierung aufheben möchten, müssen Sie die Abonnementregistrierungen manuell entfernen oder löschen.

> [!NOTE]
> Nachdem der Workflow gestartet wurde, werden im Terminalfenster möglicherweise Fehler wie in diesem Beispiel angezeigt:
>
> `message='Http request failed with unhandled exception of type 'InvalidOperationException' and message: 'System.InvalidOperationException: Synchronous operations are disallowed. Call ReadAsync or set AllowSynchronousIO to true instead.`
>
> Öffnen Sie in diesem Fall die Datei **local.settings.json** im Stammordner des Projekts, und stellen Sie sicher, dass die Eigenschaft auf `true` festgelegt ist:
>
> `"FUNCTIONS_V2_COMPATIBILITY_MODE": "true"`

<a name="manage-breakpoints"></a>

## <a name="manage-breakpoints-for-debugging"></a>Verwalten von Haltepunkten zum Debuggen

Bevor Sie Ihren Logik-App-Workflow ausführen und testen, indem Sie eine Debugsitzung starten, können Sie in der Datei **workflow.json** für jeden Workflow [Haltepunkte](https://code.visualstudio.com/docs/editor/debugging#_breakpoints) festlegen. Es ist kein anderes Setup erforderlich. 

Haltepunkte werden derzeit jedoch nur für Aktionen unterstützt, nicht für Trigger. Jede Aktionsdefinition enthält diese Haltepunktpositionen:

* Legen Sie den Starthaltepunkt in der Zeile fest, die den Namen der Aktion enthält. Wenn dieser Haltepunkt während der Debugsitzung erreicht wird, können Sie die Eingaben der Aktion überprüfen, bevor sie ausgewertet werden.

* Legen Sie den Endhaltepunkt in der Zeile fest, die die schließende geschweifte Klammer ( **}** ) der Aktion enthält. Wenn dieser Haltepunkt während der Debugsitzung erreicht wird, können Sie die Ergebnisse der Aktion überprüfen, bevor die Ausführung der Aktion endet.

Um einen Haltepunkt hinzuzufügen, führen Sie diese Schritte aus:

1. Öffnen Sie die Datei **workflow.json** für den Workflow, den Sie debuggen möchten.

1. Wählen Sie in der Zeile, in der Sie den Haltepunkt festlegen möchten, das Innere der linken Spalte aus. Um den Haltepunkt zu entfernen, wählen Sie den Haltepunkt aus.

   Wenn Sie die Debugsitzung starten, wird die Ausführungsansicht auf der linken Seite des Codefensters angezeigt, während die Debugsymbolleiste im oberen Bereich angezeigt wird.

   > [!NOTE]
   > Wenn die Ausführungsansicht nicht automatisch angezeigt wird, drücken Sie STRG+UMSCHALT+D.

1. Um die verfügbaren Informationen zu überprüfen, wenn ein Haltepunkt erreicht wird, untersuchen Sie in der Ausführungsansicht den Bereich **Variablen**.

1. Um die Workflowausführung fortzusetzen, wählen Sie auf der Symbolleiste „Debuggen“ **Fortsetzen** (Wiedergabeschaltfläche) aus.

Während der Workflowausführung können Sie jederzeit Haltepunkte hinzufügen und entfernen. Wenn Sie jedoch die **workflow.json**-Datei nach dem Start der Ausführung aktualisieren, werden Haltepunkte nicht automatisch aktualisiert. Um die Haltepunkte zu aktualisieren, starten Sie die Logik-App neu.

Allgemeine Informationen finden Sie unter [Breakpoints – Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging#_breakpoints) (Haltepunkte – Visual Studio Code).

<a name="run-test-debug-locally"></a>

## <a name="run-test-and-debug-locally"></a>Lokales Ausführen, Testen und Debuggen

Um Ihre Logik-App zu testen, führen Sie die folgenden Schritte aus, um eine Debugsitzung zu starten und die URL für den Endpunkt zu finden, der durch den Anforderungstrigger erstellt wird. Sie benötigen diese URL, damit Sie später eine Anforderung an diesen Endpunkt senden können.

1. Damit Sie einen zustandslosen Workflow leichter debuggen können, können Sie [den Ausführungsverlauf für diesen Workflow aktivieren](#enable-run-history-stateless).

1. Öffnen Sie auf der Aktivitätsleiste von Visual Studio Code das Menü **Ausführen**, und wählen Sie **Debuggen starten** (F5) aus.

   Das **Terminal** fenster wird geöffnet, sodass Sie die Debugsitzung überprüfen können.

   > [!NOTE]
   > Wenn Sie einen Fehler wie **Fehler nach der Ausführung von preLaunchTask „generateDebugSymbols“** erhalten, helfen Ihnen die Informationen im Abschnitt [Debugsitzung wird nicht gestartet](#debugging-fails-to-start) bei der Problembehandlung weiter.

1. Suchen Sie nun die Rückruf-URL für den Endpunkt im Anforderungstrigger.

   1. Öffnen Sie den Explorer-Bereich erneut, damit Sie Ihr Projekt anzeigen können.

   1. Wählen Sie im Kontextmenü der Datei **workflow.json** die Option **Übersicht** aus.

      ![Screenshot, der den Explorer-Bereich und das Kontextmenüfenster für die Datei „workflow.json“ mit ausgewähltem „Übersicht“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/open-workflow-overview.png)

   1. Suchen Sie den Wert **Rückruf-URL**, der dieser URL für den Anforderungsbeispieltrigger ähnelt:

      `http://localhost:7071/api/<workflow-name>/triggers/manual/invoke?api-version=2020-05-01-preview&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=<shared-access-signature>`

      ![Screenshot, der die Übersichtsseite Ihres Workflows mit Rückruf-URL zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/find-callback-url.png)

1. Um die Rückruf-URL zu testen, indem Sie den Logik-App-Workflow auslösen, öffnen Sie [Postman](https://www.postman.com/downloads/) oder Ihr bevorzugtes Tool zum Erstellen und Senden von Anforderungen.

   Dieses Beispiel wird mit Postman fortgesetzt. Weitere Informationen finden Sie unter [Erste Schritte mit Postman](https://learning.postman.com/docs/getting-started/introduction/).

   1. Wählen Sie auf der Postman-Symbolleiste **Neu** aus.

      ![Screenshot, der Postman mit ausgewählter Schaltfläche „Neu“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-create-request.png)

   1. Wählen Sie im Bereich **Neu erstellen** unter **Bausteine** die Option **Anforderung** aus.

   1. Geben Sie im Fenster **Anforderung speichern** unter **Anforderungsname** einen Namen für die Anforderung an, z. B. `Test workflow trigger`.

   1. Wählen Sie unter **Sammlung oder Ordner als Speicherziel auswählen** den Eintrag **Sammlung erstellen** aus.

   1. Geben Sie unter **Alle Sammlungen** einen Namen für die zu erstellende Sammlung an, um Ihre Anforderungen zu organisieren, und wählen Sie **Speichern unter <*Sammlungs-Name*>** aus. In diesem Beispiel wird `Logic Apps requests` als Name der Sammlung verwendet.

      Der Anforderungsbereich von Postman wird geöffnet, sodass Sie eine Anforderung an die Rückruf-URL für den Anforderungstrigger senden können.

      ![Screenshot, der Postman mit geöffnetem Anforderungsbereich zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-request-pane.png)

   1. Kehren Sie zu Visual Studio Code zurück. Kopieren Sie von der Übersichtsseite des Workflows den Eigenschaftswert von **Rückruf-URL**.

   1. Kehren Sie zu Postman zurück. Fügen Sie im Anforderungsbereich neben der Liste der Methoden, in der derzeit **GET** als Standardanforderungsmethode angezeigt wird, die zuvor kopierte Rückruf-URL in das Feld „Adresse“ ein, und wählen Sie **Senden** aus.

      ![Screenshot, der Postman und die Rückruf-URL im Adressfeld mit ausgewählter Schaltfläche „Senden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/postman-test-call-back-url.png)

      Der Logik-App-Beispielworkflow sendet eine E-Mail, die in etwa wie im folgenden Beispiel aussieht:

      ![Screenshot, der die wie im Beispiel beschriebene Outlook-E-Mail zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-app-result-email.png)

1. Kehren Sie in Visual Studio Code zur Übersichtsseite Ihres Workflows zurück.

   Wenn Sie einen zustandsbehafteten Workflow erstellt haben, werden auf der Übersichtsseite der Ausführungsstatus und -verlauf des Workflows angezeigt, nachdem die von Ihnen gesendete Anforderung den Workflow ausgelöst hat.

   > [!TIP]
   > Wenn der Ausführungsstatus nicht angezeigt wird, aktualisieren Sie die Übersichtsseite, indem Sie **Aktualisieren** auswählen. Für Trigger, die aufgrund von nicht erfüllten Kriterien oder nicht gefundenen Daten ausgelassen werden, erfolgt keine Ausführung.

   ![Screenshot: Übersichtsseite Ihres Workflows mit Ausführungsstatus und -verlauf](./media/create-stateful-stateless-workflows-visual-studio-code/post-trigger-call.png)

   | Ausführungsstatus | Beschreibung |
   |------------|-------------|
   | **Aborted** | Die Ausführung wurde aufgrund externer Probleme beendet oder nicht fertig gestellt, z. B. wegen eines Systemausfalls oder abgelaufenen Azure-Abonnements. |
   | **Abgebrochen** | Die Ausführung wurde ausgelöst und gestartet, es wurde jedoch eine Abbruchanforderung empfangen. |
   | **Fehler** | Mindestens eine Aktion in der Ausführung ist fehlgeschlagen. Es wurden keine nachfolgenden Aktionen im Workflow eingerichtet, um den Fehler zu verarbeiten. |
   | **Wird ausgeführt** | Die Ausführung wurde ausgelöst und wird gerade ausgeführt. Dieser Status kann jedoch auch für eine Ausführung angezeigt werden, die aufgrund von [Aktionslimits](logic-apps-limits-and-config.md) oder des [aktuellen Tarifs](https://azure.microsoft.com/pricing/details/logic-apps/) gedrosselt wird. <p><p>**Tipp**: Wenn Sie die [Diagnoseprotokollierung](monitor-logic-apps-log-analytics.md) einrichten, erhalten Sie Informationen zu ggf. aufgetretenen Drosselungsereignissen. |
   | **Erfolgreich** | Die Ausführung war erfolgreich. Wenn eine Aktion fehlgeschlagen ist, wurde dieser Fehler von einer nachfolgenden Aktion im Workflow verarbeitet. |
   | **Timeout** | Bei der Ausführung ist ein Timeout aufgetreten, weil die aktuelle Dauer die maximale Dauer von Ausführungen überschritten hat, die von der [Einstellung **Aufbewahrung des Ausführungsverlaufs in Tagen**](logic-apps-limits-and-config.md#run-duration-retention-limits) gesteuert wird. Die Dauer der Ausführung wird anhand der Startzeit der Ausführung und der maximalen Dauer von Ausführungen zu dieser Startzeit berechnet. <p><p>**Hinweis**: Wenn die Ausführungsdauer auch das aktuelle *Aufbewahrungslimit im Ausführungsverlauf* überschreitet, das ebenfalls von der [Einstellung **Aufbewahrung des Ausführungsverlaufs in Tagen**](logic-apps-limits-and-config.md#run-duration-retention-limits) gesteuert wird, wird die Ausführung durch einen täglichen Cleanupauftrag aus dem Ausführungsverlauf gelöscht. Unabhängig davon, ob bei der Ausführung ein Timeout auftritt oder ob diese fertig gestellt wird, wird die Aufbewahrungsdauer immer anhand der Startzeit der Ausführung und der *aktuellen* Aufbewahrungsdauer berechnet. Wenn Sie also die maximale Dauer einer aktiven Ausführung herabsetzen, tritt ein Timeout für die Ausführung auf. Je nachdem, ob die Ausführungsdauer die Aufbewahrungsdauer überschreitet, bleibt die Ausführung entweder unverändert oder wird aus dem Ausführungsverlauf gelöscht. |
   | **Wartet** | Die Ausführung wurde nicht gestartet oder wurde angehalten, z. B. aufgrund einer früheren Workflowinstanz, die noch ausgeführt wird. |
   |||

1. Wählen Sie zum Überprüfen der Statuswerte für jeden Schritt in einer bestimmten Ausführung sowie der Eingaben und Ausgaben des Schritts die Schaltfläche mit den Auslassungspunkten ( **...** ) für diese Ausführung aus, und wählen Sie **Ausführung anzeigen** aus.

   ![Screenshot, der die Zeile mit dem Ausführungsverlauf Ihres Workflows mit der Schaltfläche mit Auslassungspunkten und ausgewähltem „Ausführung anzeigen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/show-run-history.png)

   Visual Studio Code öffnet die Überwachungsansicht und zeigt den Status der einzelnen Ausführungsschritte an.

   ![Screenshot: einzelne Ausführungsschritte in der Workflowausführung und ihr Status](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-action-status.png)

   > [!NOTE]
   > Wenn bei einer Ausführung ein Schritt in der Überwachungsansicht den Fehler `400 Bad Request` anzeigt, wird das Problem möglicherweise durch einen längeren Trigger- oder Aktionsnamen verursacht, der dazu führt, dass der zugrunde liegende URI (Uniform Resource Identifier) das Standardzeichenlimit überschreitet. Weitere Informationen finden Sie unter [„400 – Ungültige Anforderung“](#400-bad-request).

   In der folgenden Tabelle sind die möglichen Statuswerte aufgeführt, die in den einzelnen Workflowschritte aufweisen können:

   | Status einer Aktion | Symbol | Beschreibung |
   |---------------|------|-------------|
   | **Aborted** | ![Symbol für den Aktionsstatus „Abgebrochen“][aborted-icon] | Die Aktion wurde aufgrund externer Probleme beendet oder nicht fertig gestellt, z. B. wegen eines Systemausfalls oder abgelaufenen Azure-Abonnements. |
   | **Abgebrochen** | ![Symbol für den Aktionsstatus „Storniert“][cancelled-icon] | Die Aktion wurde ausgeführt, aber es wurde eine Abbruchanforderung empfangen. |
   | **Fehlgeschlagen** | ![Symbol für den Aktionsstatus „Fehlerhaft“][failed-icon] | Die Aktion ist fehlgeschlagen. |
   | **Wird ausgeführt** | ![Symbol für den Aktionsstatus „Wird ausgeführt“][running-icon] | Die Aktion wird zurzeit ausgeführt. |
   | **Übersprungen** | ![Symbol für den Aktionsstatus „Übersprungen“][skipped-icon] | Die Aktion wurde übersprungen, weil die unmittelbar vorhergehende Aktion fehlgeschlagen ist. Eine Aktion weist eine `runAfter`-Bedingung auf, die erfordert, dass die vorherige Aktion erfolgreich abgeschlossen wurde, bevor die aktuelle Aktion ausgeführt werden kann. |
   | **Erfolgreich** | ![Symbol für den Aktionsstatus „Erfolgreich“][succeeded-icon] | Die Aktion war erfolgreich. |
   | **Erfolgreich mit Wiederholungen** | ![Symbol für den Aktionsstatus „Erfolgreich mit Wiederholungen“][succeeded-with-retries-icon] | Die Aktion war erfolgreich, jedoch erst nach mindestens einem Wiederholungsversuch. Wählen Sie diese Aktion zum Überprüfen des Wiederholungsverlaufs in der Detailansicht des Ausführungsverlaufs aus, damit Sie die Ein- und Ausgaben anzeigen können. |
   | **Timeout** | ![Symbol für Aktionsstatus „Timeout“][timed-out-icon] | Die Aktion wurde aufgrund des Timeoutlimits beendet, das durch die Einstellungen dieser Aktion gesteuert wird. |
   | **Wartet** | ![Symbol für den Aktionsstatus „Wartend“][waiting-icon] | Gilt für eine Webhookaktion, die auf eine eingehende Anforderung eines Aufrufers wartet. |
   ||||

   [aborted-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/aborted.png
   [cancelled-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/cancelled.png
   [failed-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/failed.png
   [running-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/running.png
   [skipped-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/skipped.png
   [succeeded-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/succeeded.png
   [succeeded-with-retries-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/succeeded-with-retries.png
   [timed-out-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/timed-out.png
   [waiting-icon]: ./media/create-stateful-stateless-workflows-visual-studio-code/waiting.png

1. Wählen Sie zum Überprüfen der Ein- und Ausgaben für die einzelnen Schritte den Schritt aus, den Sie untersuchen möchten.

   ![Screenshot, der den Status für jeden Schritt im Workflow sowie die Ein- und Ausgaben in der erweiterten Aktion „E-Mail senden“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details.png)

1. Um die unformatierten Ein- und Ausgaben für diesen Schritt weiter zu überprüfen, wählen Sie **Unformatierte Eingaben anzeigen** oder **Unformatierte Ausgaben anzeigen** aus.

1. Um die Debugsitzung zu beenden, wählen Sie im menü **Ausführen** den Eintrag **Debuggen beenden** (UMSCHALT+F5) aus.

<a name="return-response"></a>

## <a name="return-a-response"></a>Zurückgeben einer Antwort

Um eine Antwort an den Aufrufer zurückzugeben, der eine Anforderung an Ihre Logik-App gesendet hat, können Sie die integrierte [Antwortaktion](../connectors/connectors-native-reqres.md) für einen Workflow verwenden, der mit dem Anforderungstrigger gestartet wird.

1. Wählen Sie im Workflow-Designer unter der Aktion **E-Mail senden** die Option **Neuer Schritt** aus.

   Die Eingabeaufforderung **Vorgang auswählen** wird im Designer angezeigt, und der Bereich **Aktion hinzufügen** wird erneut geöffnet, sodass Sie die nächste Aktion auswählen können.

1. Vergewissern Sie sich, dass im Bereich **Aktion hinzufügen** unter dem Suchfeld **Aktion auswählen** die Option **Integriert** ausgewählt ist. Geben Sie `response` in das Suchfeld ein, und wählen Sie die Aktion **Antwort** aus.

   ![Screenshot, der den Workflow-Designer mit ausgewählter Antwortaktion zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/add-response-action.png)

   Wenn die Aktion **Antwort** im Designer angezeigt wird, wird automatisch der Detailbereich der Aktion geöffnet.

   ![Screenshot, der den Workflow-Designer mit geöffnetem Detailbereich der Aktion „Antwort“ und die auf den Wert der Eigenschaft „Text“ der Aktion „E-Mail senden“ festgelegte Eigenschaft „Text“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details.png)

1. Geben Sie auf der Registerkarte **Parameter** die erforderlichen Informationen für die Funktion an, die Sie aufrufen möchten.

   In diesem Beispiel wird der Wert der Eigenschaft **Text** zurückgegeben, der von der Aktion **E-Mail senden** ausgegeben wird.

   1. Klicken Sie in das Eigenschaftsfeld **Text**, damit die Liste dynamischer Inhalte und die verfügbaren Ausgabewerte des vorherigen Auslösers und der Aktionen im Workflow angezeigt werden.

      ![Screenshot, der den Detailbereich der Aktion „Antwort“ mit dem Mauszeiger in der Eigenschaft „Text“ zeigt, sodass die Liste dynamischer Inhalte angezeigt wird.](./media/create-stateful-stateless-workflows-visual-studio-code/open-dynamic-content-list.png)

   1. Wählen Sie in der Liste dynamischer Inhalte unter **E-Mail senden** die Option **Text** aus.

      ![Screenshot, der die offene Liste dynamischer Inhalte zeigt. In der Liste ist unter der Kopfzeile „E-Mail senden“ der Ausgabewert „Text“ ausgewählt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-send-email-action-body-output-value.png)

      Wenn Sie fertig sind, wird die Eigenschaft **Text** der Antwortaktion auf den Ausgabewert von **Text** für die Aktion **E-Mail senden** festgelegt.

      ![Screenshot, der den Status für jeden Schritt im Workflow sowie die Ein- und Ausgaben in der erweiterten Aktion „Antwort“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/response-action-details-body-property.png)

1. Wählen Sie im Designer **Speichern** aus.

<a name="retest-workflow"></a>

## <a name="retest-your-logic-app"></a>Erneutes Testen Ihrer Logik-App

Nachdem Sie Aktualisierungen an Ihrer Logik-App vorgenommen haben, können Sie einen weiteren Test ausführen, indem Sie den Debugger in Visual Studio erneut ausführen und eine weitere Anforderung senden, um Ihre aktualisierte Logik-App auszulösen, ähnlich wie die Schritte unter [Lokales Ausführen, Testen und Debuggen](#run-test-debug-locally).

1. Öffnen Sie auf der Aktivitätsleiste von Visual Studio Code das Menü **Ausführen**, und wählen Sie **Debuggen starten** (F5) aus.

1. Senden Sie in Postman oder Ihrem Tool zum Erstellen und Senden von Anforderungen eine weitere Anforderung, um Ihren Workflow auszulösen.

1. Wenn Sie einen zustandsbehafteten Workflow erstellt haben, überprüfen Sie auf der Übersichtsseite des Workflows den Status der letzten Ausführung. Wählen Sie zum Anzeigen des Status, der Ein- und Ausgaben für jeden Schritt in dieser Ausführung die Schaltfläche mit den Auslassungspunkten ( **...** ) für diese Ausführung aus, und wählen Sie **Ausführung anzeigen** aus.

   Hier ist beispielsweise der schrittweise Status einer Ausführung, nachdem der Beispielworkflow mit der Antwortaktion aktualisiert wurde.

   ![Screenshot, der den Status für jeden Schritt im aktualisierten Workflow sowie die Ein- und Ausgaben in der erweiterten Aktion „Antwort“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/run-history-details-rerun.png)

1. Um die Debugsitzung zu beenden, wählen Sie im menü **Ausführen** den Eintrag **Debuggen beenden** (UMSCHALT+F5) aus.

<a name="firewall-setup"></a>

##  <a name="find-domain-names-for-firewall-access"></a>Suchen von Domänennamen für den Firewallzugriff

Bevor Sie Ihren Logik-App-Workflow im Azure-Portal bereitstellen und ausführen, müssen Sie, wenn in Ihrer Umgebung strenge Netzwerkanforderungen gelten oder Firewalls vorhanden sind, die den Datenverkehr einschränken, Berechtigungen für alle Trigger- oder Aktionsverbindungen, die in Ihrem Workflow vorhanden sind, einrichten.

Führen Sie die folgenden Schritte aus, um die vollqualifizierten Domänennamen (FQDNs) für diese Verbindungen zu ermitteln:

1. Öffnen Sie in Ihrem Logik-App-Projekt die Datei **connections.json**, die erstellt wird, nachdem Sie Ihrem Workflow den ersten verbindungsbasierten Trigger oder eine Aktion hinzugefügt haben, und suchen Sie das `managedApiConnections`-Objekt.

1. Für jede von Ihnen erstellte Verbindung suchen, kopieren und speichern Sie den `connectionRuntimeUrl`-Eigenschaftswert an einem sicheren Ort, damit Sie Ihre Firewall mit diesen Informationen einrichten können.

   Diese Beispieldatei **connections.json** enthält zwei Verbindungen, eine AS2-Verbindung und eine Office 365-Verbindung mit diesen `connectionRuntimeUrl`-Werten:

   * AS2: `"connectionRuntimeUrl": https://9d51d1ffc9f77572.00.common.logic-{Azure-region}.azure-apihub.net/apim/as2/11d3fec26c87435a80737460c85f42ba`

   * Office 365: `"connectionRuntimeUrl": https://9d51d1ffc9f77572.00.common.logic-{Azure-region}.azure-apihub.net/apim/office365/668073340efe481192096ac27e7d467f`

   ```json
   {
      "managedApiConnections": {
         "as2": {
            "api": {
               "id": "/subscriptions/{Azure-subscription-ID}/providers/Microsoft.Web/locations/{Azure-region}/managedApis/as2"
            },
            "connection": {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Web/connections/{connection-resource-name}"
            },
            "connectionRuntimeUrl": https://9d51d1ffc9f77572.00.common.logic-{Azure-region}.azure-apihub.net/apim/as2/11d3fec26c87435a80737460c85f42ba,
            "authentication": {
               "type":"ManagedServiceIdentity"
            }
         },
         "office365": {
            "api": {
               "id": "/subscriptions/{Azure-subscription-ID}/providers/Microsoft.Web/locations/{Azure-region}/managedApis/office365"
            },
            "connection": {
               "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Web/connections/{connection-resource-name}"
            },
            "connectionRuntimeUrl": https://9d51d1ffc9f77572.00.common.logic-{Azure-region}.azure-apihub.net/apim/office365/668073340efe481192096ac27e7d467f,
            "authentication": {
               "type":"ManagedServiceIdentity"
            }
         }
      }
   }
   ```

<a name="deploy-azure"></a>

## <a name="deploy-to-azure"></a>Bereitstellen in Azure

In Visual Studio Code können Sie Ihr Projekt direkt in Azure veröffentlichen, wo Ihre Logik-App mit dem neuen Ressourcentyp **Logic Apps (Vorschau)** bereitgestellt wird. Ähnlich wie bei der Funktions-App-Ressource in Azure Functions erfordert die Bereitstellung für diesen neuen Ressourcentyp, dass Sie einen [Hostingplan und Tarif](../app-service/overview-hosting-plans.md) auswählen, die Sie während der Bereitstellung einrichten können. Weitere Informationen zu Hostingplänen und Preisen finden Sie in diesen Themen:

* [Hochskalieren einer App in Azure App Service](../app-service/manage-scale-up.md)
* [Skalierung und Hosting von Azure Functions](../azure-functions/functions-scale.md)

Sie können Ihre Logik-App als neue Ressource veröffentlichen. Hierbei werden automatisch alle erforderlichen Ressourcen erstellt, z. B. ein [Azure-Speicherkonto (ähnlich wie bei Funktions-App-Anforderungen)](../azure-functions/storage-considerations.md). Alternativ können Sie Ihre Logik-App in einer zuvor bereitgestellten **Logik-App**-Ressource (Vorschau) veröffentlichen, die diese Logik-App überschreibt.

### <a name="publish-to-a-new-logic-app-preview-resource"></a>Veröffentlichen als neue Ressource „Logic Apps (Vorschau)“

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus.

1. Wählen Sie auf der Symbolleiste des Bereichs **Azure: Logic Apps (Vorschau)** die Option **In Logik-App bereitstellen** aus.

   ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“ und die Symbolleiste des Bereichs mit ausgewähltem „In Logik-App bereitstellen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/deploy-to-logic-app.png)

1. Wählen Sie bei entsprechender Aufforderung das Azure-Abonnement aus, das für Ihre Logik-App-Bereitstellung verwendet werden soll.

1. Wählen Sie in der Liste, die Visual Studio Code öffnet, aus den folgenden Optionen aus:

   * **Neue Logik-App (Vorschau) in Azure erstellen** (schnell)
   * **Neue Logik-App (Vorschau) in Azure Advanced erstellen**
   * Eine zuvor bereitgestellte **Logik-App (Vorschau)** -Ressource, falls vorhanden

   Dieses Beispiel wird mit **Neue Logik-App (Vorschau) in Azure Advanced erstellen** fortgesetzt.

   ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“ und eine Liste mit ausgewähltem „Neue Logik-App (Vorschau) in Azure erstellen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-create-logic-app-options.png)

1. Um Ihre neue **Logik-App (Vorschau)** -Ressource zu erstellen, gehen Sie wie folgt vor:

   1. Geben Sie einen global eindeutigen Namen für Ihre neue Logik-App an, wobei es sich um den Namen handelt, der für die **Logik-App (Vorschau)** -Ressource verwendet werden soll. In diesem Beispiel wird `Fabrikam-Workflows-App` verwendet.

      ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“ und eine Aufforderung zur Eingabe eines Namens für die neue, zu erstellende Logik-App zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/enter-logic-app-name.png)

   1. Wählen Sie einen [Hostingplan](../app-service/overview-hosting-plans.md) für Ihre neue Logik-App aus: entweder [**App Service-Plan** (Dedicated)](../azure-functions/dedicated-plan.md) oder [**Premium**](../azure-functions/functions-premium-plan.md).

      > [!IMPORTANT]
      > Verbrauchstarife werden für diesen Ressourcentyp nicht unterstützt und sind dafür auch nicht verfügbar. Ihr ausgewählter Plan wirkt sich hierbei auf die Funktionen und Tarife aus, die später für Sie zur Verfügung stehen. Weitere Informationen finden Sie in diesen Themen: 
      >
      > * [Skalierung und Hosting von Azure Functions](../azure-functions/functions-scale.md)
      > * [App Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service/)
      >
      > Im Premium-Tarif haben Sie beispielsweise Zugriff auf Netzwerkfunktionen wie das Herstellen einer privaten Verbindung zu virtuellen Azure-Netzwerken und das Integrieren dieser Netzwerke. Diese Funktionen ähneln den Funktionen in Azure Functions für das Erstellen und Bereitstellen Ihrer Logik-Apps. 
      > Weitere Informationen finden Sie in diesen Themen:
      > 
      > * [Netzwerkoptionen von Azure Functions](../azure-functions/functions-networking-options.md)
      > * [Azure Logic Apps ohne Grenzen: Netzwerkoptionen mit Azure Logic Apps (Vorschau)](https://techcommunity.microsoft.com/t5/integrations-on-azure/logic-apps-anywhere-networking-possibilities-with-logic-app/ba-p/2105047)

      In diesem Beispiel wird der **App Service-Plan** verwendet.

      ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“und eine Aufforderung zur Auswahl von „App Service-Plan“ oder „Premium“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-hosting-plan.png)

   1. Erstellen Sie einen neuen App Service-Plan, oder wählen Sie einen vorhandenen Plan aus. In diesem Beispiel wird **Neuen App Service-Plan erstellen** ausgewählt.

      ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“und eine Aufforderung zum „Neuen App Service-Plan erstellen“ oder zum Auswählen eines vorhandenen App Service-Plans zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/create-app-service-plan.png)

   1. Geben Sie einen Namen für Ihren App Service-Plan an, und wählen Sie dann einen [Tarif](../app-service/overview-hosting-plans.md) für den Plan aus. In diesem Beispiel wird der Plan **F1 Free** ausgewählt.

      ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“und eine Aufforderung zur Auswahl eines Tarifs zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/select-pricing-tier.png)

   1. Um eine optimale Leistung zu erzielen, suchen Sie dieselbe Ressourcengruppe wie die für die Bereitstellung des Projekts verwendete, und wählen Sie sie aus.

      > [!NOTE]
      > Obwohl Sie eine andere Ressourcengruppe erstellen oder verwenden können, könnte sich dies negativ auf die Leistung auswirken. Wenn Sie eine andere Ressourcengruppe erstellen oder auswählen, nach der Anzeige der Bestätigungsaufforderung aber abbrechen, wird Ihre Bereitstellung ebenfalls abgebrochen.

   1. Wählen Sie für zustandsbehaftete Workflows **Neues Speicherkonto erstellen** oder ein vorhandenes Speicherkonto aus.

      ![Screenshot, der den Bereich „Azure: Logic Apps (Vorschau)“und eine Aufforderung zum Erstellen oder Auswählen eines Speicherkontos zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/create-storage-account.png)

   1. Wenn die Erstellungs- und Bereitstellungseinstellungen Ihrer Logik-App die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren. Dies können Sie entweder tun, wenn Sie Ihre Logik-App über Visual Studio Code bereitstellen, oder nach der Bereitstellung. Sie benötigen eine Application Insights-Instanz, können aber diese Ressource entweder [im Voraus](../azure-monitor/app/create-workspace-resource.md), beim Bereitstellen Ihrer Logik-App oder nach der Bereitstellung erstellen.

      Gehen Sie folgendermaßen vor, um die Protokollierung und Ablaufverfolgung jetzt zu aktivieren:

      1. Wählen Sie entweder eine vorhandene Application Insights-Ressource, oder **erstellen Sie eine neue Application Insights-Ressource**.

      1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Application Insights-Ressource.

      1. Wählen Sie im Ressourcenmenü **Übersicht** aus. Suchen Sie den **Instrumentierungsschlüssel**, und kopieren Sie ihn.

      1. Öffnen Sie in Visual Studio Code im Stammordner Ihres Projekts die Datei **local.settings.json**.

      1. Fügen Sie im `Values`-Objekt die `APPINSIGHTS_INSTRUMENTATIONKEY`-Eigenschaft hinzu, und legen Sie den Wert auf den Instrumentierungsschlüssel fest, z. B.:

         ```json
         {
            "IsEncrypted": false,
            "Values": {
               "AzureWebJobsStorage": "UseDevelopmentStorage=true",
               "FUNCTIONS_WORKER_RUNTIME": "dotnet",
               "APPINSIGHTS_INSTRUMENTATIONKEY": <instrumentation-key>
            }
         }
         ```

         > [!TIP]
         > Sie können überprüfen, ob die Trigger- und Aktionsnamen in der Application Insights-Instanz ordnungsgemäß angezeigt werden.
         >
         > 1. Navigieren Sie im Azure-Portal zu Ihrer Application Insights-Ressource.
         >
         > 2. Wählen Sie im Ressourcenmenü unter **Untersuchen** die Option **Anwendungszuordnung** aus.
         >
         > 3. Überprüfen Sie die Vorgangsnamen, die in der Zuordnung angezeigt werden.
         >
         > Einige eingehende Anforderungen von integrierten Triggern werden in der Anwendungszuordnung möglicherweise als Duplikate angezeigt. 
         > Anstatt das `WorkflowName.ActionName`-Format zu verwenden, verwenden diese Duplikate den Workflownamen als Vorgangsnamen und haben ihren Ursprung auf dem Azure Functions-Host.

      1. Anschließend können Sie den Schweregrad der Ablaufverfolgungsdaten, die ihre Logik-App sammelt und an Ihre Application Insights-Instanz sendet, optional anpassen.

         Bei jedem workflowbezogenen Ereignis, wenn z. B. ein Workflow ausgelöst oder eine Aktion ausgeführt wird, gibt die Runtime verschiedene Ablaufverfolgungen aus. Diese Ablaufverfolgungen decken die Lebensdauer des Workflows ab und umfassen die folgenden Ereignistypen, sind aber nicht darauf beschränkt:

         * Dienstaktivität, z. B. Starten, Beenden und Fehler.
         * Aufträge und Verteileraktivität.
         * Workflowaktivität, z. B. Trigger, Aktion und Ausführen.
         * Speicheranforderungsaktivität, z. B. Erfolg oder Fehler.
         * HTTP-Anforderungsaktivität, z. B. eingehend, ausgehend, Erfolg und Fehler.
         * Alle Entwicklungsablaufverfolgungen, z. B. Debugmeldungen.

         Jeder Ereignistyp wird einem Schweregrad zugewiesen. Beispielsweise erfasst die `Trace`-Ebene die detailliertesten Nachrichten, während die `Information`-Ebene allgemeine Aktivitäten in Ihrem Workflow erfasst, z. B. wann Logik-App, Workflow, Trigger und Aktionen gestartet und beendet werden. In dieser Tabelle werden die Schweregrade und deren Ablaufverfolgungstypen beschrieben:

         | Schweregrad | Ablaufverfolgungstyp |
         |----------------|------------|
         | Kritisch | Protokolle, in denen ein nicht behebbarer Fehler in Ihrer Logik-App beschrieben wird. |
         | Debuggen | Protokolle, die Sie für die Untersuchung während der Entwicklung verwenden können, z. B. eingehende und ausgehende HTTP-Aufrufe. |
         | Fehler | Protokolle, die auf einen Fehler bei der Workflowausführung hindeuten, aber nicht auf einen allgemeinen Fehler in Ihrer Logik-App. |
         | Information | Protokolle, die die allgemeine Aktivität in ihrer Logik-App oder im Workflow nachverfolgen, z. B.: <p><p>– Wann ein Trigger, eine Aktion oder eine Ausführung gestartet und beendet wird. <br>– Wann Ihre Logik-App gestartet oder beendet wird. |
         | Trace | Protokolle, die die detailliertesten Meldungen enthalten, z. B. Speicheranforderungen oder Verteileraktivität, sowie alle Meldungen, die sich auf die Workflowausführungsaktivität beziehen. |
         | Warnung | Protokolle, die einen ungewöhnlichen Status in Ihrer Logik-App hervorheben, jedoch nicht verhindern, dass sie ausgeführt wird. |
         |||

         Um den Schweregrad festzulegen, öffnen Sie auf der Stammebene des Projekts die Datei **host.json**, und suchen Sie nach dem `logging`-Objekt. Dieses Objekt steuert die Protokollfilterung für alle Workflows in Ihrer Logik-App und folgt dem [ASP.NET Core-Layout für die Protokolltypfilterung](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1&preserve-view=true#log-filtering).

         ```json
         {
            "version": "2.0",
            "logging": {
               "applicationInsights": {
                  "samplingExcludedTypes": "Request",
                  "samplingSettings": {
                     "isEnabled": true
                  }
               }
            }
         }
         ```

         Wenn das `logging`-Objekt kein `logLevel`-Objekt enthält, das die `Host.Triggers.Workflow`-Eigenschaft enthält, fügen Sie diese Elemente hinzu. Legen Sie die Eigenschaft auf den Schweregrad für den gewünschten Ablaufverfolgungstyp fest, z. B.:

         ```json
         {
            "version": "2.0",
            "logging": {
               "applicationInsights": {
                  "samplingExcludedTypes": "Request",
                  "samplingSettings": {
                     "isEnabled": true
                  }
               },
               "logLevel": {
                  "Host.Triggers.Workflow": "Information"
               }
            }
         }
         ```

   Wenn Sie die Bereitstellungsschritte abgeschlossen haben, beginnt Visual Studio Code mit der Erstellung und Bereitstellung der Ressourcen, die für das Veröffentlichen Ihrer Logik-App erforderlich sind.

1. Um den Bereitstellungsprozess zu überwachen und zu überprüfen, wählen Sie im Menü **Ansicht** die Option **Ausgabe** aus. Wählen Sie in der Liste der Symbolleiste des Ausgabefensters **Azure Logic Apps** aus.

   ![Screenshot, der das Ausgabefenster mit in der Symbolleistenliste ausgewähltem „Azure Logic Apps“ zusammen mit Bereitstellungsfortschritt und -statuswerten zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-deployment-output-window.png)

   Wenn Visual Studio Code die Bereitstellung Ihrer Logik-App in Azure abschließt, wird diese Meldung angezeigt:

   ![Screenshot, der eine Meldung zeigt, dass die Bereitstellung in Azure erfolgreich abgeschlossen wurde.](./media/create-stateful-stateless-workflows-visual-studio-code/deployment-to-azure-completed.png)

   Herzlichen Glückwunsch, ihre Logik-App ist jetzt live in Azure und standardmäßig aktiviert.

Als Nächstes erfahren Sie, wie Sie diese Aufgaben ausführen:

* [Hinzufügen eines leeren Workflows zum Projekt](#add-workflow-existing-project).

* [Verwalten bereitgestellter Logik-Apps in Visual Studio Code](#manage-deployed-apps-vs-code) oder mithilfe des [Azure-Portals](#manage-deployed-apps-portal).

* [Aktivieren des Ausführungsverlaufs für zustandslose Workflows](#enable-run-history-stateless).

* [Aktivieren der Überwachungsansicht im Azure-Portal für eine bereitgestellte Logik-App](#enable-monitoring).

<a name="add-workflow-existing-project"></a>

## <a name="add-blank-workflow-to-project"></a>Hinzufügen eines leeren Workflows zum Projekt

Ihr Logik-App-Projekt kann mehrere Workflows enthalten. Um dem Projekt einen leeren Workflow hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Wählen Sie auf der Aktivitätsleiste von Visual Studio Code das Azure-Symbol aus.

1. Wählen Sie im Azure-Bereich neben **Azure: Logic Apps (Vorschau)** die Option **Workflow erstellen** (Symbol für Azure Logic Apps) aus.

1. Wählen Sie den Workflowtyp aus, den Sie hinzufügen möchten: **Zustandsbehaftet** oder **Zustandslos**.

1. Geben Sie einen Namen für den Workflow ein.

Wenn Sie fertig sind, wird in Ihrem Projekt ein neuer Workflowordner zusammen mit einer **workflow.json**-Datei für die Workflowdefinition angezeigt.

<a name="manage-deployed-apps-vs-code"></a>

## <a name="manage-deployed-logic-apps-in-visual-studio-code"></a>Verwalten bereitgestellter Logik-Apps in Visual Studio Code

In Visual Studio Code können Sie alle bereitgestellten Logik-Apps in Ihrem Azure-Abonnement anzeigen, unabhängig davon, ob es sich um den ursprünglichen **Logic Apps**- oder den **Logik-App (Vorschau)** -Ressourcentyp handelt, und Aufgaben auswählen, die Ihnen bei der Verwaltung dieser Logik-Apps helfen. Für den Zugriff auf beide Ressourcentypen benötigen Sie aber beide Erweiterungen für Visual Studio Code: **Azure Logic Apps** und **Azure Logic Apps (Vorschau)** .

1. Wählen Sie auf der linken Symbolleiste das Azure-Symbol aus. Wählen Sie im Bereich **Azure: Logic Apps (Vorschau)** Ihr Abonnement, in dem alle bereitgestellten Logik-Apps für dieses Abonnement angezeigt werden.

1. Öffnen Sie die Logik-App, die Sie verwalten möchten. Wählen Sie im Kontextmenü der Logik-App die Aufgabe aus, die Sie durchführen möchten.

   Beispielsweise können Sie Aufgaben wie das Beenden, Starten, Neustarten oder Löschen Ihrer bereitgestellten Logik-App auswählen.

   ![Screenshot, der Visual Studio Code dem geöffneten Bereich der Erweiterung „Azure Logic Apps (Vorschau)“ und dem bereitgestellten Workflow zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-visual-studio-code.png)

1. Um alle Workflows in der Logik-App anzuzeigen, erweitern Sie Ihre Logik-App und dann den Knoten **Workflows**.

1. Um einen bestimmten Workflow anzuzeigen, öffnen Sie das Kontextmenü des Workflows, und wählen Sie **Im Designer öffnen** aus. Dadurch wird der Workflow im schreibgeschützten Modus geöffnet.

   Um den Workflow zu bearbeiten, haben Sie folgende Optionen:

   * Öffnen Sie in Visual Studio Code die **workflow.json**-Datei des Projekts im Workflow-Designer, nehmen Sie Ihre Änderungen vor, und stellen Sie Ihre Logik-App erneut in Azure bereit.

   * [Suchen Sie im Azure-Portal nach Ihrer Logik-App](#manage-deployed-apps-portal), und öffnen Sie sie. Suchen, bearbeiten und speichern Sie den Workflow.

1. Um die bereitgestellte Logik-App im Azure-Portal zu öffnen, öffnen Sie das Kontextmenü der Logik-App, und wählen Sie **Im Portal öffnen** aus.

   Das Azure-Portal wird in Ihrem Browser geöffnet, Sie werden automatisch beim Portal angemeldet, wenn Sie bei Visual Studio Code angemeldet sind, und Ihre Logik-App wird angezeigt.

   ![Screenshot, der die Seite des Azure-Portals für Ihre Logik-App in Visual Studio Code zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

   Sie können sich auch gesondert beim Azure-Portal anmelden, Ihre Logik-App über das Suchfeld des Portals suchen und dann Ihre Logik-App aus der Ergebnisliste auswählen.

   ![Screenshot, der das Azure-Portal und die Suchleiste mit Suchergebnissen für die bereitgestellte Logik-App zeigt, die ausgewählt angezeigt wird.](./media/create-stateful-stateless-workflows-visual-studio-code/find-deployed-workflow-azure-portal.png)

<a name="manage-deployed-apps-portal"></a>

## <a name="manage-deployed-logic-apps-in-the-portal"></a>Verwalten bereitgestellter Logik-Apps im Portal

Im Azure-Portal können Sie alle bereitgestellten Logik-Apps anzeigen, die in Ihrem Azure-Abonnement enthalten sind, unabhängig davon, ob es sich um den ursprünglichen **Logic Apps**-Ressourcentyp oder den **Logik-App (Vorschau)** -Ressourcentyp handelt. Zurzeit wird jeder Ressourcentyp in Azure als separate Kategorien organisiert und verwaltet. Um Logik-Apps zu finden, die vom Ressourcentyp **Logik-App (Vorschau)** sind, gehen Sie wie folgt vor:

1. Geben Sie in das Suchfeld des Azure-Portals `logic app preview` ein. Wenn die Ergebnisliste angezeigt wird, wählen Sie unter **Dienste** die Option **Logik-App (Vorschau)** aus.

   ![Screenshot, der das Suchfeld des Azure-Portals mit dem Suchtext „Logik-App Vorschau“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/portal-find-logic-app-preview-resource.png)

1. Suchen Sie im Bereich **Logik-App (Vorschau)** die Logik-App, die Sie aus Visual Studio Code bereitgestellt haben, und wählen Sie sie aus.

   ![Screenshot, der das Azure-Portal und die in Azure bereitgestellten Logic App (Vorschau)-Ressourcen zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/logic-app-preview-resources-pane.png)

   Das Azure-Portal öffnet die Seite der einzelnen Ressource für die ausgewählte Logik-App.

   ![Screenshot, der die Ressourcenseite Ihres Logik-App-Workflows im Azure-Portal zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-workflow-azure-portal.png)

1. Um die Workflows in dieser Logik-App auszuwählen, wählen im Menü der Logik-App **Workflows** aus.

   Im Bereich **Workflows** werden alle in der aktuellen Logik-App vorhandenen Workflows angezeigt. Dieses Beispiel zeigt den Workflow, den Sie in Visual Studio Code erstellt haben.

   ![Screenshot, der eine „Logik-App (Vorschau)“-Ressourcenseite mit geöffnetem Bereich „Workflows“ und dem bereitgestellten Workflow zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/deployed-logic-app-workflows-pane.png)

1. Um einen Workflow anzuzeigen, wählen Sie im Bereich **Workflows** diesen Workflow aus.

   Der Workflowbereich wird geöffnet und zeigt weitere Informationen und Aufgaben an, die Sie für diesen Workflow ausführen können.

   Um beispielsweise die Schritte in dem Workflow anzuzeigen, wählen Sie **Designer** aus.

   ![Screenshot, der den Bereich „Übersicht“ des ausgewählten Workflows zeigt, während im Menü „Workflow“ der ausgewählte Befehl „Designer“ angezeigt wird.](./media/create-stateful-stateless-workflows-visual-studio-code/workflow-overview-pane-select-designer.png)

   Der Workflow-Designer wird geöffnet und zeigt den Workflow an, den Sie in Visual Studio Code erstellt haben. Sie können jetzt Änderungen an diesem Workflow im Azure-Portal vornehmen.

   ![Screenshot, der den Workflow-Designer und den aus Visual Studio Code bereitgestellten Workflow zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-workflow-designer.png)

<a name="add-workflow-portal"></a>

## <a name="add-another-workflow-in-the-portal"></a>Hinzufügen eines weiteren Workflows im Portal

Über das Azure-Portal können Sie einer **Logik-App**-Ressource (Vorschau), die Sie aus Visual Studio Code bereitgestellt haben, leere Workflows hinzufügen und diese Workflows im Azure-Portal erstellen.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) Ihre bereitgestellte **Logik-App (Vorschau)** -Ressource, und wählen Sie sie aus.

1. Wählen Sie im Menü der Logik-App **Workflows** aus. Wählen Sie im Bereich **Workflows** den Befehl **Hinzufügen** aus.

   ![Screenshot, der den Bereich „Workflows“ der ausgewählten Logik-App und die Symbolleiste mit ausgewähltem Befehl „Hinzufügen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/add-new-workflow.png)

1. Geben Sie im Bereich **Neuer Workflow** einen Namen für den Workflow an. Wählen Sie entweder **Zustandsbehaftet** oder **Zustandslos** **>** **Erstellen** aus.

   Nachdem Azure Ihren neuen Workflow bereitgestellt hat, der im Bereich **Workflows** angezeigt wird, wählen Sie diesen Workflow aus, um die Verwaltung und andere Aufgaben auszuführen, z. B. das Öffnen des Designers oder der Codeansicht.

   ![Screenshot, der den ausgewählten Workflow mit Verwaltungs- und Überprüfungsoptionen zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/view-new-workflow.png)

   Wenn Sie z. B. den Designer für einen neuen Workflow öffnen, wird ein leerer Zeichenbereich angezeigt. Sie können diesen Workflow jetzt im Azure-Portal erstellen.

   ![Screenshot, der den Workflow-Designer und einen leeren Workflow zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/opened-blank-workflow-designer.png)

<a name="enable-run-history-stateless"></a>

## <a name="enable-run-history-for-stateless-workflows"></a>Aktivieren des Ausführungsverlaufs für zustandslose Workflows

Damit Sie einen zustandslosen Workflow debuggen können, können Sie den Ausführungsverlauf für diesen Workflow aktivieren und wieder deaktivieren, wenn Sie fertig sind. Führen Sie diese Schritte für Visual Studio Code aus. Wenn Sie im Azure-Portal arbeiten, lesen Sie [Aktivieren des Ausführungsverlaufs für zustandslose Workflows](create-stateful-stateless-workflows-azure-portal.md#enable-run-history-stateless).

1. Erweitern Sie in Ihrem Visual Studio Code-Projekt den Ordner **workflow-designtime**, und öffnen Sie die Datei **local.settings.json**.

1. Fügen Sie die Eigenschaft `Workflows.{yourWorkflowName}.operationOptions` hinzu, und legen Sie deren Wert auf `WithStatelessRunHistory` fest, z. B.:

   **Windows**

   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "UseDevelopmentStorage=true",
         "FUNCTIONS_WORKER_RUNTIME": "dotnet",
         "Workflows.{yourWorkflowName}.OperationOptions": "WithStatelessRunHistory"
      }
   }
   ```

   **macOS oder Linux**

   ```json
   {
      "IsEncrypted": false,
      "Values": {
         "AzureWebJobsStorage": "DefaultEndpointsProtocol=https;AccountName=fabrikamstorageacct; \
             AccountKey=<access-key>;EndpointSuffix=core.windows.net",
         "FUNCTIONS_WORKER_RUNTIME": "dotnet",
         "Workflows.{yourWorkflowName}.OperationOptions": "WithStatelessRunHistory"
      }
   }
   ```

1. Um den Ausführungsverlauf zu deaktivieren, wenn Sie fertig sind, legen Sie entweder die Eigenschaft `Workflows.{yourWorkflowName}.OperationOptions` auf `None` fest oder löschen die Eigenschaft und ihren Wert.

<a name="enable-monitoring"></a>

## <a name="enable-monitoring-view-in-the-azure-portal"></a>Aktivieren der Überwachungsansicht im Azure-Portal

Nachdem Sie eine Ressource von **Logic Apps (Vorschau)** aus Visual Studio Code in Azure bereitgestellt haben, können Sie jeden verfügbaren Ausführungsverlauf und die Details für einen Workflow in dieser Ressource überprüfen, indem Sie das Azure-Portal und die **Monitor**-Umgebung für diesen Workflow verwenden. Sie müssen jedoch zunächst die **Monitor**-Ansichtsfunktion für diese Logik-App-Ressource aktivieren.

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) die bereitgestellte **Logik-App (Vorschau)** -Ressource, und wählen Sie sie aus.

1. Wählen Sie im Menü dieser Ressource unter **API** die Option **CORS** aus.

1. Fügen Sie im Bereich **CORS** unter **Zulässige Ursprünge** das Platzhalterzeichen (*) hinzu.

1. Wenn Sie fertig sind, wählen Sie auf der **CORS**-Symbolleiste **Speichern** aus.

   ![Screenshot, der das Azure-Portal mit einer bereitgestellten Ressource von „Logic Apps (Vorschau)“ zeigt. Im Ressourcenmenü ist „CORS“ ausgewählt, mit einem neuen Eintrag für „Zulässige Ursprünge“, der auf das Platzhalterzeichen „*“ festgelegt ist.](./media/create-stateful-stateless-workflows-visual-studio-code/enable-run-history-deployed-logic-app.png)

<a name="enable-open-application-insights"></a>

## <a name="enable-or-open-application-insights-after-deployment"></a>Aktivieren oder Öffnen von Application Insights nach der Bereitstellung

Während der Workflowausführung gibt Ihre Logik-App Telemetriedaten zusammen mit anderen Ereignissen aus. Sie können diese Telemetrie verwenden, um auf verschiedene Weise einen besseren Einblick in die Qualität der Workflowausführung und die Arbeitsweise der Logic Apps-Runtime zu bekommen. Sie können den Workflow überwachen, indem Sie [Application Insights](../azure-monitor/app/app-insights-overview.md) verwenden, wodurch nahezu in Echtzeit Telemetriedaten (Livemetriken) bereitgestellt werden. Mithilfe dieser Funktion können Sie Fehler und Leistungsprobleme leichter untersuchen, wenn Sie diese Daten zum Diagnostizieren von Problemen, Einrichten von Warnungen und Erstellen von Diagrammen verwenden.

Wenn die Erstellungs- und Bereitstellungseinstellungen Ihrer Logik-App die Verwendung von [Application Insights](../azure-monitor/app/app-insights-overview.md) unterstützen, können Sie optional die Diagnoseprotokollierung und Ablaufverfolgung für Ihre Logik-App aktivieren. Dies können Sie entweder tun, wenn Sie Ihre Logik-App über Visual Studio Code bereitstellen, oder nach der Bereitstellung. Sie benötigen eine Application Insights-Instanz, können aber diese Ressource entweder [im Voraus](../azure-monitor/app/create-workspace-resource.md), beim Bereitstellen Ihrer Logik-App oder nach der Bereitstellung erstellen.

Gehen Sie folgendermaßen vor, um Application Insights auf einer bereitgestellten Logik-App zu aktivieren oder Application Insights-Daten zu überprüfen, wenn sie bereits aktiviert sind:

1. Suchen Sie im Azure-Portal nach Ihrer bereitgestellten Logik-App.

1. Wählen Sie im Menü der Logik-App unter **Einstellungen** die Option **Application Insights** aus.

1. Wenn Application Insights nicht aktiviert ist, wählen Sie im Bereich **Application Insights** die Option **Application Insights aktivieren** aus. Nachdem der Bereich aktualisiert wurde, wählen Sie unten **Anwenden** aus.

   Wenn Application Insights aktiviert ist, wählen Sie im Bereich **Application Insights** die Option **Application Insights-Daten anzeigen** aus.

Nachdem Application Insights geöffnet wurde, können Sie verschiedene Metriken für Ihre Logik-App überprüfen. Weitere Informationen finden Sie in diesen Themen:

* [Azure Logic Apps ohne Grenzen ausführen: Überwachung mit Application Insights (Teil 1)](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-monitor-with-application/ba-p/1877849)
* [Azure Logic Apps ohne Grenzen ausführen: Überwachung mit Application Insights (Teil 2)](https://techcommunity.microsoft.com/t5/integrations-on-azure/azure-logic-apps-running-anywhere-monitor-with-application/ba-p/2003332)

<a name="deploy-docker"></a>

## <a name="deploy-to-docker"></a>Bereitstellen in Docker

Sie können Ihre Logik-App in einem [Docker-Container](/visualstudio/docker/tutorials/docker-tutorial#what-is-a-container) als Hostingumgebung bereitstellen, indem Sie die [.NET CLI](/dotnet/core/tools/) verwenden. Mit diesen Befehlen können Sie das Projekt Ihrer Logik-App erstellen und veröffentlichen. Anschließend können Sie einen Docker-Container als Ziel für die Bereitstellung Ihrer Logik-App erstellen und ausführen.

Wenn Sie mit Docker nicht vertraut sind, lesen Sie die folgenden Themen:

* [Was ist Docker?](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)
* [Introduction to Containers and Docker (Einführung in Container und Docker)](/dotnet/architecture/microservices/container-docker-introduction/)
* [Einführung in .NET and Docker](/dotnet/core/docker/introduction)
* [Docker containers, images, and registries (Docker-Container, -Images und -Registrierungen)](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)
* [Tutorial: Erste Schritte mit Docker (Visual Studio Code)](/visualstudio/docker/tutorials/docker-tutorial)

### <a name="requirements"></a>Anforderungen

* Das Azure Storage-Konto, das Ihre Logik-App für die Bereitstellung verwendet

* Eine Docker-Datei für den Workflow, den Sie beim Aufbau Ihres Docker-Containers verwenden

  Diese Beispiel-Docker-Datei stellt beispielsweise eine Logik-App bereit und gibt die Verbindungszeichenfolge an, die den Zugriffsschlüssel für das Azure-Speicherkonto enthält, das für die Veröffentlichung der Logik-App im Azure-Portal verwendet wurde. Informationen zum Ermitteln dieser Zeichenfolge finden Sie unter [Beschaffen der Verbindungszeichenfolge für das Speicherkonto](#find-storage-account-connection-string). Weitere Informationen finden Sie unter [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) (Bewährte Methoden zum Schreiben von Docker-Dateien).
  
  > [!IMPORTANT]
  > Achten Sie bei Produktionsszenarien darauf, dass Sie solche Geheimnisse und sensiblen Informationen schützen und sichern, z. B. durch die Verwendung eines Schlüsseltresors. Informationen zu Docker-Dateien finden Sie unter [Erstellen von Images mit BuildKit](https://docs.docker.com/develop/develop-images/build_enhancements/) und [Verwalten von sensiblen Daten mit Docker-Geheimnissen](https://docs.docker.com/engine/swarm/secrets/).

   ```text
   FROM mcr.microsoft.com/azure-functions/node:3.0

   ENV AzureWebJobsStorage <storage-account-connection-string>
   ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
       AzureFunctionsJobHost__Logging__Console__IsEnabled=true \
       FUNCTIONS_V2_COMPATIBILITY_MODE=true

   COPY . /home/site/wwwroot

   RUN cd /home/site/wwwroot
   ```

<a name="find-storage-account-connection-string"></a>

### <a name="get-storage-account-connection-string"></a>Beschaffen der Verbindungszeichenfolge für das Speicherkonto

Bevor Sie Ihr Docker-Containerimage erstellen und ausführen können, müssen Sie die Verbindungszeichenfolge abrufen, die den Zugriffsschlüssel für Ihr Speicherkonto enthält. Sie haben dieses Speicherkonto weiter oben erstellt, entweder für die Verwendung der Erweiterung unter macOS oder Linux, oder als Sie im Azure-Portal Ihre Logik-App bereitgestellt haben.

Führen Sie die folgenden Schritte aus, um diese Verbindungszeichenfolge zu ermitteln und zu kopieren:

1. Wählen Sie im Azure-Portal im Menü des Speicherkontos unter **Einstellungen** die Option **Zugriffsschlüssel** aus. 

1. Suchen Sie im Bereich **Zugriffsschlüssel** nach der Verbindungszeichenfolge für das Speicherkonto, und kopieren Sie sie. Sie sieht in etwa wie im folgenden Beispiel aus:

   `DefaultEndpointsProtocol=https;AccountName=fabrikamstorageacct;AccountKey=<access-key>;EndpointSuffix=core.windows.net`

   ![Screenshot, der das Azure-Portal mit Speicherkonto-Zugriffsschlüsseln und kopierter Verbindungszeichenfolge anzeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/find-storage-account-connection-string.png)

   Weitere Informationen finden Sie unter [Anzeigen von Kontozugriffsschlüsseln](../storage/common/storage-account-keys-manage.md?tabs=azure-portal#view-account-access-keys).

1. Speichern Sie die Verbindungszeichenfolge an einem sicheren Ort, damit Sie sie der Docker-Datei hinzufügen können, die Sie für die Bereitstellung verwenden. 

<a name="find-storage-account-master-key"></a>

### <a name="find-master-key-for-storage-account"></a>Ermitteln des Hauptschlüssels für das Speicherkonto

Wenn Ihr Workflow einen Anforderungstrigger enthält, müssen Sie die [Rückruf-URL des Triggers ermitteln](#get-callback-url-request-trigger), nachdem Sie Ihr Docker-Containerimage erstellt und ausgeführt haben. Für diese Aufgabe müssen Sie auch den Hauptschlüsselwert für das Speicherkonto angeben, das Sie für die Bereitstellung verwenden.

1. Öffnen Sie in Ihrem Projekt die Datei **azure-webjobs-secrets/{deployment-name}/host.json**, um diesen Hauptschlüssel zu ermitteln.

1. Suchen Sie nach der Eigenschaft `AzureWebJobsStorage`, und kopieren Sie den Schlüsselwert aus diesem Abschnitt:

   ```json
   {
      <...>
      "masterKey": {
         "name": "master",
         "value": "<master-key>",
         "encrypted": false
      },
      <...>
   }
   ```

1. Speichern Sie diesen Schlüsselwert zur späteren Verwendung an einem sicheren Ort.

<a name="build-run-docker-container-image"></a>

### <a name="build-and-run-your-docker-container-image"></a>Erstellen und Ausführen Ihres Docker-Containerimages

1. Erstellen Sie Ihr Docker-Containerimage mit Ihrer Docker-Datei, und führen Sie den folgenden Befehl aus:

   `docker build --tag local/workflowcontainer .`

   Weitere Informationen finden Sie unter [docker build](https://docs.docker.com/engine/reference/commandline/build/).

1. Führen Sie den Container lokal mithilfe des folgenden Befehls aus:

   `docker run -e WEBSITE_HOSTNAME=localhost -p 8080:80 local/workflowcontainer`

   Weitere Informationen finden Sie unter [docker run](https://docs.docker.com/engine/reference/commandline/run/).

<a name="get-callback-url-request-trigger"></a>

### <a name="get-callback-url-for-request-trigger"></a>Abrufen der Rückruf-URL für den Anforderungstrigger

Rufen Sie für einen Workflow, für den ein Anforderungstrigger genutzt wird, die Rückruf-URL des Triggers ab, indem Sie die folgende Anforderung senden:

`POST /runtime/webhooks/workflow/api/management/workflows/{workflow-name}/triggers/{trigger-name}/listCallbackUrl?api-version=2020-05-01-preview&code={master-key}`

Der Wert `{trigger-name}` ist der Name für den Anforderungstrigger, der in der JSON-Definition des Workflows angezeigt wird. Der Wert `{master-key}` wird in dem Azure Storage-Konto definiert, das Sie für die Eigenschaft `AzureWebJobsStorage` in der Datei **azure-webjobs-secrets/{deployment-name}/host.json** festlegen. Weitere Informationen finden Sie unter [Ermitteln des Hauptschlüssels für das Speicherkonto](#find-storage-account-master-key).

<a name="delete-from-designer"></a>

## <a name="delete-items-from-the-designer"></a>Löschen von Elementen aus dem Designer

Führen Sie einen der folgenden Schritte aus, um ein Element in Ihrem Workflow aus dem Designer zu löschen:

* Wählen Sie das Element aus, öffnen Sie das Kontextmenü des Elements (UMSCHALT+F10), und wählen Sie **Löschen** aus. Klicken Sie auf **OK**, um dies zu bestätigen.

* Wählen Sie das Element aus, und drücken Sie dann die ENTF-Taste. Klicken Sie auf **OK**, um dies zu bestätigen.

* Wählen Sie das Element aus, sodass der Detailbereich für dieses Element geöffnet wird. Öffnen Sie in der oberen rechten Ecke des Bereichs das Menü mit den Auslassungspunkten ( **...** ), und wählen Sie **Löschen** aus. Klicken Sie auf **OK**, um dies zu bestätigen.

  ![Screenshot, der ein ausgewähltes Element im Designer mit geöffnetem Detailbereich sowie die ausgewählte Schaltfläche mit Auslassungspunkten und den Befehl „Löschen“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/delete-item-from-designer.png)

  > [!TIP]
  > Wenn das Menü mit den Auslassungspunkten nicht sichtbar ist, erweitern Sie das Visual Studio Code-Fenster, sodass im Detailbereich die Schaltfläche mit den Auslassungspunkten ( **...** ) in der oberen rechten Ecke angezeigt wird.

<a name="troubleshooting"></a>

## <a name="troubleshoot-errors-and-problems"></a>Beheben von Fehlern und Problemen

<a name="designer-fails-to-open"></a>

### <a name="designer-fails-to-open"></a>Designer wird nicht geöffnet

Beim Versuch, den Designer zu öffnen, erhalten Sie einen Fehler wie **Workflowentwurfszeit konnte nicht gestartet werden**. Falls Sie schon einmal versucht haben, den Designer zu öffnen, und Ihr Projekt anschließend nicht weiter verfolgt oder gelöscht haben, wird das Erweiterungspaket ggf. nicht richtig heruntergeladen. Führen Sie die folgenden Schritte aus, um zu überprüfen, ob dies die Ursache des Problems ist:

  1. Öffnen Sie in Visual Studio Code das Fenster „Ausgabe“. Wählen Sie im Menü **Ansicht** die Option **Ausgabe** aus.

  1. Wählen Sie in der Liste auf der Titelleiste des Fensters „Ausgabe“ **Azure Logic Apps (Vorschau)** aus, damit Sie die Ausgabe für die Erweiterung überprüfen können, z. B.:

     ![Screenshot, der das Fenster „Ausgabe“ mit ausgewählten „Azure Logic Apps“ zeigt.](./media/create-stateful-stateless-workflows-visual-studio-code/check-outout-window-azure-logic-apps.png)

  1. Überprüfen Sie die Ausgabe, und prüfen Sie, ob diese Fehlermeldung angezeigt wird:

     ```text
     A host error has occurred during startup operation '{operationID}'.
     System.Private.CoreLib: The file 'C:\Users\{userName}\AppData\Local\Temp\Functions\
     ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle.Workflows\1.1.7\bin\
     DurableTask.AzureStorage.dll' already exists.
     Value cannot be null. (Parameter 'provider')
     Application is shutting down...
     Initialization cancellation requested by runtime.
     Stopping host...
     Host shutdown completed.
     ```

   Um diesen Fehler zu beheben, löschen Sie den Ordner **ExtensionBundles** am Speicherort **...\Benutzer\{Ihr-Benutzername}\AppData\Local\Temp\Functions\ExtensionBundles**. Versuchen Sie anschließend erneut, die Datei **workflow.json** im Designer zu öffnen.

<a name="missing-triggers-actions"></a>

### <a name="new-triggers-and-actions-are-missing-from-the-designer-picker-for-previously-created-workflows"></a>In der Designerauswahl für zuvor erstellte Workflows fehlen neue Trigger und Aktionen.

„Azure Logic Apps (Vorschau)“ unterstützt integrierte Aktionen für Azure-Funktionsvorgänge, Liquid-Vorgänge und XML-Vorgänge wie **XML-Validierung** und **Transformieren von XML**. Allerdings werden diese Aktionen für zuvor erstellte Logik-Apps möglicherweise nicht für Sie zur Auswahl in der Designerauswahl angezeigt, wenn Visual Studio Code eine veraltete Version des Erweiterungspakets `Microsoft.Azure.Functions.ExtensionBundle.Workflows` verwendet.

Außerdem werden der Connector für **Azure-Funktionsvorgänge** und Aktionen nicht in der Designerauswahl angezeigt, es sei denn, Sie haben **Connectors von Azure verwenden** beim Erstellen Ihrer Logik-App aktiviert oder ausgewählt. Wenn Sie die von Azure bereitgestellten Connectors zum Zeitpunkt der Erstellung der App nicht aktiviert haben, können Sie sie in Visual Studio Code von Ihrem Projekt aus aktivieren. Öffnen Sie das **workflow.json**-Kontextmenü, und wählen Sie **Connectors von Azure verwenden** aus.

Führen Sie zur Korrektur des Problems mit dem veralteten Paket die folgenden Schritte zum Löschen des veralteten Pakets aus. Dann aktualisiert Visual Studio Code das Erweiterungspaket automatisch auf die neueste Version.

> [!NOTE]
> Diese Lösung gilt nur für Logik-Apps, die Sie mit Visual Studio Code mit der Erweiterung „Azure Logic Apps (Vorschau)“ erstellen und bereitstellen, nicht die Logik-Apps, die Sie mit dem Azure-Portal erstellt haben. Siehe [Neue Trigger und Aktionen fehlen in der Designerauswahl für früher erstellte Workflows](create-stateful-stateless-workflows-azure-portal.md#missing-triggers-actions).

1. Speichern Sie sämtliche Arbeit, die Sie nicht verlieren möchten, und schließen Sie Visual Studio.

1. Navigieren Sie auf Ihrem Computer zum folgenden Ordner, der Ordner mit Versionsangaben für das vorhandene Paket enthält:

   `...\Users\{your-username}\.azure-functions-core-tools\Functions\ExtensionBundles\Microsoft.Azure.Functions.ExtensionBundle.Workflows`

1. Löschen Sie den Versionsordner für das frühere Paket. Wenn Sie z. B. einen Ordner für Version 1.1.3 haben, löschen Sie diesen Ordner.

1. Navigieren Sie nun zum folgenden Ordner, der Ordner mit Versionsangaben für das erforderliche NuGet-Paket enthält:

   `...\Users\{your-username}\.nuget\packages\microsoft.azure.workflows.webjobs.extension`

1. Löschen Sie den Versionsordner für das frühere Paket. Wenn Sie z. B. einen Ordner für Version 1.0.0.8-preview haben, löschen Sie diesen Ordner.

1. Öffnen Sie Visual Studio Code, Ihr Projekt und die **workflow.json**-Datei im Designer erneut.

Die fehlenden Trigger und Aktionen werden nun im Designer angezeigt.

<a name="400-bad-request"></a>

### <a name="400-bad-request-appears-on-a-trigger-or-action"></a>„400 – Ungültige Anforderung“ wird für einen Trigger oder eine Aktion angezeigt

Wenn Sie nach einem Fehler in einer Ausführung diese Ausführung in der Überwachungsansicht untersuchen, kann dieser Fehler bei einem Trigger oder einer Aktion mit einem längeren Namen angezeigt werden. Ein solcher Name kann dazu führen, dass der zugrunde liegende URI (Uniform Resource Identifier) das Standardzeichenlimit überschreitet.

Um dieses Problem zu beheben und längere URIs zuzulassen, bearbeiten Sie die Registrierungsschlüssel `UrlSegmentMaxCount` und `UrlSegmentMaxLength` auf Ihrem Computer. Führen Sie dazu die folgenden Schritte aus. Die Standardwerte dieser Schlüssel werden in diesem Thema beschrieben: [Http.sys-Registrierungseinstellungen für Windows](/troubleshoot/iis/httpsys-registry-windows).

> [!IMPORTANT]
> Speichern Sie Ihre bisherige Arbeit, bevor Sie beginnen. Diese Lösung erfordert nach Abschluss der Änderungen einen Neustart Ihres Computers, damit die Änderungen wirksam werden.

1. Öffnen Sie auf Ihrem Computer das Fenster **Ausführen**, und führen Sie den Befehl `regedit` aus. Dieser öffnet den Registrierungs-Editor.

1. Klicken Sie im Feld **Benutzerkontensteuerung** auf **Ja**, um Änderungen am Computer zuzulassen.

1. Erweitern Sie im linken Bereich unter **Computer** die Knoten im Pfad **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters**, und wählen Sie dann **Parameters** aus.

1. Suchen Sie im rechten Bereich nach den Registrierungsschlüsseln `UrlSegmentMaxCount` und `UrlSegmentMaxLength`.

1. Erhöhen Sie die Werte dieser Schlüssel so, dass die URIs die von Ihnen gewünschten Namen aufnehmen können. Wenn diese Schlüssel nicht vorhanden sind, fügen Sie sie mithilfe der folgenden Schritte zum Ordner **Parameters** hinzu:

   1. Wählen Sie im Kontextmenü für **Parameters** die Option **Neu** > **DWORD-Wert (32-Bit)** aus.

   1. Geben Sie im daraufhin angezeigten Bearbeitungsfeld `UrlSegmentMaxCount` als neuen Schlüsselnamen ein.

   1. Öffnen Sie das Kontextmenü des neuen Schlüssels, und wählen Sie **Ändern** aus.

   1. Geben Sie im angezeigten Feld **DWORD-Wert (32-Bit) bearbeiten** unter **Wertdaten** den gewünschten Schlüsselwert im Hexadezimal- oder Dezimalformat ein. Der Wert `400` im Hexadezimalformat entspricht beispielsweise dem Wert `1024` im Dezimalformat.

   1. Zum Hinzufügen des Schlüsselwerts `UrlSegmentMaxLength` wiederholen Sie diese Schritte.

   Nachdem Sie die Schlüsselwerte erhöht oder hinzugefügt haben, sieht der Registrierungs-Editor wie folgt aus:

   ![Screenshot des Registrierungs-Editors](media/create-stateful-stateless-workflows-visual-studio-code/edit-registry-settings-uri-length.png)

1. Wenn Sie fertig sind, starten Sie Ihren Computer neu, damit die Änderungen wirksam werden.

<a name="debugging-fails-to-start"></a>

### <a name="debugging-session-fails-to-start"></a>Debugsitzung wird nicht gestartet

Beim Versuch, eine Debugsitzung zu starten, erhalten Sie einen Fehler wie **Fehler nach der Ausführung von preLaunchTask „generateDebugSymbols“** . Bearbeiten Sie die Datei **tasks.json** in Ihrem Projekt so, dass die Symbolgenerierung übersprungen wird, um dieses Problem zu beheben.

1. Erweitern Sie in Ihrem Projekt den Ordner **.vscode**, und öffnen Sie die Datei **tasks.json**.

1. Löschen Sie in der folgenden Aufgabe die Zeile `"dependsOn: "generateDebugSymbols"` (einschließlich des Kommas, mit dem die vorherige Zeile abgeschlossen wird). Beispiel:

   Vorher:

   ```json
    {
      "type": "func",
      "command": "host start",
      "problemMatcher": "$func-watch",
      "isBackground": true,
      "dependsOn": "generateDebugSymbols"
    }
   ```

   Nachher:

   ```json
    {
      "type": "func",
      "command": "host start",
      "problemMatcher": "$func-watch",
      "isBackground": true
    }
   ```

## <a name="next-steps"></a>Nächste Schritte

Teilen Sie uns bitte Ihre Erfahrungen mit der Erweiterung „Azure Logic Apps (Vorschau)“ mit!

* Bei Fehlern oder Problemen [erstellen Sie Ihre Probleme in GitHub](https://github.com/Azure/logicapps/issues).
* Bei Fragen, Anforderungen, Kommentaren und anderem Feedback [verwenden Sie dieses Feedbackformular](https://aka.ms/lafeedback).
