---
title: Bereitstellen von Inhalt mithilfe von FTP/S
description: Erfahren Sie, wie Sie Ihre App mithilfe von FTP oder FTPS in Azure App Service bereitstellen. Verbessern Sie die Websitesicherheit, indem Sie das unverschlüsselte Dateiübertragungsprotokoll deaktivieren.
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.topic: article
ms.date: 02/26/2021
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: c7427a1f8f528fdf405b22c4e91941ea7a915ffa
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045801"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>Bereitstellen der App in Azure App Service mithilfe von FTP/S

In diesem Artikel erfahren Sie, wie Sie eine Web-App, das mobile App-Back-End oder eine API-App mithilfe von FTP oder FTPS in [Azure App Service](./overview.md) bereitstellen.

Der FTP/S-Endpunkt für Ihre App ist bereits aktiv. Zum Aktivieren der FTP/S-Bereitstellung ist keine Konfiguration erforderlich.

> [!NOTE]
> Die Seite **Development Center (klassisch)** im Azure-Portal, bei der es sich um die alte Bereitstellung handelt, wird im März 2021 eingestellt. Diese Änderung wirkt sich nicht auf vorhandene Bereitstellungseinstellungen in Ihrer APP aus, und Sie können die APP-Bereitstellung weiterhin auf der Seite **Bereitstellungscenter** verwalten.

## <a name="get-deployment-credentials"></a>Abrufen der Anmeldeinformationen für die Bereitstellung

1. Befolgen Sie die Anweisungen unter [Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung.](deploy-configure-credentials.md), um die Anmeldeinformationen für den Anwendungsbereich zu kopieren oder die Anmeldeinformationen für den Benutzerbereich festzulegen. Sie können mit beiden Arten von Anmeldeinformationen eine Verbindung mit dem FTP/S-Endpunkt der App herstellen.

1. Erstellen Sie den FTP-Benutzernamen abhängig von der Auswahl des Bereichs der Anmeldeinformationen im folgenden Format:

    | Anwendungsbereich | Benutzerbereich |
    | - | - |
    |`<app-name>\$<app-name>`|`<app-name>\<deployment-user>`|

    ---

    In App Service wird der FTP/S-Endpunkt von Apps gemeinsam genutzt. Da die Anmeldeinformationen für den Benutzerbereich nicht mit einer bestimmten Ressource verknüpft sind, müssen Sie dem Benutzernamen im Benutzerbereich den App-Namen voranstellen, wie oben gezeigt.

## <a name="get-ftps-endpoint"></a>Abrufen des FTP/S-Endpunkts
    
# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Kopieren Sie den **FTPS-Endpunkt** auf derselben Verwaltungsseite für die App, auf der Sie die Anmeldeinformationen für die Bereitstellung kopiert haben (**Bereitstellungscenter** > **FTPS-Anmeldeinformationen**).

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Führen Sie den Befehl [az webapp deployment list-publishing-profiles](/cli/azure/webapp/deployment#az_webapp_deployment_list_publishing_profiles) aus. Im folgenden Beispiel wird ein [JMES-Pfad](https://jmespath.org/) zum Extrahieren der FTP/S-Endpunkte aus der Ausgabe verwendet.

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app-name> --resource-group <group-name> --query "[?ends_with(profileName, 'FTP')].{profileName: profileName, publishUrl: publishUrl}"
```

Jede App verfügt über zwei FTP/S-Endpunkte. Der eine verfügt über Lese-/Schreibzugriff, und der andere ist schreibgeschützt (`profileName` enthält `ReadOnly`) und wird für Datenwiederherstellungsszenarien verwendet. Zum Bereitstellen von Dateien mit FTP kopieren Sie die URL des Endpunkts mit Lese-/Schreibzugriff.

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Führen Sie den Befehl [Get-AzWebAppPublishingProfile](/powershell/module/az.websites/get-azwebapppublishingprofile) aus. Im folgenden Beispiel wird der FTP/S-Endpunkt aus der XML-Ausgabe extrahiert.

```azurepowershell-interactive
$xml = [xml](Get-AzWebAppPublishingProfile -Name <app-name> -ResourceGroupName <group-name> -OutputFile null)
$xml.SelectNodes("//publishProfile[@publishMethod=`"FTP`"]/@publishUrl").value
```

-----

## <a name="deploy-files-to-azure"></a>Bereitstellen von Dateien in Azure

1. Verwenden Sie in Ihrem FTP-Client (z.B. [Visual Studio](https://www.visualstudio.com/vs/community/), [Cyberduck](https://cyberduck.io/) oder [WinSCP](https://winscp.net/index.php)) die gesammelten Verbindungsinformationen, um eine Verbindung mit Ihrer App herzustellen.
2. Kopieren Sie Ihre Dateien und die entsprechende Verzeichnisstruktur in das Verzeichnis [ **/site/wwwroot**](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) in Azure (bzw. für WebJobs in das Verzeichnis **/site/wwwroot/App_Data/Jobs/** ).
3. Navigieren Sie zur URL Ihrer App, um sicherzustellen, dass die Anwendung richtig ausgeführt wird. 

> [!NOTE] 
> Im Gegensatz zu [Git-basierten Bereitstellungen](deploy-local-git.md) und [ZIP-Bereitstellungen](deploy-zip.md) unterstützt die FTP-Bereitstellung keine Buildautomatisierung, z. B.: 
>
> - Wiederherstellungen der Abhängigkeit (z.B. NuGet-, NPM-, PIP- oder Composer-Automatisierungen)
> - Kompilierung von .NET-Binärdateien
> - Generierung von „web.config“ (hier sehen Sie ein [Node.js-Beispiel](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps))
> 
> Generieren Sie diese erforderlichen Dateien auf Ihrem lokalen Computer manuell, und stellen Sie sie zusammen mit Ihrer App bereit.
>

## <a name="enforce-ftps"></a>Erzwingen von FTPS

Aus Sicherheitsgründen sollten Sie nur FTP über TLS/SSL zulassen. Sie können auch FTP und FTPS deaktivieren, wenn Sie keine FTP-Bereitstellung verwenden.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Wählen Sie auf der Ressourcenseite Ihrer App im [Azure-Portal](https://portal.azure.com) im linken Navigationsbereich **Konfiguration** > **Allgemeine Einstellungen** aus.

2. Um unverschlüsselte FTP-Verbindungen zu deaktivieren, aktivieren Sie **Nur FTPS** unter **FTP-Zustand**. Um FTP und FTPS vollständig zu deaktivieren, wählen Sie **Deaktiviert** aus. Klicken Sie abschließend auf **Speichern**. Bei Verwendung von **Nur FTPS** müssen Sie die Verwendung von TLS 1.2 oder höher erzwingen, indem Sie zum Blatt mit den **TLS/SSL-Einstellungen** Ihrer Web-App navigieren. TLS 1.0 und 1.1 werden für **Nur FTPS** nicht unterstützt.

    ![Deaktivieren von FTP/S](./media/app-service-deploy-ftp/disable-ftp.png)

# <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/cli)

Führen Sie den Befehl [az webapp config set](/cli/azure/webapp/deployment#az_webapp_deployment_list_publishing_profiles) mit dem Argument `--ftps-state` aus.

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <group-name> --ftps-state FtpsOnly
```

Mögliche Werte für `--ftps-state` sind `AllAllowed` (FTP und FTPS aktiviert), `Disabled` (FTP und FTPS deaktiviert) und `FtpsOnly` (nur FTPS).

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/powershell)

Führen Sie den Befehl [Set-AzWebApp](/powershell/module/az.websites/set-azwebapp) mit dem Parameter `-FtpsState` aus.

```azurepowershell-interactive
Set-AzWebApp -Name <app-name> -ResourceGroupName <group-name> -FtpsState FtpsOnly
```

Mögliche Werte für `--ftps-state` sind `AllAllowed` (FTP und FTPS aktiviert), `Disabled` (FTP und FTPS deaktiviert) und `FtpsOnly` (nur FTPS).

-----

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>Problembehandlung bei der FTP-Bereitstellung

- [Wie behebe ich Probleme bei der FTP-Bereitstellung?](#how-can-i-troubleshoot-ftp-deployment)
- [Ich kann meinen Code nicht mit FTP erreichen und veröffentlichen. Wie kann ich das Problem beheben?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Wie kann ich in Azure App Service über den passiven Modus eine Verbindung mit FTP herstellen?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

#### <a name="how-can-i-troubleshoot-ftp-deployment"></a>Wie behebe ich Probleme bei der FTP-Bereitstellung?

Der erste Schritt in der Problembehandlung bei der FTP-Bereitstellung besteht darin, ein Bereitstellungsproblem von einem Laufzeitproblem der Anwendung zu trennen.

Bereitstellungsprobleme führen in der Regel dazu, dass für Ihre Anwendung keine oder die falschen Dateien bereitgestellt werden. Sie können dies durch Untersuchen Ihrer FTP-Bereitstellung oder Auswählen einer alternativen Bereitstellungsmethode (z.B. Quellcodeverwaltung) beheben.

Laufzeitprobleme der Anwendung führen in der Regel dazu, dass die richtigen Dateien für Ihre App bereitgestellt werden, die App jedoch ein falsches Verhalten aufweist. Sie können dies beheben, indem Sie sich auf das Codeverhalten zur Laufzeit konzentrieren und spezifische Fehlerpfade untersuchen.

Weitere Informationen zur Ermittlung, ob es sich um ein Bereitstellungs- oder Laufzeitproblem handelt, finden Sie unter [Probleme mit Bereitstellung oder Laufzeit](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues).

#### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>Ich kann meinen Code nicht mit FTP erreichen und veröffentlichen. Wie kann ich das Problem beheben?
Vergewissern Sie sich, dass Sie den richtigen [Hostnamen](#get-ftps-endpoint) und die richtigen [Anmeldeinformationen](#get-deployment-credentials) eingegeben haben. Stellen Sie auch sicher, dass die folgenden FTP-Ports auf dem Computer nicht durch eine Firewall blockiert werden:

- Port für FTP-Kontrollverbindung:
- Port für FTP-Datenverbindung: 989, 10001–10300
 
#### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Wie kann ich in Azure App Service über den passiven Modus eine Verbindung mit FTP herstellen?
Azure App Service unterstützt das Herstellen einer Verbindung im aktiven und passiven Modus. Der passive Modus wird bevorzugt, da sich Bereitstellungscomputer in der Regel hinter einer Firewall (im Betriebssystem oder als Teil eines Heim- oder Firmennetzwerks) befinden. Weitere Informationen finden Sie in einem [Beispiel in der WinSCP-Dokumentation](https://winscp.net/docs/ui_login_connection). 

## <a name="more-resources"></a>Weitere Ressourcen

* [Lokale Git-Bereitstellung in Azure App Service](deploy-local-git.md)
* [Anmeldeinformationen für die Azure App Service-Bereitstellung](deploy-configure-credentials.md)
* [Beispiel: Erstellen einer Web-App und Bereitstellen von Dateien über FTP (Azure CLI)](./scripts/cli-deploy-ftp.md)
* [Beispiel: Hochladen von Dateien in eine Web-App mithilfe von FTP (PowerShell)](./scripts/powershell-deploy-ftp.md)
