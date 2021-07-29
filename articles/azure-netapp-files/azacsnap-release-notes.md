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
ms.date: 05/27/2021
ms.author: phjensen
ms.openlocfilehash: fd8d4e1bfed60aa8f3eae4d4d3c033247ab1268d
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579904"
---
# <a name="release-notes-for-azure-application-consistent-snapshot-tool"></a>Versionshinweise für das Tool für konsistente Momentaufnahmen in Azure-Anwendungen

Auf dieser Seite werden wichtige Änderungen aufgelistet, die an AzAcSnap vorgenommen wurden, um neue Funktionen bereitzustellen oder Fehler zu beheben.

## <a name="may-2021"></a>Mai 2021

### <a name="azacsnap-v501-build-2021052414837---patch-update-to-v50"></a>AzAcSnap v5.0.1 (Build: 20210524.14837) - Patchupdate auf v5.0

AzAcSnap v5.0.1 (Build: 20210524.14837) wird als Patchupdate für den Branch v5.0 mit den folgenden Fehlerbehebungen und Verbesserungen bereitgestellt:

- Verbesserte Exitcodebehandlung.  In einigen Fällen wurde der Exitcode 0 (null) ausgegeben, auch wenn ein Ausführungsfehler aufgetreten ist, bei dem er ungleich 0 (null) sein sollte.  Exitcodes sollten jetzt nur 0 (null) bei erfolgreicher Ausführung von `azacsnap` bis zum Abschluss und ungleich 0 (null) bei einem Fehler sein.  Darüber hinaus wurde die interne Fehlerbehandlung von AzAcSnap erweitert, um den Exitcode der externen Befehle (z. B. hdbsql, ssh), die von AzAcSnap ausgeführt werden, zu erfassen und auszugeben, wenn sie die Ursache des Fehlers sind.

Laden Sie die [neueste Version](https://aka.ms/azacsnapdownload) des Installationsprogramms herunter, und informieren Sie sich über die [ersten Schritte](azacsnap-get-started.md).

## <a name="april-2021"></a>April 2021

### <a name="azacsnap-v50-build-202104216349---ga-released-21-april-2021"></a>AzAcSnap v5.0 (Build: 20210421.6349) – GA-veröffentlicht (21.04.2021)

AzAcSnap v5.0 (Build: 20210421.6349) wurde allgemein verfügbar gemacht und hatte für diesen Build die folgenden Korrekturen und Verbesserungen:

- Das hdbsql-Wiederholungs-Timeout (um auf eine Antwort von SAP HANA zu warten) wird automatisch auf die Hälfte der „savePointAbortWaitSeconds“ festgelegt, um Racebedingungen zu vermeiden.  Die Einstellung für „savePointAbortWaitSeconds“ kann direkt in der JSON-Konfigurationsdatei geändert werden und muss mindestens 600 Sekunden lang sein.

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
