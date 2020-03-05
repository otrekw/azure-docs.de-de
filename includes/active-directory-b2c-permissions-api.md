---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 71a6654acd436c27bd2370646dede81778113860
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/29/2020
ms.locfileid: "78186139"
---
#### <a name="applications"></a>[Anwendungen](#tab/applications/)

1. Wählen sie **Anwendungen** und anschließend die Webanwendung aus, die Zugriff auf die API erhalten soll. Beispiel: *webapp1*.
1. Wählen Sie **API-Zugriff** und dann **Hinzufügen** aus.
1. Wählen Sie in der Dropdownliste **API auswählen** die API aus, für die der Webanwendung Zugriff gewährt werden soll. Beispiel: *webapi1*.
1. Wählen Sie in der Dropdownliste **Bereiche auswählen** die Bereiche aus, die Sie zuvor definiert haben. Beispiel: *demo.read* und *demo.write*
1. Klicken Sie auf **OK**.

#### <a name="app-registrations-preview"></a>[App-Registrierungen (Vorschau)](#tab/app-reg-preview/)

1. Wählen Sie **App-Registrierungen (Vorschau)** und anschließend die Webanwendung aus, die Zugriff auf die API haben soll. Beispiel: *webapp1*.
1. Wählen Sie unter **Verwalten** die Option **API-Berechtigungen**.
1. Wählen Sie unter **Konfigurierte Berechtigungen** die Option **Berechtigung hinzufügen** aus.
1. Wählen Sie die Registerkarte **Meine APIs** aus.
1. Wählen Sie die API aus, für die der Webanwendung Zugriff gewährt werden soll. Beispiel: *webapi1*.
1. Erweitern Sie unter **Berechtigung** den Eintrag **Demo**, und wählen Sie dann die zuvor definierten Bereiche aus. Beispiel: *demo.read* und *demo.write*
1. Wählen Sie **Berechtigungen hinzufügen** aus. Warten Sie einige Minuten, bevor Sie mit dem nächsten Schritt fortfahren.
1. Wählen Sie **Administratorzustimmung für (Name Ihres Mandanten) erteilen** aus.
1. Wählen Sie das derzeit angemeldete Administratorkonto aus, oder melden Sie sich mit einem Konto bei Ihrem Azure AD B2C-Mandanten an, dem mindestens die Rolle *Cloudanwendungsadministrator* zugewiesen wurde.
1. Wählen Sie **Akzeptieren** aus.
1. Wählen Sie **Aktualisieren** aus, und vergewissern Sie sich, dass für beide Bereiche unter **Status** der Status „Gewährt für...“ angezeigt wird. Es kann einige Minuten dauern, bis die Berechtigungen weitergegeben wurden.