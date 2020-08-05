---
title: azcopy jobs clean | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs clean“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f3e9d70ced0d2974a66717436c28c5b6914f6745
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87287143"
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

## <a name="options"></a>Tastatur

**--help**                Hilfe zu „clean“

**--with-status** Zeichenfolge   Entfernt nur die Aufträge mit dem jeweiligen Status. Verfügbare Werte: `Canceled`, `Completed`, `Failed`, `InProgress`, `All` (Standardwert `All`)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps float**      Begrenzt die Übertragungsrate (in Megabits pro Sekunde) Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string   Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

**--trusted-microsoft-suffixes** string   Gibt zusätzliche Domänensuffixe an, an die Azure Active Directory-Anmeldetoken gesendet werden können.  Der Standardwert ist ' *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net'. Alle hier aufgelisteten Werte werden zum Standardwert hinzugefügt. Aus Sicherheitsgründen sollten Sie hier nur Microsoft Azure-Domänen platzieren. Trennen Sie mehrere E-Mail-Adressen durch Semikolons voneinander.

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
