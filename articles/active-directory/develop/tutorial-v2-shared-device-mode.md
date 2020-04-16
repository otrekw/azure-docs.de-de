---
title: Verwenden des Modus für gemeinsam genutzte Geräte mit MSAL Android | Azure
description: Es wird beschrieben, wie Sie ein Android-Gerät für die Ausführung im Modus für die gemeinsame Nutzung vorbereiten und eine App für Mitarbeiter in Service und Produktion ausführen.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 1/15/2020
ms.author: hahamil
ms.reviewer: brandwe
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: b2f74d2d441007f195abd38ca26ca7fa73605318
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886431"
---
# <a name="tutorial-use-shared-device-mode-in-your-android-application"></a>Tutorial: Verwenden des Modus für gemeinsam genutzte Geräte in Ihrer Android-Anwendung

> [!NOTE]
> Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="developer-guide"></a>Entwicklerhandbuch

Dieses Handbuch enthält eine Anleitung für Entwickler zum Implementieren des Modus für gemeinsam genutzte Geräte in einer Android-Anwendung per Microsoft Authentication Library (MSAL). Informationen zum Integrieren der MSAL in Ihre Android-App, Anmelden eines Benutzers, Aufrufen des Microsoft-Graphen und Abmelden eines Benutzers finden Sie im [Tutorial zu MSAL für Android](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android).

### <a name="download-the-sample"></a>Herunterladen des Beispiels

