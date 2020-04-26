---
title: Migrieren von OMS-Updatebereitstellungen zu Azure
description: In diesem Artikel wird beschrieben, wie Sie Ihre vorhandenen OMS-Updatebereitstellungen zu Azure migrieren.
services: automation
ms.subservice: update-management
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 910f284eedbf50be5b58b6c18f02e50adda35e9a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680008"
---
# <a name="migrate-your-oms-update-deployments-to-azure"></a>Migrieren von OMS-Updatebereitstellungen zu Azure

Das Portal der Operations Management Suite (OMS) wird gerade [eingestellt](../azure-monitor/platform/oms-portal-transition.md). Alle Funktionen des OMS-Portals für die Updateverwaltung sind ab jetzt im Azure-Portal über Azure Monitor-Protokolle verfügbar. Dieser Artikel enthält alles Wissenswerte zur Migration zum Azure-Portal.

## <a name="key-information"></a>Wichtige Informationen

* Vorhandene Bereitstellungen sind weiterhin funktionsfähig. Nachdem Sie die Bereitstellung in Azure neu erstellt haben, können Sie die alte Bereitstellung in der OMS löschen.
* Alle vorhandenen Features aus der OMS sind in Azure verfügbar. Weitere Informationen zur Updateverwaltung finden Sie unter [Übersicht über die Updateverwaltung](automation-update-management.md).

## <a name="access-the-azure-portal"></a>Zugriff auf fas Azure-Portal

Klicken Sie in Ihrem OMS-Arbeitsbereich auf **In Azure öffnen**. Durch diese Auswahl gelangen Sie zum Log Analytics-Arbeitsbereich, den die OMS verwendet hat.

![In Azure öffnen – OMS-Portal](media/migrate-oms-update-deployments/link-to-azure-portal.png)

Klicken Sie im Azure-Portal auf **Automation-Konto**.

![Azure Monitor-Protokolle](media/migrate-oms-update-deployments/log-analytics.png)

Klicken Sie in Ihrem Automation-Konto auf **Updateverwaltung**.

![Updateverwaltung](media/migrate-oms-update-deployments/azure-automation.png)

Wählen Sie im Azure-Portal unter **Alle Dienste** die Option **Automation-Konten** aus. 

Wählen Sie unter **Verwaltungstools** das entsprechende Automation-Konto aus, und klicken Sie auf **Updateverwaltung**.

## <a name="recreate-existing-deployments"></a>Neuerstellen vorhandener Bereitstellungen

Alle im OMS-Portal erstellten Updatebereitstellungen verfügen über eine [gespeicherte Suche](../azure-monitor/platform/computer-groups.md), auch Computergruppe genannt. Diese weist denselben Namen wie die vorhandene Updatebereitstellung auf. Die gespeicherte Suche enthält die Liste der Computer, für die die Updatebereitstellung geplant wurde.

![Updateverwaltung](media/migrate-oms-update-deployments/oms-deployment.png)

Wenn Sie diese vorhandene gespeicherte Suche verwenden möchten, gehen Sie folgendermaßen vor:

Wechseln Sie zum Erstellen einer neuen Updatebereitstellung zum Azure-Portal, wählen Sie das verwendete Automation-Konto aus, und klicken Sie auf **Updateverwaltung**. Klicken Sie auf **Updatebereitstellung planen**.

![Updatebereitstellung planen](media/migrate-oms-update-deployments/schedule-update-deployment.png)

Der Bereich „Neue Updatebereitstellung“ wird geöffnet. Geben Sie Werte für die Eigenschaften ein, die in der folgenden Tabelle beschrieben werden, und klicken Sie auf **Erstellen**:

Wählen Sie für **Zu aktualisierende Computer** die gespeicherte Suche aus, die von der vorhandenen OMS-Bereitstellung verwendet wird.

| Eigenschaft | BESCHREIBUNG |
| --- | --- |
|Name |Eindeutiger Name zum Identifizieren der Updatebereitstellung |
|Betriebssystem| Wählen Sie zwischen **Linux** und **Windows** aus.|
|Zu aktualisierende Computer |Wählen Sie eine gespeicherte Suche oder eine importierte Gruppe aus, oder wählen Sie im Dropdownmenü „Computer“ und dann einzelne Computer aus. Bei Auswahl von **Computer** wird die Bereitschaft des Computers in der Spalte **BEREITSCHAFT DES UPDATE-AGENTS** angezeigt.</br> Weitere Informationen zu den verschiedenen Methoden zum Erstellen von Computergruppen in Azure Monitor-Protokollen finden Sie unter [Computergruppen in Azure Monitor-Protokollen](../azure-monitor/platform/computer-groups.md). |
|Updateklassifizierungen|Wählen Sie alle benötigten Updateklassifizierungen aus. CentOS unterstützt dies nicht standardmäßig.|
|Auszuschließende Updates|Geben Sie die auszuschließenden Updates ein. Geben Sie für Windows den KB-Artikel ohne das Präfix **KB** ein. Geben Sie für Linux den Paketnamen ein, oder verwenden Sie ein Platzhalterzeichen.  |
|Zeitplaneinstellungen|Wählen Sie den Startzeitpunkt aus, und wählen Sie dann unter „Wiederholung“ **Einmal** oder **Serie** aus. | 
| Wartungsfenster |Festgelegte Minutenanzahl für Updates Der Wert darf nicht kleiner als 30 Minuten oder größer als 6 Stunden sein. |
| Neustartsteuerung| Legt fest, wie Neustarts behandelt werden sollen.</br>Die verfügbaren Optionen lauten wie folgt:</br>Neu starten bei Bedarf (Standard)</br>Immer neu starten</br>Nie neu starten</br>Nur neu starten – Updates werden nicht installiert|

Klicken Sie auf **Geplante Updatebereitstellungen**, um den Status der neu erstellten Updatebereitstellung anzuzeigen.

![Neue Updatebereitstellung](media/migrate-oms-update-deployments/new-update-deployment.png)

Wenn Sie die neuen Bereitstellungen über das Azure-Portal konfiguriert haben, können die vorhandenen Bereitstellungen, wie bereits erwähnt, aus dem OMS-Portal entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Updateverwaltung in Azure finden Sie unter [Lösung für die Updateverwaltung in Azure](automation-update-management.md).
