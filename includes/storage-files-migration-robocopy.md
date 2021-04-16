---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 4/05/2021
ms.author: fauhse
ms.custom: include file
ms.openlocfilehash: 01512f0e37761915fcb6eeba8c162e1a3db62c48
ms.sourcegitcommit: bfa7d6ac93afe5f039d68c0ac389f06257223b42
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106491672"
---
```console
Robocopy /MT:16 /R:5 /W:5 /B /MIR /IT /COPY:DATSO /DCOPY:DAT /NP /NFL /NDL /UNILOG:<FilePathAndName> <SourcePath> <Dest.Path> 
```

| Schalter                | Bedeutung |
|-----------------------|---------|
| `/MT:n`               | Ermöglicht RoboCopy die Multithread-Ausführung. Für „n“ wird standardmäßig 8 verwendet, und der Höchstwert für Threads beträgt 128. Passen Sie diesen Wert an die Anzahl Ihrer Prozessorkerne und die Threadanzahl pro Kern an. Berücksichtigen Sie, ob Kerne für andere Aufgaben reserviert werden müssen, die ein Produktionsserver möglicherweise hat. |
| `/R:n`                | Die Geschwindigkeit einer RoboCopy-Ausführung kann gesteigert werden, indem Sie eine maximale Anzahl von Wiederholungsversuchen für eine Datei angeben, die beim ersten Versuch nicht kopiert werden kann. n = maximale Anzahl von Wiederholungen, bevor bei der Datei ein dauerhafter Fehler beim Kopieren in diese RoboCopy-Ausführung auftritt. Dieser Switch funktioniert am besten in Szenarien, in denen bereits klar ist, dass es mehr RoboCopy-Ausführungen gibt. Falls die Datei in dieser Ausführung nicht kopiert werden kann, wird dies beim nächsten RoboCopy-Auftrag erneut versucht. Mit diesem Ansatz ist der Kopiervorgang für Dateien, die aufgrund einer Nutzung oder von Timeoutproblemen nicht kopiert werden können, in vielen Fällen schließlich erfolgreich. |
| `/W:n`                | Dieser Switch gibt die Zeit an, die RoboCopy wartet, bevor versucht wird, eine Datei zu kopieren, die beim letzten Versuch nicht erfolgreich kopiert wurde. n = Anzahl der Sekunden, für die zwischen Wiederholungen gewartet werden soll. `/W:n` wird häufig in Verbindung mit `/R:n` verwendet. |
| `/B`                  | Führt RoboCopy in dem Modus aus, den auch eine Sicherungsanwendung verwenden würde. Diese Option ermöglicht RoboCopy das Verschieben von Dateien, für die der aktuelle Benutzer keine Berechtigungen hat. |
| `/MIR`                | Die Option *Quelle im Ziel spiegeln* ermöglicht es RoboCopy, nur Deltas zwischen der Quelle und dem Ziel kopieren zu müssen. Leere Unterverzeichnisse werden kopiert. Elemente (Dateien oder Ordner), die geändert wurden oder nicht im Ziel vorhanden sind, werden kopiert. Elemente, die im Ziel, aber nicht in der Quelle vorhanden sind, werden aus dem Ziel gelöscht. Bei Verwendung dieser Option müssen Sie berücksichtigen, dass die Quell- und Zielordnerstruktur exakt abgeglichen werden. Dieser „Abgleich“ bedeutet, dass Sie aus der richtigen Quelle und Ordnerebene in die entsprechende Ordnerebene im Ziel kopieren. Nur dann kann ein Kopiervorgang zum „Aufholen“ erfolgreich durchgeführt werden. Wenn die Quelle und das Ziel nicht übereinstimmen, führt die Verwendung von `/MIR` zu umfassenden Lösch- und wiederholten Kopiervorgängen. |
| `/IT`                 | Sorgt dafür, dass die Genauigkeit in bestimmten Spiegelungsszenarien beibehalten wird. </br>*Beispiel:* Wenn bei einer Datei zwischen zwei RoboCopy-Ausführungen eine ACL-Änderung und ein Attributupdate auftreten, wird diese als *ausgeblendet* gekennzeichnet. Ohne „/IT“ bemerkt RoboCopy die ACL-Änderung möglicherweise nicht und führt daher auch keine Übertragung an den Zielspeicherort aus. |
|`/COPY:[copyflags]`    | Die Genauigkeit der Dateikopie (Standard: `/COPY:DAT`), Kopierflags: `D` = Daten, `A` = Attribute, `T` = Zeitstempel, `S` = Sicherheit = NTFS-Zugriffssteuerungsliste, `O` = Besitzerinformationen, `U` = Überwach<u>u</u>ngsinformationen. Überwachungsinformationen können nicht in einer Azure-Dateifreigabe gespeichert werden. |
| `/DCOPY:[copyflags]`  | Die Genauigkeit für die Kopie der Verzeichnisse (Standard: `/DCOPY:DA`), Kopierflags: `D` = Daten, `A` = Attribute, `T` = Zeitstempel. |
| `/NP`                 | Der Kopierfortschritt wird für alle Dateien und Ordner nicht angezeigt. Das Anzeigen des Fortschritts reduziert die Kopierleistung erheblich. |
| `/NFL`                | Gibt an, dass Dateinamen nicht protokolliert werden. Hiermit wird die Kopierleistung verbessert. |
| `/NDL`                | Gibt an, dass Verzeichnisnamen nicht protokolliert werden. Hiermit wird die Kopierleistung verbessert. |
| `/UNILOG:<file name>` | Gibt den Status als UNICODE in die LOG-Datei aus (überschreibt vorhandenes Protokoll). |
| `/LFSM`               | **Nur für Ziele mit mehrstufigem Speicher vorgesehen** </br>Bei Verwendung von „/LFSM“ wird von RoboCopy angefordert, den „Modus mit wenig freiem Speicherplatz“ zu verwenden. Diese Option ist nur für Ziele mit mehrstufigem Speicher nützlich, bei denen möglicherweise der lokale Speicherplatz aufgebraucht ist, bevor die RoboCopy-Ausführung fertiggestellt wird. Diese Option wurde spezifisch für die Verwendung mit einem Ziel mit dem Cloudtiering der Azure-Dateisynchronisierung. Sie kann unabhängig von der Azure-Dateisynchronisierung verwendet werden. In diesem Modus pausiert die RoboCopy-Ausführung immer, wenn ein Dateikopiervorgang dazu führen würde, dass der freie Speicherplatz des Zielvolumes einen bestimmten Schwellenwert unterschreitet. Dieser Wert kann mit dem `/LFSM:n`-Formular des Flags festgelegt werden. Der Parameter `n` wird im Dualsystem festgelegt: `nKB`, `nMB` oder `nGB`. Wenn `/LFSM` ohne einen expliziten Schwellenwert festgelegt wird, wird der Schwellenwert auf 10 % der Größe des Zielvolumes festgelegt. Der Modus für niedrigen Speicherplatz ist nicht mit „/MT“, „/EFSRAW“, „/B“ und „/ZB“ kompatibel. |
| `/Z`                  | **Vorsichtig verwenden** </br>Mit dieser Option werden Dateien im Neustartmodus kopiert. Die Verwendung dieser Option wird nur in instabilen Netzwerkumgebungen empfohlen. Diese Option reduziert die Kopierleistung aufgrund der zusätzlichen Protokollierung erheblich. |
| `/ZB`                 | **Vorsichtig verwenden** </br>Mit dieser Option wird der Neustartmodus verwendet. Wenn der Zugriff verweigert wird, wird der Sicherungsmodus verwendet. Diese Option reduziert die Kopierleistung aufgrund der Prüfpunkte erheblich. |
   