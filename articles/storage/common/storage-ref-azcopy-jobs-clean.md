---
title: azcopy jobs clean | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs clean“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7ae14c3606dfe6bffa8481682843f3f2e85c2131
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74033723"
---
# <a name="azcopy-jobs-clean"></a>azcopy jobs clean

Entfernen aller Protokoll- und Plandateien für alle Aufträge

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blobspeicher](storage-use-azcopy-blobs.md)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Beispiele

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Optionen

**-h, --help**                Hilfe zu „clean“

**--with-status** string   Entfernt nur die Aufträge mit dem jeweiligen Status. Verfügbare Werte: „Canceled“, „Completed“, „Failed“, „InProgress“, „All“ (Standardwert: „All“)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps uint32**      Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string   Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
