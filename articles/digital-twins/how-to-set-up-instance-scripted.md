---
title: Einrichten einer Instanz und der Authentifizierung (per Skript)
titleSuffix: Azure Digital Twins
description: Hier erfahren Sie, wie Sie eine Instanz des Azure Digital Twins-Diensts einrichten, indem Sie ein automatisiertes Bereitstellungsskript ausführen.
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 605df0f26600f962bda7a0a0def800a91d74b022
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90562977"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-scripted"></a>Einrichten einer Azure Digital Twins-Instanz und -Authentifizierung (per Skript)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Dieser Artikel beschreibt die Schritte zum **Einrichten einer neuen Azure Digital Twins-Instanz**, einschließlich des Erstellens der Instanz und des Einrichtens der Authentifizierung. Nachdem Sie die Schritte in diesem Artikel durchgeführt haben, verfügen Sie über eine Azure Digital Twins-Instanz, die zum Programmieren bereitsteht.

Bei dieser Version dieses Artikels werden die Schritte durchgeführt, indem ein [Beispiel für ein **automatisiertes Bereitstellungsskript**](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/) zum Optimieren des Prozesses ausgeführt wird. 
* Die manuellen CLI-Schritte, die das Skript im Hintergrund durchläuft, finden Sie in der CLI-Version dieses Artikels: [*Verwenden Einrichten einer Instanz und Authentifizierung (CLI)* ](how-to-set-up-instance-cli.md).
* Eine Beschreibung der manuellen Schritte im Azure-Portal finden Sie in der Portalversion dieses Artikels: [*Verwenden Einrichten einer Instanz und Authentifizierung (Portal)* ](how-to-set-up-instance-portal.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="run-the-deployment-script"></a>Ausführen des Bereitstellungsskripts

In diesem Artikel wird ein Azure Digital Twins-Codebeispiel verwendet, um eine Azure Digital Twins-Instanz und die erforderliche Authentifizierung halbautomatisch bereitzustellen. Er kann zudem als Startpunkt zum Schreiben Ihrer eigenen Skriptinteraktionen dienen.

Das Beispielskript ist in PowerShell geschrieben. Es ist Teil der [Azure Digital Twins-Beispiele](https://docs.microsoft.com/samples/azure-samples/digital-twins-samples/digital-twins-samples/), die Sie auf Ihren Computer herunterladen können, indem Sie dem Beispiellink folgen und unterhalb des Titels auf die Schaltfläche *ZIP herunterladen* klicken.

Im heruntergeladenen Ordner mit den Beispielen befindet sich das Bereitstellungsskript unter _Azure_Digital_Twins_samples.zip > scripts > **deploy.ps1**_.

Im Folgenden sind die Schritte zum Ausführen des Bereitstellungsskripts in Cloud Shell beschrieben.
1. Öffnen Sie in Ihrem Browser ein [Azure Cloud Shell](https://shell.azure.com/)-Fenster. Melden Sie sich mithilfe des folgenden Befehls an:
    ```azurecli
    az login
    ```
    Die CLI öffnet Ihren Standardbrowser, sofern sie dazu in der Lage ist, und lädt eine Azure-Anmeldeseite. Öffnen Sie andernfalls die Browserseite *https://aka.ms/devicelogin* , und geben Sie den in Ihrem Terminal angezeigten Autorisierungscode ein.
 
2. Nachdem Sie sich angemeldet haben, klicken Sie in der Symbolleiste des Cloud Shell-Fensters auf das Symbol „Dateien hochladen/herunterladen“ und dann auf „Hochladen“.

    :::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-upload.png" alt-text="Cloud Shell-Fenster: Auswahl der Option „Hochladen“":::

    Navigieren Sie zur Datei _**deploy.ps1**_ auf Ihrem Computer, und klicken Sie auf „Öffnen“. Dadurch wird die Datei in Cloud Shell hochgeladen, sodass Sie diese im Cloud Shell-Fenster ausführen können.

3. Führen Sie das Skript aus, indem Sie im Cloud Shell-Fenster den Befehl `./deploy.ps1` absenden. Während das Skript die automatisierten Einrichtungsschritte durchläuft, werden Sie dazu aufgefordert, die folgenden Werte zu übergeben:
    * Für die Instanz: die *Abonnement-ID* Ihres zu verwendenden Azure-Abonnements.
    * Für die Instanz: einen *Speicherort*, an dem Sie die Instanz bereitstellen möchten. Informationen zu Regionen mit Unterstützung von Azure Digital Twins finden Sie unter [*Verfügbare Produkte nach Region*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
    * Für die Instanz: einen Namen für die *Ressourcengruppe*. Sie können eine vorhandene Ressourcengruppe verwenden oder einen neuen Namen einer zu erstellenden Ressourcengruppe eingeben.
    * Für die Instanz: einen *Namen* für Ihre Azure Digital Twins-Instanz. Der Name der neuen Instanz muss innerhalb der Region für Ihr Abonnement eindeutig sein (d. h., wenn Ihr Abonnement eine weitere Azure Digital Twins-Instanz in der Region aufweist, die bereits den von Ihnen ausgewählten Namen verwendet, werden Sie aufgefordert, einen anderen Namen auszuwählen).
    * Für die App-Registrierung: einen *Azure AD-Anwendungsanzeigenamen*, der der Registrierung zugeordnet werden soll. Über diese App-Registrierung konfigurieren Sie die Zugriffsberechtigungen für die [Azure Digital Twins-APIs](how-to-use-apis-sdks.md). Ihre Client-App wird später für die App-Registrierung authentifiziert. Daraufhin werden ihr die konfigurierten Zugriffsberechtigungen für die APIs erteilt.
    * Für die App-Registrierung: eine *Antwort-URL der Azure AD-Anwendung* für die Azure AD-Anwendung. Verwenden Sie `http://localhost`. Das Skript richtet einen *URI für einen öffentlichen Client bzw. einen nativen URI (mobil und Desktop)* für sie ein.

Dieses Skript erstellt eine Azure Digital Twins-Instanz, weist Ihrem Azure-Benutzer die Rolle *Azure Digital Twins-Besitzer (Vorschau)* auf der Instanz zu und richtet eine Azure AD-App-Registrierung für die Verwendung durch Ihre Client-App ein.

>[!NOTE]
>Zurzeit gibt es ein **bekanntes Problem** beim skriptgesteuerten Setup, bei dem einige Benutzer (insbesondere Benutzer mit persönlichen [Microsoft-Konten (MSAs)](https://account.microsoft.com/account)) möglicherweise feststellen, dass die **Rollenzuweisung für _Azure Digital Twins-Besitzer (Vorschauversion)_ nicht erstellt wurde**.
>
>Sie können die Rollenzuweisung über den Abschnitt [*Überprüfen der Benutzerrollenzuweisung*](#verify-user-role-assignment) weiter unten in diesem Artikel überprüfen und bei Bedarf die Rollenzuweisung manuell über das [Azure-Portal](how-to-set-up-instance-portal.md#set-up-user-access-permissions) oder mithilfe der [Befehlszeilenschnittstelle](how-to-set-up-instance-cli.md#set-up-user-access-permissions) manuell einrichten.
>
>Weitere Informationen zu diesem Problem finden Sie unter [*Problembehandlung: Bekannte Probleme in Azure Digital Twins*](troubleshoot-known-issues.md#missing-role-assignment-after-scripted-setup).

Im Folgenden finden Sie einen Auszug aus dem Ausgabeprotokoll des Skripts:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png" alt-text="Cloud Shell-Fenster mit Eingabe- und Ausgabeprotokoll beim Ausführen des Bereitstellungsskripts" lightbox="media/how-to-set-up-instance/cloud-shell/deployment-script-output.png":::

Wird das Skript erfolgreich abgeschlossen, zeigt die Ausgabe am Ende `Deployment completed successfully` an. Andernfalls beheben Sie das in der Fehlermeldung erwähnte Problem, und führen Sie das Skript erneut aus. Es überspringt die bereits abgeschlossenen Schritte und führt das Anfordern von Eingaben ab dem Punkt fort, an dem die Ausführung zuvor unterbrochen wurde.

Nach Abschluss des Skripts verfügen Sie über eine einsatzbereite Azure Digital Twins-Instanz mit Berechtigungen zur Verwaltung dieser. Zudem haben Sie Berechtigungen für den Zugriff durch eine Client-App eingerichtet.

> [!NOTE]
> Das Skript weist die erforderliche Verwaltungsrolle innerhalb von Azure Digital Twins (*Azure Digital Twins-Besitzer (Vorschau)* ) momentan demselben Benutzer zu, der das Skript in Cloud Shell ausführt. Falls erforderlich, können Sie diese Rolle nun über das Azure-Portal ([Anweisungen](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) oder die CLI ([Anweisungen](how-to-set-up-instance-cli.md#set-up-user-access-permissions)) einem anderen Benutzer zuweisen, der für die Verwaltung der Instanz zuständig ist.

## <a name="collect-important-values"></a>Erfassen wichtiger Werte

Einige wichtige Werte der vom Skript eingerichteten Ressourcen könnten für Ihre weitere Arbeit mit der Azure Digital Twins-Instanz erforderlich sein. In diesem Abschnitt verwenden Sie das [Azure-Portal](https://portal.azure.com), um diese Werte zu erfassen. Sie sollten diese an einem sicheren Ort speichern oder zu diesem Abschnitt zurückkehren, sobald Sie diese später benötigen.

Wenn andere Benutzer in der Instanz programmieren, sollten Sie diese Werte auch mit ihnen teilen.

### <a name="collect-instance-values"></a>Erfassen von Instanzwerten

Im [Azure-Portal](https://portal.azure.com) finden Sie Ihre Azure Digital Twins-Instanz, indem Sie in der Suchleiste des Portals nach dem Namen Ihrer Instanz suchen.

Wenn Sie darauf klicken, wird die *Übersichtsseite* der Instanz geöffnet. Notieren Sie sich ihren *Namen*, die *Ressourcengruppe* und den *Hostnamen*. Diese benötigen Sie möglicherweise später zum Identifizieren der Instanz und zum Herstellen einer Verbindung mit dieser.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Hervorheben der wichtigen Werte auf der Übersichtsseite der Instanz":::

### <a name="collect-app-registration-values"></a>Erfassen von Werten der App-Registrierung 

Zwei wichtige Werte der App-Registrierung werden später benötigt, um [Client-App-Authentifizierungscode für die Azure Digital Twins-APIs zu schreiben](how-to-authenticate-client.md). 

Sie finden diese, indem Sie [diesem Link](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) folgen, um zur Übersichtsseite der Azure AD-App-Registrierung im Azure-Portal zu navigieren. Auf dieser Seite werden alle App-Registrierungen angezeigt, die in Ihrem Abonnement erstellt wurden.

Die neu erstellte App-Registrierung sollte in dieser Liste angezeigt werden. Klicken Sie darauf, um die Details anzuzeigen:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Portalansicht der wichtigen Werte für die App-Registrierung":::

Notieren Sie sich die *Anwendungs-ID (Client)* und die *Verzeichnis-ID (Mandant)* , die auf **Ihrer** Seite angezeigt werden. Wenn Sie nicht die Person sind, die Code für Clientanwendungen schreiben wird, sollten Sie diese Werte mit der Person teilen, die dafür zuständig sein wird.

## <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Die vom Skript erstellten Ressourcen und eingerichteten Berechtigungen können Sie über das [Azure-Portal](https://portal.azure.com) überprüfen.

### <a name="verify-instance"></a>Überprüfen der Instanz

Über die [Azure Digital Twins-Seite](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.DigitalTwins%2FdigitalTwinsInstances) im Azure-Portal können Sie überprüfen, ob Ihre Instanz erstellt wurde. Auf dieser Seite sind alle Ihre Azure Digital Twins-Instanzen aufgelistet. Suchen Sie in der Liste nach dem Namen Ihrer neu erstellten Instanz.

### <a name="verify-user-role-assignment"></a>Überprüfen der Benutzerrollenzuweisung

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

> [!NOTE]
> Beachten Sie, dass das Skript diese erforderliche Rolle derzeit demselben Benutzer zuweist, der auch das Skript in Cloud Shell ausführt. Falls erforderlich, können Sie diese Rolle nun über das Azure-Portal ([Anweisungen](how-to-set-up-instance-portal.md#set-up-user-access-permissions)) oder die CLI ([Anweisungen](how-to-set-up-instance-cli.md#set-up-user-access-permissions)) einem anderen Benutzer zuweisen, der für die Verwaltung der Instanz zuständig ist.
>
> Sie können auch das Portal oder die CLI verwenden, um Ihre eigene Rollenzuweisung zu wiederholen, wenn beim Skriptsetup Probleme aufgetreten sind.

### <a name="verify-app-registration"></a>Überprüfen der App-Registrierung

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

Vergewissern Sie sich zunächst, dass die Azure Digital Twins-Berechtigungseinstellungen für die Registrierung ordnungsgemäß festgelegt wurden. Wählen Sie hierzu auf der Menüleiste *Manifest* aus, um den Manifestcode der App-Registrierung anzuzeigen. Scrollen Sie zum unteren Bereich des Codefensters, und suchen Sie nach diesen Feldern unter `requiredResourceAccess`. Die Werte sollten den Werten im nachstehenden Screenshot entsprechen:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

## <a name="other-possible-steps-for-your-organization"></a>Weitere mögliche Schritte für Ihre Organisation

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Nächste Schritte

Testen Sie einzelne REST-API-Aufrufe für Ihre Instanz mithilfe der Befehle der Azure Digital Twins-CLI: 
* [az dt reference](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt?view=azure-cli-latest)
* [*Gewusst wie: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle*](how-to-use-cli.md)

Unter folgendem Link erfahren Sie außerdem, wie Sie eine Verbindung zwischen Ihrer Clientanwendung und Ihrer Instanz herstellen, indem Sie den Authentifizierungscode der Client-App schreiben:
* [*Verwenden Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md)
