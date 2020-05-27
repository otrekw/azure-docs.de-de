---
title: Sprachanpassung in Azure AD-Benutzerflows
description: Erfahren Sie mehr über das Anpassen der Oberfläche für die Sprache in Ihren Benutzerflows.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/06/2020
ms.author: mimart
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: ed56acc9276177951919a3bc63c8a1dc3876e1c4
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83594699"
---
# <a name="language-customization-in-azure-active-directory-preview"></a>Sprachanpassung in Azure Active Directory (Vorschau)
|     |
| --- |
| Die Self-Service-Registrierung ist eine öffentliche Previewfunktion von Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Mit der Sprachanpassung in Azure Active Directory (Azure AD) kann Ihr Benutzerflow verschiedene Sprachen abdecken, um die Anforderungen Ihrer Benutzer zu erfüllen. Microsoft stellt Übersetzungen für [36 Sprachen](#supported-languages) bereit. Auch wenn Ihre Benutzeroberfläche nur für eine einzelne Sprache bestimmt ist, können Sie die Attributnamen auf der Attributerfassungsseite anpassen.

## <a name="how-language-customization-works"></a>Funktionsweise der Sprachanpassung

Die Sprachanpassung ist standardmäßig für Benutzer aktiviert, die sich registrieren. Dadurch soll eine konsistente Registrierungserfahrung sichergestellt werden. Sie können mithilfe von Sprachen die Zeichenfolgen anpassen, die Benutzern während der Registrierung im Rahmen der Attributerfassung angezeigt werden.

> [!NOTE]
> Falls Sie benutzerdefinierte Benutzerattribute verwenden, müssen Sie Ihre eigenen Übersetzungen angeben. Weitere Informationen finden Sie unter [Anpassen von Zeichenfolgen](#customize-your-strings).

## <a name="customize-your-strings"></a>Anpassen von Zeichenfolgen

Mit der Sprachanpassung können Sie alle Zeichenfolgen Ihres Benutzerflows anpassen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) als Azure AD-Administrator an.
2. Wählen Sie unter **Azure-Dienste** die Option **Azure Active Directory** aus.
3. Wählen Sie im Menü auf der linken Seite **Externe Identitäten** aus.
4. Wählen Sie **Benutzerflows (Vorschau)** aus.
3. Wählen Sie den Benutzerflow aus, den Sie für Übersetzungen aktivieren möchten.
4. Wählen Sie **Sprachen** aus.
5. Wählen Sie auf der Seite **Sprachen** für den Benutzerflow die Sprache aus, die Sie anpassen möchten.
6. Erweitern Sie die **Attributerfassungsseite**.
7. Klicken Sie auf **Standardeinstellungen herunterladen** (oder **Außerkraftsetzungen herunterladen**, wenn Sie diese Sprache zuvor bearbeitet haben).

Bei diesen Schritten erhalten Sie eine JSON-Datei, die Sie nutzen können, um mit dem Bearbeiten Ihrer Zeichenfolgen zu beginnen.

### <a name="change-any-string-on-the-page"></a>Ändern einer beliebigen Zeichenfolge auf der Seite

1. Öffnen Sie die JSON-Datei, die Sie im Rahmen der obigen Anleitung heruntergeladen haben, in einem JSON-Editor.
1. Suchen Sie nach dem Element, das Sie ändern möchten. Sie können nach der `StringId` der gewünschten Zeichenfolge oder nach dem Attribut `Value` suchen, das Sie ändern möchten.
1. Aktualisieren Sie das `Value`-Attribut mit den Daten, die angezeigt werden sollen.
1. Ändern Sie `Override` für jede Zeichenfolge, die Sie ändern möchten, in `true`.
1. Speichern Sie die Datei, und laden Sie Ihre Änderungen hoch. (Das Steuerelement zum Hochladen befindet sich dort, wo Sie auch die JSON-Datei heruntergeladen haben.)

> [!IMPORTANT]
> Stellen Sie beim Überschreiben einer Zeichenfolge sicher, dass Sie den Wert für `Override` auf `true` festlegen. Wenn der Wert nicht geändert wird, wird der Eintrag ignoriert.

### <a name="change-extension-attributes"></a>Ändern von Erweiterungsattributen

Wenn Sie die Zeichenfolge für ein benutzerdefiniertes Benutzerattribut ändern oder dem JSON-Code eine Zeichenfolge hinzufügen möchten, verwenden Sie das folgende Format:

```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Ersetzen Sie `<ExtensionAttribute>` durch den Namen Ihres benutzerdefinierten Benutzerattributs.

Ersetzen Sie `<ExtensionAttributeValue>` durch die neue Zeichenfolge, die angezeigt werden soll.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Angeben einer Liste mit Werten mithilfe von „LocalizedCollections“

Wenn Sie eine feste Liste mit Werten für Antworten bereitstellen möchten, müssen Sie ein Attribut vom Typ `LocalizedCollections` erstellen. Bei `LocalizedCollections` handelt es sich um ein Array mit Paaren aus `Name` und `Value`. Die Reihenfolge der Elemente ist dieselbe, in der sie angezeigt werden. Verwenden Sie zum Hinzufügen von `LocalizedCollections` das folgende Format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` ist das Benutzerattribut, für das dieses Attribut vom Typ `LocalizedCollections` eine Antwort darstellt.
* `Name` ist der Wert, der dem Benutzer angezeigt wird.
* `Value` wird im Anspruch zurückgegeben, wenn diese Option ausgewählt wird.

### <a name="upload-your-changes"></a>Hochladen von Änderungen

1. Kehren Sie nach dem Ändern der JSON-Datei wieder zu Ihrem Mandanten zurück.
1. Wählen Sie **Benutzerflows** aus, und klicken Sie auf den Benutzerflow, den Sie für Übersetzungen aktivieren möchten.
1. Wählen Sie **Sprachen** aus.
1. Wählen Sie die gewünschte Zielsprache für die Übersetzung aus.
1. Wählen Sie **Attributerfassungsseite** aus.
1. Klicken Sie auf das Ordnersymbol, und wählen Sie die hochzuladende JSON-Datei aus.

Die Änderungen werden automatisch in Ihrem Benutzerflow gespeichert.

## <a name="additional-information"></a>Zusätzliche Informationen

### <a name="page-ui-customization-labels-as-overrides"></a>Bezeichnungen der Seite für die Benutzeroberflächenanpassung als Außerkraftsetzungen

Wenn Sie die Sprachanpassung aktivieren, werden Ihre bisherigen Bearbeitungen für Bezeichnungen, die Sie auf der Seite für die Benutzeroberflächenanpassung vorgenommen haben, in einer JSON-Datei für Englisch (en) gespeichert. Sie können Ihre Bezeichnungen und anderen Zeichenfolgen weiterhin ändern, indem Sie Sprachressourcen in die Sprachanpassung hochladen.

### <a name="up-to-date-translations"></a>Aktuelle Übersetzungen

Microsoft ist bemüht, Ihnen möglichst aktuelle Übersetzungen zur Verfügung zu stellen. Die Übersetzungen werden kontinuierlich optimiert und auf ihre Konformität überprüft. Microsoft ermittelt Fehler und Änderungen für die globale Terminologie und nimmt geeignete Aktualisierungen in Ihrem Benutzerflow vor.

### <a name="support-for-right-to-left-languages"></a>Unterstützung für von rechts nach links geschriebene Sprachen

Von rechts nach links geschriebene Sprachen werden von Microsoft derzeit nicht unterstützt. Dies erreichen Sie, indem Sie benutzerdefinierte Gebietsschemas verwenden und mithilfe von CSS ändern, wie Zeichenfolgen angezeigt werden. Sollten Sie dieses Feature benötigen, stimmen Sie im [Azure-Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag) dafür ab.

### <a name="social-identity-provider-translations"></a>Übersetzungen für den Fall „Soziales Netzwerk als Identitätsanbieter“

Für Anmeldungen per sozialem Netzwerk stellt Microsoft den OIDC-Parameter `ui_locales` bereit. Einige soziale Netzwerke, die als Identitätsanbieter fungieren, erkennen diesen jedoch nicht an. Dazu zählen auch Facebook und Google.

### <a name="browser-behavior"></a>Browserverhalten

Von Chrome und Firefox wird jeweils die eigene festgelegte Sprache angefordert. Wenn es sich dabei um eine unterstützte Sprache handelt, wird sie vor der Standardsprache angezeigt. Für Microsoft Edge wird derzeit keine Sprache angefordert, sondern es wird gleich die Standardsprache verwendet.

## <a name="supported-languages"></a>Unterstützte Sprachen

Azure AD bietet Unterstützung für die folgenden Sprachen. Sprachen für Benutzerflows werden von Azure AD bereitgestellt. Die Sprachen für Benachrichtigungen von Multi-Factor Authentication (MFA) werden von [Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) bereitgestellt.

| Sprache              | Sprachcode | Benutzerabläufe         | MFA-Benachrichtigungen  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arabisch                | ar            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Bulgarisch             | bg            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Bengalisch                | bn            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Katalanisch               | ca            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Tschechisch                 | cs            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Dänisch                | da            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Deutsch                | de            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Griechisch                 | el            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Englisch               | en            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Spanisch               | es            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Estnisch              | et            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Baskisch                | eu            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Finnisch               | fi            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Französisch                | fr            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Galizisch              | gl            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Gujarati              | gu            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Hebräisch                | he            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Hindi                 | hi            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Kroatisch              | hr            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Ungarisch             | hu            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Indonesisch            | id            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Italienisch               | it            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Japanisch              | ja            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Kasachisch                | kk            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Kannada               | kn            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Koreanisch                | ko            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Litauisch            | lt            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Lettisch               | lv            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Malayalam             | ml            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Marathi               | mr            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Malaiisch                 | ms            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Norwegisch Bokmål      | nb            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Niederländisch                 | nl            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Norwegisch             | nein            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Pandschabi               | pa            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Polnisch                | pl            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Portugiesisch (Brasilien)   | pt-br         | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Portugiesisch (Portugal) | pt-pt         | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Rumänisch              | ro            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Russisch               | ru            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Slowakisch                | sk            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Slowenisch             | sl            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Serbisch (Kyrillisch)    | sr-cryl-cs    | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Serbisch (Lateinisch)       | sr-latn-cs    | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Schwedisch               | sv            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Tamilisch                 | ta            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Telugu                | te            | ![ja](./media/user-flow-customize-language/yes.png) | ![nein](./media/user-flow-customize-language/no.png) |
| Thailändisch                  | th            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Türkisch               | tr            | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Ukrainisch             | uk            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Vietnamesisch            | vi            | ![nein](./media/user-flow-customize-language/no.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Chinesisch (vereinfacht)  | zh-hans       | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |
| Chinesisch (traditionell) | zh-hant       | ![ja](./media/user-flow-customize-language/yes.png) | ![ja](./media/user-flow-customize-language/yes.png) |