---
title: Überprüfen des Status von Azure Import/Export-Aufträgen (V1) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie mithilfe von Protokolldateien, die vom Import- oder Exportvorgang erstellt wurden, der Status von Aufträgen anzeigen.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/26/2017
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: 2fa5cfcf0cb6c20a8a0b64651da0b365e12a3d05
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056321"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Überprüfen des Status von Azure Import/Export-Aufträgen mithilfe von Kopierprotokolldateien
Wenn der Dienst „Microsoft Azure Import/Export“ Laufwerke im Rahmen eines Import- oder Exportauftrags verarbeitet, schreibt er Kopierprotokolldateien in das Speicherkonto, in das oder aus dem Sie Blobs importiert bzw. exportiert haben. Die Protokolldatei enthält ausführliche Statusinformationen zu den einzelnen importierten oder exportierten Dateien. Der Dienst gibt die URL für jede Kopierprotokolldatei zurück, wenn Sie den Status eines abgeschlossenen Auftrags abfragen. Weitere Informationen finden Sie unter [Abrufen von Aufträgen](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get).  

## <a name="example-urls"></a>Beispiel-URLs

Im Anschluss finden Sie Beispiel-URLs für Kopierprotokolldateien für einen Importauftrag mit zwei Laufwerken:  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  

 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  

 Informationen zum Format von Kopierprotokollen sowie eine vollständige Liste mit Statuscodes finden Sie unter [Format der Protokolldateien des Import/Export-Diensts](../storage-import-export-file-format-log.md).  

## <a name="next-steps"></a>Nächste Schritte

 * [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)   
 * [Vorbereiten von Festplatten für einen Importauftrag](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Reparieren eines Importauftrags](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Reparieren eines Exportauftrags](../storage-import-export-tool-repairing-an-export-job-v1.md)
