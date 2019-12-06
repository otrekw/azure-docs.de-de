---
title: Bereitstellen von Azure FarmBeats
description: Beschreibt, wie FarmBeats bereitgestellt wird.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 0fc7eb6c3b582c4fc95d78397c4cb2820ebb4ea8
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534242"
---
# <a name="deploy-farmbeats"></a>Bereitstellen von FarmBeats

In diesem Artikel wird beschrieben, wie Sie Azure FarmBeats einrichten.

Azure FarmBeats ist eine branchenspezifische, erweiterbare Lösung für die datengesteuerte Landwirtschaft. Sie ermöglicht die nahtlose Bereitstellung und Verbindung von Sensorgeräten mit der Azure-Cloud und unterstützt die Sammlung und Aggregation von Telemetriedaten. Azure FarmBeats verfügt über verschiedene Sensoren, wie Kameras, Drohnen und Bodensensoren, und unterstützt die Verwaltung von Geräten über die Cloud – von der Azure-Infrastruktur und Azure-Diensten für IoT-fähige Geräte (Internet der Dinge) bis hin zu einer erweiterbaren Web-App und mobilen App, die Visualisierungen, Benachrichtigungen und Erkenntnisse liefern.

> [!NOTE]
> Azure FarmBeats wird nur in öffentlichen Cloudumgebungen unterstützt. Weitere Informationen zur Cloudumgebung finden Sie unter [Azure](https://azure.microsoft.com/overview/what-is-a-public-cloud/).

Azure FarmBeats verfügt über die folgenden beiden Komponenten:

- **Datenhub**: Der Datenhub stellt die Plattformebene von Azure FarmBeats dar. Hier können Sie Daten erstellen, speichern und verarbeiten und Erkenntnisse aus vorhandenen oder neuen Datenpipelines ziehen. Diese Plattformebene eignet sich zum Ausführen und Erstellen von Pipelines und Modellen für landwirtschaftliche Daten.

- **Accelerator**: Der Accelerator stellt die Lösungsebene von Azure FarmBeats dar. Er verfügt über eine integrierte Anwendung, um die Fähigkeiten von Azure FarmBeats anhand der vorgefertigten Landwirtschaftsmodelle zu veranschaulichen. Mithilfe dieser Lösungsebene können Sie innerhalb des landwirtschaftlichen Betriebs Grenzen festlegen und aus den landwirtschaftlichen Daten im Kontext dieser Grenzen Erkenntnisse gewinnen.

Eine schnelle Bereitstellung von Azure FarmBeats sollte weniger als eine Stunde dauern. Die Kosten für Datenhub und Accelerator variieren je nach Nutzung.

## <a name="deployed-resources"></a>Bereitgestellte Ressourcen

Bei der Bereitstellung von Azure FarmBeats werden die unten aufgeführten Ressourcen in Ihrem Abonnement erstellt:

|Nr.  |Ressourcenname  |Azure FarmBeats-Komponente  |
|---------|---------|---------|
|1  |       Azure Cosmos DB   |  Datenhub       |
|2  |    Application Insights      |     Datenhub/Accelerator     |
|3  |Azure Cache for Redis   |Datenhub   |
|4  |       Azure Key Vault    |  Datenhub/Accelerator        |
|5  |    Time Series Insights       |     Datenhub      |
|6 |      EventHub-Namespace    |  Datenhub       |
|7  |    Azure Data Factory V2       |     Datenhub/Accelerator      |
|8  |Batch-Konto    |Datenhub   |
|9  |       Speicherkonto     |  Datenhub/Accelerator        |
|10  |    Logik-App        |     Datenhub      |
|11  |    API-Verbindung        |     Datenhub      |
|12|      App Service      |  Datenhub/Accelerator       |
|13 |    App Services-Plan        |     Datenhub/Accelerator      |
|14 |Azure Maps-Konto     |Accelerator    |
|15 |       Time Series Insights      |  Datenhub     |

Azure FarmBeats kann vom Azure Marketplace heruntergeladen werden. Sie können direkt im Azure-Portal darauf zugreifen.  

## <a name="create-azure-farmbeats-offer-on-marketplace"></a>Erstellen eines Azure FarmBeats-Angebots im Marketplace

Gehen Sie folgendermaßen vor, um ein Azure FarmBeats-Angebot im Marketplace zu erstellen:

1. Melden Sie sich beim Azure-Portal an, und wählen Sie oben rechts Ihr Konto aus. Wechseln Sie zum Azure AD-Mandanten, unter dem Microsoft Azure FarmBeats bereitgestellt werden soll.
2. Azure FarmBeats ist im Azure Marketplace verfügbar. Wählen Sie auf der Marketplace-Seite „Jetzt herunterladen“ aus.
3. Wählen Sie „Erstellen“ aus, und geben Sie die folgenden Informationen ein:
    - Einen Abonnementnamen.
    - Einen vorhandenen Ressourcengruppennamen (nur eine leere Ressourcengruppe). Oder Sie erstellen eine neue Ressourcengruppe, um Azure FarmBeats bereitzustellen. Notieren Sie sich diese Ressourcengruppe in den nachfolgenden Abschnitten.
4. Geben Sie die Region ein, in der Azure FarmBeats installiert werden soll. Derzeit wird Azure FarmBeats in den folgenden Regionen unterstützt: USA, Mitte; Europa, Westen; USA, Osten 2; Europa, Norden; USA, Westen; Asien, Südosten; USA, Osten; Australien, Osten; USA, Westen 2
5. Klicken Sie auf **OK**.
Die Seite „Nutzungsbedingungen“ wird angezeigt. Lesen Sie die Standardbedingungen für den Marketplace, oder wählen Sie den Link aus, um die Nutzungsbedingungen zu lesen.
6. Wählen Sie **Schließen** aus, aktivieren Sie dann das Kontrollkästchen „Ich stimme zu“, und wählen Sie dann **Erstellen** aus.
7. Sie haben den Endbenutzer-Lizenzvertrag (EULA) für Azure FarmBeats im Marketplace nun erfolgreich unterzeichnet.  
7. Um mit der Bereitstellung fortzufahren, führen Sie die nächsten Schritte in diesem Leitfaden aus.

> [!NOTE]
> Die Vervollständigung des Angebots im Azure Marketplace stellt nur einen Teil des Setups dar. Führen Sie die folgenden Anweisungen aus, um die Bereitstellung von Azure FarmBeats in Ihrem Azure-Abonnement abzuschließen.

## <a name="prepare"></a>Vorbereiten

Zum Bereitstellen von Azure FarmBeats benötigen Sie die folgenden Berechtigungen:

- Mandant: Lesezugriff
- Abonnement: Mitwirkender oder Besitzer
- Ressourcengruppe: Besitzer

## <a name="before-you-begin"></a>Voraussetzungen

Stellen Sie vor dem Start der Bereitstellung sicher, dass Sie über Folgendes verfügen:

- Sentinel-Konto
- Azure Active Directory-App-Registrierung

## <a name="create-a-sentinel-account"></a>Erstellen eines Sentinel-Kontos    

Ein Sentinel-Konto hilft Ihnen beim Herunterladen der Sentinel-Satellitenbilder von der offiziellen Website auf Ihr Gerät. Führen Sie folgende Schritte durch, um ein kostenloses Konto zu erstellen:

Wechseln Sie zur Adresse https://scihub.copernicus.eu/dhus/#/self-registration. Geben Sie auf der Registrierungsseite Vornamen, Nachnamen, Benutzernamen, Kennwort und E-Mail-Adresse ein.
Zur Bestätigung wird eine Überprüfungs-E-Mail an die registrierte E-Mail-Adresse gesendet. Wählen Sie den Link aus, und bestätigen Sie die Adresse. Der Registrierungsvorgang ist abgeschlossen.

## <a name="create-azure-ad-app-registration"></a>Erstellen einer Azure AD-App-Registrierung

Zur Authentifizierung und Autorisierung in Azure FarmBeats benötigen Sie eine Azure Active Directory-Anwendungsregistrierung. Dabei gilt Folgendes:

- Fall 1: Diese kann vom Installationsprogramm automatisch erstellt werden (vorausgesetzt, Sie verfügen über die erforderlichen Zugriffsberechtigungen für den Mandanten, das Abonnement und die Ressourcengruppe).
- Fall 2: Sie können diese vor der Bereitstellung von Azure FarmBeats erstellen und konfigurieren (erfordert manuelle Schritte).

**Fall 1**: Wenn Sie über die Berechtigung zum Erstellen einer AAD-App-Registrierung verfügen, können Sie diesen Schritt überspringen und die App-Registrierung vom Installationsprogramm ausführen lassen. Fahren Sie mit dem nächsten Abschnitt fort: [Vorbereiten der Datei „input.json“](#prepare-input-json-file)

Wenn Sie bereits über ein Abonnement verfügen, können Sie direkt zum nächsten Verfahren übergehen.

**Fall 2**: Diese Methode wird bevorzugt, wenn Sie keine ausreichenden Berechtigungen zum Erstellen und Konfigurieren einer Azure AD-App-Registrierung innerhalb Ihres Abonnements besitzen. Bitten Sie Ihren Administrator, das [benutzerdefinierte Skript](https://aka.ms/FarmBeatsAADScript) zu verwenden. Dadurch kann der IT-Administrator die Azure AD-App-Registrierung im Azure-Portal automatisch generieren und konfigurieren. Nach der Ausführung dieses benutzerdefinierten Skripts in der PowerShell-Umgebung muss Ihnen der IT-Administrator die resultierende Azure Active Directory-Anwendungsclient-ID und das zugehörige Kennwortgeheimnis mitteilen. Notieren Sie sich diese Werte.

Führen Sie die folgenden Schritte aus, um das Skript für die Azure AD-Anwendungsregistrierung auszuführen:

1. Laden Sie das [Skript](https://aka.ms/FarmBeatsAADScript) herunter.
2. Melden Sie sich beim Azure-Portal an, und wählen Sie Ihr Abonnement und den AD-Mandanten aus.
3. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.

    ![FarmBeats-Projekt](./media/prepare-for-deployment/navigation-bar-1.png)


4. Benutzer, die sich erstmalig anmelden, werden aufgefordert, ein Abonnement auszuwählen, um ein Speicherkonto und eine Microsoft Azure Files-Freigabe zu erstellen. Klicken Sie auf **Speicher erstellen**.
5. Erstmalige Benutzer werden aufgefordert, ihre bevorzugte Shell-Oberfläche auszuwählen – Bash oder PowerShell. Wählen Sie „PowerShell“ aus.
6. Laden Sie das Skript (aus Schritt 1) in Cloud Shell hoch, und notieren Sie sich den Speicherort der hochgeladenen Datei.

    > [!NOTE]
    > Standardmäßig wird es in Ihr Basisverzeichnis hochgeladen.

    Führen Sie das folgende Skript aus:

    ```azurepowershell-interactive
    ./create_aad_script.ps1
    ```
7. Notieren Sie sich die Azure AD-Anwendungs-ID und den geheimen Clientschlüssel. Sie müssen diese der Person mitteilen, die Azure FarmBeats bereitstellt.

### <a name="prepare-input-json-file"></a>Vorbereiten der input.json-Datei

Erstellen Sie im Rahmen der Installation wie folgt eine input.json-Datei:

```json
    {  
       "sku":"both",
       "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
       "datahubResourceGroup":"dummy-test-dh1",
       "location":"westus2",
       "datahubWebsiteName":"dummy-test-dh1",
       "acceleratorResourceGroup":" dummy-test-acc1",
       "acceleratorWebsiteName":" dummy-test-acc1",
       "sentinelUsername":"dummy-dev",
       "notificationEmailAddress":"dummy@yourorg.com",
       "updateIfExists":true
    }
```

Diese Datei ist Ihre Eingabedatei für Azure Cloud Shell und Parameter, deren Werte bei der Installation verwendet werde Alle Parameter in der JSON-Datei müssen durch entsprechende Werte ersetzt oder entfernt werden. Wenn sie entfernt werden, werden Sie vom Installationsprogramm während der Installation aufgefordert.


Überprüfen Sie die Parameter, bevor Sie die Datei vorbereiten.

|Get-Help | BESCHREIBUNG|
|--- | ---|
|sku  | Bietet die Möglichkeit, eine oder beide Komponenten von Azure FarmBeats herunterzuladen. Gibt an, welche Komponenten heruntergeladen werden sollen. Verwenden Sie „onlydatabhub“, um nur den Datenhub zu installieren. Verwenden Sie „both“, um den Datenhub und den Accelerator zu installieren.|
|subscriptionId | Das Abonnement für die Installation von Azure FarmBeats|
|datahubResourceGroup| Der Name der Ressourcengruppe für Datenhubressourcen|
|location |Der Speicherort, an dem die Ressourcen erstellt werden sollen|
|acceleratorWebsiteName |Ein eindeutiges URL-Präfix zum Benennen des Datenhubs|
|acceleratorResourceGroup  | Ein eindeutiges URL-Präfix zum Benennen Ihrer Accelerator-Website|
|datahubWebsiteName  | Ein eindeutiges URL-Präfix zum Benennen der Datenhubwebsite |
|sentinelUsername | Der Benutzername für die Anmeldung: https://scihub.copernicus.eu/dhus/#/self-registration|
|notificationEmailAddress  | Die E-Mail-Adresse, über die Benachrichtigungen zu Warnungen empfangen werden, die Sie im Datenhub konfigurieren|
|updateIfExists|[Optional] Parameter, der nur dann in der input.json-Datei enthalten sein darf, wenn Sie eine bestehende Azure FarmBeats-Instanz aktualisieren möchten. Für ein Upgrade müssen andere Details, wie z.B. die Namen der Ressourcengruppen, Standorte usw., identisch sein.|
|aadAppClientId | [**Optional**] Ein Parameter, der nur in „Input.Json“ eingefügt werden muss, wenn die Azure AD-App bereits vorhanden ist.  |
|aadAppClientSecret  | [**Optional**] Ein Parameter, der nur in „Input.Json“ eingefügt werden muss, wenn die Azure AD-App bereits vorhanden ist.|

## <a name="deploy-within-cloud-shell-browser-based-command-line"></a>Bereitstellen in der browserbasierten Befehlszeile von Cloud Shell

Während des oben gezeigten Marketplace-Workflows haben Sie eine Ressourcengruppe erstellt und den Endbenutzer-Lizenzvertrag unterzeichnet, der im Rahmen der eigentlichen Bereitstellung nochmal gelesen werden kann. Die Bereitstellung kann über Azure Cloud Shell (browserbasierte Befehlszeile) unter Verwendung der Bash-Umgebung erfolgen. Fahren Sie mit den nächsten Abschnitten fort, um die Bereitstellung über Cloud Shell durchzuführen.

> [!NOTE]
> Inaktive Cloud Shell-Sitzungen laufen nach 20 Minuten ab. Versuchen Sie, die Bereitstellung innerhalb dieses Zeitraums abzuschließen.

1. Melden Sie sich beim Azure-Portal an, und wählen Sie das gewünschte Abonnement und den AD-Mandanten aus.
2. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.
3. Wenn Sie Cloud Shell zum ersten Mal benutzen, werden Sie aufgefordert, ein Abonnement auszuwählen, um ein Speicherkonto und eine Microsoft Azure Files-Freigabe zu erstellen.
4. Klicken Sie auf **Speicher erstellen**.  

Wählen Sie die Umgebung als Bash (und nicht PowerShell) aus.

## <a name="deployment-scenario-1"></a>Bereitstellungsszenario 1

Das Installationsprogramm erstellt die Azure AD-App Registrierung (oben genannter Fall 1)

1. Kopieren Sie die folgende Vorlage, und nennen Sie sie „input.json“.  
Beispieleingabe für die JSON-Datei:

    ```json
    {  
       "sku":"both", 
       "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx", 
       "datahubResourceGroup":"dummy-test-dh1", 
       "location":"eastus2", 
       "datahubWebsiteName":"dummy-test-dh1", 
       "acceleratorResourceGroup":" dummy-test-acc1",   
       "acceleratorWebsiteName":" dummy-test-acc1", 
       "sentinelUsername":"dummy-dev", 
       "notificationEmailAddress":" dummy@microsoft.com", 
       "updateIfExists":true 
    }
    ```

2. Speichern Sie die Datei, und notieren Sie sich den Pfad (auf Ihrem lokalen Computer).
3. Wechseln Sie zu Azure Cloud Shell, und wählen Sie nach erfolgreicher Authentifizierung den Upload aus (siehe das hervorgehobene Symbol in der Abbildung unten). Laden Sie dann die Datei „input.json“ in den Cloud Shell-Speicher hoch.  

    ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-2-1.png)

4. Wechseln Sie in Cloud Shell zu Ihrem Basisverzeichnis. Standardmäßig ist „/home/<username> festgelegt.
5. Geben oder fügen Sie den folgenden Befehl in Cloud Shell ein. Achten Sie darauf, den Pfad zur Datei „input.json“ zu ändern, und drücken Sie EINGABE.

   ```bash
      wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
     Vom Installationsprogramm werden automatisch alle Abhängigkeiten heruntergeladen und die Bereitstellung erstellt. Sie werden aufgefordert, dem Endbenutzer-Lizenzvertrag (EULA) von Azure FarmBeats zuzustimmen.

     - Geben Sie „Y“ ein, wenn Sie zustimmen und mit dem nächsten Schritt fortfahren möchten.
     - Geben Sie „N“ ein, wenn Sie den Bedingungen nicht zustimmen. Dann wird die Bereitstellung beendet.

6. Anschließend werden Sie aufgefordert, ein Zugriffstoken für die Bereitstellung einzugeben. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.

    > [!NOTE]
    > Das Token läuft nach 60 Minuten ab. Wenn er abläuft, können Sie neu beginnen, indem Sie den Bereitstellungsbefehl erneut eingeben.

7. Wenn Sie dazu aufgefordert werden, geben Sie das Kennwort für Ihr Sentinel-Konto ein.
8. Nach der Überprüfung wird die Bereitstellung vom Installationsprogramm gestartet. Das kann etwa 20 Minuten dauern.
9. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, erhalten Sie die folgenden Ausgabelinks:

 - **Datenhub-URL**: Swagger-Link zum Testen von Azure FarmBeats-APIs
 - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des intelligenten Azure FarmBeats-Accelerators für landwirtschaftliche Daten.
 - **Bereitstellungsprotokolldatei**: Protokolldatei, die während der Bereitstellung erstellt wird Sie kann bei Bedarf für die Problembehandlung verwendet werden.

## <a name="deployment-scenario-2"></a>Bereitstellungsszenario 2

Zur Bereitstellung wird die vorhandene Azure Active Directory-App-Registrierung verwendet (oben genannter Fall 2)

1. Kopieren Sie die folgende JSON-Datei, die die Azure-Anwendungsclient-ID und das Kennwort in der Datei „input.json“ enthält, und speichern Sie diese.

    ```json
   {

   "sku":"both",
   "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
   "datahubResourceGroup":"dummy-test-dh1",   
   "location":"westus2",   
   "datahubWebsiteName":"dummy-test-dh1",   
   "acceleratorResourceGroup":"dummy-test-acc1",   
   "acceleratorWebsiteName":"dummy-test-acc1",   
   "sentinelUsername":"dummy-dev",
   "notificationEmailAddress":"dummyuser@org1.com",
   "updateIfExists": true,
   "aadAppClientId": "lmtlemlemylmylkmerkywmkm823",
   "aadAppClientSecret": "Kxxxqxxxxu*kxcxxx3Yxxu5xx/db[xxx"

   }
   ```

Führen Sie die restlichen Schritte aus:

2. Notieren Sie sich den Pfad zur Datei „input.json“ (auf Ihrem lokalen Computer).
3. Wechseln Sie noch einmal zu Azure Cloud Shell, und wählen Sie nach erfolgreicher Authentifizierung die Schaltfläche zum Hochladen aus (siehe das hervorgehobene Symbol in der Abbildung unten). Laden Sie dann die Datei „input.json“ in den Cloud Shell-Speicher hoch.

    ![FarmBeats-Projekt](./media/prepare-for-deployment/bash-2-1.png)

4. Wechseln Sie in Cloud Shell zu Ihrem Basisverzeichnis. Standardmäßig ist „/home/<username> festgelegt.
5. Geben oder fügen Sie den folgenden Befehl in Cloud Shell ein. Achten Sie darauf, den Pfad zur Datei „input.json“ zu ändern, und drücken Sie EINGABE.

    ```bash
    wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```

Folgen Sie den Anweisungen auf dem Bildschirm.

6. Vom Skript werden automatisch alle Abhängigkeiten heruntergeladen und die Bereitstellung erstellt.
7. Sie werden aufgefordert, den Endbenutzer-Lizenzvertrag (EULA) von Azure FarmBeats zu lesen und ihm zuzustimmen.

    - Geben Sie „Y“ ein, wenn Sie zustimmen und mit dem nächsten Schritt fortfahren möchten.
    - Geben Sie „N“ ein, wenn Sie den Bedingungen nicht zustimmen. Dann wird die Bereitstellung beendet.

8. Sie werden aufgefordert, ein Zugriffstoken für die Bereitstellung einzugeben. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.
9. Nach der Überprüfung wird die Ressourcenerstellung vom Installationsprogramm nun gestartet. Das kann etwa 20 Minuten dauern. Die Sitzung in der Cloud Shell muss während dieser Zeit aktiv bleiben.
10. Nachdem die Bereitstellung erfolgreich durchgeführt wurde, erhalten Sie die folgenden Ausgabelinks:

 - **Datenhub-URL**: Swagger-Link zum Testen von Azure FarmBeats-APIs
 - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des Azure FarmBeats-Accelerators.
 - **Bereitstellungsprotokolldatei**: Protokolldatei, die während der Bereitstellung erstellt wird. Sie kann bei Bedarf für die Problembehandlung verwendet werden.

Falls Probleme auftreten, informieren Sie sich unter [Problembehandlung](troubleshoot-project-farmbeats.md).


## <a name="validate-deployment"></a>Überprüfen der Bereitstellung

### <a name="data-hub"></a>Datenhub

Nachdem die Installation des Datenhubs abgeschlossen ist, erhalten Sie die URL für den Zugriff auf die Azure FarmBeats-APIs über die Swagger-Oberfläche im folgenden Format: https://\<Ihr-Datenhub-Websitename>.azurewebsites.net/swagger

1. Um sich über Swagger anzumelden, kopieren Sie die URL und fügen sie in den Browser ein.
2. Melden Sie sich mit den Anmeldeinformationen für das Azure-Portal an.
3. Integritätstest (optional)

     - Sie können sich mit dem Datenhublink, den Sie nach einer erfolgreichen Bereitstellung als Ausgabe erhalten haben, erfolgreich beim Swagger-Portal anmelden.
     - Get-API für erweiterte Typen – Wählen Sie „Try it out/Execute“ aus.
     - Sie sollten Code 200 als Serverantwort und keine Ausnahme wie „403 – Nicht autorisierter Benutzer“ erhalten.

### <a name="accelerator"></a>Accelerator

Nachdem die Installation des Accelerators abgeschlossen ist, erhalten Sie die URL für den Zugriff auf die Azure FarmBeats-Benutzeroberfläche im folgenden Format: https://\<Accelerator-Websitename>.azurewebsites.net

1. Um sich vom Accelerator aus anzumelden, kopieren Sie die URL und fügen sie in den Browser ein.
2. Melden Sie sich mit den Anmeldeinformationen für das Azure-Portal an.

## <a name="upgrade"></a>Upgrade

Die Upgradeschritte sind mit den Schritten für die Erstinstallation vergleichbar. Folgen Sie diesen Schritten:

1. Melden Sie sich beim Azure-Portal an, und wählen Sie das gewünschte Abonnement und den AD-Mandanten aus.
2. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.

   ![FarmBeats-Projekt](./media/prepare-for-deployment/navigation-bar-1.png)

3. Wählen Sie aus dem Dropdownmenü auf der linken Seite der Shell die Umgebung als „Bash“ aus.
4. Nehmen Sie nur unbedingt erforderliche Änderungen an der Datei „input.json“ vor, und laden Sie diese in Azure Cloud Shell hoch. Beispielsweise können Sie Ihre E-Mail-Adresse für die Benachrichtigung aktualisieren, die Sie empfangen möchten.
5. Laden Sie die input.json-Datei in Azure Cloud Shell hoch.
6. Geben oder fügen Sie die folgenden beiden Befehl in Cloud Shell ein. Stellen Sie sicher, dass Sie den Pfad zur input.json-Datei ändern, und drücken Sie die Eingabetaste.

    ```bash
    wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
Folgen Sie den Anweisungen auf dem Bildschirm:

7. Die erforderlichen Eingaben werden zur Laufzeit automatisch vom Installationsprogramm angefordert:
8. Geben Sie ein Zugriffstoken für die Bereitstellung ein. Kopieren Sie den generierten Code, und melden Sie sich mit Ihren Azure-Anmeldeinformationen bei https://microsoft.com/devicelogin an.
9. Sentinel-Kennwort
10. Nach der Überprüfung wird nun die Ressourcenerstellung vom Installationsprogramm gestartet. Das kann etwa 20 Minuten dauern.
11. Nachdem die Bereitstellung erfolgreich abgeschlossen wurde, erhalten Sie die folgenden Ausgabelinks:
 - **Datenhub-URL**: Swagger-Link zum Testen von Azure FarmBeats-APIs
 - **Accelerator-URL**: Benutzeroberfläche zum Erkunden des Azure FarmBeats-Accelerators.
 - **Bereitstellungsprotokolldatei**: In dieser Datei werden Protokolle während der Bereitstellung gespeichert. Sie kann für die Problembehandlung verwendet werden.

> [!NOTE]
> Notieren Sie sich die oben genannten Werte für die spätere Verwendung.


## <a name="uninstall"></a>Deinstallieren

Derzeit wird keine automatische Deinstallation von Azure FarmBeats durch das Installationsprogramm unterstützt. Um den Datenhub oder den Accelerator zu entfernen, löschen Sie im Azure-Portal die Ressourcengruppe, in der diese Komponenten installiert sind, oder löschen Sie die Ressourcen manuell.

Wenn Sie den Datenhub und Accelerator beispielsweise in zwei verschiedenen Ressourcengruppen bereitgestellt haben, löschen Sie diese Ressourcengruppen wie folgt:

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie oben rechts Ihr Konto aus, und wechseln Sie zum gewünschten Azure AD-Mandanten, in dem Sie Azure FarmBeats bereitstellen möchten.

   > [!NOTE]
   > Der Datenhub ist erforderlich, damit der Accelerator ordnungsgemäß funktioniert. Es wird davon abgeraten, den Datenhub ohne den Accelerator zu deinstallieren.

3. Wählen Sie „Ressourcengruppen“ aus, und geben Sie den Namen der Datenhub- oder Accelerator-Ressourcengruppe ein, die Sie löschen möchten.
4. Klicken Sie auf den Namen der Ressourcengruppe. Geben Sie den Namen zur Sicherheit nochmals ein, und wählen Sie „Löschen“ aus, um die Ressourcengruppe und alle zugrunde liegenden Ressourcen zu entfernen.
5. Alternativ können Sie jede Ressource manuell löschen. Dies wird jedoch nicht empfohlen.
7. Wechseln Sie zum Löschen/Deinstallieren des Datenhubs direkt zur Ressourcengruppe in Azure, und löschen Sie die Ressourcengruppe dort.

## <a name="next-steps"></a>Nächste Schritte

Sie haben Azure FarmBeats bereitgestellt. Erfahren Sie jetzt, wie Sie [landwirtschaftliche Betriebe erstellen](manage-farms.md#create-farms).
