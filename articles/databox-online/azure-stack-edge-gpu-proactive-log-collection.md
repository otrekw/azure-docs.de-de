---
title: Grundlegendes zur proaktiven Protokollsammlung auf Azure Stack Edge Pro-Geräten
description: Hier erfahren Sie, wie die proaktive Protokollsammlung auf einem Azure Stack Edge Pro-Gerät ausgeführt und wie sie deaktiviert wird.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 02/23/2021
ms.author: alkohli
ms.openlocfilehash: 064af116112f0b530ac0cc9b5755dcec2cf0bd07
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101722064"
---
# <a name="proactive-log-collection-on-your-azure-stack-edge-device"></a>Proaktive Protokollsammlung auf Azure Stack Edge-Geräten

Die proaktive Protokollsammlung erfasst Indikatoren zur Systemintegrität auf Ihrem Azure Stack Edge-Gerät, um bei Geräteproblemen effiziente Maßnahmen zur Problembehandlung einleiten zu können. Die proaktive Protokollsammlung ist standardmäßig aktiviert. In diesem Artikel wird beschrieben, was protokolliert wird, wie die Daten von Microsoft verarbeitet werden und wie die proaktive Protokollsammlung deaktiviert oder aktiviert wird. 

Die Informationen in diesem Artikel beziehen sich auf Geräte vom Typ „Azure Stack Edge Pro – GPU“, „Azure Stack Edge Pro R“ und „Azure Stack Edge Mini R“.

## <a name="about-proactive-log-collection"></a>Informationen zur proaktiven Protokollsammlung

Die Kundendienst- und Technikerteams bei Microsoft nutzen die Systemprotokolle von Ihrem Azure Stack Edge-Gerät, um Probleme, die während des Betriebs auftreten können, effizient identifizieren und beheben zu können. Die proaktive Protokollsammlung ist ein Verfahren, mit dem Microsoft benachrichtigt wird, dass von der Azure Stack Edge-Appliance des Kunden ein Problem/Ereignis erkannt wurde. Ereignisse, die nachverfolgt werden, finden Sie unter [Indikatoren der proaktiven Protokollsammlung](#proactive-log-collection-indicators). Die Supportprotokolle zu diesem Problem werden automatisch in ein Azure Storage-Konto hochgeladen, das von Microsoft verwaltet und gesteuert wird. Der Microsoft-Support und Microsoft-Techniker prüfen diese Supportprotokolle, um die beste Vorgehensweise für das Beheben des Kundenproblems zu ermitteln.

> [!NOTE]
> Diese Protokolle werden nur zu Debuggingzwecken verwendet und bieten den Kunden Unterstützung bei Problemen.


## <a name="enabling-proactive-log-collection"></a>Aktivieren der proaktiven Protokollsammlung

Die proaktive Protokollsammlung ist standardmäßig aktiviert. Sie können die proaktive Protokollsammlung deaktivieren, während Sie das Gerät über die lokale Benutzeroberfläche aktivieren. 

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche des Geräts zur Seite **Erste Schritte**.

2. Wählen Sie auf der Kachel **Aktivierung** die Option **Aktivieren** aus. 

    ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche 1](./media/azure-stack-edge-pro-r-deploy-activate/activate-1.png)

3. Im Bereich **Aktivieren**:

   1. Geben Sie den **Aktivierungsschlüssel** ein, den Sie unter [Aktivierungsschlüssel für Azure Stack Edge Pro R abrufen](azure-stack-edge-pro-r-deploy-prep.md#get-the-activation-key) abgerufen haben.

      Nach der Aktivierung ist die proaktive Protokollsammlung standardmäßig aktiviert, sodass Microsoft Protokolle basierend auf dem Integritätsstatus des Geräts sammeln kann. Diese Protokolle werden in ein Azure Storage-Konto hochgeladen. 

      Sie können die proaktive Protokollsammlung deaktivieren und so verhindern, dass Microsoft Protokolle sammelt.

   1. Wenn Sie die proaktive Protokollsammlung auf dem Gerät deaktivieren möchten, wählen Sie **Deaktivieren** aus.

   1. Wählen Sie **Aktivieren** aus.

   ![Seite „Clouddetails“ auf der lokalen Webbenutzeroberfläche 2](./media/azure-stack-edge-pro-r-deploy-activate/activate-2.png)

## <a name="proactive-log-collection-indicators"></a>Indikatoren der proaktiven Protokollsammlung

Nachdem die proaktive Protokollsammlung aktiviert wurde, werden Protokolle automatisch hochgeladen, wenn eines der folgenden Ereignisse auf dem Gerät erkannt wird:  


|Warnung/Fehler/Bedingung  |BESCHREIBUNG  |
|---------|---------|
|AcsUnhealthyCondition     |Azure-konsistente Dienste sind fehlerhaft.         |
|IOTEdgeAgentNotRunningCondition      |IoT Edge-Agent wird nicht ausgeführt.         |
|UpdateInstallFailedEvent | Das Update konnte nicht installiert werden.        |

 
Microsoft wird dieser Liste in Zukunft noch weitere Ereignisse hinzufügen. Für neue Ereignisse wird keine erneute Einwilligung benötigt. Auf dieser Seite finden Sie Informationen zu den aktuellen Indikatoren der proaktiven Protokollsammlung.    
 

## <a name="other-log-collection-methods"></a>Andere Methoden der Protokollsammlung

Neben der proaktiven Protokollsammlung, bei der bestimmte Protokolle für ein bestimmtes erkanntes Problem gesammelt werden, können Ihnen andere Protokollsammlungen ein tiefer gehendes Verständnis von Systemintegrität und Systemverhalten bieten. Diese anderen Protokolle können normalerweise während einer Supportanfrage gesammelt oder durch Microsoft anhand von Telemetriedaten aus dem Gerät ausgelöst werden.

## <a name="handling-data"></a>Verarbeiten der Daten

Wenn die proaktive Protokollsammlung aktiviert wird, erklärt sich der Kunde damit einverstanden, dass Microsoft Protokolle vom Azure Stack Edge-Gerät sammelt, wie in diesem Dokument beschrieben wird. Darüber hinaus erklären sich die Kunden damit einverstanden, dass diese Protokolle in ein von Microsoft verwaltetes und gesteuertes Azure Storage-Konto hochgeladen und dort aufbewahrt werden.

Microsoft nutzt die Daten ausschließlich zur Problembehandlung bei der Systemintegrität und bei Problemen. Die Daten werden nicht ohne Einwilligung der Kunden für Marketing-, Werbungs- oder andere kommerzielle Zwecke verwendet. 

Die von Microsoft gesammelten Daten werden gemäß unseren standardmäßigen Datenschutzbestimmungen behandelt. Sollte ein Kunde seine Einwilligung für die proaktive Protokollsammlung widerrufen, betrifft dies keine Daten, die mit Einwilligung vor dem Widerruf gesammelt wurden.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [ein Supportpaket erhalten](azure-stack-edge-gpu-troubleshoot.md#collect-support-package).