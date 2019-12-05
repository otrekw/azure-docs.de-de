---
title: include file
description: include file
services: notification-hubs
author: sethmanheim
ms.service: notification-hubs
ms.topic: include
ms.date: 11/21/2019
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: ef2b98821b28d8a49e5f16bf1c6ac176eb8b5793
ms.sourcegitcommit: dd0304e3a17ab36e02cf9148d5fe22deaac18118
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/22/2019
ms.locfileid: "74407257"
---
## <a name="generate-the-certificate-signing-request-file"></a>Erstellen der Zertifikatsignieranforderungsdatei

Der Apple Push Notification Service (APNs) verwendet Zertifikate zur Authentifizierung Ihrer Pushbenachrichtigungen. Befolgen Sie diese Anweisungen, um das erforderliche Pushzertifikat zum Senden und Empfangen von Benachrichtigungen zu erstellen. Weitere Informationen zu diesen Konzepten finden Sie in der offiziellen Dokumentation zum [Apple Push Notification Service](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).

Erstellen Sie die Zertifikatsignieranforderungsdatei (CSR-Datei), die von Apple zur Generierung eines signierten Pushzertifikats verwendet wird.

1. Führen Sie auf Ihrem Mac das Tool "Schlüsselbundverwaltung" aus. Es kann im Ordner **Dienstprogramme** oder im Ordner **Andere** auf dem Launchpad geöffnet werden.

