---
title: Hinzufügen zusätzlicher Benutzer zu einem Lab in Azure Lab Services
description: In diesem Artikel erfahren Sie, wie ein Administrator einen Benutzer als Besitzer eines Labs in Azure Lab Services hinzufügen kann.
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
ms.date: 02/14/2020
ms.author: spelluru
ms.openlocfilehash: 9302cadf0b900c9faac382bad86b51cea7d828fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443515"
---
# <a name="how-to-add-additional-owners-to-an-existing-lab-in-azure-lab-services"></a>Hinzufügen zusätzlicher Benutzer zu einem vorhandenen Lab in Azure Lab Services
In diesem Artikel erfahren Sie, wie ein Administrator zusätzliche Besitzer zu einem vorhandenen Lab hinzufügen kann.

## <a name="add-user-to-the-reader-role-for-the-lab-account"></a>Hinzufügen von Benutzern zur Leserrolle für das Labkonto
Wenn Sie einem vorhandenen Lab einen Benutzer als zusätzlichen Besitzer hinzufügen möchten, müssen Sie dem Benutzer zunächst **Lese**-Berechtigung für das Lab-Konto erteilen.

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