---
title: Einrichten einer Instanz und der Authentifizierung (Portal)
titleSuffix: Azure Digital Twins
description: Informationen zum Einrichten einer Instanz des Azure Digital Twins-Diensts mithilfe des Azure-Portals
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 1f7486f1080c0fbb25b1be6ab70bb647a546ceca
ms.sourcegitcommit: 3bf69c5a5be48c2c7a979373895b4fae3f746757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2020
ms.locfileid: "88234988"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Einrichten einer Azure Digital Twins-Instanz und der Authentifizierung (Portal)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Dieser Artikel behandelt die Schritte zum **Einrichten einer neuen Azure Digital Twins-Instanz**, einschließlich des Erstellens der Instanz und des Einrichtens der Authentifizierung. Nachdem Sie diesen Artikel durchgearbeitet haben, verfügen Sie über eine Azure Digital Twins-Instanz, die für die Programmierung bereitsteht.

In dieser Version dieses Artikels werden diese Schritte manuell nacheinander mithilfe des Azure-Portals durchlaufen. Das Azure-Portal ist eine webbasierte, zentrale Konsole, die eine Alternative zu Befehlszeilentools darstellt.
* Wenn Sie diese Schritte manuell mithilfe der CLI durchlaufen möchten, finden Sie weitere Informationen in der CLI-Version dieses Artikels: [*Vorgehensweise: Einrichten einer Instanz und der Authentifizierung (CLI)* ](how-to-set-up-instance-cli.md).
* Ein Beispiel zur Ausführung eines automatisierten Setups mit einem Bereitstellungsskript finden Sie in der Skriptversion dieses Artikels: [*Verwenden Einrichten einer Instanz und der Authentifizierung (über ein Skript)* ](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Erstellen der Azure Digital Twins-Instanz

In diesem Abschnitt **erstellen Sie eine neue Azure Digital Twins-Instanz** über das [Azure-Portal](https://ms.portal.azure.com/). Navigieren Sie zum Portal, und melden Sie sich mit Ihren Anmeldeinformationen an.

Sobald Sie im Portal sind, wählen Sie im Menü auf der Homepage der Azure-Dienste _Ressource erstellen_ aus.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Auswählen von „Ressource erstellen“ auf der Homepage des Azure-Portals":::

Suchen Sie im Suchfeld nach *Azure Digital Twins*, und wählen Sie den Dienst **Azure Digital Twins (Vorschau)** aus den Ergebnissen aus. Wählen Sie die Schaltfläche _Erstellen_ aus, um eine neue Instanz des Diensts zu erstellen.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Auswählen von „Erstellen“ auf der Seite „Azure Digital Twins-Dienst“":::

Geben Sie auf der folgenden Seite *Ressource erstellen* die unten angegebenen Werte ein:
* **Abonnement**: Das Azure-Abonnement, das Sie verwenden.
  - **Ressourcengruppe**: Eine Ressourcengruppe, in der die Instanz bereitgestellt werden soll. Wenn Sie nicht bereits eine vorhandene Ressourcengruppe in Erwägung ziehen, können Sie hier eine Ressourcengruppe erstellen, indem Sie den Link *Neu erstellen* auswählen und einen Namen für eine neue Ressourcengruppe eingeben
* **Standort**: Eine für Azure Digital Twins aktivierte Region für die Bereitstellung. Weitere Informationen zur regionalen Unterstützung finden Sie unter [*Verfügbare Azure-Produkte nach Region (Azure Digital Twins)* ](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Ressourcenname**: Ein Name für Ihre Azure Digital Twins-Instanz. Der Name der neuen Instanz muss innerhalb der Region für Ihr Abonnement eindeutig sein (d. h. wenn Ihr Abonnement eine weitere Azure Digital Twins-Instanz in der Region aufweist, die bereits den von Ihnen ausgewählten Namen verwendet, werden Sie aufgefordert, einen anderen Namen auszuwählen).

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Eingeben der beschriebenen Werte zum Erstellen einer Azure Digital Twins-Ressource":::

Wenn Sie fertig sind, wählen Sie _Überprüfen und erstellen_ aus. Dadurch gelangen Sie zu einer Zusammenfassungsseite, auf der Sie die eingegebenen Instanzdetails überprüfen und _Erstellen_ auswählen können. 

### <a name="verify-success-and-collect-important-values"></a>Überprüfen des Erfolgs und Erfassen wichtiger Werte

Nachdem Sie *Erstellen* ausgewählt haben, können Sie den Status der Bereitstellung Ihrer Instanz in Ihren Azure-Benachrichtigungen auf der Symbolleiste des Portals anzeigen. In der Benachrichtigung wird angezeigt, wenn die Bereitstellung erfolgreich war, und Sie können die Schaltfläche _Zu Ressource wechseln_ auswählen, um Ihre erstellte Instanz anzuzeigen.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Ansicht von Azure-Benachrichtigungen mit einer erfolgreichen Bereitstellung und Hervorhebung der Schaltfläche „Zu Ressource wechseln“":::

Wenn bei der Bereitstellung ein Fehler auftritt, wird in der Benachrichtigung alternativ die Ursache angegeben. Beachten Sie die Ratschläge in der Fehlermeldung, und versuchen Sie dann erneut, die Instanz zu erstellen.

>[!TIP]
>Nachdem die Instanz erstellt wurde, können Sie jederzeit zu ihrer Seite zurückkehren, indem Sie in der Suchleiste des Azure-Portals nach dem Namen Ihrer Instanz suchen.

Notieren Sie sich von der Seite *Übersicht* der Instanz den *Namen*, die *Ressourcengruppe* und den *Hostnamen*. Dies sind alles wichtige Werte, die Sie möglicherweise benötigen, wenn Sie mit Ihrer Azure Digital Twins-Instanz weiterarbeiten. Wenn andere Benutzer in der Instanz programmieren, sollten Sie diese Werte mit ihnen teilen.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Hervorheben der wichtigen Werte von der Übersichtsseite der Instanz":::

Die Azure Digital Twins-Instanz ist jetzt einsatzbereit. Im nächsten Schritt stellen Sie die entsprechenden Azure-Benutzerberechtigungen für die Verwaltung der Instanz zur Verfügung.

## <a name="set-up-user-access-permissions"></a>Einrichten von Benutzerzugriffsberechtigungen

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Öffnen Sie zunächst die Seite für Ihre Azure Digital Twins-Instanz im Azure-Portal. Wählen Sie im Menü der Instanz *Zugriffssteuerung (IAM)* aus. Wählen Sie unter *Rollenzuweisung hinzufügen* die Schaltfläche *Hinzufügen* aus.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="Auswählen der Option zum Hinzufügen einer Rollenzuweisung auf der Seite „Zugriffssteuerung (IAM)“":::

Geben Sie auf der folgenden Seite *Rollenzuweisung hinzufügen* die Werte ein (dies muss ein Benutzer im Azure-Abonnement mit [ausreichenden Berechtigungen](#prerequisites-permission-requirements) erledigen):
* **Rolle**: Wählen Sie im Dropdownmenü *Azure Digital Twins-Besitzer (Vorschau)* aus.
* **Zugriff zuweisen zu**: Wählen Sie im Dropdownmenü *Azure AD-Benutzer, -Gruppe oder -Dienstprinzipal* aus.
* **Select**: Suchen Sie nach dem Namen oder der E-Mail-Adresse des Benutzers, der zugewiesen werden soll. Wenn Sie das Ergebnis auswählen, wird der Benutzer im Abschnitt *Ausgewählte Mitglieder* angezeigt.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Eingeben der aufgelisteten Felder im Dialogfeld „Rollenzuweisung hinzufügen“":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Nachdem Sie die Werte eingegeben haben, klicken Sie auf die Schaltfläche *Speichern*.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Sie können die Rollenzuweisung, die Sie eingerichtet haben, unter *Zugriffssteuerung (IAM) > Rollenzuweisungen* anzeigen. Der Benutzer sollte in der Liste mit der Rolle *Azure Digital Twins-Besitzer (Vorschau)* angezeigt werden. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Anzeigen der Rollenzuweisungen für eine Azure Digital Twins-Instanz im Azure-Portal":::

Sie verfügen nun über eine einsatzbereite Azure Digital Twins-Instanz und haben Berechtigungen zugewiesen, um diese zu verwalten. Im nächsten Schritt richten Sie die Berechtigungen für eine Client-App ein, um darauf zuzugreifen.

## <a name="set-up-access-permissions-for-client-applications"></a>Einrichten von Zugriffsberechtigungen für Clientanwendungen

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Navigieren Sie zunächst zu [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) im Azure-Portal (Sie können diesen Link verwenden oder nach diesem Eintrag über die Suchleiste des Portals suchen). Wählen Sie *App-Registrierungen* aus dem Dienstmenü und dann *+ Neue Registrierung* aus.

:::image type="content" source="media/how-to-set-up-instance/portal/new-registration.png" alt-text="Ansicht der Azure AD-Dienstseite im Azure-Portal mit hervorgehobener Menüoption „App-Registrierungen“ und Schaltfläche „+ Neue Registrierung“":::

Geben Sie auf der folgenden Seite *Anwendung registrieren* die angeforderten Werte ein:
* **Name**: Ein Azure AD-Anwendungsanzeigename, der der Registrierung zugeordnet werden soll.
* **Unterstützte Kontotypen**: Wählen Sie *Nur Konten in diesem Organisationsverzeichnis (nur Standardverzeichnis – einzelner Mandant)* aus.
* **Umleitungs-URI**: Eine *Azure AD-Antwort-URL der Anwendung* für die Azure AD-Anwendung. Sie können `http://localhost` verwenden.

Wenn Sie fertig sind, klicken Sie auf die Schaltfläche *Registrieren*.

:::image type="content" source="media/how-to-set-up-instance/portal/register-an-application.png" alt-text="Ansicht der Seite „Anwendung registrieren“ mit den eingegebenen beschriebenen Werten":::

Wenn die Einrichtung der Registrierung abgeschlossen ist, leitet das Portal Sie zur Detailseite weiter.

### <a name="provide-azure-digital-twins-api-permission"></a>Bereitstellen der API-Berechtigung für Azure Digital Twins

Konfigurieren Sie nun die App-Registrierung, die Sie erstellt haben, mit Baselineberechtigungen für die Azure Digital Twins-APIs.

Wählen Sie auf der Portalseite für Ihre App-Registrierung im Menü *API-Berechtigungen* aus. Klicken Sie auf der folgenden Berechtigungsseite auf die Schaltfläche *+ Berechtigung* hinzufügen.

:::image type="content" source="media/how-to-set-up-instance/portal/add-permission.png" alt-text="Ansicht der App-Registrierung im Azure-Portal mit hervorgehobener Menüoption „App-Berechtigungen“ und Schaltfläche „+ Berechtigung hinzufügen“":::

Wechseln Sie auf der folgenden Seite *API-Berechtigungen anfordern* zur Registerkarte *Von meiner Organisation verwendete APIs*, und suchen Sie dann nach *azure digital twins*. Wählen Sie *Azure Digital Twins* aus den Suchergebnissen aus, um mit dem Zuweisen von Berechtigungen für die Azure Digital Twins-APIs fortzufahren.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-1.png" alt-text="Ansicht des Suchergebnisses „Azure Digital Twins“ der Seite „API-Berechtigungen anfordern“":::

>[!NOTE]
> Wenn Sie noch über eine bestehende Azure Digital Twins-Instanz aus der vorherigen öffentlichen Vorschau des Diensts (vor Juli 2020) in Ihrem Abonnement verfügen, müssen Sie stattdessen nach *Azure Smart Spaces Service* suchen. Dies ist ein älterer Name für dieselbe Gruppe von APIs und wird Ihre Erfahrung über diesen Schritt hinaus nicht verändern.

Nun wählen Sie die Berechtigungen aus, die für diese APIs erteilt werden sollen. Erweitern Sie die Berechtigung **Lesen (1)** , und aktivieren Sie das Kontrollkästchen *Read.Write*, um diesem App-Registrierungsleser und -writer Berechtigungen zu erteilen.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-2.png" alt-text="Ansicht der Seite „API-Berechtigungen anfordern“ mit ausgewählten Berechtigungen „Read.Write“ für die Azure Digital Twins-APIs":::

Klicken Sie auf *Berechtigungen hinzufügen*, wenn Sie fertig sind.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Vergewissern Sie sich, dass auf der Seite *API-Berechtigungen* nun ein Eintrag für Azure Digital Twins vorhanden ist, der die Lese-/Schreibberechtigungen widerspiegelt:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-api-permissions.png" alt-text="Portalansicht der API-Berechtigungen für die Azure AD-App-Registrierung, die „Lese-/Schreibzugriff“ für Azure Digital Twins anzeigt":::

Sie können die Verbindung mit Azure Digital Twins auch in der Datei *manifest.json* der App-Registrierung überprüfen, die automatisch mit den Informationen zu Azure Digital Twins aktualisiert wurde, als Sie die API-Berechtigungen hinzugefügt haben.

Wählen Sie hierzu *Manifest* aus dem Menü aus, um den Manifestcode der App-Registrierung anzuzeigen. Scrollen Sie zum Ende des Codefensters, und suchen Sie nach diesen Feldern unter `requiredResourceAccess`. Die Werte sollten den Werten im folgenden Screenshot entsprechen:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-manifest.png" alt-text="Portalansicht des Manifests für die Azure AD-App-Registrierung. Geschachtelt unter „requiredResourceAccess“ ist ein resourceAppId-Wert von 0b07f429-9f4b-4714-9392-cc5e8e80c8b0 und ein Wert “resourceAccess > id“ von 4589bd03-58cb-4e6c-b17f-b580e39652f8"::: vorhanden.

### <a name="collect-important-values"></a>Erfassen wichtiger Werte

Wählen Sie nun *Übersicht* in der Menüleiste aus, um die Details der App-Registrierung anzuzeigen:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Portalansicht der wichtigen Werte für die App-Registrierung":::

Notieren Sie sich die *Anwendungs-ID (Client)* und die *Verzeichnis-ID (Mandant)* , die auf **Ihrer** Seite angezeigt werden. Diese Werte sind später zum [Authentifizieren einer Client-App mit den Azure Digital Twins-APIs](how-to-authenticate-client.md) erforderlich. Wenn Sie nicht die Person sind, die Code für solche Anwendungen schreiben wird, sollten Sie diese Werte mit der Person teilen, die solche Anwendungen programmieren wird.

### <a name="other-possible-steps-for-your-organization"></a>Weitere mögliche Schritte für Ihre Organisation

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihre Clientanwendung mit Ihrer Instanz verbinden, indem Sie den Authentifizierungscode der Clientanwendung schreiben:
* [*Verwenden Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md)