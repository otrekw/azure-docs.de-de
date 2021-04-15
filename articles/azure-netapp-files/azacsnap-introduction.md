---
title: Grundlegendes zum Tool für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files | Microsoft-Dokumentation
description: Einführung in das Tool für konsistente Momentaufnahmen in Azure-Anwendungen, das Sie mit Azure NetApp Files verwenden können.
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
ms.date: 12/14/2020
ms.author: phjensen
ms.openlocfilehash: 4ba679459686340396e0e4d65344295c0fa9c4be
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104869955"
---
# <a name="what-is-azure-application-consistent-snapshot-tool-preview"></a>Grundlegendes zum Tool für konsistente Momentaufnahmen in Azure-Anwendungen für Azure NetApp Files (Vorschau)

Das Tools für konsistente Momentaufnahmen in Azure-Anwendungen (AzAcSnap) ist ein Befehlszeilentool, das den Datenschutz für Datenbanken von Drittanbietern ermöglicht, indem es die gesamte Orchestrierung abwickelt, die erforderlich ist, damit die Anwendungsdaten konsistent sind, bevor eine Speichermomentaufnahme erstellt wird, nach der die Daten in einem Betriebszustand zurückgegeben werden.

## <a name="supported-platforms-and-os"></a>Unterstützte Plattformen und Betriebssysteme

