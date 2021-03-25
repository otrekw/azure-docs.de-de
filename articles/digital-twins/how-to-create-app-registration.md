---
title: Erstellen einer App-Registrierung
titleSuffix: Azure Digital Twins
description: Erfahren Sie, wie eine Azure AD-App-Registrierung als Authentifizierungsoption für Client-Apps erstellt wird.
author: baanders
ms.author: baanders
ms.date: 10/13/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: acb5457f10c54a741a738dd8a1008e703b0f23b0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102051020"
---
# <a name="create-an-app-registration-to-use-with-azure-digital-twins"></a>Erstellen einer App-Registrierung für die Verwendung mit Azure Digital Twins

Bei der Verwendung einer Azure Digital Twins-Instanz ist es üblich, über Clientanwendungen mit dieser Instanz zu interagieren – etwa mit einer benutzerdefinierten Client-App oder mit einem Beispiel wie [Azure Digital Twins-Explorer](quickstart-adt-explorer.md). Diese Anwendungen müssen sich bei Azure Digital Twins authentifizieren, um damit zu interagieren, und einige der [Authentifizierungsmechanismen](how-to-authenticate-client.md), die Apps verwenden können, umfassen eine [Azure AD](../active-directory/fundamentals/active-directory-whatis.md)-**App-Registrierung** (Azure Active Directory).

Dies ist nicht für alle Authentifizierungsszenarien erforderlich. Wenn Sie jedoch eine Authentifizierungsstrategie oder ein Codebeispiel verwenden, die bzw. das eine App-Registrierung erfordert, einschließlich einer **Client-ID** und **Manten-ID**, zeigt Ihnen dieser Artikel, wie Sie eine solche einrichten.

## <a name="using-azure-ad-app-registrations"></a>Verwenden von Azure AD-App-Registrierungen

[Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) ist der cloudbasierte Identitäts- und Zugriffsverwaltungsdienst von Microsoft. Das Einrichten einer **App-Registrierung** in Azure AD ist eine Möglichkeit, einer Client-App Zugriff auf Azure Digital Twins zu gewähren.

Über diese App-Registrierung konfigurieren Sie die Zugriffsberechtigungen für die [Azure Digital Twins-APIs](how-to-use-apis-sdks.md). Ihre Client-App kann sich später für die App-Registrierung mit den **Client- und Mandanten-ID-Werten** der Registrierung authentifizieren. Daraufhin werden ihr die konfigurierten Zugriffsberechtigungen für die APIs erteilt.

>[!TIP]
> Es empfiehlt sich, bei Bedarf jeweils eine neue App-Registrierung einzurichten. Wenn Sie diesen Vorgang nur ein Mal ausführen möchten, können Sie *alternativ* eine einzelne App-Registrierung einrichten, die von allen Szenarien gemeinsam verwendet wird.

## <a name="create-the-registration"></a>Erstellen der Registrierung

Navigieren Sie zunächst zu [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) im Azure-Portal (Sie können diesen Link verwenden oder nach diesem Eintrag über die Suchleiste des Portals suchen). Wählen Sie *App-Registrierungen* aus dem Dienstmenü und dann *+ Neue Registrierung* aus.

:::image type="content" source="media/how-to-create-app-registration/new-registration.png" alt-text="Ansicht der Azure AD-Dienstseite im Azure-Portal mit hervorgehobener Menüoption „App-Registrierungen“ und Schaltfläche „+ Neue Registrierung“":::

Geben Sie auf der folgenden Seite *Anwendung registrieren* die angeforderten Werte ein:
* **Name**: Ein Azure AD-Anwendungsanzeigename, der der Registrierung zugeordnet werden soll.
* **Unterstützte Kontotypen**: Wählen Sie *Nur Konten in diesem Organisationsverzeichnis (nur Standardverzeichnis – einzelner Mandant)* aus.
* **Umleitungs-URI**: Eine *Azure AD-Antwort-URL der Anwendung* für die Azure AD-Anwendung. Fügen Sie einen URI vom Typ *Öffentlicher Client/nativ (mobil und Desktop)* für `http://localhost` hinzu.

Wenn Sie fertig sind, klicken Sie auf die Schaltfläche *Registrieren*.

