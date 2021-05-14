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
ms.date: 04/21/2021
ms.author: phjensen
ms.openlocfilehash: 269ef95835e5284806e7c1eaa76980e635c3d1dd
ms.sourcegitcommit: ad921e1cde8fb973f39c31d0b3f7f3c77495600f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/25/2021
ms.locfileid: "107946545"
---
# <a name="release-notes-for-azure-application-consistent-snapshot-tool"></a>Versionshinweise für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen

Auf dieser Seite werden wichtige Änderungen aufgelistet, die an AzAcSnap vorgenommen wurden, um neue Funktionen bereitzustellen oder Fehler zu beheben.

## <a name="april-2021"></a>April 2021

### <a name="azacsnap-v50-build-202104216349---ga-released-21-april-2021"></a>AzAcSnap v5.0 (Build: 20210421.6349) – GA-veröffentlicht (21.04.2021)

AzAcSnap v5.0 (Build: 20210421.6349) wurde allgemein verfügbar gemacht und hatte für diesen Build die folgenden Korrekturen und Verbesserungen:

- Das hdbsql-Wiederholungs-Timeout (um auf eine Antwort von SAP HANA zu warten) wird automatisch auf die Hälfte der „savePointAbortWaitSeconds“ festgelegt, um Racebedingungen zu vermeiden.  Die Einstellung für „savePointAbortWaitSeconds“ kann direkt in der JSON-Konfigurationsdatei geändert werden und muss mindestens 600 Sekunden lang sein.

Laden Sie die [neueste Version](https://aka.ms/azacsnapdownload) des Installationsprogramms herunter, und informieren Sie sich über die [ersten Schritte](azacsnap-get-started.md).

## <a name="march-2021"></a>März-2021

### <a name="azacsnap-v50-preview-build2021031830771"></a>AzAcSnap v5.0 Preview (Build: 20210318.30771)

AzAcSnap v 5.0 Preview (Build: 20210318.30771) wurde mit den folgenden Korrekturen und Verbesserungen freigegeben:

- Die Notwendigkeit, den AZACSNAP-Benutzer in den SAP HANA-Mandantendatenbanken hinzuzufügen, wurde beseitigt. Weitere Informationen finden Sie im Abschnitt [Aktivieren der Kommunikation mit SAP HANA](azacsnap-installation.md#enable-communication-with-sap-hana).
- Fix, um eine [Wiederherstellung](azacsnap-cmd-ref-restore.md) mit Volumes zuzulassen, die mit manueller QoS konfiguriert sind.
- Mutex-Steuerelement zum Drosseln von SSH-Verbindungen für Azure (große Instanzen) hinzugefügt.
- Fix für das Installationsprogramm für die Behandlung von Pfadnamen mit Leerzeichen und anderen verwandten Problemen.
- Als Vorbereitung für die Unterstützung anderer Datenbankserver wurde der optionale Parameter „--hanasid“ in „--dbsid“ geändert.

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit dem Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-get-started.md)
