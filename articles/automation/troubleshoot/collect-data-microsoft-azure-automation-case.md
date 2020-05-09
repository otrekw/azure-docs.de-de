---
title: Zu erfassende Daten beim Öffnen eines Falls für Microsoft Azure Automation | Microsoft-Dokumentation
description: In diesem Artikel werden einige der Informationen beschrieben, die Sie zusammenstellen sollten, bevor Sie beim Microsoft Azure-Support eine Anfrage zu Azure Automation erstellen.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/23/2019
ms.author: v-miegge
ms.openlocfilehash: e1d5d791a58f301991819b41757b9021f6e30fc0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2020
ms.locfileid: "81679404"
---
# <a name="data-to-collect-when-you-open-a-case-for-microsoft-azure-automation"></a>Zu erfassende Daten beim Öffnen eines Falls für Microsoft Azure Automation

In diesem Artikel werden einige der Informationen beschrieben, die Sie zusammenstellen sollten, bevor Sie beim Microsoft Azure-Support eine Anfrage zu Azure Automation erstellen. Diese Informationen sind nicht erforderlich, um eine Anfrage zu öffnen. Sie können Microsoft aber dabei helfen, Ihr Problem schneller zu lösen. Darüber hinaus besteht die Möglichkeit, dass der Supportmitarbeiter Sie nach dem Öffnen der Anfrage um diese Daten bittet.

## <a name="basic-data"></a>Grundlegende Daten

Erfassen Sie die grundlegenden Daten, die im Knowledge Base-Artikel [4034605 – skriptbasiertes Erfassen von Diagnosedaten zu Azure Automation](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) beschrieben werden.

## <a name="data-for-update-management-issues-on-linux"></a>Daten für Probleme mit der Updateverwaltung in Linux

1. Führen Sie über die in KB [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgeführten Punkte hinaus das folgende Tool zur Protokollerfassung aus:

   [OMS Linux Agent-Protokollsammler](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)
 
2. Komprimieren Sie den Inhalt des Ordners **/var/opt/microsoft/omsagent/run/automationworker/** , und senden Sie die komprimierte Datei an den Azure-Support.
 
3. Überprüfen Sie, ob die ID für den Arbeitsbereich, für den der Protokollanalyse-Agent für Linux Berichte erstellt, der ID für den Arbeitsbereich entspricht, der auf Updates überwacht wird.

## <a name="data-for-update-management-issues-on-windows"></a>Daten für Probleme mit der Updateverwaltung in Windows

1. Erfassen Sie Daten für die Elemente, die im KB-Artikel [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgeführt sind.

2. Exportieren Sie die folgenden Ereignisprotokolle in das EVTX-Format:

   * System
   * Application
   * Sicherheit
   * Operations Manager
   * Microsoft-SMA/Operational

3. Überprüfen Sie, ob die ID für den Arbeitsbereich, für den der Agent Berichte erstellt, der ID für den Arbeitsbereich entspricht, der von Windows Updates überwacht wird.

## <a name="data-for-job-issues"></a>Daten für Probleme mit Aufträgen

1. Erfassen Sie Daten für die Elemente, die im KB-Artikel [4034605](https://support.microsoft.com/help/4034605/how-to-capture-azure-automation-scripted-diagnostics) aufgeführt sind.

2. Erfassen Sie die Auftrags-ID für den Auftrag, bei dem ein Problem besteht:

   1. Navigieren Sie im Azure-Portal zu **Automation-Konten**.
   2. Wählen Sie das Automation-Konto aus, für das Sie die Problembehandlung durchführen, und notieren Sie sich den Namen.
   3. Wählen Sie **Aufträge** aus.
   4. Wählen Sie den Auftrag aus, für den Sie die Problembehandlung durchführen.
   5. Suchen Sie im Bereich „Auftragszusammenfassung“ nach dem GUID-Wert in **Auftrags.ID**.

   ![Auftrags-ID im Bereich „Auftragszusammenfassung“](media/collect-data-microsoft-azure-automation-case/job-summary-job-id.png)

3. Erfassen Sie ein Beispiel des Skripts, das Sie ausführen.

4. Sammeln der Protokolldateien:

   1. Navigieren Sie im Azure-Portal zu **Automation-Konten**.
   2. Wählen Sie das Automation-Konto aus, für das Sie die Problembehandlung durchführen.
   3. Wählen Sie **Aufträge** aus.
   4. Wählen Sie den Auftrag aus, für den Sie die Problembehandlung durchführen.
   5. Wählen Sie **Alle Protokolle** aus.
   6. Erfassen Sie die Daten im resultierenden Bereich.

   ![Unter „Alle Protokolle“ aufgeführte Daten](media/collect-data-microsoft-azure-automation-case/all-logs-data.png)

## <a name="data-for-module-issues"></a>Daten für Probleme mit Modulen

Erfassen Sie zusätzlich zu den [grundlegenden Datenelementen](#basic-data) die folgenden Informationen:

* Die Schritte, die Sie ausgeführt haben, damit das Problem reproduziert werden kann.
* Screenshots von allen Fehlermeldungen.
* Screenshots der aktuellen Module und ihrer Versionsnummern.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie weitere Hilfe benötigen, gehen Sie folgendermaßen vor:

* Erhalten Sie Antworten von Azure-Experten über [Azure-Foren](https://azure.microsoft.com/support/forums/).
* Nutzen Sie [@AzureSupport](https://twitter.com/azuresupport) – das offizielle Microsoft Azure-Konto zur Verbesserung der Benutzerfreundlichkeit. Hierüber hat die Azure-Community Zugriff auf die richtigen Ressourcen: Antworten, Support und Experten.
* Erstellen Sie einen Azure-Supportfall. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie **Support erhalten**aus.
