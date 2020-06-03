---
title: azcopy remove | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy remove“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 05/04/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: ab085b9a41120a9f56c1c2e39a89def8c3893747
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/30/2020
ms.locfileid: "84221084"
---
# <a name="azcopy-remove"></a>azcopy remove

Löscht Blobs oder Dateien aus einem Azure Storage-Konto.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>Verwandte konzeptionelle Artikel

- [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md)
- [Übertragen von Daten mit AzCopy und Blobspeicher](storage-use-azcopy-blobs.md)
- [Übertragen von Daten mit AzCopy und Dateispeicher](storage-use-azcopy-files.md)
- [Konfigurieren, Optimieren und Problembehandlung in AzCopy](storage-use-azcopy-configure.md)

## <a name="examples"></a>Beispiele

Entfernen Sie einen einzelnen Blob mit SAS:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Entfernen Sie ein gesamtes virtuelles Verzeichnis mit einer SAS (Shared Access Signature):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Entfernen Sie nur die obersten Blobs in einem virtuellen Verzeichnis, aber nicht die zugehörigen Unterverzeichnisse:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Entfernen Sie eine Teilmenge von Blobs in einem virtuellen Verzeichnis (z.B. nur JPG- und PDF-Dateien oder wenn der Blobname „exactName“ lautet):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include-pattern="*.jpg;*.pdf;exactName"
```

Entfernen Sie ein gesamtes virtuelles Verzeichnis, doch schließen Sie bestimmte Blobs aus dem Umfang aus (z.B. alle Blobs, die mit „foo“ beginnen oder mit „bar“ enden):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude-pattern="foo*;*bar"
```

Entfernen Sie bestimmte Blobs und virtuelle Verzeichnisse, indem Sie ihre relativen Pfade (NICHT URL-codiert) in eine Datei einfügen:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/parent/dir]" --recursive=true --list-of-files=/usr/bar/list.txt
file content:
  dir1/dir2
  blob1
  blob2

```

Entfernen Sie eine einzelne Datei aus einem Blob Storage-Konto, das über einen hierarchischen Namespace verfügt („include“/„exclude“ nicht unterstützt).

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Entfernen Sie ein einzelnes Verzeichnis aus einem Blob Storage-Konto, das über einen hierarchischen Namespace verfügt („include“/„exclude“ nicht unterstützt):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Tastatur

**--delete-snapshots**-Zeichenfolge: Standardmäßig schlagen Löschvorgänge fehl, wenn ein Blob über Momentaufnahmen verfügt. Geben Sie „include“ an, um das Stammblob und alle zugehörigen Momentaufnahmen zu entfernen. Alternativ können Sie auch „only“ angeben, um nur die Momentaufnahmen zu entfernen und den Stammblob zu behalten.

**--exclude-path**-Zeichenfolge: schließt diese Pfade beim Entfernen aus. Diese Option unterstützt keine Platzhalterzeichen (*). Überprüft das Präfix des relativen Pfads. Beispiel: myFolder;myFolder/subDirName/file.pdf

**--exclude-pattern string**   Schließt Dateien aus, deren Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName

**--force-if-read-only**: erzwingt beim Löschen von Azure Files-Dateien oder -Ordnern, dass der Löschvorgang ausgeführt wird, auch wenn für das vorhandene Objekt das Nur-Lese-Attribut festgelegt ist

**-h, --help**: Hilfe beim Entfernen

**--include-path** string      Schließt nur diese Pfade beim Entfernen ein. Diese Option unterstützt keine Platzhalterzeichen (*). Überprüft das Präfix des relativen Pfads. Beispiel: myFolder;myFolder/subDirName/file.pdf

**--include-pattern** string   Schließt nur Dateien ein, deren Name der Musterliste entspricht. Beispiel: *.jpg;* .pdf;exactName

**--list-of-files** string     Definiert den Speicherort einer Datei, die die Liste der zu löschenden Dateien und Verzeichnisse enthält. Die relativen Pfade sollten durch Zeilenumbrüche getrennt werden, und die Pfade sollten NICHT URL-codiert sein.

**--log-level** string         Definiert, wie ausführlich die Protokolldatei sein soll. Folgende Ebenen sind verfügbar: INFO (alle Anforderungen/Antworten), WARNING (langsame Antworten), ERROR (nur fehlgeschlagene Anforderungen) und NONE (keine Ausgabeprotokolle). (Der Standardwert lautet „INFO“.) (Standardwert: „INFO“)

**--recursive**                Überprüft Unterverzeichnisse bei der Synchronisierung zwischen Verzeichnissen rekursiv.

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|
|--trusted-microsoft-suffixes-Zeichenfolge   |Gibt zusätzliche Domänensuffixe an, an die Azure Active Directory-Anmeldetoken gesendet werden können.  Der Standardwert ist ' *.core.windows.net;* .core.chinacloudapi.cn; *.core.cloudapi.de;* .core.usgovcloudapi.net'. Alle hier aufgelisteten Werte werden zum Standardwert hinzugefügt. Aus Sicherheitsgründen sollten Sie hier nur Microsoft Azure-Domänen platzieren. Trennen Sie mehrere E-Mail-Adressen durch Semikolons voneinander.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
