---
title: Einrichten einer Instanz und der Authentifizierung (CLI)
titleSuffix: Azure Digital Twins
description: Informationen zum Einrichten einer Instanz des Azure Digital Twins-Diensts mithilfe der CLI
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 287ee62acf3a078c4b47803060f61c9dd4134ab7
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408159"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-cli"></a>Einrichten einer Azure Digital Twins-Instanz und der Authentifizierung (CLI)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Dieser Artikel behandelt die Schritte zum **Einrichten einer neuen Azure Digital Twins-Instanz**, einschließlich des Erstellens der Instanz und des Einrichtens der Authentifizierung. Nachdem Sie diesen Artikel durchgearbeitet haben, verfügen Sie über eine Azure Digital Twins-Instanz, die für die Programmierung bereitsteht.

In der vorliegenden Version dieses Artikels werden diese Schritte manuell nacheinander mithilfe der CLI durchlaufen.
* Wenn Sie diese Schritte manuell mithilfe des Azure-Portals durchlaufen möchten, finden Sie weitere Informationen in der Portal-Version dieses Artikels: [*Verwenden Einrichten einer Instanz und der Authentifizierung (Portal)* ](how-to-set-up-instance-portal.md).
* Ein Beispiel zur Ausführung einer automatisierten Einrichtung mit einem Bereitstellungsskript finden Sie in der Skriptversion dieses Artikels: [*Verwenden Einrichten einer Instanz und der Authentifizierung (per Skript)* ](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
[!INCLUDE [digital-twins-setup-role-cli.md](../../includes/digital-twins-setup-role-cli.md)]

## <a name="set-up-cloud-shell-session"></a>Einrichten einer Cloud Shell-Sitzung
[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Erstellen der Azure Digital Twins-Instanz

In diesem Abschnitt **erstellen Sie eine neue Azure Digital Twins-Instanz** mithilfe des Cloud Shell-Befehls. Hierzu müssen Sie Folgendes bereitstellen:
* Eine Ressourcengruppe, in der die Instanz bereitgestellt werden soll. Wenn Sie noch nicht über eine Ressourcengruppe verfügen, können Sie diese jetzt mit dem folgenden Befehl erstellen:
    ```azurecli
    az group create --location <region> --name <name-for-your-resource-group>
    ```
* Eine Region für die Bereitstellung. Informationen zu Regionen mit Unterstützung von Azure Digital Twins finden Sie unter [*Verfügbare Produkte nach Region*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* Ein Name für Ihre Instanz. Der Name der neuen Instanz muss innerhalb der Region für Ihr Abonnement eindeutig sein (d. h., wenn Ihr Abonnement eine weitere Azure Digital Twins-Instanz in der Region aufweist, die bereits den von Ihnen ausgewählten Namen verwendet, werden Sie aufgefordert, einen anderen Namen auszuwählen).

Verwenden Sie im folgenden Befehl diese Werte, um die Instanz zu erstellen:

```azurecli
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Wenn die Instanz erfolgreich erstellt wurde, sieht das Ergebnis in Cloud Shell in etwa wie folgt aus und gibt Informationen über die von Ihnen erstellte Ressource aus:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/create-instance.png" alt-text="Befehlsfenster mit erfolgreicher Erstellung der Ressourcengruppe und der Azure Digital Twins-Instanz":::

Beachten Sie die Angaben für *hostName*, *name* und *resourceGroup* der Azure Digital Twins-Instanz in der Ausgabe. Dies sind alles wichtige Werte, die Sie möglicherweise benötigen, wenn Sie mit Ihrer Azure Digital Twins-Instanz weiterarbeiten, um die Authentifizierung und die zugehörigen Azure-Ressourcen einzurichten. Wenn andere Benutzer in der Instanz programmieren, sollten Sie ihnen diese Werte mitteilen.

> [!TIP]
> Sie können diese Eigenschaften sowie alle Eigenschaften Ihrer Instanz jederzeit durch Ausführen von `az dt show --dt-name <your-Azure-Digital-Twins-instance>` einsehen.

Die Azure Digital Twins-Instanz ist jetzt einsatzbereit. Im nächsten Schritt stellen Sie die entsprechenden Azure-Benutzerberechtigungen für die Verwaltung der Instanz zur Verfügung.

## <a name="set-up-user-access-permissions"></a>Einrichten von Benutzerzugriffsberechtigungen

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Verwenden Sie den folgenden Befehl, um die Rolle zuzuweisen (dieser Vorgang muss von einem Besitzer des Azure-Abonnements ausgeführt werden):

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<Azure-AD-email-of-user-to-assign>" --role "Azure Digital Twins Owner (Preview)"
```

Die Ausgabe dieses Befehls enthält Informationen zur erstellten Rollenzuweisung.

> [!TIP]
> Wenn Sie stattdessen den Fehler *400: BadRequest* erhalten, führen Sie den folgenden Befehl aus, um die *Objekt-ID* des Benutzers abzurufen:
> ```azurecli
> az ad user show --id <Azure-AD-email-of-user-to-assign> --query objectId
> ```
> Wiederholen Sie dann den Befehl zur Rollenzuweisung unter Verwendung der *Objekt-ID* des Benutzers anstelle seiner E-Mail-Adresse.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

Sie verfügen nun über eine einsatzbereite Azure Digital Twins-Instanz und haben Berechtigungen zugewiesen, um diese zu verwalten. Im nächsten Schritt richten Sie die Berechtigungen für eine Client-App ein, um darauf zuzugreifen.

## <a name="set-up-access-permissions-for-client-applications"></a>Einrichten von Zugriffsberechtigungen für Clientanwendungen

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Zum Erstellen einer App-Registrierung müssen Sie die Ressourcen-IDs für die Azure Digital Twins-APIs und die Baselineberechtigungen für die API angeben.

Erstellen Sie in Ihrem Arbeitsverzeichnis eine neue Datei, und geben Sie den folgenden JSON-Codeausschnitt ein, um diese Details zu konfigurieren: 

```json
[{
    "resourceAppId": "0b07f429-9f4b-4714-9392-cc5e8e80c8b0",
    "resourceAccess": [
     {
       "id": "4589bd03-58cb-4e6c-b17f-b580e39652f8",
       "type": "Scope"
     }
    ]
}]
``` 

Speichern Sie diese Datei unter _**manifest.json**_.

> [!NOTE] 
> Es gibt einige Stellen, an denen die benutzerfreundliche, lesbare Zeichenfolge `https://digitaltwins.azure.net` für die App-ID der Azure Digital Twins-Ressource anstelle der GUID `0b07f429-9f4b-4714-9392-cc5e8e80c8b0` verwendet werden kann. So wird in vielen Beispielen in dieser Dokumentation die Authentifizierung bei der MSAL-Bibliothek beschrieben, für die die benutzerfreundliche Zeichenfolge verwendet werden kann. Bei diesem Schritt beim Erstellen der App-Registrierung ist jedoch wie oben dargestellt das GUID-Format der ID erforderlich. 

Als Nächstes laden Sie diese Datei in Cloud Shell hoch. Klicken Sie im Cloud Shell-Fenster auf das Symbol „Dateien hochladen/herunterladen“, und wählen Sie „Hochladen“ aus.

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-upload.png" alt-text="Cloud Shell-Fenster: Auswahl der Option „Hochladen“":::
Navigieren Sie zu der eben erstellten Datei *manifest.json*, und wählen Sie „Öffnen“ aus.

Führen Sie den folgenden Befehl aus, um eine App-Registrierung zu erstellen (und ersetzen Sie dabei die Platzhalter):

```azurecli
az ad app create --display-name <name-for-your-app> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

Im Folgenden finden Sie einen Auszug der Ausgabe dieses Befehls, der Informationen über die von Ihnen erstellte Registrierung anzeigt:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/new-app-registration.png" alt-text="Cloud Shell-Ausgabe der neuen Azure AD-App-Registrierung":::

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

Vergewissern Sie sich zunächst, dass die Einstellungen der hochgeladenen Datei *manifest.json* bei der Registrierung ordnungsgemäß festgelegt wurden. Wählen Sie hierzu auf der Menüleiste *Manifest* aus, um den Manifestcode der App-Registrierung anzuzeigen. Scrollen Sie zum Ende des Codefensters, und suchen Sie unter `requiredResourceAccess` nach den Feldern Ihrer Datei *manifest.json*:

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

### <a name="collect-important-values"></a>Erfassen wichtiger Werte

Wählen Sie nun *Übersicht* in der Menüleiste aus, um die Details der App-Registrierung anzuzeigen:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Portalansicht der wichtigen Werte für die App-Registrierung":::

Notieren Sie sich die *Anwendungs-ID (Client)* und die *Verzeichnis-ID (Mandant)* , die auf **Ihrer** Seite angezeigt werden. Diese Werte sind später zum [Authentifizieren einer Client-App mit den Azure Digital Twins-APIs](how-to-authenticate-client.md) erforderlich. Wenn Sie nicht die Person sind, die Code für solche Anwendungen schreiben wird, sollten Sie diese Werte mit der Person teilen, die derartige Anwendungen programmieren wird.

### <a name="other-possible-steps-for-your-organization"></a>Weitere mögliche Schritte für Ihre Organisation

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihre Clientanwendung mit Ihrer Instanz verbinden, indem Sie den Authentifizierungscode der Clientanwendung schreiben:
* [*Verwenden Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md)