---
title: Bereitstellen eines Verwaltungstools – Azure
description: Hier erfahren Sie, wie Sie ein Benutzeroberflächentool zum Verwalten von Windows Virtual Desktop-Ressourcen installieren.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 11/09/2019
ms.author: helohr
ms.openlocfilehash: ad0c67cea6a5a9b487cd47aa7c10d10da1438050
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74384283"
---
# <a name="tutorial-deploy-a-management-tool"></a>Tutorial: Bereitstellen eines Verwaltungstools

Das Verwaltungstool umfasst eine Benutzeroberfläche zum Verwalten von Microsoft Virtual Desktop-Ressourcen. In diesem Tutorial erfahren Sie, wie Sie das Verwaltungstool bereitstellen und eine Verbindung damit herstellen.

>[!NOTE]
>Diese Anweisungen beziehen sich auf eine spezielle Konfiguration für Windows Virtual Desktop, die mit den in Ihrer Organisation vorhandenen Prozessen verwendet werden kann.

## <a name="important-considerations"></a>Wichtige Hinweise

Da für die App die Einwilligung für die Interaktion mit Windows Virtual Desktop erforderlich ist, unterstützt dieses Tool keine Business-to-Business-Szenarien (B2B). Das Verwaltungstool muss für jedes Abonnement des Azure Active Directory-Mandanten (AAD) separat bereitgestellt werden.

Dieses Verwaltungstool ist ein Beispiel. Microsoft stellt wichtige Sicherheits- und Qualitätsupdates zur Verfügung. Der [Quellcode steht auf GitHub zur Verfügung](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy). Kunden und Partnern wird empfohlen, das Tool an ihre Geschäftsanforderungen anzupassen.

Die folgenden Browser sind mit dem Verwaltungstool kompatibel:
- Google Chrome 68 oder höher
- Microsoft Edge 40.15063 oder höher
- Mozilla Firefox 52.0 oder höher
- Safari 10 oder höher (nur macOS)

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Benötigte Komponenten zum Ausführen der Azure Resource Manager-Vorlage

Vor dem Bereitstellen der Azure Resource Manager-Vorlage benötigen Sie einen Azure Active Directory-Benutzer für die Bereitstellung der Verwaltungsbenutzeroberfläche. Für diesen Benutzer gilt Folgendes:

- Der Benutzer muss Azure Multi-Factor Authentication (MFA) deaktiviert haben.
- Er muss über die Berechtigung zum Erstellen von Ressourcen in Ihrem Azure-Abonnement verfügen.
- Er muss über die Berechtigung zum Erstellen einer Azure AD-Anwendung verfügen. Gehen Sie folgendermaßen vor, um zu überprüfen, ob der Benutzer über die [erforderlichen Berechtigungen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions) verfügt.

Nach der Bereitstellung der Azure Resource Manager-Vorlage sollten Sie die Verwaltungsbenutzeroberfläche zur Überprüfung starten. Für diesen Benutzer gilt Folgendes:
- Er muss über eine Rollenzuweisung zum Anzeigen oder Bearbeiten Ihres Windows Virtual Desktop-Mandanten verfügen.

## <a name="run-the-azure-resource-manager-template-to-provision-the-management-ui"></a>Ausführen der Azure Resource Manager-Vorlage zum Bereitstellen der Verwaltungsbenutzeroberfläche

