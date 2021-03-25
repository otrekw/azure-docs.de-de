---
title: Manuelles Beheben von ServiceNow-Synchronisierungsproblemen
description: Setzen Sie die Verbindung mit ServiceNow zurück, damit Warnungen in Microsoft Azure ServiceNow wieder aufrufen können.
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/17/2021
ms.openlocfilehash: 664f1522775d96b813b7cd99bdffdb26630497f0
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037437"
---
# <a name="how-to-manually-fix-sync-problems"></a>Manuelles Beheben von Synchronisierungsproblemen

Azure Monitor kann Verbindungen mit ITSM-Drittanbietern (IT Service Management) herstellen. ServiceNow ist einer dieser Anbieter.

Aus Sicherheitsgründen müssen Sie möglicherweise das Authentifizierungstoken für die Verbindung mit ServiceNow aktualisieren.
Verwenden Sie das folgende Synchronisierungsverfahren, um die Verbindung erneut zu aktivieren und das Token zu aktualisieren:

1. Suchen Sie die Lösung im oberen Suchbanner, und wählen Sie dann die relevanten Lösungen aus.

    ![Screenshot: Oberes Suchbanner und Auswählen der relevanten Lösungen](media/itsmc-resync-servicenow/solution-search-8-bit.png)

1. Wählen Sie auf dem Bildschirm der Lösung im Abonnementfilter „Alles auswählen“ aus, und filtern Sie dann nach „ServiceDesk“.

    ![Screenshot, der zeigt, wo Sie „Alles auswählen“ auswählen und wo Sie nach ServiceDesk filtern](media/itsmc-resync-servicenow/solutions-list-8-bit.png)

1. Wählen Sie die Lösung Ihrer ITSM-Verbindung aus.
1. Wählen Sie im linken Banner die ITSM-Verbindung aus.

    ![Screenshot, der zeigt, wo Sie ITSM-Verbindungen auswählen](media/itsmc-resync-servicenow/itsm-connector-8-bit.png)

1. Wählen Sie in der Liste jeden Connector aus. 
    1. Klicken Sie jeweils auf den Namen des Connectors, um ihn zu konfigurieren.
    1. Löschen Sie alle Connectors, die nicht mehr verwendet werden.

    1. Aktualisieren Sie die Felder entsprechend [diesen Definitionen](./itsmc-connections.md) anhand Ihres Partnertyps.

    1. Klicken Sie auf „Synchronisieren“.

       ![Screenshot, auf dem die Schaltfläche „Synchronisieren“ hervorgehoben ist](media/itsmc-resync-servicenow/resync-8-bit-2.png)

    1. Klicken Sie auf „Speichern“.

        ![Neue Verbindung](media/itsmc-resync-servicenow/save-8-bit.png)

f.    Überprüfen Sie die Benachrichtigungen, um festzustellen, ob der Vorgang gestartet wurde.
