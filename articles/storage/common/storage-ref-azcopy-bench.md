---
title: azcopy bench | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy bench“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 40ff6c6c76e255945681e678ef296ffcf9978f61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84485185"
---
# <a name="azcopy-benchmark"></a>azcopy bench

Führt einen Leistungsvergleichstest aus, indem Testdaten an ein angegebenes Ziel hochgeladen werden. Die Testdaten werden automatisch generiert.

Mit dem Befehl „bench“ wird derselbe Uploadvorgang wie mit „copy“ ausgeführt. Allerdings bestehen folgende Unterschiede:

  - Es gibt keinen Quellparameter.  Der Befehl erfordert nur eine Ziel-URL. 
  
  - Die Nutzlast wird durch Befehlszeilenparameter definiert, die steuern, wie viele Dateien automatisch generiert werden und wie groß diese Dateien sind. Die Dateien werden vollständig im Arbeitsspeicher generiert. Es wird kein Datenträger verwendet.
  
  - Es werden nur einige der optionalen Parameter unterstützt, die für den Befehl „copy“ verfügbar sind.
  
  - Es werden zusätzliche Diagnosen ausgeführt und ausgegeben.
  
  - Die übertragenen Daten werden am Ende des Testlaufs standardmäßig gelöscht.

Der Vergleichstestmodus wird automatisch auf die Anzahl der parallelen TCP-Verbindungen abgestimmt, die maximalen Durchsatz gewährleisten. Die Anzahl wird am Ende angezeigt. Um die automatische Abstimmung zu verhindern, legen Sie die Umgebungsvariable AZCOPY_CONCURRENCY_VALUE auf eine bestimmte Anzahl von Verbindungen fest.

Alle üblichen Authentifizierungstypen werden unterstützt. Der einfachste Ansatz für Vergleichstests besteht normalerweise jedoch darin, einen leeren Container mit einem SAS-Token zu erstellen und die SAS-Authentifizierung zu verwenden.

## <a name="examples"></a>Beispiele

```azcopy
azcopy benchmark [destination] [flags]
```

Ausführen eines Vergleichstests mit Standardparametern (geeignet für das Testen von Netzwerken mit bis zu 1 GBit/s):

- azcopy bench "https://[account].blob.core.windows.net/[container]?<SAS>"

Ausführen eines Vergleichstests, bei dem 100 Dateien von jeweils 2 GiB hochgeladen werden (geeignet für das Testen von schnellen Netzwerken, z. B. mit 10 GBit/s):

- azcopy bench "https://[account].blob.core.windows.net/[container]?<SAS>"--file-count 100 --size-per-file 2G

Ausführen eines Vergleichstests, allerdings werden 50.000 Dateien von jeweils 8 MiB verwendet und deren MD5-Hashes berechnet. (Funktioniert auf dieselbe Weise wie mit dem `--put-md5`-Flag im Befehl „copy“.) Mit `--put-md5` soll bei Vergleichstests ermittelt werden, ob die MD5-Berechnung den Durchsatz der ausgewählten Dateianzahl und -größe beeinflusst:

- azcopy bench "https://[account].blob.core.windows.net/[container]?<SAS>" --file-count 50000 --size-per-file 8M --put-md5

## <a name="options"></a>Tastatur

**--blob-type** string  Definiert den Typ des Blobs am Ziel. Wird verwendet, um Vergleichstests für unterschiedliche Blobtypen zu ermöglichen. Identisch mit dem gleichnamigen Parameter im Befehl „copy“ (Standardwert: „Detect“).

**--block-size-mb** float  Verwendet diese Blockgröße (in MiB angegeben). Der Standardwert wird anhand der Dateigröße automatisch berechnet. Dezimalzahlen sind zulässig, z. B. 0,25. Identisch mit dem gleichnamigen Parameter im Befehl „copy“.

**--delete-test-data**  Falls „true“, werden die Vergleichsdaten am Ende des Vergleichstests gelöscht.  Legen Sie den Wert auf „false“ fest, wenn die Daten am Ziel erhalten bleiben sollen, beispielsweise für manuelle Tests außerhalb des Vergleichstestmodus (Standardwert: „true“).

**--file-count** uint  Die Anzahl der zu verwendenden automatisch generierten Datendateien (standardmäßig 100).

**-h, --help**  Hilfe zu „bench“

**--log-level** string  Definiert, wie ausführlich die Protokolldatei sein soll. Verfügbare Stufen: INFO (alle Anforderungen/Antworten), WARNING (langsame Antworten), ERROR (nur fehlgeschlagene Anforderungen) und NONE (keine Ausgabeprotokolle). (Standardwert: „Info“)

**--put-md5**  Erstellt einen MD5-Hash jeder Datei und speichert den Hash als „Content-MD5“-Eigenschaft des Zielblobs bzw. der Zieldatei. (Standardmäßig wird der Hash NICHT erstellt.) Identisch mit dem gleichnamigen Parameter im Befehl „copy“.

**--size-per-file** string  Die Größe jeder automatisch generierten Datendatei. Ein numerischer Wert, auf den unmittelbar K, M oder G folgen muss. Beispiel: 12k oder 200G (Standardwert: „250M“)

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

**--cap-mbps uint32**  Begrenzt die Übertragungsrate (in Megabits pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.

**--output-type** string  Das Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“. (Standardwert: „text“)

**--trusted-microsoft-suffixes** string   Gibt zusätzliche Domänensuffixe an, an die Azure Active Directory-Anmeldetoken gesendet werden können.  Der Standardwert ist ' *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net'. Alle hier aufgelisteten Werte werden zum Standardwert hinzugefügt. Aus Sicherheitsgründen sollten Sie hier nur Microsoft Azure-Domänen platzieren. Trennen Sie mehrere E-Mail-Adressen durch Semikolons voneinander.

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
