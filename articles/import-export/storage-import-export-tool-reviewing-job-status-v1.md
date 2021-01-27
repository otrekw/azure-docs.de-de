---
title: Überprüfen des Status von Azure Import/Export-Aufträgen (V1) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Protokolldateien, die vom Import- oder Exportvorgang erstellt wurden, der Status von Aufträgen anzeigen.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/19/2021
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: f634ceb60ae78d4d825c73bd2e98da2fb951b374
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2021
ms.locfileid: "98705984"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Überprüfen des Status von Azure Import/Export-Aufträgen mithilfe von Kopierprotokolldateien
Wenn der Dienst „Microsoft Azure Import/Export“ Laufwerke im Rahmen eines Import- oder Exportauftrags verarbeitet, schreibt er Kopierprotokolldateien in das Speicherkonto, in das oder aus dem Sie Blobs importiert bzw. exportiert haben. Die Protokolldatei enthält ausführliche Statusinformationen zu den einzelnen importierten oder exportierten Dateien. Der Dienst gibt die URL für jede Kopierprotokolldatei zurück, wenn Sie den Status eines abgeschlossenen Auftrags abfragen. Weitere Informationen finden Sie unter [Abrufen von Aufträgen](/rest/api/storageimportexport/Jobs/Get).  

## <a name="example-urls"></a>Beispiel-URLs

Im Anschluss finden Sie Beispiel-URLs für Kopierprotokolldateien für einen Importauftrag mit zwei Laufwerken:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Informationen zum Format von Kopierprotokollen sowie eine vollständige Liste mit Statuscodes finden Sie unter [Format der Protokolldateien des Import/Export-Diensts](/previous-versions/azure/storage/common/storage-import-export-file-format-log).  

## <a name="next-steps"></a>Nächste Schritte

 * [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)   
 * [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-data-to-blobs.md#step-1-prepare-the-drives)   
 * [Reparieren eines Importauftrags](./storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparieren eines Exportauftrags](./storage-import-export-tool-repairing-an-export-job-v1.md)