Klonen Sie die [Beispielanwendung](https://github.com/Azure-Samples/ms-identity-android-java/) über GitHub. Das Beispiel funktioniert im [Modus mit einem oder mehreren Konten](https://docs.microsoft.com/azure/active-directory/develop/single-multi-account).

### <a name="add-the-msal-sdk-to-your-local-maven-repository"></a>Hinzufügen des MSAL SDK zu Ihrem lokalen Maven-Repository

Falls Sie die Beispiel-App nicht verwenden, müssen Sie die MSAL der Datei „build.gradle“ wie folgt hinzufügen:

```gradle
dependencies{
  implementation 'com.microsoft.identity.client.msal:1.0.+'
}
```

### <a name="configure-your-app-to-use-shared-device-mode"></a>Konfigurieren Ihrer App für den Modus für gemeinsam genutzte Geräte

Weitere Informationen zum Einrichten Ihrer config-Datei finden Sie in der [Dokumentation zur Konfiguration](https://docs.microsoft.com/azure/active-directory/develop/msal-configuration).

Legen Sie `"shared_device_mode_supported"` in Ihrer MSAL-Konfigurationsdatei auf `true` fest.

Es kann auch sein, dass Sie die Unterstützung des Modus mit mehreren Konten nicht planen. Dies kann beispielsweise der Fall sein, wenn Sie kein gemeinsam genutztes Gerät verwenden und Benutzer sich mit mehr als einem Konto gleichzeitig an der App anmelden können. Falls dies zutrifft, sollten Sie `"account_mode"` auf `"SINGLE"` festlegen. So wird sichergestellt, dass für Ihre App immer `ISingleAccountPublicClientApplication` genutzt wird, und die MSAL-Integration wird erheblich vereinfacht. Der Standardwert von `"account_mode"` lautet `"MULTIPLE"`. Daher ist es wichtig, diesen Wert in der config-Datei zu ändern, wenn Sie den Modus `"single account"` nutzen.

Hier ist ein Beispiel für die Datei „auth_config.json“ angegeben, die im Verzeichnis **app**>**main**>**res**>**raw** der Beispiel-App enthalten ist:

```json
{
 "client_id":"Client ID after app registration at https://aka.ms/MobileAppReg",
 "authorization_user_agent":"DEFAULT",
 "redirect_uri":"Redirect URI after app registration at https://aka.ms/MobileAppReg",
 "account_mode":"SINGLE",
 "broker_redirect_uri_registered": true,
 "shared_device_mode_supported": true,
 "authorities":[
  {
   "type":"AAD",
   "audience":{
     "type": "AzureADandPersonalMicrosoftAccount",
     "tenant_id":"common"
   }
  }
 ]
}
```

### <a name="detect-shared-device-mode"></a>Erkennen des Modus für gemeinsam genutzte Geräte

Im Modus für gemeinsam genutzte Geräte können Sie Android-Geräte so konfigurieren, dass sie von mehreren Mitarbeitern gemeinsam genutzt werden können, während gleichzeitig eine auf Microsoft Identity basierende Verwaltung der Geräte erfolgt. Mitarbeiter können sich bei ihren Geräten anmelden und schnell auf Kundeninformationen zugreifen. Nachdem die Mitarbeiter ihre Schicht bzw. Aufgabe beendet haben, können sie sich mit nur einem Klick von allen Apps auf dem gemeinsam genutzten Gerät abmelden. Das Gerät ist dann sofort für die Nutzung durch den nächsten Mitarbeiter bereit.

Ermitteln Sie mit `isSharedDevice()`, ob eine App auf einem Gerät ausgeführt wird, das sich im Modus für gemeinsam genutzte Geräte befindet. Anhand dieses Flags kann Ihre App ermitteln, ob die Benutzeroberfläche entsprechend geändert werden soll.

Im Codeausschnitt unten wird veranschaulicht, wie Sie `isSharedDevice()` nutzen können.  Er stammt aus der `SingleAccountModeFragment`-Klasse in der Beispiel-App:

```Java
deviceModeTextView.setText(mSingleAccountApp.isSharedDevice() ?"Shared" :"Non-Shared");
```

### <a name="initialize-the-publicclientapplication-object"></a>Initialisieren des PublicClientApplication-Objekts

Wenn Sie `"account_mode":"SINGLE"` in der config-Datei für MSAL festlegen, können Sie das zurückgegebene Anwendungsobjekt problemlos in ein `ISingleAccountPublicCLientApplication`-Element umwandeln.

```java
private ISingleAccountPublicClientApplication mSingleAccountApp;

/*Configure your sample app and save state for this activity*/
PublicClientApplication.create(this.getApplicationCOntext(),
  R.raw.auth_config,
  new PublicClientApplication.ApplicationCreatedListener(){
  @Override
  public void onCreated(IPublicClientApplication application){
  mSingleAccountApp = (ISingleAccountPublicClientApplication)application;
  loadAccount();
  }
  @Override
  public void onError(MsalException exception{
  /*Fail to initialize PublicClientApplication */
  }
});
```

### <a name="detect-single-vs-multiple-account-mode"></a>Erkennen des Moduls für ein bzw. mehrere Konten

Wenn Sie eine App schreiben, die nur für Mitarbeiter in Service und Produktion auf einem gemeinsam genutzten Gerät eingesetzt wird, empfehlen wir Ihnen, für die App nur die Unterstützung des Modus mit einem einzelnen Konto vorzusehen. Dies gilt für die meisten Anwendungen, bei denen es um die Abarbeitung von Aufgaben geht, z. B. in Apps für Gesundheitsdaten oder Abrechnungsvorgänge und den meisten branchenspezifischen Apps. Auf diese Weise wird die Entwicklungsarbeit vereinfacht, weil viele Features des SDK nicht abgedeckt werden müssen.

Wenn Ihre App mehrere Konten und den Modus für gemeinsam genutzte Geräte unterstützt, müssen Sie eine Typüberprüfung und Umwandlung auf die entsprechende Oberfläche durchführen. Dies ist unten veranschaulicht.

```java
private IPublicClientApplication mApplication;

        if (mApplication instanceOf IMultipleAccountPublicClientApplication) {
          IMultipleAccountPublicClientApplication multipleAccountApplication = (IMultipleAccountPublicClientApplication) mApplication;
          ...
        } else if (mApplication instanceOf    ISingleAccountPublicClientApplication) {
           ISingleAccountPublicClientApplication singleAccountApplication = (ISingleAccountPublicClientApplication) mApplication;
            ...
        }
```

### <a name="get-the-signed-in-user-and-determine-if-a-user-has-changed-on-the-device"></a>Abrufen des angemeldeten Benutzers und Ermitteln, ob sich der Benutzer des Geräts geändert hat

Mit der `loadAccount`-Methode wird das Konto des angemeldeten Benutzers abgerufen. Mit der `onAccountChanged`-Methode wird ermittelt, ob sich der angemeldete Benutzer geändert hat. Wenn dies der Fall ist, wird eine Bereinigung durchgeführt:

```java
private void loadAccount()
{
  mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback()
  {
    @Overide
    public void onAccountLoaded(@Nullable IAccount activeAccount)
    {
      if (activeAccount != null)
      {
        signedInUser = activeAccount;
        mSingleAccountApp.acquireTokenSilentAsync(SCOPES,"http://login.microsoftonline.com/common",getAuthSilentCallback());
      }
    }
    @Override
    public void on AccountChanged(@Nullable IAccount priorAccount, @Nullable Iaccount currentAccount)
    {
      if (currentAccount == null)
      {
        //Perform a cleanup task as the signed-in account changed.
        updateSingedOutUI();
      }
    }
    @Override
    public void onError(@NonNull Exception exception)
    {
    }
  }
}
```

### <a name="globally-sign-in-a-user"></a>Globales Anmelden eines Benutzers

Hiermit wird ein Benutzer über das Gerät mit der Authenticator-App bei anderen Apps angemeldet, die MSAL verwenden:

```java
private void onSignInClicked()
{
  mSingleAccountApp.signIn(getActivity(), SCOPES, null, getAuthInteractiveCallback());
}
```

### <a name="globally-sign-out-a-user"></a>Globales Abmelden eines Benutzers

Mit dem folgenden Code wird das angemeldete Konto entfernt, und zwischengespeicherte Token werden nicht nur für die App gelöscht, sondern auch von dem Gerät, das sich im Modus für die gemeinsame Nutzung befindet:

```java
private void onSignOutClicked()
{
  mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback()
  {
    @Override
    public void onSignOut()
    {
      updateSignedOutUI();
    }
    @Override
    public void onError(@NonNull MsalException exception)
    {
      /*failed to remove account with an exception*/
    }
  });
}
```

## <a name="administrator-guide"></a>Administratorhandbuch

In den folgenden Schritten wird beschrieben, wie Sie Ihre Anwendung im Azure-Portal einrichten und für Ihr Gerät den Modus für die gemeinsame Nutzung festlegen.

### <a name="register-your-application-in-azure-active-directory"></a>Registrieren der Anwendung in Azure Active Directory

Registrieren Sie Ihre Anwendung zunächst unter dem Mandanten Ihrer Organisation. Geben Sie anschließend die unten angegebenen Werte in „auth_config.json“ ein, damit Ihre Anwendung richtig ausgeführt wird.

Weitere Informationen zur Vorgehensweise finden Sie unter [Anwendung registrieren](https://docs.microsoft.com/azure/active-directory/develop/tutorial-v2-android#register-your-application).

> [!NOTE]
> Verwenden Sie beim Registrieren Ihrer App die Schnellstartanleitung auf der linken Seite, und wählen Sie **Android** aus. Sie gelangen auf eine Seite, auf der Sie zum Eingeben von **Paketname** und **Signaturhash** für Ihre App aufgefordert werden. Dies ist sehr wichtig, um sicherzustellen, dass Ihre App-Konfiguration funktioniert. Sie erhalten dann ein Konfigurationsobjekt für Ihre App, das Sie ausschneiden und in die Datei „auth_config.json“ einfügen können.

![Bildschirm für App-Registrierung](media/tutorial-v2-shared-device-mode/register-app.png) Wählen Sie die Option **Diese Änderung für mich vornehmen** aus, und geben Sie die Werte, die von der Schnellstartanleitung abgefragt werden, im Azure-Portal an. Nach Abschluss dieses Vorgangs generieren wir alle Konfigurationsdateien, die Sie benötigen.

![Bildschirm mit Konfigurationsinformationen für die App](media/tutorial-v2-shared-device-mode/config-info.png)

## <a name="set-up-a-tenant"></a>Einrichten eines Mandanten

Richten Sie zu Testzwecken auf Ihrem Mandanten Folgendes ein: mindestens zwei Mitarbeiter, einen Cloudgeräteadministrator und einen globalen Administrator. Legen Sie im Azure-Portal den Cloudgeräteadministrator fest, indem Sie die Organisationsrollen ändern. Greifen Sie im Azure-Portal auf Ihre Organisationsrollen zu, indem Sie **Azure Active Directory** > **Rollen und Administratoren** > **Cloudgeräteadministrator** auswählen. Fügen Sie die Benutzer hinzu, die ein Gerät in den Modus für gemeinsam genutzte Geräte versetzen können.

## <a name="set-up-an-android-device-in-shared-mode"></a>Einrichten eines Android-Geräts im gemeinsam genutzten Modus

### <a name="download-the-authenticator-app"></a>Herunterladen der Authenticator-App

Laden Sie die Microsoft Authenticator-App aus dem Google Play Store herunter. Wenn Sie die App bereits heruntergeladen haben, sollten Sie sicherstellen, dass es sich um die neueste Version handelt.

### <a name="authenticator-app-settings--registering-the-device-in-the-cloud"></a>Einstellungen der Authenticator-App und Registrieren des Geräts in der Cloud

Starten Sie die Authenticator-App, und navigieren Sie zur Hauptseite des Kontos. Wenn die Seite **Konto hinzufügen** angezeigt wird, können Sie das Gerät für die gemeinsame Nutzung bereitstellen.

![Authenticator-Bildschirm „Konto hinzufügen“](media/tutorial-v2-shared-device-mode/authenticator-add-account.png)

 Wechseln Sie in der Menüleiste auf der rechten Seite zum Bereich **Einstellungen**. Wählen Sie unter **Geschäfts-, Schul- oder Unikonten** die Option **Geräteregistrierung** aus.

 ![Authenticator-Bildschirm „Konto hinzufügen“](media/tutorial-v2-shared-device-mode/authenticator-settings.png)

 Wenn Sie auf diese Schaltfläche klicken, werden Sie aufgefordert, den Zugriff auf die Gerätekontakte zu autorisieren. Der Grund ist die Kontointegration von Android auf dem Gerät. Wählen Sie **Zulassen** aus.

 ![Authenticator-Bildschirm „Konto hinzufügen“](media/tutorial-v2-shared-device-mode/authenticator-allow-screen.png)

Der Cloudgeräteadministrator sollte unter **Oder als gemeinsam genutztes Gerät registrieren** seine geschäftliche E-Mail-Adresse für die Organisation eingeben. Klicken Sie anschließend auf die Schaltfläche **Als gemeinsam genutztes Gerät registrieren**, und geben Sie Ihre Anmeldeinformationen ein.

![Bildschirm zum Registrieren des Geräts](media/tutorial-v2-shared-device-mode/register-device.png)

![Anmeldung](media/tutorial-v2-shared-device-mode/sign-in.png)

Das Gerät befindet sich jetzt im Modus für gemeinsame Nutzung.

![Bildschirm zum Registrieren des Geräts](media/tutorial-v2-shared-device-mode/shared-device-mode-screen.png)

 Alle An- und Abmeldungen auf dem Gerät sind globaler Art. Dies bedeutet, dass sie für alle Apps gelten, die in MSAL und Microsoft Authenticator auf dem Gerät integriert sind. Sie können nun Anwendungen auf dem Gerät bereitstellen, für die Funktionen des Modus für gemeinsam genutzte Geräte verwendet werden.

## <a name="view-the-shared-device-in-the-azure-portal"></a>Anzeigen des gemeinsam genutzten Geräts im Azure-Portal

Nachdem Sie ein Gerät in den Modus für gemeinsame Nutzung versetzt haben, ist es für Ihre Organisation bekannt und wird im Organisationsmandanten nachverfolgt. Sie können Ihre gemeinsam genutzten Geräte anzeigen, indem Sie sich im Azure-Portal auf dem Blatt „Azure Active Directory“ den **Join-Typ** ansehen.

![Blatt „Alle Geräte“ im Azure-Portal](media/tutorial-v2-shared-device-mode/registered-device-screen.png)

## <a name="running-the-sample-app"></a>Ausführen der Beispiel-App

Die Beispielanwendung ist eine einfache App, mit der die Graph-API Ihrer Organisation aufgerufen wird. Bei der ersten Ausführung werden Sie zum Einwilligen aufgefordert, weil es sich um eine neue Anwendung für Ihr Mitarbeiterkonto handelt.

![Bildschirm mit Konfigurationsinformationen für die App](media/tutorial-v2-shared-device-mode/run-app-permissions-requested.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Modus für gemeinsam genutzte Geräte finden Sie unter [Modus für gemeinsam genutzte Geräte für Android-Geräte](msal-android-shared-devices.md).