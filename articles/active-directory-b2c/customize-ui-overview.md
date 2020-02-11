---
title: Anpassen der Benutzeroberfläche
titleSuffix: Azure AD B2C
description: In diesem Artikel erfahren Sie, wie Sie die Benutzeroberfläche Ihrer Anwendungen anpassen, die Azure Active Directory B2C verwenden.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: f171d9d71d3e6f8fa57671578502675442293793
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/31/2020
ms.locfileid: "76908933"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Anpassen der Benutzeroberfläche in Azure Active Directory B2C

Das Branding und Anpassen der Benutzeroberfläche, die Azure Active Directory B2C (Azure AD B2C) für Ihre Kunden anzeigt, trägt zu einer nahtlosen Benutzererfahrung in Ihrer Anwendung bei. Diese Benutzererfahrung umfasst Registrieren, Anmelden, Bearbeiten von Profilen und Zurücksetzen von Kennwörtern. In diesem Artikel werden die Methoden zum Anpassen der Benutzeroberfläche für Benutzerflows und benutzerdefinierte Richtlinien erläutert.

## <a name="ui-customization-in-different-scenarios"></a>Anpassen der Benutzeroberfläche in verschiedenen Szenarien

Es gibt mehrere, für unterschiedliche Szenarien geeignete Möglichkeiten, die Benutzeroberfläche für die Benutzererfahrung in Ihrer Anwendung anzupassen.

### <a name="user-flows"></a>Benutzerabläufe

Wenn Sie [Benutzerflows](user-flow-overview.md) verwenden, können Sie das Aussehen Ihrer Benutzerflow-Seiten mithilfe von integrierten *Seitenlayoutvorlagen* oder mit eigenem HTML und CSS anpassen. Beide Methoden werden später in diesem Artikel beschrieben.

Sie konfigurieren die Anpassung der Benutzeroberfläche für Benutzerflows im [Azure-Portal](tutorial-customize-ui.md) an.

