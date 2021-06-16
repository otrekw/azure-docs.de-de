---
title: Konfigurieren des Vorlesens
titleSuffix: Azure Applied AI Services
description: In diesem Artikel erfahren Sie, wie Sie die verschiedenen Optionen für das Vorlesen konfigurieren.
author: metanMSFT
manager: guillasi
ms.service: applied-ai-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 2694525c406a40a4c133eb3199d2222542905ae6
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/10/2021
ms.locfileid: "111893388"
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