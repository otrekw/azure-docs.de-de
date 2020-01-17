---
title: Hinzufügen von Benutzern als Labbesitzer in Azure Lab Services
description: In diesem Artikel erfahren Sie, wie ein Administrator einen Benutzer als Besitzer eines Labs hinzufügen kann.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/20/2019
ms.author: spelluru
ms.openlocfilehash: 9e007ee6c95843b7e8ef9d7a701237034811489e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75475252"
---
# <a name="how-to-add-a-user-as-an-owner-of-a-classroom-lab-in-azure-lab-services"></a>Hinzufügen eines Benutzers als Besitzer eines Classroom-Labs in Azure Lab Services
In diesem Artikel erfahren Sie, wie Sie einen Benutzer in Azure Lab Services als Besitzer eines Labs hinzufügen.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Hinzufügen von Benutzern zur Leserrolle für das Labkonto
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus. Suchen Sie nach **Lab-Dienste**, und klicken Sie auf das Suchergebnis.
3. Wählen Sie Ihr **Labkonto** aus der Liste aus. 
2. Wählen Sie im linken Menü der Seite **Labkonto** die Option **Zugriffssteuerung (IAM)** aus. 
2. Klicken Sie auf der Seite **Zugriffssteuerung (IAM)** in der Symbolleiste auf **Hinzufügen**, und wählen Sie dann **Rollenzuweisung hinzufügen** aus.

    ![Rollenzuweisung für das Labkonto ](../media/how-to-add-user-lab-owner/lab-account-access-control-page.png)
3. Führen Sie die folgenden Schritte auf der Seite **Rollenzuweisung hinzufügen** durch: 
    1. Wählen Sie die **Rolle** **Leser** aus. 
    2. Wählen Sie den Benutzer aus. 
    3. Wählen Sie **Speichern** aus. 

        ![Hinzufügen von Benutzern zur Leserrolle für das Labkonto ](../media/how-to-add-user-lab-owner/reader-lab-account.png)

## <a name="add-user-to-the-owner-role-for-the-lab"></a>Hinzufügen von Benutzern zur Benutzerrolle für das Lab

1. Kehren Sie zurück zur Seite **Labkonto**, und klicken Sie im linken Menü auf **Alle Labs**.
2. Wählen Sie das **Lab** aus, zu dem Sie einen Benutzer als Besitzer hinzufügen möchten. 
    
    ![Auswählen des Labs ](../media/how-to-add-user-lab-owner/select-lab.png)    
3. Wählen Sie auf der Seite **Lab** im linken Menü die Option **Zugriffssteuerung (IAM)** aus.
4. Klicken Sie auf der Seite **Zugriffssteuerung (IAM)** in der Symbolleiste auf **Hinzufügen**, und wählen Sie dann **Rollenzuweisung hinzufügen** aus.
5. Führen Sie die folgenden Schritte auf der Seite **Rollenzuweisung hinzufügen** durch: 
    1. Wählen Sie die **Rolle** **Besitzer** aus. 
    2. Wählen Sie den Benutzer aus. 
    3. Wählen Sie **Speichern** aus. 

## <a name="next-steps"></a>Nächste Schritte
Bestätigen Sie, dass das Lab dem Benutzer angezeigt wird, sobald er sich beim [Lab Services-Portal](https://labs.azure.com) anmeldet.