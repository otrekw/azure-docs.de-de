---
title: Konfigurieren der Übersetzung
titleSuffix: Azure Cognitive Services
description: In diesem Artikel erfahren Sie, wie Sie die verschiedenen Optionen für die Übersetzung konfigurieren.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 0a6ed6ecea216a36cfc9da4ef36a2bddc69cc6a8
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2020
ms.locfileid: "92633143"
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