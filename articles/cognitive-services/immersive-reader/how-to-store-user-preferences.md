---
title: Speichern der Benutzereinstellungen
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die Benutzereinstellungen speichern.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metan
ms.openlocfilehash: ddae4a99964e438c003fe0ff0db91c5808fa7631
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91761106"
---
# <a name="how-to-store-user-preferences"></a>Vorgehensweise: Speichern der Benutzereinstellungen

In diesem Artikel wird veranschaulicht, wie die Einstellungen der Benutzeroberfläche des Benutzers (offiziell bekannt als **Benutzereinstellungen**) über die Optionen [-preferences](./reference.md#options) und [-onPreferencesChanged](./reference.md#options) des SDK des plastischen Readers gespeichert werden können.

Wenn die SDK-Option [CookiePolicy](./reference.md#cookiepolicy-options) auf *Aktiviert* festgelegt ist, speichert die Plastischer Reader-Anwendung die **Benutzereinstellungen** (Textgröße, Farbdesign, Schriftart usw.) in Cookies, die für einen bestimmten Browser und ein bestimmtes Gerät lokal gespeichert sind. Jedes Mal, wenn der Benutzer den plastischen Reader auf demselben Browser und Gerät startet, wird er mit den Einstellungen des Benutzers aus seiner letzten Sitzung auf diesem Gerät geöffnet. Wenn der Benutzer den plastischen Reader jedoch auf einem anderen Browser oder Gerät öffnet, werden die Einstellungen zunächst mit den Standardeinstellungen des plastischen Readers konfiguriert, sodass der Benutzer für jedes Gerät, das er mithilfe des plastischen Readers verwendet, seine Voreinstellungen neu festlegen muss usw. Die SDK-Optionen `-preferences` und `-onPreferencesChanged` des plastischen Readers bieten eine Möglichkeit für Anwendungen, die Voreinstellungen eines Benutzers über verschiedene Browser und Geräte hinweg per Roaming bereitzustellen, sodass der Benutzer eine konsistente Erfahrung hat, egal wo er die Anwendung verwendet.

Zunächst wird durch die Bereitstellung der SDK-Rückrufoption `-onPreferencesChanged` beim Start der Plastischer Reader-Anwendung eine `-preferences`-Zeichenfolge an die Hostanwendung zurückgesendet, wenn der Benutzer seine Einstellungen während der Sitzung des plastischen Readers ändert. Die Hostanwendung ist dann für die Speicherung der Benutzereinstellungen in ihrem eigenen System verantwortlich. Wenn derselbe Benutzer dann den plastischen Reader erneut startet, kann die Hostanwendung die Einstellungen dieses Benutzers aus dem Speicher abrufen und sie als SDK-Zeichenfolgenoption `-preferences` beim Start der Plastischer Reader-Anwendung bereitstellen, sodass die Voreinstellungen des Benutzers wiederhergestellt werden.

Diese Funktionalität stellt eine alternative Möglichkeit zum Speichern der **Benutzereinstellungen** dar, wenn die Verwendung von Cookies nicht erwünscht oder nicht möglich ist.

> [!CAUTION]
> **WICHTIG** Versuchen Sie nicht, die Werte der `-preferences`-Zeichenfolge, die an die und von der Plastischer Reader-Anwendung gesendet werden, programmgesteuert zu ändern, da dies zu unerwartetem Verhalten führen kann, das die Benutzerfreundlichkeit für Ihre Kunden verringert. Hostanwendungen sollten der Zeichenfolge `-preferences` niemals einen benutzerdefinierten Wert zuweisen oder sie manipulieren. Wenn Sie die Zeichenfolgenoption `-preferences` verwenden, verwenden Sie nur genau den Wert, der von der Rückrufoption `-onPreferencesChanged` zurückgegeben wurde.

## <a name="how-to-enable-storing-user-preferences"></a>Aktivieren der Speicherung von Benutzereinstellungen

Der Parameter [launchAsync](./reference.md#launchasync) `options` des SDKs für den plastischen Reader enthält den Rückruf `-onPreferencesChanged`. Diese Funktion wird immer dann aufgerufen, wenn der Benutzer seine Voreinstellungen ändert. Der Parameter `value` enthält eine Zeichenfolge, die die aktuellen Einstellungen des Benutzers darstellt. Diese Zeichenfolge wird dann für diesen Benutzer von der Hostanwendung gespeichert.

```typescript
const options = {
    onPreferencesChanged: (value: string) => {
        // Store user preferences here
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="how-to-load-user-preferences-into-the-immersive-reader"></a>Vorgehensweise beim Laden von Benutzereinstellungen in den Plastischen Reader

Übergeben Sie die Benutzereinstellungen mithilfe der `-preferences`-Option an den plastischen Reader. Nachfolgend finden Sie ein einfaches Beispiel zum Speichern und Laden der Benutzereinstellungen:

```typescript
const storedUserPreferences = localStorage.getItem("USER_PREFERENCES");
let userPreferences = storedUserPreferences === null ? null : storedUserPreferences;
const options = {
    preferences: userPreferences,
    onPreferencesChanged: (value: string) => {
        userPreferences = value;
        localStorage.setItem("USER_PREFERENCES", userPreferences);
    }
};
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).