1. Wählen Sie die Option **Schlüsselbundverwaltung** aus, erweitern Sie **Zertifikatsassistent**, und wählen Sie dann **Zertifikat einer Zertifizierungsinstanz anfordern** aus.

    ![Anfordern eines neuen Zertifikats mithilfe der Schlüsselbundverwaltung](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

1. Wählen Sie Ihre **E-Mail-Adresse des Benutzers** aus, und geben Sie den Wert für **Allgemeiner Name** ein. Vergewissern Sie sich, dass **Saved to disk** (Auf Datenträger gespeichert) aktiviert ist, und wählen Sie dann **Weiter** aus. Lassen Sie das Feld **CA Email Address** (E-Mail der Zertifizierungsstelle) leer, da dieser Wert nicht erforderlich ist.

    ![Erforderliche Zertifikatinformationen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-csr-info.png)

1. Geben Sie im Feld **Speichern unter** einen Namen für die CSR-Datei ein, und wählen Sie unter **Where** (Ort) den Speicherort und anschließend **Speichern** aus.

    ![Auswählen eines Dateinamens für das Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Dadurch wird die CSR-Datei am ausgewählten Speicherort gespeichert. Der Standardspeicherort lautet **Desktop**. Merken Sie sich den für diese Datei festgelegten Speicherort.

Registrieren Sie als Nächstes Ihre App bei Apple, aktivieren Sie Pushbenachrichtigungen, und laden Sie die exportierte CSR-Datei hoch, um ein Pushzertifikat zu erstellen.

## <a name="register-your-app-for-push-notifications"></a>Registrieren der App für Pushbenachrichtigungen

Zum Senden von Pushbenachrichtigungen an eine iOS-App registrieren Sie Ihre Anwendung bei Apple, und registrieren Sie sie auch für Pushbenachrichtigungen.  

1. Falls Sie Ihre App noch nicht registriert haben, navigieren Sie im Apple Developer Center zum [iOS-Bereitstellungsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456). Melden Sie sich mit Ihrer Apple-ID beim Portal an, und wählen Sie **Bezeichner** aus. Wählen Sie dann **+** aus, um eine neue App zu registrieren.

    ![iOS-Bereitstellungsportal: Seite mit App-IDs](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Aktivieren Sie auf dem Bildschirm zum **Registrieren eines neuen Bezeichners** das Optionsfeld **App-IDs**. Klicken Sie anschließend auf **Weiter**.

    ![Seite zum Registrieren einer neuen ID im iOS-Bereitstellungsportal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-ios-appids-new.png)

3. Aktualisieren Sie die folgenden drei Werte für Ihre neue App, und wählen Sie dann **Weiter** aus:

   * **Beschreibung:** Geben Sie einen aussagekräftigen Namen für Ihre App ein.

   * **Paket-ID**: Geben Sie eine Paket-ID im Format **Organisationsbezeichner.Produktname** ein, wie im [Leitfaden zur App-Verteilung](https://help.apple.com/xcode/mac/current/#/dev91fe7130a) erwähnt. Die unter *Organization Identifier* (Organisationskennung) und *Product Name* (Produktname) eingegebenen Werte müssen der Organisationskennung und dem Produktnamen entsprechen, die Sie beim Erstellen des Xcode-Projekts verwenden. Im folgenden Screenshot wird der Wert **NotificationHubs** als Organisationskennung und **GetStarted** als Produktname verwendet. Stellen Sie sicher, dass der Wert für den **Paketbezeichner** mit dem Wert in Ihrem Xcode-Projekt übereinstimmt, sodass Xcode das richtige Veröffentlichungsprofil verwendet.

      ![Seite zum Registrieren einer App-ID im iOS-Bereitstellungsportal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-bundle.png)

   * **Push Notifications** (Pushbenachrichtigungen): Aktivieren Sie die Option **Pushbenachrichtigungen** im Bereich **Funktionen**.

      ![Formular zum Registrieren einer neuen App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-push.png)

      Hierdurch wird Ihre App-ID generiert, und Sie werden zur Bestätigung der Daten aufgefordert. Wählen Sie **Weiter** und dann **Registrieren** aus, um die neue App-ID zu bestätigen.

      ![Neue App-ID bestätigen](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-appid-register.png)

      Nachdem Sie **Registrieren** ausgewählt haben, wird die neue App-ID als Position auf der Seite **Zertifikate, Bezeichner und Profile** angezeigt.

4. Suchen Sie auf der Seite **Zertifikate, Bezeichner und Profile** unter **Bezeichner** die soeben erstellte App-ID-Position, und wählen Sie die zugehörige Zeile aus, um den Bildschirm zum **Bearbeiten Ihrer App-ID-Konfiguration** anzuzeigen.

5. Scrollen Sie nach unten zur aktivierten Option **Pushbenachrichtigungen**, und wählen Sie dann **Konfigurieren** aus, um das Zertifikat zu erstellen.

    ![Bearbeiten der Seite für App-IDs](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-edit-appid.png)

6. Das Fenster für die **SSL-Zertifikate von Apple Push Notification Service** wird angezeigt. Wählen Sie im Abschnitt des **SSL-Zertifikats für die Entwicklung** die Schaltfläche **Zertifikat erstellen** aus.

    ![Schaltfläche zum Erstellen eines Zertifikats für die App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Der Bildschirm **Ein neues Zertifikat erstellen** wird angezeigt.

    > [!NOTE]
    > In diesem Lernprogramm wird ein Entwicklungszertifikat verwendet. Derselbe Prozess wird auch zum Registrieren eines Produktionszertifikats durchgeführt. Achten Sie darauf, dass Sie denselben Zertifikattyp beim Senden von Benachrichtigungen verwenden.

1. Klicken Sie auf **Datei auswählen**, wechseln Sie zum Speicherort der in der ersten Aufgabe erstellten CSR-Datei, und doppelklicken Sie dann auf den Namen des Zertifikats, um es zu laden. Klicken Sie anschließend auf **Weiter**.

1. Wählen Sie nach der Erstellung des Zertifikats im Portal die Schaltfläche **Herunterladen** aus. Speichern Sie das Zertifikat, und merken Sie sich den Speicherort.

    ![Downloadseite für das generierte Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Das Zertifikat wird heruntergeladen und auf Ihrem Computer im Ordner **Downloads** gespeichert.

    ![Suchen der Zertifikatdatei im Ordner „Downloads“](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [!NOTE]
    > Standardmäßig heißt das heruntergeladene Entwicklungszertifikat **aps_development.cer**.

1. Doppelklicken Sie auf dem heruntergeladenen Pushzertifikat **aps_development.cer**. Das neue Zertifikat wird im Schlüsselbund installiert, wie in der folgenden Abbildung gezeigt:

    ![Zertifikatliste der Schlüsselbundverwaltung mit neuem Zertifikat](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [!NOTE]
    > Der Name in Ihrem Zertifikat kann anders lauten, ihm wird jedoch **Apple Development iOS Push Services** vorangestellt.

1. Klicken Sie in der Kategorie **Zertifikate** in der Schlüsselbundverwaltung mit der rechten Maustaste auf das neu erstellte Pushzertifikat. Wählen Sie die Option **Exportieren**, benennen Sie die Datei, wählen Sie das Format **.p12** aus, und wählen Sie dann die Option **Speichern**.

    ![Exportieren des Zertifikats im p12-Format](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-export-cert-p12.png)

    Sie können das Zertifikat mit einem Kennwort schützen, aber dies ist optional. Wenn Sie die Kennworterstellung umgehen möchten, klicken Sie auf **OK**. Notieren Sie sich den Dateinamen und den Speicherort des exportierten P12-Zertifikats, Sie werden zum Aktivieren der Authentifizierung mit APNs verwendet.

    > [!NOTE]
    > Dateiname und Speicherort Ihrer P12-Datei können sich von den in diesem Tutorial abgebildeten Informationen unterscheiden.

## <a name="create-a-provisioning-profile-for-the-app"></a>Erstellen eines Bereitstellungsprofils für die App

1. Wechseln Sie zurück zum [iOS-Bereitstellungsportal](https://go.microsoft.com/fwlink/p/?LinkId=272456), wählen Sie **Zertifikate, Bezeichner und Profile** aus, wählen Sie im Menü auf der linken Seite **Profile** und dann **+** aus, um ein neues Profil zu erstellen. Der Bildschirm zum **Registrieren eines neuen Bereitstellungsprofils** wird angezeigt.

1. Wählen Sie unter **Entwicklung**, die Option **iOS-App-Entwicklung** als Bereitstellungsprofiltyp aus, und wählen Sie anschließend **Weiter**.

    ![Bereitstellungsprofilliste](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

1. Wählen Sie anschließend die von Ihnen erstellte App-ID in der Dropdownliste **App ID** (App-ID) und dann **Weiter** aus.

    ![Auswählen der App-ID](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)

1. Wählen Sie im Fenster **Select certificates** (Zertifikate auswählen) das Entwicklungszertifikat aus, das Sie für die Codesignierung verwenden, und wählen Sie dann **Continue** (Weiter). Dabei handelt es sich nicht um das erstellte Pushzertifikat. Wenn keines vorhanden ist, müssen Sie es erstellen. Wenn ein Zertifikat vorhanden ist, fahren Sie mit dem nächsten Schritt fort. Erstellen eines Entwicklungszertifikat, wenn noch keines vorhanden ist:

    1. Wenn **No Certificates are available** (Keine Zertifikate verfügbar) angezeigt wird, wählen Sie **Create Certificate** (Zertifikat erstellen) aus.
    2. Wählen Sie im Abschnitt **Software** die Option **Apple Development** (Apple-Entwicklung) aus. Klicken Sie anschließend auf **Weiter**.
    3. Wählen Sie auf dem Bildschirm **Neues Zertifikat erstellen** die Option **Datei auswählen**.
    4. Navigieren Sie zum Zertifikat **Zertifikatsignieranforderung**, das Sie zuvor erstellt haben, wählen Sie es aus, und wählen Sie dann **Öffnen** aus.
    5. Wählen Sie **Weiter**.
    6. Laden Sie das Entwicklungszertifikat herunter, und merken Sie sich den Speicherort.

1. Wechseln Sie zurück zur Seite **Zertifikate, Bezeichner und Profile**, wählen Sie im Menü auf der linken Seite **Profile** und dann **+** aus, um ein neues Profil zu erstellen. Der Bildschirm zum **Registrieren eines neuen Bereitstellungsprofils** wird angezeigt.

1. Wählen Sie im Fenster **Select certificates** (Zertifikate auswählen) das soeben erstellte Entwicklungszertifikat aus. Klicken Sie anschließend auf **Weiter**.

1. Wählen Sie nun die Geräte aus, die zum Testen verwendet werden sollen, und wählen Sie dann **Weiter** aus.

1. Wählen Sie schließlich im Feld **Provisioning Profile Name** (Name des Bereitstellungsprofils) einen Namen für das Profil, und wählen Sie **Generate** (Generieren) aus.

    ![Auswählen eines Bereitstellungsprofilnamens](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)

1. Nachdem das neue Bereitstellungsprofil erstellt wurde, wählen Sie **Herunterladen** aus. Merken Sie sich den Speicherort des Profils.

1. Navigieren Sie zum Speicherort des Bereitstellungsprofils, und doppelklicken Sie darauf, um es auf dem Xcode-Entwicklungscomputer zu installieren.

## <a name="create-a-notification-hub"></a>Erstellen eines Notification Hubs

In diesem Abschnitt erstellen Sie einen Notification Hub und konfigurieren die Authentifizierung mit APNs unter Verwendung des zuvor erstellten Pushzertifikats vom Typ „.p12“. Wenn Sie einen bereits erstellten Notification Hub verwenden möchten, können Sie mit Schritt 5 fortfahren.

[!INCLUDE [notification-hubs-portal-create-new-hub](notification-hubs-portal-create-new-hub.md)]

## <a name="configure-your-notification-hub-with-apns-information"></a>Konfigurieren Ihres Notification Hubs mit APNs-Informationen

1. Wählen Sie unter **Notification Services** die Option **Apple (APNS)** aus.

1. Wählen Sie **Certificate**aus.

1. Wählen Sie das Dateisymbol aus.

1. Wählen Sie die zuvor exportierte P12-Datei aus, und wählen Sie dann **Öffnen**.

1. Geben Sie bei Bedarf das richtige Kennwort an.

1. Wählen Sie den Modus **Sandbox** aus. Verwenden Sie den Modus **Produktion** nur dann, wenn Sie Pushbenachrichtigungen an Benutzer senden möchten, die Ihre App im Store erworben haben.

    ![Konfigurieren der APNs-Zertifizierung im Azure-Portal](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-apple-config-cert.png)

1. Wählen Sie **Speichern** aus.

Sie haben Ihren Notification Hub mit APNs-Informationen konfiguriert. Sie verfügen außerdem über die Verbindungszeichenfolgen, die Sie zum Registrieren Ihrer App und zum Senden von Pushbenachrichtigungen benötigen.
