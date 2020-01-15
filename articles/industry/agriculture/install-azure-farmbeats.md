---
title: Installieren von Azure FarmBeats
description: In diesem Artikel wird beschrieben, wie Sie Azure FarmBeats in Ihrem Azure-Abonnement installieren.
author: usha-rathnavel
ms.topic: article
ms.date: 12/11/2019
ms.author: usrathna
ms.openlocfilehash: d1a6bdfb38431e18eb305b223ce8ee2467804052
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475744"
---
# <a name="install-azure-farmbeats"></a>Installieren von Azure FarmBeats

In diesem Artikel wird beschrieben, wie Sie Azure FarmBeats in Ihrem Azure-Abonnement installieren.

Azure FarmBeats ist ein Business-to-Business-Angebot, das in Azure Marketplace verfügbar ist. Es ermöglicht die Aggregation von landwirtschaftlichen Datasets über mehrere Anbieter hinweg sowie die Generierung von verwertbaren Erkenntnissen. Azure FarmBeats erreicht dies, indem es Ihnen ermöglicht, Modelle der künstlichen Intelligenz (KI) oder des maschinellen Lernens (ML) auf der Basis von zusammengeführten Datasets zu erstellen. Die zwei Hauptkomponenten von Azure FarmBeats sind:

- **Datenhub:** Eine API-Ebene, die eine anbieterübergreifende Aggregation, Normalisierung und Kontextualisierung verschiedener Datasets ermöglicht, die landwirtschaftliche Daten enthalten.

- **Accelerator:** Eine Beispielwebanwendung, die auf dem Datenhub aufsetzt. Es unterstützt Ihre Modellentwicklung und -visualisierung. Der Accelerator nutzt Azure FarmBeats-APIs, um die Visualisierung von erfassten Sensordaten in Form von Diagrammen und die Visualisierung der Modellausgabe als Karten zu veranschaulichen.

## <a name="before-you-start"></a>Vorbereitung
### <a name="components-installed"></a>Installierte Komponenten

Wenn Sie Azure FarmBeats installieren, werden die folgenden Ressourcen in Ihrem Azure-Abonnement zur Verfügung gestellt:

| Installierte Azure-Ressourcen  | Azure FarmBeats-Komponente  |
|---------|---------|
| Application Insights   |      Datenhub und Accelerator      |
| App Service     |     Datenhub und Accelerator     |
| App Service-Plan   | Datenhub und Accelerator  |
| APIConnection    |  Datenhub       |
| Azure Cache for Redis       | Datenhub      |
| Azure Cosmos DB   |  Datenhub       |
| Azure Data Factory V2       |     Datenhub und Accelerator      |
| Azure Batch-Konto    | Datenhub   |
| Azure-Schlüsseltresor |  Datenhub und Accelerator        |
| Azure Maps-Konto       |     Accelerator    |
| Event Hub-Namespace    |     Datenhub      |
| Logik-App      |  Datenhub       |
| Speicherkonto      |     Datenhub und Accelerator      |
| Time Series Insights     |    Datenhub    |

### <a name="costs-incurred"></a>Angefallene Kosten

