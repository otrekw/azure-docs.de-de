---
title: Konfigurieren des Vorlesens
titleSuffix: Azure Applied AI Services
description: In diesem Artikel erfahren Sie, wie Sie die verschiedenen Optionen für das Vorlesen konfigurieren.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 27b40631c5fdbe4f502a9e779e9a79d3050b3a3e
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110377778"
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