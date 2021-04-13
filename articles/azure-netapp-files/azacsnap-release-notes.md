---
title: Versionshinweise für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Bietet Versionshinweise zum Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
services: azure-netapp-files
documentationcenter: ''
author: Phil-Jensen
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/22/2021
ms.author: phjensen
ms.openlocfilehash: 01fb93dcd0a1d5c1db615c47d7811a0baa863c9b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "105111395"
---
# <a name="release-notes-for-azure-application-consistent-snapshot-tool-preview"></a>Versionshinweise für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen (Vorschau)

Auf dieser Seite werden wichtige Änderungen aufgelistet, die an AzAcSnap vorgenommen wurden, um neue Funktionen bereitzustellen oder Fehler zu beheben.

## <a name="march-2021"></a>März-2021

### <a name="azacsnap-v50-preview-build2021031830771"></a>AzAcSnap v5.0 Preview (Build: 20210318.30771)

AzAcSnap v 5.0 Preview (Build: 20210318.30771) wurde mit den folgenden Korrekturen und Verbesserungen freigegeben:

- Die Notwendigkeit, den AZACSNAP-Benutzer in den SAP HANA-Mandantendatenbanken hinzuzufügen, wurde beseitigt. Weitere Informationen finden Sie im Abschnitt [Aktivieren der Kommunikation mit SAP HANA](azacsnap-installation.md#enable-communication-with-sap-hana).
- Fix, um eine [Wiederherstellung](azacsnap-cmd-ref-restore.md) mit Volumes zuzulassen, die mit manueller QoS konfiguriert sind.
- Mutex-Steuerelement zum Drosseln von SSH-Verbindungen für Azure (große Instanzen) hinzugefügt.
- Fix für das Installationsprogramm für die Behandlung von Pfadnamen mit Leerzeichen und anderen verwandten Problemen.
- Als Vorbereitung für die Unterstützung anderer Datenbankserver wurde der optionale Parameter „--hanasid“ in „--dbsid“ geändert.

Laden Sie die [neueste Version](https://aka.ms/azacsnapdownload) des Installationsprogramms herunter, und informieren Sie sich über die [ersten Schritte](azacsnap-get-started.md).

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit dem Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-get-started.md)
