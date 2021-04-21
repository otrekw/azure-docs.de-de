---
title: azcopy jobs resume | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy jobs resume“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 497aa3a77397a8a587badc0911176cd092eb1f23
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107503336"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Setzt den vorhandenen Auftrag mit der angegebenen Auftrags-ID fort.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blobspeicher](./storage-use-azcopy-v10.md#transfer-data)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|–destination-sas string|Ziel-SAS des Ziels für die angegebene Auftrags-ID|
|–exclude string|Filter: Schließen Sie diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags aus. Dateien sollten durch „;“ getrennt werden.|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „resume“.|
|–include string|Filter: Schließen Sie nur diese fehlgeschlagene(n) Übertragung(en) bei der Fortsetzung des Auftrags ein. Dateien sollten durch „;“ getrennt werden.|
|–source-sas string |Quell-SAS der Quelle für die angegebene Auftrags-ID|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|--cap-mbps float|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|
|--trusted-microsoft-suffixes-Zeichenfolge   |Gibt zusätzliche Domänensuffixe an, an die Azure Active Directory-Anmeldetoken gesendet werden können.  Der Standardwert ist ' *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net'. Alle hier aufgelisteten Werte werden zum Standardwert hinzugefügt. Aus Sicherheitsgründen sollten Sie hier nur Microsoft Azure-Domänen platzieren. Trennen Sie mehrere E-Mail-Adressen durch Semikolons voneinander.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy jobs](storage-ref-azcopy-jobs.md)