- **Datenbanken**
  - SAP HANA (Details finden Sie in der [Unterstützungsmatrix](azacsnap-get-started.md#snapshot-support-matrix-from-sap))

- **Betriebssysteme**
  - SUSE Linux Enterprise Server 12 und höher
  - Red Hat Enterprise Linux 7 und höher

## <a name="benefits-of-using-azacsnap"></a>Vorteile von AzAcSnap

AzAcSnap nutzt die Funktionen für Momentaufnahmen und Replikation von Volumes von Azure NetApp Files und Azure (große Instanz).  Ihnen bieten sich folgende Vorteile:

- **Anwendungskonsistenter Datenschutz** AzAcSnap ist eine zentrale Lösung für die Sicherung kritischer Datenbankdateien. Sie stellt Datenbankkonsistenz vor dem Erstellen einer Momentaufnahme eines Speichervolumes sicher. Dadurch ist gewährleistet, dass die Momentaufnahme des Speichervolumes für die Datenbankwiederherstellung genutzt werden kann.
- **Verwaltung des Datenbankkatalogs** Wenn Sie AzAcSnap mit einer Datenbank mit integriertem Sicherungskatalog verwenden, werden die Datensätze innerhalb des Katalogs mithilfe von Speichermomentaufnahmen auf dem neuesten Stand gehalten.  Diese Funktion ermöglicht einem Datenbankadministrator, die Sicherungsaktivität einzusehen.
- **Ad-hoc-Volumeschutz** Diese Funktion ist hilfreich für Nicht-Datenbankvolumes, die keine Inaktivierung von Anwendungen benötigen, ehe eine Momentaufnahme des Speichers erstellt wird.  Beispiele hierfür sind SAP HANA-Volumes für die Protokollsicherung oder SAPTRANS-Volumes.
- **Klonen von Speichervolumes** Diese Funktion bietet speichereffiziente Klone von Speichervolumes für Entwicklungs- und Testzwecke.
- **Unterstützung der Notfallwiederherstellung** AzAcSnap nutzt die Replikation von Speichervolumes, um Optionen für die Wiederherstellung replizierter, konsistenter Momentaufnahmen von Anwendungen an einem Remotestandort bereitzustellen.

AzAcSnap ist eine einzelne Binärdatei,  die keine zusätzlichen Agents oder Plug-Ins benötigt, um mit der Datenbank oder dem Speicher zu interagieren (Azure NetApp Files über Azure Resource Manager und Azure [große Instanz] über SSH).  AzAcSnap muss auf einem System installiert werden, das mit Datenbank und Speicher verbunden ist.  Die flexible Installation und Konfiguration ermöglicht jedoch entweder eine einzelne zentrale Installation oder eine vollständig verteilte Installation mit Kopien für jede Datenbankinstallation.

## <a name="architecture-overview"></a>Übersicht über die Architektur

AzAcSnap kann auf demselben Host wie die Datenbank (SAP HANA) oder auf einem zentralisierten System installiert werden.  Es muss jedoch eine Netzwerkverbindung mit den Datenbankservern und dem Speicher-Back-End bestehen (Azure Resource Manager für Azure NetApp Files oder SSH für Azure [große Instanz]).

AzAcSnap ist eine schlanke Anwendung, die normalerweise mithilfe eines externen Schedulers ausgeführt wird.  Auf den meisten Linux-Systemen ist dies `cron`, worauf sich die Dokumentation konzentriert.  Aber der Scheduler kann auch ein anderes Tool sein, solange er das Shellprofil des Benutzers `azacsnap` importieren kann.  Das Importieren der Umgebungseinstellungen des Benutzers stellt sicher, dass Dateipfade und Berechtigungen ordnungsgemäß initialisiert werden.

## <a name="command-synopsis"></a>Befehlsübersicht

Das allgemeine Format der Befehle ist wie folgt: `azacsnap -c [command] --[command] [sub-command] --[flag-name] [flag-value]`.

## <a name="command-options"></a>Befehlsoptionen

Die Befehlsoptionen sind wie folgt, wobei die Befehle als Hauptaufzählungszeichen und die zugehörigen Unterbefehle als eingerückte Aufzählungszeichen dargestellt sind:

- **`-h`** bietet eine erweiterte Befehlszeilenhilfe mit Beispielen zur Verwendung von AzAcSnap.
- **`-c configure`** Dieser Befehl bietet eine interaktive Benutzeroberfläche im Frage- und Antwortformat zum Erstellen oder Ändern der Konfigurationsdatei `azacsnap` (Standard = `azacsnap.json`).
  - **`--configuration new`** erstellt eine neue Konfigurationsdatei.
  - **`--configuration edit`** bearbeitet eine vorhandene Konfigurationsdatei.
  - Weitere Informationen finden Sie in der [Referenz zum Befehl „configure“](azacsnap-cmd-ref-configure.md).
- **`-c test`** überprüft die Konfigurationsdatei und testet die Konnektivität.
  - **`--test hana`** testet die Verbindung mit der SAP HANA-Instanz.
  - **`--test storage`** testet die Kommunikation mit der zugrunde liegenden Speicherschnittstelle, indem eine temporäre Speichermomentaufnahme für alle konfigurierten `data`-Volumes erstellt wird und diese dann entfernt werden.
  - **`--test all`** führt die Tests `hana` und `storage` nacheinander aus.
  - Weitere Informationen finden Sie in der [Referenz zum Befehl „test“](azacsnap-cmd-ref-test.md).
- **`-c backup`** ist der primäre Befehl zum Ausführen von Momentaufnahmen des datenbankkonsistenten Speichers für Daten (SAP HANA-Datenvolumes) und andere Volumes (z. B. gemeinsam genutzte, Protokollsicherungs- oder Startvolumes).
  - **`--volume data`** dient zum Erstellen von Momentaufnahmen aller Volumes im Abschnitt `dataVolume` der Konfigurationsdatei.
  - **`--volume other`** dient zum Erstellen von Momentaufnahmen aller Volumes im Abschnitt `otherVolume` der Konfigurationsdatei.
  - Weitere Informationen finden Sie in der [Referenz zum Befehl „backup“](azacsnap-cmd-ref-backup.md).
- **`-c details`** stellt Informationen zu Momentaufnahmen oder zur Replikation bereit.
  - **`--details snapshots`** bietet eine Liste mit grundlegenden Details zu den Momentaufnahmen für jedes konfigurierte Volume.
  - **`--details replication`** stellt grundlegende Details zum Replikationsstatus des Produktionsstandorts für den Notfallwiederherstellungsstandort bereit.
  - Weitere Informationen finden Sie in der [Referenz zum Befehl „details“](azacsnap-cmd-ref-details.md).
- **`-c delete`** Dieser Befehl löscht eine Speichermomentaufnahme oder Gruppe von Momentaufnahmen. Sie können die SAP HANA-Sicherungs-ID in HANA Studio oder den Namen der Speichermomentaufnahme verwenden. Die Sicherungs-ID ist nur mit den `hana`-Momentaufnahmen verknüpft, die für die Daten- und freigegebenen Volumes erstellt werden. Andernfalls wird bei Eingabe des Namens der Momentaufnahme nach allen Momentaufnahmen gesucht, die mit dem eingegebenen Namen der Momentaufnahme übereinstimmen.
  - Weitere Informationen finden Sie unter [delete](azacsnap-cmd-ref-delete.md).
- **`-c restore`** bietet zwei Methoden zum Wiederherstellen einer Momentaufnahme auf einem Volume. Entweder wird ein neues Volume auf Basis der Momentaufnahme erstellt, oder ein Volume wird auf einen Vorschaustatus zurückgesetzt.
  - **`--restore snaptovol`** Erstellt ein neues Volume basierend auf der letzten Momentaufnahme auf dem Zielvolume.
  - **`-c restore --restore revertvolume`** Setzt das Zielvolume auf Grundlage der letzten Momentaufnahme auf einen früheren Zustand zurück.
  - Weitere Informationen finden Sie in der [Referenz zum Befehl „restore“](azacsnap-cmd-ref-restore.md).
- **`[--configfile <configfilename>]`** Der optionale Befehlszeilenparameter zum Bereitstellen eines anderen JSON-Konfigurationsdateinamens.  Dies ist besonders nützlich, um eine separate Konfigurationsdatei pro SID (z. B. `--configfile H80.json`) zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

- [Erste Schritte mit dem Tool für konsistente Momentaufnahmen in Azure-Anwendungen](azacsnap-get-started.md)
