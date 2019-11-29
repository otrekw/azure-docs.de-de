---
title: azcopy jobs resume | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs resume“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 3ee7879475801660b5200dddca88a0a81b2b6b9b
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74034141"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Setzt den vorhandenen Auftrag mit der angegebenen Auftrags-ID fort.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blob Storage](storage-use-azcopy-blobs.md)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|–destination-sas string|Ziel-SAS des Ziels für die angegebene JobId.|
|–exclude string|Filter: Schließen Sie diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags aus. Dateien sollten durch „;“ getrennt werden.|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „resume“.|
|–include string|Filter: Schließen Sie nur diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags ein. Dateien sollten durch „;“ getrennt werden.|
|–source-sas string |Quell-SAS der Quelle für die angegebene JobId.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
