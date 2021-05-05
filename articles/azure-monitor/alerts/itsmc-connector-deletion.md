---
title: Löschen nicht verwendeter ITSM-Connectors
description: In diesem Artikel wird erläutert, wie Sie ITSM-Connectors und die zugehörigen Aktionsgruppen löschen.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 3dc84ea6def9b762527226dbeb3e2eaab78ec200
ms.sourcegitcommit: aa00fecfa3ad1c26ab6f5502163a3246cfb99ec3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2021
ms.locfileid: "107387937"
---
# <a name="delete-unused-itsm-connectors"></a>Löschen nicht verwendeter ITSM-Connectors

Das Löschen nicht verwendeter ITSM-Connectors (IT Service Management) umfasst zwei Phasen. Sie löschen alle Aktionen, die einem ITSM-Connector zugeordnet sind, und löschen dann den Connector selbst. Sie löschen zuerst die Aktionen, weil Aktionen ohne einen Connector möglicherweise Fehler in Ihrem Abonnement verursachen.

## <a name="delete-associated-actions"></a>Löschen zugeordneter Aktionen

1. Wählen Sie im Azure-Portal die Option **Überwachen** aus.
  
    ![Screenshot: Auswahl von „Überwachen“](media/itsmc-connector-deletion/itsmc-monitor-selection.png)

2. Wählen Sie **Warnungen**.
   
    ![Screenshot: Auswahl von „Warnungen“](media/itsmc-connector-deletion/itsmc-alert-selection.png)

3. Wählen Sie **Aktionen verwalten** aus.
   
    ![Screenshot: Auswahl von „Aktionen verwalten“](media/itsmc-connector-deletion/itsmc-actions-selection.png)

4. Wählen Sie eine Aktionsgruppe aus, die dem ITSM-Connector zugeordnet ist, den Sie löschen möchten. In diesem Artikel wird als Beispiel ein Cherwell-Connector verwendet.
   
    ![Screenshot: Aktionen, die dem Cherwell-Connector zugeordnet sind](media/itsmc-connector-deletion/itsmc-actions-screen.png)

5. Überprüfen Sie die Informationen, und klicken Sie dann auf **Aktionsgruppe löschen**.

    ![Screenshot: Aktionsgruppeninformationen und Schaltfläche zum Löschen der Gruppe](media/itsmc-connector-deletion/itsmc-action-deletion.png)

## <a name="delete-the-connector"></a>Löschen des Connectors

1. Suchen Sie über die Suchleiste nach **ServiceDesk**. Wählen Sie anschließend in der Liste der Ressourcen **ServiceDesk** aus.

    ![Screenshot: Suchen nach und Auswählen von „ServiceDesk“](media/itsmc-connector-deletion/itsmc-connector-selection.png)

2. Wählen Sie **ITSM-Verbindungen** und anschließend den Cherwell-Connector aus.

    ![Screenshot: Cherwell-ITSM-Connector](media/itsmc-connector-deletion/itsmc-cherwell-connector.png)

3. Klicken Sie auf **Löschen**.

    ![Screenshot: Schaltfläche „Löschen“ für den ITSM-Connector](media/itsmc-connector-deletion/itsmc-connector-deletion.png)

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung für einen ITSM-Connector](./itsmc-resync-servicenow.md)
