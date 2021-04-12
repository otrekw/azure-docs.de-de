---
title: Problembehandlung bei Import- und Exportproblemen in Azure Import/Export | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie häufige Probleme bei der Verwendung von Azure Import/Export behandeln.
author: v-dalc
services: storage
ms.service: storage
ms.topic: troubleshooting
ms.date: 01/19/2021
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 555529b52d586078ba7e1832373ec126ba545c11
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "98705980"
---
# <a name="troubleshoot-issues-in-azure-importexport"></a>Behandeln von Problemen in Azure Import/Export
In diesem Artikel wird beschrieben, wie Sie häufige Probleme beim Importieren und Exportieren von Daten in Azure Import/Export beheben können.

## <a name="a-copy-session-failed-what-i-should-do"></a>Bei einer Kopiersitzung ist ein Fehler aufgetreten. Wie soll ich vorgehen?  

Wenn ein Fehler in einer Kopiersitzung auftritt, gibt es zwei Optionen:  
* Wenn der Fehler durch Wiederholen behoben werden kann, falls z. B. die Netzwerkfreigabe kurze Zeit offline war und nun wieder online ist, können Sie die Kopiersitzung fortsetzen.
* Wenn der Fehler nicht durch erneutes Versuchen behoben werden kann, etwa wenn Sie das falsche Quellverzeichnis der Dateien in den Befehlszeilenparametern angegeben haben, müssen Sie die Kopiersitzung abbrechen.
 
<!--For information about resuming and aborting copy sessions, see [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md  - Article we removed from TOC. File remains.-->

## <a name="i-cant-resume-or-abort-a-copy-session"></a>Ich kann eine Kopiersitzung nicht fortsetzen oder abbrechen.

Falls die Kopiersitzung die erste Kopiersitzung für ein Laufwerk ist, muss die Fehlermeldung wie folgt lauten: „The first copy session cannot be resumed or aborted.“ (Die erste Kopiersitzung kann nicht fortgesetzt oder abgebrochen werden.). In diesem Fall können Sie die alte Journaldatei löschen und den Befehl erneut ausführen.  

Wenn eine Kopiersitzung nicht die erste Kopiersitzung für ein Laufwerk ist, kann die Sitzung immer fortgesetzt oder abgebrochen werden.  

## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Ich habe die Journaldatei verloren. Kann ich den Auftrag trotzdem erstellen?

Die Journaldatei für ein Laufwerk enthält vollständige Sitzungsinformationen aus der Datenkopie. Wenn Sie dem Laufwerk Dateien hinzufügen, wird die Journaldatei verwendet, um einen Importauftrag zu erstellen. Wenn die Journaldatei verloren geht, müssen Sie alle Kopiersitzungen für das Laufwerk wiederholen.

## <a name="next-steps"></a>Nächste Schritte

* [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)
* [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-data-to-blobs.md#step-1-prepare-the-drives)
* [Überprüfen des Auftragsstatus mit Protokollkopiedateien](storage-import-export-tool-reviewing-job-status-v1.md)
* [Reparieren eines Importauftrags](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Reparieren eines Exportauftrags](storage-import-export-tool-repairing-an-export-job-v1.md)