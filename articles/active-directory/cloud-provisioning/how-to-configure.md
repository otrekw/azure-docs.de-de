---
title: Konfiguration eines neuen Agent für die Azure AD Connect-Cloudbereitstellung
description: In diesem Thema wird die Installation der Cloudbereitstellung beschrieben.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f08a1359cfd8a2793d92315a6b03567b0b3f847d
ms.sourcegitcommit: d614a9fc1cc044ff8ba898297aad638858504efa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2019
ms.locfileid: "74997121"
---
# <a name="azure-ad-connect-cloud-provisioning-new-configuration"></a>Neue Konfiguration für die Azure AD Connect-Cloudbereitstellung

Nachdem Sie den Agent installiert haben, müssen Sie sich beim Azure-Portal anmelden und die Bereitstellung konfigurieren.  Führen Sie die folgenden Schritte aus, um den Agent zu aktivieren:

## <a name="configure-provisioning"></a>Konfigurieren der Bereitstellung
Führen Sie die folgenden Schritte aus, um die Bereitstellung zu konfigurieren:

1.  Klicken Sie im Azure AD-Portal auf **Azure Active Directory**.
2.  Klicken Sie auf **Azure AD Connect**.
3.  Wählen Sie **Bereitstellung verwalten (Vorschau)** 
![](media/how-to-configure/manage1.png) aus.

4.  Klicken Sie auf **Neue Konfiguration**.
5.  Auf dem Konfigurationsbildschirm ist die lokale Domäne bereits eingetragen.
6. Geben Sie eine **E-Mail-Adresse für Benachrichtigungen** ein. An diese E-Mail-Adresse wird eine Benachrichtigung gesendet, wenn die Bereitstellung nicht fehlerfrei ist.  
8. Verschieben Sie den Auswahlregler auf **Aktivieren**, und klicken Sie auf **Speichern**.
![](media/tutorial-single-forest/configure2.png)

## <a name="scoping-provisioning-to-specific-users-and-groups"></a>Beschränken der Bereitstellung auf bestimmte Benutzer und Gruppen
Wenn der Agent nur bestimmte Benutzer und Gruppen synchronisieren soll, können Sie eine Bereichsbeschränkung festlegen. Für den Bereich können Sie lokale AD-Gruppen oder Organisationseinheiten verwenden. Gruppen und Organisationseinheiten können im Rahmen einer Konfiguration nicht konfiguriert werden. 

1.  Klicken Sie im Azure AD-Portal auf **Azure Active Directory**.
2.  Klicken Sie auf **Azure AD Connect**.
3.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
4.  Klicken Sie unter **Konfiguration** auf Ihre Konfiguration.  
![](media/how-to-configure/scope1.png)

5.  Wählen Sie unter **Konfigurieren** die Option **Alle Benutzer** aus, um den Bereich der Konfigurationsregel zu ändern.
![](media/how-to-configure/scope2.png)

6. Auf der rechten Seite können Sie den Bereich so ändern, dass er nur die Sicherheitsgruppen enthält, indem Sie den DN (Distinguished Name) der Gruppe eingeben und auf **Hinzufügen** klicken.
![](media/how-to-configure/scope3.png)

7. Sie können den Bereich aber auch so ändern, dass er nur bestimmte Organisationseinheiten enthält. Klicken Sie auf **Fertig** und dann auf **Speichern**.
![](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Erneutes Starten der Bereitstellung 
Wenn Sie nicht auf die nächste geplante Ausführung warten möchten, können Sie mithilfe der Schaltfläche „Bereitstellung neu starten“ die Ausführung der Bereitstellung auslösen. 
1.  Klicken Sie im Azure AD-Portal auf **Azure Active Directory**.
2.  Klicken Sie auf **Azure AD Connect**.
3.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
4.  Klicken Sie unter **Konfiguration** auf Ihre Konfiguration.  
![](media/how-to-configure/scope1.png)

5.  Klicken Sie am oberen Rand auf **Bereitstellung neu starten**.

## <a name="removing-a-configuration"></a>Entfernen einer Konfiguration
Wenn Sie eine Konfiguration löschen möchten, können Sie dazu die folgenden Schritte ausführen.

1.  Klicken Sie im Azure AD-Portal auf **Azure Active Directory**.
2.  Klicken Sie auf **Azure AD Connect**.
3.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
4.  Klicken Sie unter **Konfiguration** auf Ihre Konfiguration.  
![](media/how-to-configure/scope1.png)

5.  Klicken Sie am oberen Rand auf **Löschen**.
![](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Vor dem Löschen einer Konfiguration wird keine Bestätigung angezeigt. Vergewissern Sie sich daher, dass Sie diese Aktion tatsächlich ausführen möchten, bevor Sie auf **Löschen** klicken.


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist eine Bereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
