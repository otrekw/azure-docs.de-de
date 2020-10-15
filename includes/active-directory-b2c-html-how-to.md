---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/19/2020
ms.author: mimart
ms.openlocfilehash: 9b660bf20c90a84780175e70573c96a0ce1b0b7d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91377074"
---
## <a name="use-custom-page-content"></a>Verwenden benutzerdefinierter Seiteninhalte

Mit der Funktion zum Anpassen der Benutzeroberfläche können Sie das Aussehen und Verhalten benutzerdefinierter Richtlinien anpassen. Außerdem können Sie die Konsistenz von Marken und visuellen Elementen zwischen Ihrer Anwendung und Azure AD B2C verwalten.

### <a name="how-it-works"></a>Funktionsweise

Azure AD B2C führt mithilfe von [Cross-Origin Resource Sharing (CORS)](https://www.w3.org/TR/cors/) (ursprungsübergreifende Ressourcenfreigabe) den Code im Browser Ihres Kunden aus. Zur Laufzeit wird Inhalt über eine URL geladen, die Sie in Ihrem Benutzerflow oder Ihrer benutzerdefinierten Richtlinie angeben. Der Inhalt jeder Seite in der Benutzererfahrung wird von der URL geladen, die Sie für die jeweilige Seite angeben. Nachdem Inhalt über die URL geladen wurde, wird er mit einem von Azure AD B2C eingefügten HTML-Fragment zusammengeführt, und die Seite wird für Ihren Kunden angezeigt.

![Benutzerdefinierte Seiteninhalte (Rand)](./media/active-directory-b2c-html-templates/html-content-merging.png)

## <a name="custom-html-page-content"></a>Benutzerdefinierte HTML-Seiteninhalte

Erstellen Sie eine HTML-Seite mit Ihrem eigenen Branding, um Ihren benutzerdefinierten Seiteninhalt bereitzustellen. Bei dieser Seite kann es sich um eine statische `*.html`-Seite oder eine dynamische Seite wie .NET, Node.js oder PHP handeln.

Ihr benutzerdefinierter Seiteninhalt darf beliebige HTML-Elemente (einschließlich CSS und JavaScript), jedoch keine unsicheren Elemente wie iframes enthalten. Das einzige erforderliche Element ist ein „div“-Element, bei dem `id` auf `api` festgelegt ist, wie z. B. `<div id="api"></div>` auf der HTML-Seite.

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Product Brand Name</title>
</head>
<body>
    <div id="api"></div>
</body>
</html>
```

### <a name="customize-the-default-azure-ad-b2c-pages"></a>Anpassen der Azure AD B2C-Standardseiten

Anstatt benutzerdefinierte Seiteninhalte von Grund auf neu zu erstellen, können Sie die Inhalte von Azure AD B2C-Standardseiten anpassen.

In der folgenden Tabelle sind die von Azure AD B2C bereitgestellten Standardseiteninhalte aufgelistet. Laden Sie die Dateien herunter, und verwenden Sie sie als Ausgangspunkt für das Erstellen eigener benutzerdefinierter Seiten.

| Standardseite | BESCHREIBUNG | ID für Inhaltsdefinition<br/>(nur benutzerdefinierte Richtlinie) |
|:-----------------------|:--------|-------------|
| [exception.html](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Fehlerseite**: Diese Seite wird angezeigt, wenn eine Ausnahme oder ein Fehler auftreten. | *api.error* |
| [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) |  **Selbstbestätigte Seite**. Verwenden Sie diese Datei als benutzerdefinierten Seiteninhalt für Seiten zum Anmelden über ein soziales Netzwerk, zum Registrieren eines lokalen Kontos, zum Anmelden mit einem lokalen Konto, zum Zurücksetzen des Kennworts usw. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z.B. ein Texteingabefeld, ein Kennworteingabefeld, ein Optionsfeld, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. | *api.localaccountsignin*, *api.localaccountsignup*, *api.localaccountpasswordreset*, *api.selfasserted* |
| [multifactor-1.0.0.html](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Seite „Multi-Factor Authentication“** : Auf dieser Seite können Benutzer während der Registrierung oder Anmeldung ihre Telefonnummern verifizieren (per SMS oder Sprachnachricht). | *api.phonefactor* |
| [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Seite für Profilaktualisierung**: Diese Seite enthält ein Formular, auf das Benutzer zum Aktualisieren des Profils zugreifen können. Diese Seite ähnelt der Seite für die Registrierung bei einem Konto für ein soziales Netzwerk, mit Ausnahme der Felder für die Kennworteingabe. | *api.selfasserted.profileupdate* |
| [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Einheitliche Seite für Registrierung oder Anmeldung**: Auf dieser Seite werden der Registrierungs- und der Anmeldevorgang für Benutzer durchgeführt. Benutzer können Unternehmensidentitätsanbieter, Identitätsanbieter in Form von sozialen Netzwerken wie Facebook und Google+ oder lokale Konten verwenden. | *api.signuporsignin* |

## <a name="hosting-the-page-content"></a>Hosten des Seiteninhalts

Wenn Sie eigene HTML- und CSS-Dateien zum Anpassen der Benutzeroberfläche verwenden, hosten Sie Ihre Inhalte für die Benutzeroberfläche auf einem beliebigen öffentlich verfügbaren HTTPS-Endpunkt, der CORS unterstützt. Beispiele sind [Azure Blob Storage](../articles/storage/blobs/storage-blobs-introduction.md), [Azure App Services](/azure/app-service/), Webserver, CDNs, AWS S3 oder Dateifreigabesysteme.

## <a name="guidelines-for-using-custom-page-content"></a>Richtlinien zum Verwenden benutzerdefinierter Seiteninhalte

- Verwenden Sie eine absolute URL, wenn Sie externe Ressourcen wie Medien-, CSS- und JavaScript-Dateien in Ihre HTML-Datei einbeziehen.
- Mit der [Seitenlayoutversion](../articles/active-directory-b2c/page-layout.md) 1.2.0 und höher können Sie in den HTML-Tags das Attribut `data-preload="true"` hinzufügen, um die Ladereihenfolge für CSS und JavaScript zu steuern. Mit `data-preload=true` wird die Seite erstellt, bevor sie dem Benutzer angezeigt wird. Mit diesem Attribut wird ein „Flackern“ der Seite verhindert, da die CSS-Datei vorab geladen und die unformatierte HTML-Datei dem Benutzer nicht angezeigt wird. Der folgende HTML-Codeausschnitt zeigt die Verwendung des `data-preload`-Tags.
  ```HTML
  <link href="https://path-to-your-file/sample.css" rel="stylesheet" type="text/css" data-preload="true"/>
  ```
- Wir empfehlen, mit dem Standardseiteninhalt zu beginnen und darauf aufzubauen.
- Sie können JavaScript kann sowohl für [Benutzerflows](../articles/active-directory-b2c/user-flow-javascript-overview.md) als auch für [benutzerdefinierte Richtlinien](../articles/active-directory-b2c/javascript-samples.md) in Ihre benutzerdefinierte Inhalte einbeziehen.
- Unterstützte Browserversionen sind:
  - Internet Explorer 11, 10 und Microsoft Edge
  - Eingeschränkte Unterstützung für Internet Explorer 9 und 8
  - Google Chrome 42.0 und höher
  - Mozilla Firefox 38.0 und höher
  - Safari für iOS und macOS, Version 12 und höher
- Aufgrund von Sicherheitseinschränkungen unterstützt Azure AD B2C keine HTML-Elemente vom Typ `frame`, `iframe` oder `form`.

## <a name="custom-page-content-walkthrough"></a>Exemplarische Vorgehensweise für benutzerdefinierte Seiteninhalte

Übersicht über den Prozess:

1. Vorbereiten eines Speicherorts zum Hosten Ihrer benutzerdefinierten Seiteninhalte (öffentlich zugänglicher, CORS-fähiger HTTPS-Endpunkt)
1. Herunterladen und Anpassen einer Datei mit Standardseiteninhalt, z. B. `unified.html`
1. Veröffentlichen des benutzerdefinierten Seiteninhalts über den öffentlich verfügbaren HTTPS-Endpunkt.
1. Festlegen der Ressourcenfreigabe zwischen verschiedenen Ursprüngen (CORS) für Ihre Web-App
1. Verweisen in Ihrer Richtlinie auf den URI für den benutzerdefinierten Richtlinieninhalt

### <a name="1-create-your-html-content"></a>1. Erstellen des HTML-Inhalts

Erstellen Sie benutzerdefinierte Seiteninhalte mit dem Markennamen Ihres Produkts im Titel.

1. Kopieren Sie den folgenden HTML-Codeausschnitt. Hierbei handelt es sich um einen wohlgeformten HTML5-Ausschnitt mit einem leeren Element namens *\<div id="api"\>\</div\>* , das sich in den *\<body\>* -Tags befindet. Dieses Element gibt an, wo Azure AD B2C-Inhalt eingefügt werden soll.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

1. Fügen Sie den kopierten Codeausschnitt in einen Text-Editor ein, und speichern Sie die Datei dann unter dem Namen *customize-ui.html*.

> [!NOTE]
> HTML-Formularelemente werden bei der Verwendung von login.microsoftonline.com aufgrund von Sicherheitseinschränkungen entfernt. Nutzen Sie [b2clogin.com](../articles/active-directory-b2c/b2clogin.md), wenn Sie HTML-Formularelemente in Ihren benutzerdefinierten HTML-Inhalten verwenden möchten.

### <a name="2-create-an-azure-blob-storage-account"></a>2. Erstellen eines Azure Blob Storage-Kontos

In diesem Artikel wird Azure Blob Storage zum Hosten unserer Inhalte verwendet. Sie können angeben, dass Ihre Inhalte auf einem Webserver gehostet werden sollen, aber Sie müssen [auf Ihrem Webserver CORS aktivieren](https://enable-cors.org/server.html).

Führen Sie die folgenden Schritte aus, um Ihren HTML-Inhalt im Blob-Speicher zu hosten:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im Menü **Hub** die Option **Neu** > **Speicher** > **Speicherkonto**.
1. Wählen Sie ein **Abonnement** für Ihr Speicherkonto aus.
1. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus.
1. Geben Sie einen eindeutigen **Namen** für Ihr Speicherkonto ein.
1. Wählen Sie den **geografischen Standort** für das Speicherkonto aus.
1. Als **Bereitstellungsmodell** können Sie **Resource Manager** unverändert lassen.
1. Für **Leistung** können Sie **Standard** übernehmen.
1. Ändern Sie **Kontoart** in **Blob Storage**.
1. Für **Replikation** können Sie **RA-GRS** übernehmen.
1. Für **Zugriffstarif** können Sie **Heiß** übernehmen.
1. Wählen Sie **Überprüfen + erstellen** aus, um das Speicherkonto zu erstellen.
    Nach Abschluss der Bereitstellung wird die Seite **Speicherkonto** automatisch geöffnet.

#### <a name="21-create-a-container"></a>2.1 Erstellen eines Containers

Führen Sie die folgenden Schritte aus, um im Blob-Speicher einen öffentlichen Container zu erstellen:

1. Wählen Sie unter **Blob-Dienst** im linken Menü die Option **Blobs** aus.
1. Wählen Sie **+ Container** aus.
1. Geben Sie im Feld **Name** die Zeichenfolge *root* ein. Dies kann ein Name Ihrer Wahl sein, z. B. *contoso*, aber in diesem Beispiel verwenden wir der Einfachheit halber *root*.
1. Wählen Sie für **Öffentliche Zugriffsebene** die Option **Blob** aus, und klicken Sie dann auf **OK**.
1. Wählen Sie **root** aus, um den neuen Container zu öffnen.

#### <a name="22-upload-your-custom-page-content-files"></a>2.2 Hochladen von Dateien mit benutzerdefinierten Seiteninhalten

1. Wählen Sie die Option **Hochladen**.
1. Wählen Sie das Ordnersymbol neben **Datei auswählen** aus.
1. Navigieren Sie zur Datei **customize-ui.html**, die Sie zuvor im Abschnitt „Seite für die Benutzeroberflächenanpassung“ erstellt haben, und wählen Sie die Datei aus.
1. Wenn Sie die Datei in einen Unterordner hochladen möchten, erweitern Sie die Option **Erweitert**, und geben Sie im Feld **In Ordner hochladen** einen Ordnernamen ein.
1. Wählen Sie die Option **Hochladen**.
1. Wählen Sie das Blob **customize-ui.html** aus, das Sie hochgeladen haben.
1. Wählen Sie rechts neben dem Textfeld **URL** das Symbol **In Zwischenablage kopieren** aus, um die URL in die Zwischenablage zu kopieren.
1. Navigieren Sie im Webbrowser zu der kopierten URL, um zu überprüfen, ob auf das hochgeladene Blob zugegriffen werden kann. Wenn ein Zugriff darauf nicht möglich ist und beispielsweise ein Fehler vom Typ `ResourceNotFound` auftritt, stellen Sie sicher, dass der Zugriffstyp für den Container auf **Blob** festgelegt ist.

### <a name="3-configure-cors"></a>3. Konfigurieren von CORS

Führen Sie die folgenden Schritte aus, um den Blob-Speicher für die Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS) zu konfigurieren:

1. Wählen Sie im Menü **CORS** aus.
1. Geben Sie für **Zulässige Ursprünge** den Wert `https://your-tenant-name.b2clogin.com` ein. Ersetzen Sie `your-tenant-name` durch den Namen des Azure AD B2C-Mandanten. Beispiel: `https://fabrikam.b2clogin.com`. Verwenden Sie bei der Eingabe Ihres Mandantennamens ausschließlich Kleinbuchstaben.
1. Wählen Sie für **Zulässige Methoden** sowohl `GET` als auch `OPTIONS` aus.
1. Geben Sie für **Zulässige Header** ein Sternchen (*) ein.
1. Geben Sie für **Verfügbar gemachte Header** ein Sternchen (*) ein.
1. Für **Max. Alter** geben Sie 200 ein.
1. Wählen Sie **Speichern** aus.

#### <a name="31-test-cors"></a>3.1 Testen von CORS

Führen Sie die folgenden Schritte aus, um zu überprüfen, ob Sie bereit sind:

1. Wiederholen Sie den CORS-Konfigurationsschritt. Geben Sie für **Zulässige Ursprünge** den Wert `https://www.test-cors.org` ein.
1. Navigieren Sie zu [www.test-cors.org](https://www.test-cors.org/). 
1. Fügen Sie im Feld **Remote-URL** die URL Ihrer HTML-Datei ein. Zum Beispiel, `https://your-account.blob.core.windows.net/root/azure-ad-b2c/unified.html`
1. Wählen Sie **Anforderung senden** aus.
    Das Ergebnis sollte `XHR status: 200` lauten. 
    Wenn ein Fehler ausgegeben wird, sollten Sie sich vergewissern, ob Ihre CORS-Einstellungen richtig sind. Außerdem müssen Sie unter Umständen Ihren Browsercache löschen oder eine InPrivate-Browsersitzung öffnen, indem Sie STRG+UMSCHALT+P drücken.
