---
author: AlexandraKemperMS
ms.author: alkemper
ms.service: azure-app-configuration
ms.topic: include
ms.date: 05/03/2021
ms.openlocfilehash: 7f5f062e35aad6b7623f1a2f97ab3b9133266ac6
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2021
ms.locfileid: "108748275"
---
Weisen Sie den im Rahmen der Aufgabe verwendeten Anmeldeinformationen die passenden App Configuration-Rollenzuweisungen zu, damit von der Aufgabe auf den App Configuration-Speicher zugegriffen werden kann.

1. Gehen Sie zu Ihrem App Configuration-Zielspeicher. 
1. Wählen Sie im linken Menü **Zugriffssteuerung (IAM)** aus.
1. Wählen Sie im rechten Bereich **Rollenzuweisungen hinzufügen** aus.
    
    :::image type="content" source="./media/azure-app-configuration-role-assignment/add-role-assignment-button.png" alt-text="Screenshot der Schaltfläche „Rollenzuweisungen hinzufügen“.":::

1. Wählen Sie für **Rolle** die Option **App Configuration-Datenbesitzer** aus. Diese Rolle ermöglicht der Aufgabe das Ausführen von Lese- und Schreibvorgängen für den App Configuration-Speicher. 
1. Wählen Sie den Dienstprinzipal aus, der der Dienstverbindung zugeordnet ist, die Sie im vorherigen Abschnitt erstellt haben.

    :::image type="content" source="./media/azure-app-configuration-role-assignment/add-role-assignment.png" alt-text="Screenshot des Dialogfelds „Rollenzuweisung hinzufügen“.":::