Stellen Sie zunächst sicher, dass eine Einwilligung für die Server- und Client-Apps vorliegt, indem Sie zur [Windows Virtual Desktop-Seite für die Einwilligung](https://rdweb.wvd.microsoft.com) für die dargestellte Azure Active Directory-Instanz (AAD) wechseln.

Gehen Sie wie folgt vor, um die Azure Ressource Manager-Vorlage bereitzustellen:

1. Wechseln Sie zur GitHub-Seite [Azure RDS-Templates](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy).
2. Stellen Sie die Vorlage in Azure bereit.
    - Wenn Sie die Bereitstellung unter einem Enterprise-Abonnement durchführen, scrollen Sie nach unten, und wählen Sie **Bereitstellung in Azure** aus. Siehe dazu [Anleitung für Vorlagenparameter](#guidance-for-template-parameters).
    - Gehen Sie für die Bereitstellung in Azure folgendermaßen vor, wenn Sie die Bereitstellung unter einem Cloud Solution Provider-Abonnement durchführen:
        1. Scrollen Sie nach unten, klicken Sie mit der rechten Maustaste auf **In Azure bereitstellen**, und wählen Sie dann **Link-Adresse kopieren**.
        2. Öffnen Sie einen Text-Editor, z. B. Editor, und fügen Sie den Link ein.
        3. Geben Sie direkt nach <https://portal.azure.com/> und vor dem Hashtag (#) ein at-Zeichen (@) gefolgt vom Namen der Mandantendomäne ein. Beispiel für das Format: <https://portal.azure.com/@Contoso.onmicrosoft.com#create/>.
        4. Melden Sie sich am Azure-Portal als Benutzer mit den Berechtigungen „Administrator/Mitwirkender“ am Cloud Solution Provider-Abonnement an.
        5. Fügen Sie den Link, den Sie in den Text-Editor kopiert haben, in die Adressleiste ein.

### <a name="guidance-for-template-parameters"></a>Anleitung für Vorlagenparameter
Die Parameter zum Konfigurieren des Tools werden wie folgt eingegeben:

- Wählen Sie für den **isServicePrincipal**-Parameter die Option **false** aus.
- Geben Sie als Anmeldeinformationen Ihre Azure Active Directory-Anmeldeinformationen ein, wobei Sie die mehrstufige Authentifizierung deaktivieren. Diese Anmeldeinformationen werden für Ihre Anmeldung bei Azure verwendet und um die Ressourcen für die Azure AD-Anwendung und die Azure-Web-App zu erstellen. Weitere Informationen finden Sie unter [Benötigte Komponenten zum Ausführen der Azure Resource Manager-Vorlage](#what-you-need-to-run-the-azure-resource-manager-template).
- Verwenden Sie für **applicationName** einen eindeutigen Namen für die App, die in Ihrer Azure Active Directory-Instanz registriert wird. Dieser Name wird auch für die Web-App-URL verwendet. Sie können beispielsweise einen Namen wie „Apr3UX“ verwenden.

## <a name="provide-consent-for-the-management-tool"></a>Erteilen der Einwilligung für das Verwaltungstool

Nach der Ausführung der GitHub-Azure Resource Manager-Vorlage wird im Azure-Portal eine Ressourcengruppe mit zwei App Services und einem App Service-Plan angezeigt.

Bevor Sie sich anmelden und das Verwaltungstool verwenden, müssen Sie die Einwilligung für die neue Azure Active Directory-Anwendung erteilen, die mit dem Verwaltungstool verknüpft ist. Durch die erteilte Einwilligung kann das Verwaltungstool Windows Virtual Desktop-Verwaltungsaufrufe im Namen des Benutzers senden, der bei dem Tool angemeldet ist.

![Screenshot: Bereitgestellte Berechtigungen nach erfolgter Einwilligung für das Benutzeroberflächen-Verwaltungstool](media/management-ui-delegated-permissions.png)

Um zu ermitteln, welchen Benutzer Sie für die Anmeldung bei dem Tool verwenden können, öffnen Sie die [Seite für die Benutzereinstellungen Ihrer Azure Active Directory-Instanz](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/), und notieren Sie den Wert für **Benutzer können Apps den Zugriff auf Unternehmensdaten in ihrem Namen gestatten**.

![Screenshot, in dem angezeigt wird, ob Benutzer die Einwilligung für Anwendungen nur für ihren Benutzer erteilen können.](media/management-ui-user-consent-allowed.png)

- Wenn der Wert auf **Ja** festgelegt ist, können Sie sich mit einem Benutzerkonto bei Azure Active Directory anmelden und die Einwilligung nur für diesen Benutzer erteilen. Wenn Sie sich jedoch später mit einem anderen Benutzerkonto beim Verwaltungstool anmelden, müssen Sie die gleiche Einwilligung erneut erteilen.
- Wenn der Wert auf **Nein** festgelegt ist, müssen Sie sich als globaler Administrator bei Azure Active Directory anmelden und eine Administratoreinwilligung für alle Benutzer im Verzeichnis erteilen. Für keine anderen Benutzer wird eine Zustimmungsaufforderung angezeigt.


Nachdem Sie entschieden haben, welchen Benutzer Sie für die Erteilung der Einwilligung verwenden, gehen Sie folgendermaßen vor, um die Einwilligung für das Tool zu erteilen:

1. Wechseln Sie zu Ihren Azure-Ressourcen, wählen Sie die Azure App Services-Ressource mit dem Namen aus, den Sie in der Vorlage angegeben haben (z. B. Apr3UX), und navigieren Sie zu der damit verknüpften URL, z. B. <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Melden Sie sich mit dem entsprechenden Azure Active Directory-Benutzerkonto an.
3. Wenn Sie sich als globaler Administrator authentifiziert haben, können Sie nun das Kontrollkästchen**Zustimmung im Namen Ihrer Organisation** aktivieren. Wählen Sie **Akzeptieren** aus, um die Einwilligung zu erteilen.
   
   ![Screenshot: Vollständige Einwilligungsseite, die der Benutzer oder Administrator anzeigt](media/management-ui-consent-page.png)

Damit gelangen Sie nun zum Verwaltungstool.

## <a name="use-the-management-tool"></a>Verwenden des Verwaltungstools

Nach der Erteilung der Einwilligung für die Organisation oder einen bestimmten Benutzer können Sie jederzeit auf das Verwaltungstool zugreifen.

Führen Sie die folgenden Schritte aus, um das Tool zu starten:

1. Wählen Sie die Azure App Services-Ressource mit dem Namen aus, den Sie in der Vorlage angegeben haben (z. B. Apr3UX), und navigieren Sie zu der damit verknüpften URL, z. B <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Melden Sie sich mit Ihren Windows Virtual Desktop-Anmeldeinformationen an.
3. Wählen Sie bei der Aufforderung zum Auswählen einer Mandantengruppe in der Dropdownliste den Eintrag **Default Tenant Group** (Standardmandantengruppe) aus.
4. Wenn Sie eine Standardmandantengruppe auswählen, sollte rechts in Ihrem Fenster ein Menü angezeigt werden. Suchen Sie in diesem Menü nach dem Namen Ihrer Mandantengruppe, und wählen Sie sie aus.

> [!NOTE]
> Wenn Sie eine benutzerdefinierte Mandantengruppe festgelegt haben, geben Sie den Namen manuell ein, anstatt eine Auswahl in der Dropdownliste zu treffen.

## <a name="report-issues"></a>Melden von Problemen

Falls Probleme mit dem Verwaltungstool oder mit anderen Windows Virtual Desktop-Tools auftreten, sollten Sie die Anleitung unter [ARM-Vorlagen für Remotedesktopdienste](https://github.com/Azure/RDS-Templates/blob/master/README.md) befolgen, um diese auf GitHub zu melden.

## <a name="next-steps"></a>Nächste Schritte

Sie haben erfahren, wie Sie das Verwaltungstool bereitstellen und eine Verbindung damit herstellen. Im nächsten Schritt erfahren Sie nun, wie Sie mithilfe von Azure Service Health Dienstprobleme und Integritätsempfehlungen überwachen.

> [!div class="nextstepaction"]
> [Tutorial: Einrichten von Dienstwarnungen](./set-up-service-alerts.md)
