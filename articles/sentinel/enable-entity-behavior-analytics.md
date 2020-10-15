---
title: Verwenden von Entity Behavior Analytics zum Erkennen komplexer Bedrohungen | Microsoft-Dokumentation
description: Aktivieren von User and Entity Behavior Analytics in Azure Sentinel und Konfigurieren von Datenquellen
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2020
ms.author: yelevin
ms.openlocfilehash: c55ea0e7753faa6dc21b955d63a57d96e3849f70
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90993263"
---
# <a name="enable-user-and-entity-behavior-analytics-ueba-in-azure-sentinel"></a>Aktivieren von User and Entity Behavior Analytics (UEBA) in Azure Sentinel 



## <a name="prerequisites"></a>Voraussetzungen

- Ihrem Benutzer muss eine der Rollen **Globaler Administrator** oder **Sicherheitsadministrator** in Azure AD zugewiesen werden, damit Sie UEBA aktivieren oder deaktivieren können. Für die Ausführung ist dies nicht erforderlich.

## <a name="how-to-enable-user-and-entity-behavior-analytics"></a>Aktivieren von User and Entity Behavior Analytics

1. Wählen Sie im Azure Sentinel-Navigationsmenü die Option **Entity behavior (Vorschau)** aus.

1. Schalten Sie den Umschalter unter der Überschrift **Aktivieren** auf **Ein** um.

1. Klicken Sie auf die Schaltfläche **Datenquellen auswählen**.

1. Aktivieren Sie im Bereich **Datenquellenauswahl** die Kontrollkästchen neben den Datenquellen, für die Sie UEBA aktivieren möchten, und wählen Sie dann **Anwenden** aus.

    > [!NOTE]
    >
    > In der unteren Hälfte des Bereichs **Datenquellenauswahl** wird eine Liste der von UEBA unterstützten Datenquellen angezeigt, die Sie noch nicht aktiviert haben. 
    >
    > Nachdem Sie UEBA aktiviert haben, können Sie beim Herstellen einer Verbindung mit neuen Datenquellen diese im Bereich des Datenconnectors direkt für UEBA aktivieren (sofern sie UEBA-fähig sind).

1. Wählen Sie **Go to entity search** (Zur Entitätsssuche) aus. Dadurch gelangen Sie zum Bereich für die Entitätssuche, der in Zukunft angezeigt wird, wenn Sie im Hauptmenü von Azure Sentinel **Entity behavior** auswählen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument haben Sie erfahren, wie Sie die User and Entity Behavior Analytics (UEBA) in Azure Sentinel aktivieren und konfigurieren. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats-built-in.md).
