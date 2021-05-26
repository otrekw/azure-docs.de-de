---
title: Konfigurieren der Übersetzung
titleSuffix: Azure Applied AI Services
description: In diesem Artikel erfahren Sie, wie Sie die verschiedenen Optionen für die Übersetzung konfigurieren.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 7ffcc613a9a1e5222f0f812b4a2a9e0eb774ddbc
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2021
ms.locfileid: "110377761"
---
# <a name="how-to-configure-translation"></a>Konfigurieren der Übersetzung

In diesem Artikel wird veranschaulicht, wie Sie die verschiedenen Optionen für die Übersetzung im plastischen Reader konfigurieren.

## <a name="configure-translation-language"></a>Konfigurieren der zu übersetzenden Sprache

Der Parameter `options` enthält alle Flags, die zum Konfigurieren der Übersetzung verwendet werden können. Legen Sie den Parameter `language` auf die Sprache fest, in die Sie übersetzen möchten. Die vollständige Liste unterstützter Sprachen finden Sie unter [Sprachunterstützung](./language-support.md).

```typescript
const options = {
    translationOptions: {
        language: 'fr-FR'
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="automatically-translate-the-document-on-load"></a>Automatisches Übersetzen des Dokuments beim Laden

Legen Sie `autoEnableDocumentTranslation` auf `true` fest, um das automatische Übersetzen des Dokuments beim Laden des plastischen Readers zu aktivieren.

```typescript
const options = {
    translationOptions: {
        autoEnableDocumentTranslation: true
    }
};
```

## <a name="automatically-enable-word-translation"></a>Automatisches Aktivieren der Übersetzung einzelner Wörter

Legen Sie `autoEnableWordTranslation` auf `true` fest, um die Übersetzung einzelner Wörter zu aktivieren.

```typescript
const options = {
    translationOptions: {
        autoEnableWordTranslation: true
    }
};
```

## <a name="next-steps"></a>Nächste Schritte

* Lesen Sie die [Referenz zum SDK für den plastischen Reader](./reference.md).