Die Kosten für Azure FarmBeats stellen eine Aggregation der Kosten der zugrunde liegenden Azure-Dienste dar. Preise zu Azure-Diensten können mit dem [Preisrechner](https://azure.microsoft.com/pricing/calculator) berechnet werden. Die tatsächlichen Kosten für die gesamte Installation variieren je nach Nutzung. Die stationären Kosten für die beiden Komponenten sind:

* Datenhub – weniger als 10 USD pro Tag
* Accelerator – weniger als 2 USD pro Tag

### <a name="regions-supported"></a>Unterstützte Regionen

Derzeit wird Azure FarmBeats in öffentlichen Cloudumgebungen in den folgenden Regionen unterstützt:
* Australien (Osten)
* USA (Mitte)
* East US
* USA (Ost) 2
* USA (Westen)
* USA, Westen 2
* Nordeuropa
* Europa, Westen
* Asien, Südosten

### <a name="time-taken"></a>Benötigte Zeit

Das gesamte Setup von Azure FarmBeats, einschließlich der Vorbereitung und Installation, wird weniger als eine Stunde dauern.

## <a name="prerequisites"></a>Voraussetzungen    

Bevor Sie mit der eigentlichen Installation von Azure FarmBeats beginnen, müssen Sie die folgenden Schritte durchführen:

### <a name="create-sentinel-account"></a>Erstellen eines Sentinel-Kontos
Ihr Azure FarmBeats-Setup ermöglicht es Ihnen, kostenlose Satellitenbilder von der [Sentinel-2](https://scihub.copernicus.eu/)-Satellitenmission der Europäischen Weltraumbehörde für Ihren landwirtschaftlichen Betrieb zu erhalten. Zum Konfigurieren dieses Setups benötigen Sie ein Sentinel-Konto.

Führen Sie folgende Schritte durch, um ein kostenloses Sentinel-Konto zu erstellen:

1. Wechseln Sie zur offiziellen [Registrierungsseite](https://scihub.copernicus.eu/dhus/#/self-registration).
2. Stellen Sie die erforderlichen Angaben (Vorname, Nachname, Benutzername, Kennwort und E-Mail-ID) bereit und füllen Sie das Formular aus.
3. Ein Link für die Überprüfung wird an die registrierte E-Mail-ID gesendet. Wählen Sie den in der E-Mail angegebenen Link und schließen Sie die Überprüfung ab.

Ihr Registrierungsprozess ist abgeschlossen, sobald Sie die Überprüfung durchgeführt haben. Notieren Sie sich Ihren **Sentinel-Benutzernamen** und Ihr **Sentinel-Kennwort**.

### <a name="decide-subscription-and-region"></a>Auswählen des Abonnements und der Region

Sie benötigen die Azure-Abonnement-ID und die Region, in der Sie Azure FarmBeats installieren möchten. Wählen Sie eine der Regionen aus, die im Abschnitt [Unterstützte Regionen](#regions-supported) aufgelistet sind.

Notieren Sie sich die **Azure-Abonnement-ID** und die **Azure-Region**.

### <a name="verify-permissions"></a>Überprüfen von Berechtigungen

Sie müssen überprüfen, ob Sie über ausreichende Rechte und Berechtigungen in dem Azure-Mandanten verfügen, in dem Sie Azure FarmBeats installieren möchten.

Sie können Ihre Zugriffsberechtigungen im Azure-Portal überprüfen, indem Sie die Anweisungen zur [rollenbasierten Zugriffssteuerung](https://docs.microsoft.com/azure/role-based-access-control/check-access) befolgen.

Zum Installieren von Azure FarmBeats benötigen Sie die folgenden Berechtigungen:
- Mandant – Lesezugriff
- Abonnement – Mitwirkender oder Besitzer
- Ressourcengruppe – Besitzer

Darüber hinaus erfordert Azure FarmBeats die Registrierung von Azure Active Directory-Anwendungen. Es gibt zwei Möglichkeiten, das erforderliche Azure AD-Setup durchzuführen:

**Fall 1**: Sie verfügen über **Schreibrechte** in dem Azure-Mandanten, in dem die Installation durchgeführt wird. Dieser Fall bedeutet, dass Sie über die erforderlichen Berechtigungen verfügen, um die AAD-App-Registrierung während der Installation dynamisch zu erstellen.

Sie können direkt zum Abschnitt [Durchführen der Marketplace-Registrierung](#complete-azure-marketplace-sign-up) wechseln.


**Fall 2**: Sie besitzen KEINE **Schreibrechte** im Azure-Mandanten. Dieser Fall tritt häufig auf, wenn Sie versuchen, Azure FarmBeats im Azure-Abonnement Ihres Unternehmens zu installieren und Ihr **Schreibzugriff** nur auf die Ressourcengruppe beschränkt ist, die Sie besitzen.
Fordern Sie in diesem Fall Ihren IT-Administrator auf, die folgenden Schritte zu befolgen, um die Registrierung der Azure AD-App im Azure-Portal automatisch zu generieren und abzuschließen.

1. Laden Sie das [AAD-App-Generatorskript](https://aka.ms/FarmBeatsAADScript) herunter und extrahieren Sie es auf Ihrem lokalen Computer.
2. Melden Sie sich beim Azure-Portal an und wählen Sie Ihr Abonnement und den Azure AD-Mandanten aus.
3. Starten Sie Cloud Shell über die obere Symbolleiste des Azure-Portals.

    ![FarmBeats-Projekt](./media/install-azure-farmbeats/navigation-bar-1.png)

4. Wählen Sie PowerShell als bevorzugte Shell-Erfahrung aus. Benutzer, die sich erstmalig anmelden, werden aufgefordert, ein Abonnement auszuwählen und ein Speicherkonto zu erstellen. Schließen Sie das Setup wie vorgegeben ab.
5. Laden Sie das Skript (aus dem ersten Schritt) in Cloud Shell hoch, und notieren Sie sich den Speicherort der hochgeladenen Datei.

    > [!NOTE]
    > Standardmäßig wird die Datei in Ihr Basisverzeichnis hochgeladen.

6. Wechseln Sie mit dem Befehl „cd“ in das Basisverzeichnis und führen Sie das folgende Skript aus:

      ```azurepowershell-interactive
            ./create_aad_script.ps1
      ```
7. Geben Sie den Namen der **Datenhub-Website** und der **Accelerator-Website** ein. Notieren Sie sich die Ausgabe des Skripts und geben Sie sie an die Person weiter, die Azure FarmBeats installiert.

Sobald Ihr IT-Administrator Ihnen die erforderlichen Details zur Verfügung stellt, notieren Sie sich die **AAD-Client-ID, den geheimen AAD-Clientschlüssel, den Namen der Datenhub-Website sowie den Namen der Accelerator-Website**.

   > [!NOTE]
   > Wenn Sie den Anweisungen aus Fall 2 folgen, vergessen Sie nicht, die AAD-Client-ID und den geheimen AAD-Clientschlüssel als separate Parameter in der [Parameterdatei](#prepare-parameters-file) hinzuzufügen.

Jetzt verfügen Sie über alle erforderlichen Informationen, um mit dem nächsten Abschnitt fortzufahren.

### <a name="complete-azure-marketplace-sign-up"></a>Vollständigen der Azure Marketplace-Registrierung

Sie müssen sich für das Azure FarmBeats Angebot im Azure Marketplace registrieren, bevor Sie mit der auf der Cloud Shell basierenden Installation beginnen können. Befolgen Sie die folgenden Schritte, um die Registrierung abzuschließen:

1.  Melden Sie sich beim Azure-Portal an. Wählen Sie oben rechts Ihr Konto aus, und wechseln Sie zum Azure AD-Mandanten, in dem Sie Azure FarmBeats installieren möchten.

2.  Wechseln Sie innerhalb des Portals zum Azure Marketplace und suchen Sie dort nach **Azure FarmBeats**.

3.  Es wird ein neues Fenster mit einer Übersicht über Azure FarmBeats angezeigt. Wählen Sie **Create** aus.

4.  Es wird ein neues Fenster angezeigt. Schließen Sie den Registrierungsvorgang ab, indem Sie das richtige Abonnement, die richtige Ressourcengruppe und den richtigen Speicherort auswählen, an dem Sie Azure FarmBeats installieren möchten.

5.  Sobald die eingegebenen Daten bestätigt sind, wählen Sie **OK** aus. Die Seite „Nutzungsbedingungen“ wird angezeigt. Überprüfen Sie die Bedingungen und wählen Sie **Erstellen** aus, um den Registrierungsvorgang abzuschließen.

Dieser Schritt schließt den Registrierungsvorgang im Azure Marketplace ab. Sie sind jetzt bereit, die Vorbereitung der Parameterdatei zu starten.

### <a name="prepare-parameters-file"></a>Vorbereiten der Parameterdatei
Der letzte Schritt in der Phase der Voraussetzungen ist die Erstellung einer JSON-Datei, die als Eingabe bei der Installation der Cloud Shell dient. Die Parameter in der JSON-Datei müssen durch entsprechende Werte ersetzt werden.

Eine JSON-Beispieldatei wird unten bereitgestellt. Laden Sie das Beispiel herunter und aktualisieren Sie die erforderlichen Angaben.

```json
{  
    "sku":"both",
    "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
    "datahubResourceGroup":"dummy-test-dh1",
    "location":"westus2",
    "datahubWebsiteName":"dummy-test-dh1",
    "acceleratorResourceGroup":"dummy-test-acc1",
    "acceleratorWebsiteName":"dummy-test-acc1",
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummy@yourorg.com",
    "updateIfExists":true
}
```

In der folgenden Parametertabelle können Sie mehr über die einzelnen Parameter erfahren.

| Parameter | value|
|--- | ---|
|sku  | Bietet dem Benutzer die Möglichkeit, sowohl Datenhub als auch Accelerator oder nur den Datenhub zu installieren. Soll nur der Datenhub installiert werden, verwenden Sie „datahub“. Sollen Datenhub und Accelerator installiert werden, verwenden Sie „both“.|
|subscriptionId | Gibt das Azure-Abonnement zur Installation von Azure FarmBeats an.|
|datahubResourceGroup| Gibt den Namen der Ressourcengruppe für Ihre Datenhub-Ressourcen an. Geben Sie hier den Namen der Ressourcengruppe ein, die Sie im Azure Marketplace erstellt haben.|
|location |Der Standort bzw. die Azure-Region, wo Sie Azure FarmBeats installieren möchten.|
|datahubWebsiteName  | Das eindeutige URL-Präfix für Ihre Datenhub-Webanwendung. |
|acceleratorResourceGroup  | Gibt den Namen der Ressourcengruppe für Ihre Accelerator-Ressourcen an.|
|acceleratorWebsiteName |Das eindeutige URL-Präfix für Ihre Accelerator-Webanwendung.|
|sentinelUsername | Der Benutzername zum Abrufen von Sentinel-Satellitenbildern.|
|notificationEmailAddress  | Die E-Mail-Adresse, über die Benachrichtigungen zu Warnungen empfangen werden sollen, die Sie in Ihrem Datenhub konfigurieren.|
|updateIfExists| (Optional) Ein Parameter, der nur dann in der Parameterdatei enthalten sein darf, wenn Sie eine bereits vorhandene Azure FarmBeats-Instanz upgraden möchten. Bei einem Upgrade müssen andere Angaben wie Ressourcengruppennamen und Standorte identisch sein.|
|aadAppClientId | (Optional) Ein Parameter, der nur dann in der Parameterdatei enthalten sein darf, wenn Sie eine vorab erstellte AAD-App verwenden. Weitere Informationen finden Sie unter Fall 2 im Abschnitt [Überprüfen von Berechtigungen](#verify-permissions). |
|aadAppClientSecret  | (Optional) Ein Parameter, der nur dann in der Parameterdatei enthalten sein darf, wenn Sie eine vorab erstellte AAD-App verwenden. Weitere Informationen finden Sie unter Fall 2 im Abschnitt [Überprüfen von Berechtigungen](#verify-permissions).|

Erstellen Sie auf der Grundlage der obigen Tabelle und der JSON-Beispieldatei Ihre JSON-Parameterdatei und speichern Sie diese auf Ihrem lokalen Computer.

## <a name="install"></a>Installieren

Die eigentliche Installation der Azure FarmBeats-Ressourcen erfolgt in der browserbasierten Befehlszeilenschnittstelle der Cloud Shell unter Verwendung der Bash-Umgebung. Gehen Sie wie im Anschluss beschrieben vor, um Azure FarmBeats zu installieren:

1. Melden Sie sich beim Azure-Portal an. Wählen Sie das Azure-Abonnement und den Mandanten aus, für den Sie Azure FarmBeats installieren möchten.
2. Starten Sie **Cloud Shell** über die obere rechte Symbolleiste des Azure-Portals.
3. Wählen Sie Bash als bevorzugte Shell-Erfahrung aus. Wählen Sie die Schaltfläche **Upload** aus (im nachfolgenden Bild hervorgehoben) und laden Sie die vorbereitete JSON-Parameterdatei hoch.

      ![FarmBeats-Projekt](./media/install-azure-farmbeats/bash-2-1.png)

4. **Kopieren** Sie den folgenden Befehl und **ersetzen Sie \<Benutzername>** durch den richtigen Wert, sodass der Befehl auf den richtigen Pfad der hochgeladenen Datei zeigt.

    ```bash
          wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
5. Führen Sie den geänderten Befehl aus, um den Installationsvorgang zu starten. Sie werden zu Folgendem aufgefordert:
 - Den **Lizenzbedingungen von Azure FarmBeats** zuzustimmen. Geben Sie „J“ ein, um mit dem nächsten Schritt fortzufahren, wenn Sie den Nutzungsbedingungen zustimmen. Geben Sie „N“ ein, um die Installation zu beenden, wenn Sie den Nutzungsbedingungen nicht zustimmen.

 - Als nächstes werden Sie aufgefordert, ein Zugriffstoken für die Installation einzugeben. Kopieren Sie den generierten Code, und melden Sie sich auf der [Seite für die Geräteanmeldung](https://microsoft.com/devicelogin) mit Ihren **Azure-Anmeldeinformationen** an.

 - Sobald die Anmeldung erfolgreich abgeschlossen ist, wird das Installationsprogramm nach dem Kennwort für Ihr Sentinel-Konto fragen. Geben Sie das **Kennwort für Ihr Sentinel-Konto** ein.

6. Die Parameterdatei wird überprüft und die Installation der Azure-Ressourcen beginnt. Die Installation dauert etwa **25 Minuten**.    
> [!NOTE]
> Inaktive Cloud Shell-Sitzungen laufen nach **20 Minuten** ab. Lassen Sie die Cloud Shell-Sitzung aktiv, während das Installationsprogramm die Azure-Ressourcen bereitstellt. Wenn für die Sitzung ein Timeout eintritt, müssen Sie den Installationsvorgang neu starten.

Nachdem die Installation abgeschlossen ist, erhalten Sie die folgenden Ausgabelinks:
* **Datenhub-URL**: Der Swagger-Link für den Zugriff auf die Datenhub-APIs.
* **Accelerator-URL**: Die Webanwendung zum Untersuchen des Azure FarmBeats-Accelerators.
* **Protokolldatei des Installationsprogramms**: Die Protokolldatei, die Details der Installation enthält. Diese Protokolldatei kann ggf. zur Problembehandlung bei der Installation verwendet werden.

Sie können die Fertigstellung Ihrer Azure FarmBeats-Installation überprüfen, indem Sie die folgenden Prüfungen durchführen:

**Datenhub**
1. Melden Sie sich unter der angegebenen Accelerator-URL (im Format **https://\<IhrDatenhub-Websitename>.azurewebsites.net/swagger**) mit Ihren Azure-Anmeldeinformationen an.
2. Es sollten die verschiedenen FarmBeats-API-Objekte angezeigt und REST-Vorgänge für die APIs durchgeführt werden können.

**Accelerator**
1. Melden Sie sich unter der angegebenen Accelerator-URL (im Format **https://\<IhrAccelerator-Websitename>.azurewebsites.net/swagger**) mit Ihren Azure-Anmeldeinformationen an.
2. Sie sollten in der Lage sein, die Accelerator-Benutzeroberfläche mit einer Option zum Erstellen von landwirtschaftlichen Betrieben in Ihrem Browser anzuzeigen.

Ihre Möglichkeit, die oben genannten Vorgänge durchzuführen, weist auf eine erfolgreiche Installation von Azure FarmBeats hin.

## <a name="upgrade"></a>Aktualisieren
In der Public Preview-Version müssen Sie, um ein Upgrade für eine vorhandene Installation von Azure FarmBeats durchzuführen, den Installationsbefehl in der Cloud Shell erneut ausführen, wobei der zusätzliche Parameter **updateIfExists** in der Parameterdatei auf **true** festgelegt wird. Informationen über den Parameter „update“ finden Sie in der letzten Zeile des folgenden JSON-Beispiels.

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
Der Befehl aktualisiert die vorhandene Azure FarmBeats-Installation auf die neueste Version und stellt Ihnen die Ausgabelinks zur Verfügung.

## <a name="uninstall"></a>Deinstallieren

Führen Sie die folgenden Schritte aus, um den Azure FarmBeats-Datenhub oder -Accelerator zu deinstallieren:

1.  Melden Sie sich beim Azure-Portal an und **löschen Sie die Ressourcengruppen**, in denen diese Komponenten installiert sind.

2.  Wechseln Sie zu Azure Active Directory und löschen Sie die mit der Azure FarmBeats-Installation verknüpfte **Azure AD-Anwendung**. Dadurch wird die Azure FarmBeats-Installation aus Ihrem Azure-Abonnement entfernt.

## <a name="next-steps"></a>Nächste Schritte
Sie haben erfahren, wie Sie Azure FarmBeats in Ihrem Azure-Abonnement installieren. Erfahren Sie, wie Sie Benutzer zu Ihrer Azure RemoteApp-Sammlung [hinzufügen](manage-users-in-azure-farmbeats.md#manage-users).
