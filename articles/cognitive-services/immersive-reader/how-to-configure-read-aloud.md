---
title: Konfigurieren des Vorlesens
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die verschiedenen Optionen für das Vorlesen konfigurieren.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 8b45fe07b4bd42059199197bc5b99f20f6b2a8cf
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102608715"
---
# <a name="how-to-configure-read-aloud"></a>Konfigurieren des Vorlesens

In diesem Artikel wird veranschaulicht, wie Sie die verschiedenen Optionen für das Vorlesen im plastischen Reader konfigurieren.

## <a name="automatically-start-read-aloud"></a>Automatisches Starten des Vorlesens

Der Parameter `options` enthält alle Flags, die zum Konfigurieren des Vorlesens verwendet werden können. Legen Sie `autoplay` auf `true` fest, um das automatische Starten des Vorlesens nach dem Start des plastischen Readers zu aktivieren.

```typescript
const options = {
    readAloudOptions: {
        autoplay: true
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

> [!NOTE]
> Aufgrund von Browsereinschränkungen wird die automatische Wiedergabe in Safari nicht unterstützt.

## <a name="configure-the-voice"></a>Konfigurieren der Stimme

Legen Sie `voice` entweder auf `male` oder `female` fest. Nicht alle Sprachen unterstützen beide Stimmen. Weitere Informationen finden Sie auf der Seite [Sprachunterstützung](./language-support.md).

```typescript
const options = {
    readAloudOptions: {
        voice: 'female'
    }
};
```

## <a name="configure-playback-speed"></a>Konfigurieren der Wiedergabegeschwindigkeit

Legen Sie `speed` auf einen Wert im Bereich `0.5` (50 %) und `2.5` (250 %) fest. Werte außerhalb dieses Bereichs werden entweder auf 0,5 oder 2,5 festgelegt.

```typescript
const options = {
    readAloudOptions: {
        speed: 1.5
    }
};
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).