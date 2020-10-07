---
title: include file
description: include file
services: storage
author: twooley
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: twooley
ms.custom: include file
ms.openlocfilehash: 358de5761f3535b5f1e75d82539ee8e3ecdbd289
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91665601"
---
**Azure Data Lake Storage Gen2** ist kein dedizierter Dienst- oder Speicherkontotyp. Es handelt sich dabei um das aktuelle Release von Funktionen für Big Data-Analysen.  Diese Funktionen sind in einem Speicherkonto vom Typ „Allgemein v2“ oder „BlockBlobStorage“ verfügbar, und Sie können sie durch Aktivieren des Features **Hierarchischer Namespace** des Kontos abrufen. Informationen zu Skalierungszielen finden Sie in den folgenden Artikeln: 

- [Skalierbarkeitsziele für Blob Storage](https://docs.microsoft.com/azure/storage/blobs/scalability-targets#scale-targets-for-blob-storage)
- [Skalierbarkeitsziele für Standardspeicherkonten](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account?toc=/azure/storage/blobs/toc.json#scale-targets-for-standard-storage-accounts)

**Azure Data Lake Storage Gen1** ist ein dedizierter Dienst. Es handelt sich dabei um ein unternehmensweites riesiges Repository für Big Data-Analyseworkloads. Sie können mit Data Lake Storage Gen1 Daten von beliebiger Größe, Art und Erfassungsgeschwindigkeit zur Durchführung operativer und explorativer Analysen an einem einzigen Ort erfassen. Es gibt keine Beschränkung für die Datenmenge, die Sie in einem Data Lake Storage Gen1-Konto speichern können.

| **Ressource** | **Begrenzung** | **Kommentare** |
| --- | --- | --- |
| Maximale Anzahl von Data Lake Storage Gen1-Konten pro Abonnement pro Region |10 | Wenden Sie sich an den Support, um eine Erhöhung dieses Grenzwerts anzufordern. |
| Maximale Anzahl von Zugriffs-ACLs pro Datei oder Ordner |32 | Dies ist eine harte Grenze. Verwenden von Gruppen zum Verwalten des Zugriffs mit weniger Einträgen. |
| Maximale Anzahl von standardmäßiger ACLs pro Datei oder Ordner |32 | Dies ist eine harte Grenze. Verwenden von Gruppen zum Verwalten des Zugriffs mit weniger Einträgen. |
