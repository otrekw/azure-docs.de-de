---
title: azcopy jobs list | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs list“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: e61177c6a216c4a43f17d9725034cbe3000604e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "74037558"
---
# <a name="azcopy-jobs-list"></a>azcopy jobs list

Zeigt Informationen zu allen Aufträgen an.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy jobs list [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blobspeicher](storage-use-azcopy-blobs.md)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Tastatur

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „list“.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
