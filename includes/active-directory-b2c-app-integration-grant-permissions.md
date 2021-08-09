---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 06/11/2021
ms.author: mimart
ms.openlocfilehash: 6d62dda341c6acb783bd2525c2f6ba7a76b312ec
ms.sourcegitcommit: 8651d19fca8c5f709cbb22bfcbe2fd4a1c8e429f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112072998"
---
1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Wählen Sie die Registerkarte **Meine APIs** aus.
1. Wählen Sie die API aus, für die der Webanwendung Zugriff gewährt werden soll. Beispiel: *my-api1*.
1. Erweitern Sie unter **Berechtigung** den Eintrag **Aufgaben**, und wählen Sie die zuvor definierten Bereiche aus. Beispiel: *tasks.read* und *tasks.write*.
1. Wählen Sie **Berechtigungen hinzufügen** aus.
1. Wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie **Ja** aus.
1. Wählen Sie **Aktualisieren** aus, und vergewissern Sie sich, dass für beide Bereiche unter **Status** der Status „Gewährt für...“ angezeigt wird.
1. Wählen Sie in der Liste **Konfigurierte Berechtigungen** Ihren Bereich aus, und kopieren Sie den vollständigen Namen des Bereichs. 

    ![Abrufen des Bereichs Ihrer Webanwendung](./media/active-directory-b2c-app-integration-grant-permissions/get-azure-ad-b2c-app-permissions.png)  