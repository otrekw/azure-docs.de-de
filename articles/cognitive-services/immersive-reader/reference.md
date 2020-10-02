---
title: Plastischer Reader-SDK – Referenz
titleSuffix: Azure Cognitive Services
description: Das Plastischer Reader-SDK enthält eine JavaScript-Bibliothek, die es Ihnen ermöglicht, den Plastischen Reader in Ihre Anwendung zu integrieren.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 73322cdee151969e6e765690284bbffc1c871f4e
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090192"
---
# <a name="immersive-reader-javascript-sdk-reference-v11"></a>Referenz zum JavaScript-SDK für den Plastischen Reader (v1.1)

Das Plastischer Reader-SDK enthält eine JavaScript-Bibliothek, die es Ihnen ermöglicht, den Plastischen Reader in Ihre Anwendung zu integrieren.

## <a name="functions"></a>Functions

Das SDK macht folgende Funktionen verfügbar:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

<br>

## <a name="launchasync"></a>launchAsync

Startet den Plastischen Reader in einem `iframe` in Ihrer Webanwendung. Beachten Sie, dass die Inhaltsgröße auf 50 MB beschränkt ist.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

#### <a name="launchasync-parameters"></a>launchAsync-Parameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| `token` | Zeichenfolge | Das Azure AD-Authentifizierungstoken. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen einer Ressource für den plastischen Reader](./how-to-create-immersive-reader.md). |
| `subdomain` | Zeichenfolge | Die benutzerdefinierte Unterdomäne ihrer Plastischer Reader-Ressource in Azure. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen einer Ressource für den plastischen Reader](./how-to-create-immersive-reader.md). |
| `content` | [Inhalt](#content) | Ein Objekt mit dem Inhalt, der im Plastischen Reader angezeigt werden soll. |
| `options` | [Optionen](#options) | Optionen zum Konfigurieren bestimmter Verhaltensweisen des Plastischen Readers. Optional. |

#### <a name="returns"></a>Rückgabe

Gibt ein `Promise<LaunchResponse>` zurück, das beim Laden des Plastischen Readers aufgelöst wird. `Promise` wird zu einem [`LaunchResponse`](#launchresponse)-Objekt aufgelöst.

#### <a name="exceptions"></a>Ausnahmen

Das zurückgegebene `Promise` wird mit einem [`Error`](#error)-Objekt zurückgewiesen, wenn das Laden des Plastischen Readers fehlschlägt. Weitere Informationen finden Sie in den [Fehlercodes](#error-codes).

<br>

## <a name="close"></a>close

Schließt den Plastischen Reader.

Ein Beispiel eines Anwendungsfalls für diese Funktion ist, wenn die Schaltfläche „Beenden“ durch die Einstellung ```hideExitButton: true``` in [Optionen](#options) ausgeblendet wird. Dann kann eine andere Schaltfläche (z. B. der Pfeil „Zurück“ eines mobilen Headers) diese ```close```-Funktion aufrufen, wenn sie angeklickt wird.

```typescript
close(): void;
```

<br>

## <a name="immersive-reader-launch-button"></a>Startschaltfläche für plastischen Reader

Das SDK stellt einen Standardstil für die Schaltfläche zum Starten des Plastischen Readers bereit. Verwenden Sie das Klassenattribut `immersive-reader-button` zum Aktivieren dieses Stils. Weitere Informationen finden Sie unter [Vorgehensweise: Anpassen der Schaltfläche des plastischen Readers](./how-to-customize-launch-button.md).

```html
<div class='immersive-reader-button'></div>
```

#### <a name="optional-attributes"></a>Optionale Attribute

Verwenden Sie die folgenden Attribute, um das Aussehen und Verhalten der Schaltfläche zu konfigurieren.

| attribute | BESCHREIBUNG |
| --------- | ----------- |
| `data-button-style` | Legt den Stil der Schaltfläche fest. Kann `icon`, `text` oder `iconAndText` sein. Der Standardwert lautet `icon`. |
| `data-locale` | Legt das Gebietsschema fest. Zum Beispiel: `en-US` oder `fr-FR`. Der Standardwert ist „Englisch“ `en`. |
| `data-icon-px-size` | Legt die Größe des Symbols in Pixel fest. Der Standardwert ist „20px“. |

<br>

## <a name="renderbuttons"></a>renderButtons

Die Funktion ```renderButtons``` ist nicht erforderlich, wenn Sie den Leitfaden [Vorgehensweise: Anpassen der Schaltfläche des plastischen Readers](./how-to-customize-launch-button.md) verwenden.

Diese Funktion formatiert und aktualisiert die Schaltflächenelemente für „Plastischer Reader“ des Dokuments. Wenn ```options.elements``` angegeben ist, werden die Schaltflächen innerhalb jedes in ```options.elements``` angegebenen Elements gerendert. Die Verwendung des Parameters ```options.elements``` ist hilfreich, wenn Sie mehrere Abschnitte in Ihrem Dokument besitzen, auf denen der plastische Reader gestartet werden soll, und Sie eine vereinfachte Möglichkeit zum Rendern mehrerer Schaltflächen mit demselben Format wünschen oder die Schaltflächen mit einem einfachen und konsistenten Entwurfsmuster darstellen möchten. Um diese Funktion mit dem Parameter [renderButtons options](#renderbuttons-options) zu verwenden, rufen Sie ```ImmersiveReader.renderButtons(options: RenderButtonsOptions);``` beim Laden der Seite auf, wie im folgenden Codeausschnitt gezeigt. Andernfalls werden die Schaltflächen innerhalb der Elemente des Dokuments gerendert, die die Klasse ```immersive-reader-button``` aufweisen, wie in [Vorgehensweise: Anpassen der Schaltfläche des plastischen Readers](./how-to-customize-launch-button.md) gezeigt.

```typescript
// This snippet assumes there are two empty div elements in
// the page HTML, button1 and button2.
const btn1: HTMLDivElement = document.getElementById('button1');
const btn2: HTMLDivElement = document.getElementById('button2');
const btns: HTMLDivElement[] = [btn1, btn2];
ImmersiveReader.renderButtons({elements: btns});
```

Weitere Renderingoptionen finden Sie oben unter [Optionale Attribute](#optional-attributes). Fügen Sie jedem ```HTMLDivElement``` in Ihrem HTML-Code der Seite eines der Optionsattribute hinzu, um diese Optionen zu verwenden.

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

#### <a name="renderbuttons-parameters"></a>renderButtons-Parameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| `options` | [renderButtons options](#renderbuttons-options) | Optionen zum Konfigurieren bestimmter Verhaltensweisen der Funktion „renderButtons“. Optional. |

### <a name="renderbuttons-options"></a>renderButtons Options

Optionen für das Rendering der Schaltflächen des Plastischen Readers.

```typescript
{
    elements: HTMLDivElement[];
}
```

#### <a name="renderbuttons-options-parameters"></a>renderButtons Options-Parameter

| Einstellung | Typ | Beschreibung |
| ------- | ---- | ----------- |
| Elemente | HTMLDivElement[] | Elemente, in denen die Schaltflächen des plastischen Readers gerendert werden. |

##### `-elements`
```Parameters
Type: HTMLDivElement[]
Required: false
```

<br>

## <a name="launchresponse"></a>LaunchResponse

Enthält die Antwort aus dem Aufruf von `ImmersiveReader.launchAsync`. Beachten Sie, dass ein Verweis auf den `iframe`, der den Plastischen Reader enthält, über `container.firstChild` aufgerufen werden kann.

```typescript
{
    container: HTMLDivElement;
    sessionId: string;
}
```

#### <a name="launchresponse-parameters"></a>LaunchResponse-Parameter

| Einstellung | Typ | Beschreibung |
| ------- | ---- | ----------- |
| Container | HTMLDivElement | HTML-Element, das das „iframe“ des plastischen Readers enthält. |
| sessionID | String | Global eindeutiger Bezeichner für diese Sitzung, der zum Debuggen verwendet wird. |
 
## <a name="error"></a>Fehler

Enthält Informationen zu einem Fehler.

```typescript
{
    code: string;
    message: string;
}
```

#### <a name="error-parameters"></a>Fehlerparameter

| Einstellung | Typ | Beschreibung |
| ------- | ---- | ----------- |
| code | String | Einer aus einer Reihe von Fehlercodes. Weitere Informationen finden Sie unter [Fehlercodes](#error-codes). |
| message | String | Lesbare Darstellung des Fehlers. |

#### <a name="error-codes"></a>Fehlercodes

| Code | BESCHREIBUNG |
| ---- | ----------- |
| BadArgument | Das übergebene Argument ist ungültig, siehe `message`-Parameter des [Fehlers](#error). |
| Timeout | Der Plastische Reader konnte innerhalb des angegebenen Zeitlimits nicht geladen werden. |
| TokenExpired | Das angegebene Token ist abgelaufen. |
| Gedrosselt | Das Limit für die Aufrufrate wurde überschritten. |

<br>

## <a name="types"></a>Typen

### <a name="content"></a>Inhalt

Enthält den Inhalt, der im Plastischen Reader angezeigt werden soll.

```typescript
{
    title?: string;
    chunks: Chunk[];
}
```

#### <a name="content-parameters"></a>Inhaltsparameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| title | String | Titeltext, der oben auf dem plastischen Reader angezeigt wird (optional). |
| Blöcke | [Chunk[]](#chunk) | Array von Blöcken |

##### `-title`
```Parameters
Type: String
Required: false
Default value: "Immersive Reader" 
```

##### `-chunks`
```Parameters
Type: Chunk[]
Required: true
Default value: null 
```

<br>

### <a name="chunk"></a>Block

Ein einzelner Datenblock, der in den Inhalt des Plastischen Readers übertragen wird.

```typescript
{
    content: string;
    lang?: string;
    mimeType?: string;
}
```

#### <a name="chunk-parameters"></a>Blockparameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| Inhalt | String | Die Zeichenfolge, die den an den plastischen Reader gesendeten Inhalt enthält. |
| lang | String | Sprache des Texts, der Wert ist im IETF BCP 47-Sprachtagformat angegeben, z. B. „en“, „es-ES“. Die Sprache wird automatisch erkannt, wenn sie nicht angegeben ist. Siehe [Unterstützte Sprachen](#supported-languages). |
| mimeType | Zeichenfolge | Nur-Text-, MathML-, HTML- und Microsoft Word DOCX-Formate werden unterstützt. Weitere Informationen finden Sie unter [Unterstützte MIME-Typen](#supported-mime-types). |

##### `-content`
```Parameters
Type: String
Required: true
Default value: null 
```

##### `-lang`
```Parameters
Type: String
Required: false
Default value: Automatically detected 
```

##### `-mimeType`
```Parameters
Type: String
Required: false
Default value: "text/plain"
```

#### <a name="supported-mime-types"></a>Unterstützte MIME-Typen

| MIME-Typ | BESCHREIBUNG |
| --------- | ----------- |
| text/plain | Nur-Text. |
| text/html | HTML-Inhalt. [Weitere Informationen](#html-support)|
| application/mathml+xml | Mathematische Markupsprache (MathML). [Weitere Informationen](./how-to/display-math.md)
| application/vnd.openxmlformats-officedocument.wordprocessingml.document | Dokument im DOCX-Format von Microsoft Word.


<br>

## <a name="options"></a>Tastatur

Enthält Eigenschaften, die bestimmte Verhaltensweisen des Plastischen Readers konfigurieren.

```typescript
{
    uiLang?: string;
    timeout?: number;
    uiZIndex?: number;
    useWebview?: boolean;
    onExit?: () => any;
    allowFullscreen?: boolean;
    hideExitButton?: boolean;
    cookiePolicy?: CookiePolicy;
    disableFirstRun?: boolean;
    readAloudOptions?: ReadAloudOptions;
    translationOptions?: TranslationOptions;
    displayOptions?: DisplayOptions;
    preferences?: string;
    onPreferencesChanged?: (value: string) => any;
    customDomain?: string;
}
```

#### <a name="options-parameters"></a>Optionsparameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| uiLang | String | Sprache der Benutzeroberfläche, der Wert ist im IETF BCP 47-Sprachtagformat angegeben, z. B. „en“, „es-ES“. Standardmäßig wird die Browsersprache verwendet, falls nicht angegeben. |
| timeout | Nr. | Dauer (in Millisekunden) bevor bei [launchAsync](#launchasync) ein Timeoutfehler auftritt (Standard ist 15000 ms). Dieser Timeout gilt nur für den anfänglichen Start der Seite „Reader“, wobei der Erfolg beim Öffnen der Seite „Reader“ und dem Start des Drehfelds beobachtet wird. Die Anpassung des Timeouts sollte nicht erforderlich sein. |
| uiZIndex | Nr. | Der Z-Index von „iFrame“, das erstellt wird (der Standardwert ist 1000). |
| useWebview | Boolean| Verwenden Sie ein webview-Tag anstelle von „iframe“, um die Kompatibilität mit Chrome-Apps sicherzustellen (der Standardwert ist „false“). |
| onExit | Funktion | Wird ausgeführt, wenn der plastische Reader beendet wird. |
| allowFullscreen | Boolean | Die Möglichkeit zum Vollbildschirm zu wechseln (der Standardwert ist „true“). |
| hideExitButton | Boolean | Ob der Pfeil der Schaltfläche „Beenden“ des plastischen Readers ausgeblendet werden soll (der Standardwert ist „false“). Dies sollte nur dann „true“ ergeben, wenn es einen alternativen Mechanismus zum Beenden des plastischen Readers gibt (z. B. den Pfeil „Zurück“ einer mobilen Symbolleiste). |
| cookiePolicy | [CookiePolicy](#cookiepolicy-options) | Einstellung für die Verwendung von Cookies für den plastischen Reader (der Standardwert ist *CookiePolicy.Disable*). Es liegt in der Verantwortung der Hostanwendung, die erforderliche Zustimmung des Benutzers in Übereinstimmung mit der Cookiekonformitätsrichtlinie der EU einzuholen. Weitere Informationen finden Sie unter den [Optionen für Cookierichtlinien](#cookiepolicy-options). |
| disableFirstRun | Boolean | Deaktiviert die zuerst ausgeführte Umgebung. |
| readAloudOptions | [ReadAloudOptions](#readaloudoptions) | Optionen zum Konfigurieren des lauten Vorlesens. |
| translationOptions | [TranslationOptions](#translationoptions) | Optionen zum Konfigurieren der Übersetzung. |
| displayOptions | [DisplayOptions](#displayoptions) | Optionen zum Konfigurieren von Textgröße, Schriftart usw. |
| Voreinstellungen | String | Von onPreferencesChanged zurückgegebene Zeichenfolge, die die Voreinstellungen des Benutzers im plastischen Reader darstellt. Weitere Informationen finden Sie unter [Parameter für Einstellungen](#settings-parameters) und [Vorgehensweise: Speichern der Benutzereinstellungen](./how-to-store-user-preferences.md). |
| onPreferencesChanged | Funktion | Wird ausgeführt, wenn sich die Voreinstellungen des Benutzers geändert haben. Weitere Informationen finden Sie unter [Vorgehensweise: Speichern der Benutzereinstellungen](./how-to-store-user-preferences.md). |
| customDomain | String | Für die interne Verwendung reserviert. Benutzerdefinierte Domäne, in der die Web-App des plastischen Readers gehostet wird (der Standardwert ist „NULL“). |

##### `-uiLang`
```Parameters
Type: String
Required: false
Default value: User's browser language 
```

##### `-timeout`
```Parameters
Type: Number
Required: false
Default value: 15000
```

##### `-uiZIndex`
```Parameters
Type: Number
Required: false
Default value: 1000
```

##### `-onExit`
```Parameters
Type: Function
Required: false
Default value: null
```

##### `-preferences`

> [!CAUTION]
> **WICHTIG** Versuchen Sie nicht, die Werte der `-preferences`-Zeichenfolge, die an die und von der Plastischer Reader-Anwendung gesendet werden, programmgesteuert zu ändern, da dies zu unerwartetem Verhalten führen kann, das die Benutzerfreundlichkeit für Ihre Kunden verringert.

```Parameters
Type: String
Required: false
Default value: null
```

##### `-onPreferencesChanged`
```Parameters
Type: Function
Required: false
Default value: null
```

##### `-customDomain`
```Parameters
Type: String
Required: false
Default value: null
```

<br>

## <a name="readaloudoptions"></a>ReadAloudOptions

```typescript
type ReadAloudOptions = {
    voice?: string;
    speed?: number;
    autoplay?: boolean;
};
```

#### <a name="readaloudoptions-parameters"></a>ReadAloudOptions-Parameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| voice | String | Stimme, entweder „Weiblich“ oder „Männlich“. Beachten Sie, dass nicht alle Sprachen beide Geschlechter unterstützen. |
| Geschwindigkeit | Nr. | Die Wiedergabegeschwindigkeit muss zwischen 0,5 und 2,5 (einschließlich) liegen. |
| autoPlay | Boolean | Starten Sie automatisch das laute Vorlesen, wenn der plastische Reader geladen wird. |

##### `-voice`
```Parameters
Type: String
Required: false
Default value: "Female" or "Male" (determined by language) 
Values available: "Female", "Male"
```

##### `-speed`
```Parameters
Type: Number
Required: false
Default value: 1
Values available: 0.5, 0.75, 1, 1.25, 1.5, 1.75, 2, 2.25, 2.5
```

> [!NOTE]
> Aufgrund von Browsereinschränkungen wird die automatische Wiedergabe in Safari nicht unterstützt.

<br>

## <a name="translationoptions"></a>TranslationOptions

```typescript
type TranslationOptions = {
    language: string;
    autoEnableDocumentTranslation?: boolean;
    autoEnableWordTranslation?: boolean;
};
```

#### <a name="translationoptions-parameters"></a>TranslationOptions-Parameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| language | String | Legt die Übersetzungssprache fest, der Wert ist im IETF BCP 47-Sprachtagformat angegeben, z. B. „fr-FR“, „es-MX“, „zh-Hans-CN“. Erforderlich zur automatischen Aktivierung der Wort- oder Dokumentübersetzung. |
| autoEnableDocumentTranslation | Boolean | Übersetzen Sie automatisch das gesamte Dokument. |
| autoEnableWordTranslation | Boolean | Aktivieren Sie automatisch die Übersetzung von Wörtern. |

##### `-language`
```Parameters
Type: String
Required: true
Default value: null 
Values available: See the Supported Languages section
```

<br>

## <a name="displayoptions"></a>DisplayOptions

```typescript
type DisplayOptions = {
    textSize?: number;
    increaseSpacing?: boolean;
    fontFamily?: string;
};
```

#### <a name="displayoptions-parameters"></a>DisplayOptions-Parameter

| Name | Typ | Beschreibung |
| ---- | ---- |------------ |
| textSize | Nr. | Legt die gewählte Textgröße fest. |
| increaseSpacing | Boolean | Legt fest, ob der Textabstand aktiviert oder deaktiviert wird. |
| fontFamily | String | Legt die gewählte Schriftart fest („Calibri“, „ComicSans“ oder „Sitka“). |

##### `-textSize`
```Parameters
Type: Number
Required: false
Default value: 20, 36 or 42 (Determined by screen size)
Values available: 14, 20, 28, 36, 42, 48, 56, 64, 72, 84, 96
```

##### `-fontFamily`
```Parameters
Type: String
Required: false
Default value: "Calibri"
Values available: "Calibri", "Sitka", "ComicSans"
```

<br>

## <a name="cookiepolicy-options"></a>CookiePolicy-Optionen

```typescript
enum CookiePolicy { Disable, Enable }
```

**Die unten aufgeführten Einstellungen dienen nur zu Informationszwecken**. Der plastische Reader speichert seine Einstellungen bzw. Benutzervoreinstellungen in Cookies. Diese *cookiePolicy*-Option **deaktiviert** standardmäßig die Verwendung von Cookies, um den Gesetzen der Cookiekonformitätsrichtlinie der EU zu entsprechen. Wenn Sie Cookies wieder aktivieren und die Standardfunktionalität für die Benutzervoreinstellungen des plastischen Reader wiederherstellen möchten, müssen Sie sicherstellen, dass Ihre Website oder Anwendung die ordnungsgemäße Zustimmung des Benutzers zur Aktivierung von Cookies einholt. Um Cookies im plastischen Reader wieder zu aktivieren, müssen Sie dann beim Start des plastischen Readers die Option *cookiePolicy* explizit auf *CookiePolicy.Enable* festlegen. Die folgende Tabelle beschreibt, welche Einstellungen der plastische Reader in seinem Cookie speichert, wenn die Option *cookiePolicy* aktiviert ist.

#### <a name="settings-parameters"></a>Parameter für Einstellungen

| Einstellung | Typ | Beschreibung |
| ------- | ---- | ----------- |
| textSize | Nr. | Legt die gewählte Textgröße fest. |
| fontFamily | String | Legt die gewählte Schriftart fest („Calibri“, „ComicSans“ oder „Sitka“). |
| textSpacing | Nr. | Legt fest, ob der Textabstand aktiviert oder deaktiviert wird. |
| formattingEnabled | Boolean | Legt fest, ob die HTML-Formatierung aktiviert oder deaktiviert wird. |
| Design | String | Legt das gewählte Thema fest (z. B. „Hell“, „Dunkel“...). |
| syllabificationEnabled | Boolean | Legt fest, ob die Silbentrennung aktiviert oder deaktiviert wird. |
| nounHighlightingEnabled | Boolean | Legt fest, ob die Hervorhebung von Nomen aktiviert oder deaktiviert ist. |
| nounHighlightingColor | String | Legt die gewählte Farbe für die Hervorhebung der Nomen fest. |
| verbHighlightingEnabled | Boolean | Legt fest, ob die Hervorhebung von Verben aktiviert oder deaktiviert wird. |
| verbHighlightingColor | String | Legt die gewählte Farbe für die Hervorhebung der Verben fest. |
| adjectiveHighlightingEnabled | Boolean | Legt fest, ob die Hervorhebung von Adjektiven aktiviert oder deaktiviert wird. |
| adjectiveHighlightingColor | String | Legt die gewählte Farbe für die Hervorhebung der Adjektive fest. |
| adverbHighlightingEnabled | Boolean | Legt fest, ob die Hervorhebung von Adverben aktiviert oder deaktiviert wird. |
| adverbHighlightingColor | String | Legt die gewählte Farbe für die Hervorhebung der Adverben fest. |
| pictureDictionaryEnabled | Boolean | Legt fest, ob das Bildwörterbuch aktiviert oder deaktiviert wird. |
| posLabelsEnabled | Boolean | Legt fest, ob die hochgestellte Beschriftung jedes hervorgehobenen Satzteils aktiviert oder deaktiviert wird.  |

<br>

## <a name="supported-languages"></a>Unterstützte Sprachen

Das Übersetzungsfeature des plastischen Readers unterstützt viele Sprachen. Weitere Informationen finden Sie [in diesem Artikel](https://www.onenote.com/learningtools/languagesupport) .

<br>

## <a name="html-support"></a>HTML-Unterstützung

Wenn die Formatierung aktiviert ist, wird der folgende Inhalt im plastischen Reader als HTML gerendert.

| HTML | Unterstützter Inhalt |
| --------- | ----------- |
| Schriftschnitte | Bold, Italic, Underline, Code, Strikethrough, Superscript, Subscript |
| Unsortierte Listen | Disc, Circle, Square |
| Sortierte Listen | Decimal, Upper-Alpha, Lower-Alpha, Upper-Roman, Lower-Roman |

Nicht unterstützte Tags werden vergleichbar gerendert. Bilder und Tabellen werden derzeit nicht unterstützt.

<br>

## <a name="browser-support"></a>Browserunterstützung

Verwenden Sie die neuesten Versionen der folgenden Browser, um mit dem Plastischen Reader optimale Ergebnisse zu erzielen.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

<br>

## <a name="next-steps"></a>Nächste Schritte

* Schauen Sie sich das [SDK für den plastischen Reader auf GitHub](https://github.com/microsoft/immersive-reader-sdk) an.
* [Schnellstart: Erstellen einer Web-App zum Starten des Plastischen Readers (C#)](./quickstarts/client-libraries.md?pivots=programming-language-csharp)