> [!TIP]
> Wenn Sie nur das Bannerlogo, das Hintergrundbild und die Hintergrundfarbe Ihrer Benutzerflowseiten ändern möchten, können Sie das Feature [Unternehmensbranding (Vorschauversion)](#company-branding-preview) ausprobieren, das weiter unten in diesem Artikel beschrieben wird.

### <a name="custom-policies"></a>Benutzerdefinierte Richtlinien

Wenn Sie [benutzerdefinierte Richtlinien](custom-policy-overview.md) verwenden, um in Ihrer Anwendung Registrierung bzw. Anmeldung, Kennwortzurücksetzung oder Profilbearbeitung bereitzustellen, verwenden Sie die [Richtliniendateien zum Anpassen der Benutzeroberfläche](custom-policy-ui-customization.md).

Wenn Sie dynamische Inhalte auf Grundlage der Entscheidung eines Kunden bereitstellen müssen, verwenden Sie benutzerdefinierte Richtlinien, [die Seiteninhalte dynamisch ändern können](custom-policy-ui-customization-dynamic.md), in Abhängigkeit von einem Parameter, der in einer Abfragezeichenfolge gesendet wird. Sie können z. B. das Hintergrundbild auf der Azure AD B2C-Registrierungs- oder Anmeldeseite auf der Grundlage eines Parameters ändern, den Sie von der Web- oder Mobilanwendung übergeben.

### <a name="javascript"></a>JavaScript

Sie können clientseitigen JavaScript-Code sowohl in [Benutzerflows](user-flow-javascript-overview.md) als auch in [benutzerdefinierten Richtlinien](page-layout.md) aktivieren.

### <a name="sign-in-only-ui-customization"></a>Anpassung der Benutzeroberfläche nur für die Anmeldung

Wenn Sie nur eine Anmeldung mit der begleitenden Seite für die Kennwortzurücksetzung sowie Verifizierungs-E-Mails bereitstellen, verwenden Sie dieselben Anpassungsschritte, die für eine [Azure AD-Anmeldeseite](../active-directory/fundamentals/customize-branding.md) verwendet werden.

Wenn Kunden versuchen, ihr Profil zu bearbeiten, bevor sie sich anmelden, werden sie zu einer Seite umgeleitet, die Sie mithilfe derselben Schritte anpassen, die zum Anpassen der Anmeldeseite von Azure AD verwendet werden.

## <a name="page-layout-templates"></a>Seitenlayoutvorlagen

Benutzerflows bieten eine Reihe von integrierten Vorlagen, unter denen Sie auswählen können, um Ihren Seiten mit Benutzererfahrung einen professionellen Look zu verleihen. Diese Layoutvorlagen können auch als Ausgangspunkt für eigene Anpassungsschritte dienen.

Wählen Sie im linken Menü unter **Anpassen** die Option **Seitenlayouts** und anschließend **Vorlage** aus.

![Dropdownmenü zur Vorlagenauswahl auf der Seite des Benutzerflows im Azure-Portal](media/customize-ui-overview/template-selection.png)

Wählen Sie anschließend eine Vorlage aus der Liste aus. Hier sind einige Beispiele für die Anmeldeseiten für die einzelnen Vorlagen:

| Ozeanblau | Schiefergrau | Klassisch |
|:-:|:-:|:-:|
|![Beispiel für die gerenderte Vorlage „Ozeanblau“ auf der Seite für die Registrierung/Anmeldung](media/customize-ui-overview/template-ocean-blue.png)|![Beispiel für die gerenderte Vorlage „Schiefergrau“ auf der Seite für die Registrierung/Anmeldung](media/customize-ui-overview/template-slate-gray.png)|![Beispiel für die gerenderte Vorlage „Klassisch“ auf der Seite für die Registrierung/Anmeldung](media/customize-ui-overview/template-classic.png)|

Wenn Sie eine Vorlage auswählen, wird das ausgewählte Layout auf alle Seiten in Ihrem Benutzerflow angewendet, und im Feld **Benutzerdefinierter Seiten-URI** wird der URI für jede Seite angezeigt.

## <a name="custom-html-and-css"></a>Benutzerdefiniertes HTML und CSS

Wenn Sie Ihr eigenes Richtlinienlayout mit benutzerdefiniertem HTML- und CSS-Code gestalten möchten, können Sie dazu für jeden der Layoutnamen in Ihrer Richtlinie die Umschaltfläche „Benutzerdefinierten Seiteninhalt verwenden“ verwenden. Gehen Sie bei der Konfiguration des benutzerdefinierten Layouts wie folgt vor:

Azure AD B2C führt den Code im Browser Ihres Kunden aus. Hierbei wird ein Ansatz namens [Cross-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS, Ressourcenfreigabe zwischen verschiedenen Ursprüngen) verfolgt.

Zur Laufzeit wird Inhalt über eine URL geladen, die Sie in einem Benutzerflow oder einer benutzerdefinierten Richtlinie angeben. Der Inhalt jeder Seite in der Benutzererfahrung wird von der URL geladen, die Sie für die jeweilige Seite angeben. Nachdem Inhalt über die URL geladen wurde, wird er mit einem von Azure AD B2C eingefügten HTML-Fragment zusammengeführt, und die Seite wird für Ihren Kunden angezeigt.

Bevor Sie zum Anpassen der Benutzeroberfläche Ihre eigenen HTML- und CSS-Dateien verwenden, beachten Sie die folgenden Hinweise:

- Azure AD B2C **führt** HTML-Inhalt in Ihren Seiten zusammen. Versuchen Sie nicht, den Standardinhalt zu kopieren oder zu ändern, den Azure AD B2C bereitstellt. Es wird empfohlen, eigene HTML-Inhalte von Grund auf neu zu erstellen und den Standardinhalt als Referenz zu verwenden.
- **JavaScript** kann sowohl für [Benutzerflows](user-flow-javascript-overview.md) als auch für [benutzerdefinierte Richtlinien](javascript-samples.md) in benutzerdefinierte Inhalte eingeschlossen werden.
- Unterstützte **Browserversionen**:
  - Internet Explorer 11, 10 und Microsoft Edge
  - Eingeschränkte Unterstützung für Internet Explorer 9 und 8
  - Google Chrome 42.0 und höher
  - Mozilla Firefox 38.0 und höher
- Schließen Sie keine **Form-Tags** in Ihr HTML ein. Form-Tags stören die POST-Operationen, die von dem durch Azure AD B2C injizierten HTML generiert werden.

### <a name="where-do-i-store-ui-content"></a>Wo speichere ich Inhalte der Benutzeroberfläche?

Wenn Sie eigene HTML- und CSS-Dateien zum Anpassen der Benutzeroberfläche verwenden, können Sie Ihren Benutzeroberflächeninhalt auf einem beliebigen öffentlich verfügbaren HTTPS-Endpunkt hosten, der CORS unterstützt. Beispiele sind [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md), Webserver, CDNs, AWS S3 und Dateifreigabesysteme.

Der wichtige Punkt hierbei ist, dass Sie die Inhalte auf einem öffentlich verfügbaren HTTPS-Endpunkt (mit aktiviertem CORS) hosten. Sie müssen eine absolute URL verwenden, wenn Sie ihn in Ihrem Inhalt angeben.

## <a name="get-started-with-custom-html-and-css"></a>Erste Schritte mit benutzerdefiniertem HTML und CSS

Befolgen Sie die unten stehenden Richtlinien, wenn Sie eigenes HTML und CSS auf Ihren Seiten mit Benutzererfahrung verwenden möchten.

- Erstellen Sie an einer beliebigen Stelle in `<body>` wohlgeformten HTML-Inhalt mit einem leeren `<div id="api"></div>`-Element. Dieses Element markiert die Stelle, an der der Azure AD B2C-Inhalt eingefügt wird. Im folgenden Beispiel wird eine minimale Seite gezeigt:

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- Formatieren Sie die Elemente der Benutzeroberfläche, die von Azure AD B2C in Ihre Seite einfügt werden, mithilfe von CSS. Das folgende Beispiel zeigt eine einfachen CSS-Datei, die außerdem Einstellungen für die Registrierung eingefügter HTML-Elemente enthält:

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center;
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center;
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Hosten Sie diese Inhalte auf einem HTTPS-Endpunkt (auf dem CORS zulässig ist). Bei der Konfiguration von CORS müssen die Anforderungsmethoden GET und OPTIONS aktiviert werden.
- Erstellen oder bearbeiten Sie einen Benutzerflow oder eine benutzerdefinierte Richtlinie, um den erstellten Inhalt zu verwenden.

### <a name="html-fragments-from-azure-ad-b2c"></a>HTML-Fragmente aus Azure AD B2C

In der folgenden Tabelle finden Sie die HTML-Fragmente, die Azure AD B2C im `<div id="api"></div>`-Element in Ihrem Inhalt zusammenführt.

| Eingefügte Seite | Beschreibung von HTML |
| ------------- | ------------------- |
| Auswahl des Identitätsanbieters | Enthält eine Liste mit Schaltflächen für Identitätsanbieter, aus denen der Benutzer bei der Registrierung oder Anmeldung auswählen kann. Bei diesen Schaltflächen handelt es sich um Identitätsanbieter in Form von sozialen Netzwerken wie Facebook, Google oder lokale Konten (basierend auf E-Mail-Adressen oder Benutzernamen). |
| Registrierung mit lokalem Konto | Enthält ein Formular für die Registrierung eines lokalen Kontos, das auf einer E-Mail-Adresse oder einem Benutzernamen basiert. Das Formular kann verschiedene Eingabesteuerelemente enthalten, z. B. Texteingabefelder, Kennworteingabefelder, Optionsfelder, Dropdownfelder mit einer Auswahlmöglichkeit und Kontrollkästchen mit mehreren Optionen. |
| Registrierung über ein Konto eines sozialen Netzwerks | Kann angezeigt werden, wenn die Registrierung mit einem vorhandenen Konto bei einem sozialen Netzwerk wie Facebook oder Google als Identitätsanbieter erfolgt. Wird verwendet, wenn zusätzliche Informationen vom Kunden mithilfe eines Registrierungsformulars erfasst werden müssen. |
| Einheitliche Registrierung oder Anmeldung | Verarbeitet sowohl die Registrierung als auch die Anmeldung von Kunden. Diese können als Identitätsanbieter soziale Netzwerke wie z. B. Facebook, Google oder lokale Konten verwenden. |
| Multi-Factor Authentication | Kunden können während der Registrierung oder Anmeldung ihre Telefonnummern verifizieren (per SMS oder Sprachnachricht). |
| Fehler | Stellt dem Kunden Fehlerinformationen bereit. |

## <a name="company-branding-preview"></a>Unternehmensbranding (Vorschauversion)

In Azure Active Directory können Sie mithilfe des Features [Unternehmensbranding](../active-directory/fundamentals/customize-branding.md) Ihre Benutzerflowseiten mit einem Bannerlogo, einem Hintergrundbild und einer Hintergrundfarbe anpassen.

Zum Anpassen Ihrer Benutzerflowseiten konfigurieren Sie zuerst das Unternehmensbranding in Azure Active Directory, und dann aktivieren Sie es in den Seitenlayouts Ihrer Benutzerflows in Azure AD B2C.

[!INCLUDE [preview note](../../includes/active-directory-b2c-public-preview.md)]

### <a name="configure-company-branding"></a>Konfigurieren des Unternehmensbrandings

Legen Sie zuerst das Bannerlogo, das Hintergrundbild und die Hintergrundfarbe in **Unternehmensbranding** fest.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Wählen Sie im oberen Menü den Filter **Verzeichnis und Abonnement** aus, und wählen Sie dann das Verzeichnis aus, das Ihren Azure AD B2C-Mandanten enthält.
1. Suchen Sie im Azure-Portal nach **Azure AD B2C**, und wählen Sie diese Option dann aus.
1. Wählen Sie unter **Verwalten** die Option **Unternehmensbranding** aus.
1. Führen Sie die unter [Hinzufügen von Branding zur Azure Active Directory-Anmeldeseite Ihrer Organisation](../active-directory/fundamentals/customize-branding.md) aufgeführten Schritte aus.

Beachten Sie beim Konfigurieren des Unternehmensbrandings in Azure AD B2C die folgenden Punkte:

* Das Unternehmensbranding in Azure AD B2C ist derzeit auf die Anpassung des **Hintergrundbilds**, des **Bannerlogos** und der **Hintergrundfarbe** beschränkt. Die anderen Eigenschaften im Bereich „Unternehmensbranding“ (z. B. die unter **Erweiterte Einstellungen**) werden *nicht unterstützt*.
* Auf Ihren Benutzerflowseiten wird die Hintergrundfarbe angezeigt, bevor das Hintergrundbild geladen wird. Es wird empfohlen, eine Hintergrundfarbe auszuwählen, die weitestgehend den Farben in Ihrem Hintergrundbild entspricht, um ein reibungsloseres Ladeerlebnis zu ermöglichen.
* Das Bannerlogo wird in den Überprüfungs-E-Mails angezeigt, die an Ihre Benutzer gesendet werden, wenn sie einen Benutzerflow für die Registrierung initiieren.

### <a name="enable-branding-in-user-flow-pages"></a>Aktivieren von Branding auf Benutzerflowseiten

Nachdem Sie das Unternehmensbranding konfiguriert haben, können Sie es in Ihren Benutzerflows aktivieren.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Azure AD B2C** aus.
1. Wählen Sie unter **Richtlinien** die Option **Benutzerflows (Richtlinien)** aus.
1. Wählen Sie den Benutzerflow aus, für den Sie das Unternehmensbranding aktivieren möchten. Für die Benutzerflowtypen *Anmeldung v1* und *Profilbearbeitung v1* wird das Unternehmensbranding **nicht unterstützt**.
1. Wählen Sie unter **Anpassen** die Option **Seitenlayouts** aus, und wählen Sie dann das Layout aus, das Sie mit Branding versehen möchten. Wählen Sie z. B. **Einheitliche Seite für Registrierung oder Anmeldung** aus.
1. Wählen Sie für **Seitenlayoutversion (Vorschau)** die Version **1.2.0** oder eine höhere Version aus.
1. Wählen Sie **Speichern** aus.

Wenn Sie alle Seiten im Benutzerflow mit Branding versehen möchten, legen Sie die Seitenlayoutversion für jedes Seitenlayout im Benutzerflow fest.

![Seitenlayoutauswahl in Azure AD B2C im Azure-Portal](media/customize-ui-overview/portal-02-page-layout-select.png)

In diesem kommentierten Beispiel sehen Sie ein benutzerdefiniertes Bannerlogo und Hintergrundbild auf einer Benutzerflowseite für die *Registrierung und Anmeldung*, für die die Vorlage „Ozeanblau“ verwendet wird:

![Von Azure AD B2C bereitgestellte Registrierungs-/Anmeldeseite mit Branding](media/customize-ui-overview/template-ocean-blue-branded.png)

### <a name="use-company-branding-assets-in-custom-html"></a>Verwenden von Unternehmensbrandingobjekten in benutzerdefiniertem HTML

Wenn Sie Ihre Unternehmensbrandingobjekte in benutzerdefiniertem HTML verwenden möchten, fügen Sie die folgenden Tags außerhalb des Tags `<div id="api">` hinzu:

```HTML
<img data-tenant-branding-background="true" />
<img data-tenant-branding-logo="true" alt="Company Logo" />
```

Die Bildquelle wird durch die des Hintergrundbilds und Bannerlogos ersetzt. Wie im Abschnitt [Erste Schritte mit benutzerdefiniertem HTML und CSS](#get-started-with-custom-html-and-css) beschrieben, verwenden Sie CSS-Klassen, um die Objekte auf der Seite zu formatieren und zu positionieren.

## <a name="localize-content"></a>Lokalisieren von Inhalt

Sie lokalisieren Ihren HTML-Inhalt, indem Sie die [Sprachanpassung](user-flow-language-customization.md) in Ihrem Azure AD B2C-Mandanten aktivieren. Wenn Sie dieses Feature aktivieren, kann Azure AD B2C den Open ID Connect-Parameter `ui-locales` an Ihren Endpunkt weiterleiten. Ihr Inhaltsserver kann diesen Parameter verwenden, um benutzerdefinierte, sprachspezifische HTML-Seiten bereitzustellen.

Inhalt kann auf Grundlage des verwendeten Gebietsschemas aus unterschiedlichen Quellen abgerufen werden. Richten Sie in Ihrem CORS-fähigen Endpunkt eine Ordnerstruktur zum Hosten von Inhalten für bestimmte Sprachen ein. Wenn Sie den Platzhalterwert `{Culture:RFC5646}` verwenden, wird der passende Inhalt abgerufen.

Beispielsweise könnte Ihr benutzerdefinierter Seiten-URI wie folgt aussehen:

```HTTP
https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html
```

Sie können die Seite in Französisch laden, indem Sie Inhalt abrufen aus:

```HTTP
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="examples"></a>Beispiele

Im Repository [B2C-AzureBlobStorage-Client](https://github.com/azureadquickstarts/b2c-azureblobstorage-client) auf GitHub finden Sie verschiedene Beispielvorlagendateien.

Die HTML- und CSS-Beispieldateien in den Vorlagen befinden sich im Verzeichnis [/sample_templates](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates).

## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie **Benutzerflows** verwenden, können Sie anhand des folgenden Tutorials mit dem Anpassen Ihrer Benutzeroberfläche beginnen:

    [Anpassen der Benutzeroberfläche Ihrer Anwendungen in Azure Active Directory B2C](tutorial-customize-ui.md).
- Wenn Sie **benutzerdefinierte Richtlinien** verwenden, können Sie anhand des folgenden Artikels mit dem Anpassen der Benutzeroberfläche beginnen:

    [Anpassen der Benutzeroberfläche einer Anwendung mithilfe einer benutzerdefinierten Richtlinie in Azure Active Directory B2C](custom-policy-ui-customization.md).
