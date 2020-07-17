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
ms.openlocfilehash: beb053551dc1fa28672c488b31dfb29ca3b53651
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2020
ms.locfileid: "85486874"
---
# <a name="how-to-store-user-preferences"></a>Vorgehensweise: Speichern der Benutzereinstellungen

In diesem Artikel wird veranschaulicht, wie die Benutzereinstellungen gespeichert werden. Diese Funktionalität stellt eine alternative Möglichkeit zum Speichern der Benutzereinstellungen dar, wenn die Verwendung von Cookies nicht erwünscht oder nicht möglich ist.

## <a name="how-to-enable-storing-user-preferences"></a>Aktivieren der Speicherung von Benutzereinstellungen

Der Parameter `options` enthält den Rückruf `onPreferencesChanged`. Diese Funktion wird immer dann aufgerufen, wenn der Benutzer seine Einstellungen ändert (z. B. durch Ändern der Textgröße, der Designfarbe, der Schriftart usw.). Der Parameter `value` enthält eine Zeichenfolge, die die aktuellen Einstellungen des Benutzers darstellt. Diese Zeichenfolge kann mit jedem Mechanismus Ihrer Wahl gespeichert werden.

```typescript
const options = {
    onPreferencesChanged: (value: string) => {
        // Store user preferences here
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="how-to-load-user-preferences-into-the-immersive-reader"></a>Vorgehensweise beim Laden von Benutzereinstellungen in den Plastischen Reader

Übergeben Sie die Benutzereinstellungen mithilfe des `preferences`-Parameters an den Plastischen Reader. Nachfolgend finden Sie ein einfaches Beispiel zum Speichern und Laden der Benutzereinstellungen:

```typescript
let userPreferences = null;
const options = {
    preferences: userPreferences,
    onPreferencesChanged: (value: string) => {
        userPreferences = value;
    }
};
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).