:::image type="content" source="media/how-to-create-app-registration/register-an-application.png" alt-text="Ansicht der Seite „Anwendung registrieren“ mit den eingegebenen beschriebenen Werten":::

Wenn die Einrichtung der Registrierung abgeschlossen ist, leitet das Portal Sie zur Detailseite weiter.

## <a name="collect-client-id-and-tenant-id"></a>Erfassen der Client-ID und Mandanten-ID

Erfassen Sie anschließend einige wichtige Werte zur App -Registrierung von der zugehörigen Detailseite:

:::image type="content" source="media/how-to-create-app-registration/app-important-values.png" alt-text="Portalansicht der wichtigen Werte für die App-Registrierung":::

Notieren Sie sich die _**Anwendungs-ID (Client)**_ und die _**Verzeichnis-ID (Mandant)**_, die auf **Ihrer** Seite angezeigt werden. Dies sind die Werte, die eine Client-App benötigt, um diese Registrierung für die Authentifizierung mit Azure Digital Twins zu verwenden.

## <a name="provide-azure-digital-twins-api-permission"></a>Bereitstellen der API-Berechtigung für Azure Digital Twins

Konfigurieren Sie nun die App-Registrierung, die Sie erstellt haben, mit Baselineberechtigungen für die Azure Digital Twins-APIs.

Wählen Sie auf der Portalseite für Ihre App-Registrierung im Menü *API-Berechtigungen* aus. Klicken Sie auf der folgenden Berechtigungsseite auf die Schaltfläche *+ Berechtigung* hinzufügen.

:::image type="content" source="media/how-to-create-app-registration/add-permission.png" alt-text="Ansicht der App-Registrierung im Azure-Portal mit hervorgehobener Menüoption „App-Berechtigungen“ und Schaltfläche „+ Berechtigung hinzufügen“":::

Wechseln Sie auf der folgenden Seite *API-Berechtigungen anfordern* zur Registerkarte *Von meiner Organisation verwendete APIs*, und suchen Sie dann nach *azure digital twins*. Wählen Sie _**Azure Digital Twins**_ aus den Suchergebnissen aus, um mit dem Zuweisen von Berechtigungen für die Azure Digital Twins-APIs fortzufahren.

:::image type="content" source="media/how-to-create-app-registration/request-api-permissions-1.png" alt-text="Ansicht des Suchergebnisses „Azure Digital Twins“ der Seite „API-Berechtigungen anfordern“ mit der Anwendungs-ID (Client) 0b07f429-9f4b-4714-9392-cc5e8e80c8b0.":::

>[!NOTE]
> Wenn Ihr Abonnement noch über eine vorhandene Azure Digital Twins-Instanz aus der vorherigen öffentlichen Vorschau des Diensts (vor Juli 2020) verfügt, müssen Sie stattdessen nach _**Azure Smart Spaces Service**_ suchen und diese Option auswählen. Dies ist ein älterer Name für die gleiche Sammlung von APIs (beachten Sie, dass die *Anwendungs-ID (Client)* mit der ID im Screenshot oben identisch ist), und Ihre Vorgehensweise ändert sich über diesen Schritt hinaus nicht mehr.
> :::image type="content" source="media/how-to-create-app-registration/request-api-permissions-1-smart-spaces.png" alt-text="Ansicht des Suchergebnisses „Azure Smart Spaces Service“ der Seite „API-Berechtigungen anfordern“":::

Nun wählen Sie die Berechtigungen aus, die für diese APIs erteilt werden sollen. Erweitern Sie die Berechtigung **Lesen (1)** , und aktivieren Sie das Kontrollkästchen *Read.Write*, um diesem App-Registrierungsleser und -writer Berechtigungen zu erteilen.

:::image type="content" source="media/how-to-create-app-registration/request-api-permissions-2.png" alt-text="Ansicht der Seite „API-Berechtigungen anfordern“ mit ausgewählten Berechtigungen „Read.Write“ für die Azure Digital Twins-APIs":::

Klicken Sie auf *Berechtigungen hinzufügen*, wenn Sie fertig sind.

### <a name="verify-success"></a>Überprüfen des erfolgreichen Abschlusses

Vergewissern Sie sich, dass auf der Seite *API-Berechtigungen* nun ein Eintrag für Azure Digital Twins vorhanden ist, der Lese-/Schreibberechtigungen widerspiegelt:

:::image type="content" source="media/how-to-create-app-registration/verify-api-permissions.png" alt-text="Portalansicht der API-Berechtigungen für die Azure AD-App-Registrierung, die „Lese-/Schreibzugriff“ für Azure Digital Twins anzeigt":::

Sie können die Verbindung mit Azure Digital Twins auch in der Datei *manifest.json* der App-Registrierung überprüfen, die automatisch mit den Informationen zu Azure Digital Twins aktualisiert wurde, als Sie die API-Berechtigungen hinzugefügt haben.

Wählen Sie hierzu *Manifest* aus dem Menü aus, um den Manifestcode der App-Registrierung anzuzeigen. Scrollen Sie zum Ende des Codefensters, und suchen Sie nach diesen Feldern unter `requiredResourceAccess`. Die Werte sollten den Werten im folgenden Screenshot entsprechen:

:::image type="content" source="media/how-to-create-app-registration/verify-manifest.png" alt-text="Portalansicht des Manifests für die Azure AD-App-Registrierung. Geschachtelt unter „requiredResourceAccess“ ist ein resourceAppId-Wert von 0b07f429-9f4b-4714-9392-cc5e8e80c8b0 und ein Wert “resourceAccess > id“ von 4589bd03-58cb-4e6c-b17f-b580e39652f8"::: vorhanden.

Wenn diese Werte fehlen, wiederholen Sie die Schritte im [Abschnitt zum Hinzufügen der API-Berechtigung](#provide-azure-digital-twins-api-permission).

## <a name="other-possible-steps-for-your-organization"></a>Weitere mögliche Schritte für Ihre Organisation

Ihre Organisation erfordert möglicherweise zusätzliche Aktionen von Abonnementbesitzern/Administratoren, um eine App-Registrierung erfolgreich einzurichten. Welche Schritte erforderlich sind, hängt von den spezifischen Einstellungen Ihrer Organisation ab.

Im Folgenden finden Sie einige häufig vorkommende Aktivitäten, die ein Besitzer/Administrator für das Abonnement möglicherweise ausführen muss. Diese und andere Vorgänge können über die Seite [*Azure AD-App-Registrierungen*](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) im Azure-Portal ausgeführt werden.
* Erteilen Sie eine Administratoreinwilligung für die App-Registrierung. In Ihrer Organisation ist möglicherweise die Einstellung *Administratoreinwilligung erforderlich* global in Azure AD für alle App-Registrierungen in Ihrem Abonnement aktiviert. Wenn dies der Fall ist, muss der Besitzer/Administrator diese Schaltfläche für Ihr Unternehmen auf der Seite *API-Berechtigungen* der App-Registrierung auswählen, damit die App-Registrierung gültig ist:

    :::image type="content" source="media/how-to-create-app-registration/grant-admin-consent.png" alt-text="Portalansicht der Schaltfläche „Administratoreinwilligung gewähren“ unter „API-Berechtigungen“":::
  - Wenn die Einwilligung erfolgreich gewährt wurde, sollte der Eintrag für Azure Digital Twins unter *Status* dann den Wert _Gewährt für **(Ihr Unternehmen)**_ anzeigen.
   
    :::image type="content" source="media/how-to-create-app-registration/granted-admin-consent-done.png" alt-text="Portalansicht der für das Unternehmen unter „API-Berechtigungen“ gewährten Administratoreinwilligung":::
* Aktivieren des öffentlichen Clientzugriffs
* Festlegen bestimmter Antwort-URLs für den Web- und Desktopzugriff
* Zulassen von impliziten OAuth2-Authentifizierungsflows

Weitere Informationen zur App-Registrierung und zu den verschiedenen diesbezüglichen Einrichtungsoptionen finden Sie unter [*Registrieren einer Anwendung bei der Microsoft Identity Platform*](/graph/auth-register-app-v2).

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine Azure AD-App-Registrierung festgelegt, die zur Authentifizierung von Clientanwendungen mit den Azure Digital Twins-APIs verwendet werden kann.

Informieren Sie sich im nächsten Schritt über Authentifizierungsmechanismen, darunter einen Mechanismus, der App-Registrierungen verwendet, und andere, bei denen dies nicht der Fall ist:
* [*Verwenden Schreiben von App-Authentifizierungscode*](how-to-authenticate-client.md)