---
title: Konfiguration eines neuen Agent für die Azure AD Connect-Cloudbereitstellung
description: In diesem Artikel wird die Installation der Cloudbereitstellung beschrieben.
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
ms.openlocfilehash: 853950cf441007eac0170f32f28f2d9c16a71292
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75549430"
---
# <a name="azure-ad-connect-cloud-provisioning-new-agent-configuration"></a>Konfiguration eines neuen Agent für die Azure AD Connect-Cloudbereitstellung

Nachdem Sie den Agent installiert haben, müssen Sie sich beim Azure-Portal anmelden und die Azure Active Directory (Azure AD) Connect-Cloudbereitstellung konfigurieren. Führen Sie die folgenden Schritte aus, um den Agent zu aktivieren.

## <a name="configure-provisioning"></a>Konfigurieren der Bereitstellung
Gehen Sie zum Konfigurieren der Bereitstellung wie folgt vor.

1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
1.  Wählen Sie **Azure AD Connect** aus.
1.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.

    ![Bereitstellung verwalten (Vorschau)](media/how-to-configure/manage1.png)

1.  Wählen Sie **Neue Konfiguration** aus.
1.  Auf dem Konfigurationsbildschirm ist die lokale Domäne bereits vorausgefüllt.
1.  Geben Sie eine **E-Mail-Adresse für Benachrichtigungen** ein. An diese E-Mail-Adresse wird eine Benachrichtigung gesendet, wenn die Bereitstellung nicht fehlerfrei ist.
1.  Verschieben Sie den Auswahlregler auf **Aktivieren**, und wählen Sie **Speichern** aus.

    ![Azure AD-Bereitstellung (Vorschau)](media/tutorial-single-forest/configure2.png)

## <a name="scope-provisioning-to-specific-users-and-groups"></a>Beschränken der Bereitstellung auf bestimmte Benutzer und Gruppen
Durch Verwenden lokaler Active Directory-Gruppen oder Organisationseinheiten können Sie den Agent so einrichten, dass nur bestimmte Benutzer und Gruppen synchronisiert werden. Gruppen und Organisationseinheiten können im Rahmen einer Konfiguration nicht konfiguriert werden. 

1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
1.  Wählen Sie **Azure AD Connect** aus.
1.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
1.  Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.

    ![Konfigurationsabschnitt](media/how-to-configure/scope1.png)

1.  Wählen Sie unter **Konfigurieren** die Option **Alle Benutzer** aus, um den Bereich der Konfigurationsregel zu ändern.

    ![Option „Alle Benutzer“](media/how-to-configure/scope2.png)

1. Auf der rechten Seite können Sie den Bereich so ändern, dass nur Sicherheitsgruppen einbezogen werden. Geben Sie den Distinguished Name der Gruppe ein, und wählen Sie **Hinzufügen** aus.

    ![Option „Ausgewählte Sicherheitsgruppen“](media/how-to-configure/scope3.png)

1.  Sie können den Bereich auch so ändern, dass nur bestimmte Organisationseinheiten eingeschlossen werden. Wählen Sie **Fertig** und dann **Speichern** aus.

    ![Option „Ausgewählte Organisationseinheiten“](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Erneutes Starten der Bereitstellung 
Wenn Sie nicht auf die nächste geplante Ausführung warten möchten, können Sie mithilfe der Schaltfläche **Bereitstellung neu starten** die Ausführung der Bereitstellung auslösen. 
1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
1.  Wählen Sie **Azure AD Connect** aus.
1.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
1.  Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.

    ![Konfigurationsauswahl zum Neustart der Bereitstellung](media/how-to-configure/scope1.png)

1.  Wählen Sie am oberen Rand **Bereitstellung neu starten** aus.

## <a name="remove-a-configuration"></a>Entfernen einer Konfiguration
Führen Sie die folgenden Schritte aus, um eine Konfiguration zu löschen.

1.  Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.
1.  Wählen Sie **Azure AD Connect** aus.
1.  Wählen Sie **Bereitstellung verwalten (Vorschau)** aus.
1.  Wählen Sie unter **Konfiguration** Ihre Konfiguration aus.

    ![Konfigurationsauswahl zum Entfernen der Konfiguration](media/how-to-configure/scope1.png)

1.  Wählen Sie am oberen Rand des Konfigurationsbildschirms **Löschen** aus.

    ![Schaltfläche „Löschen“](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>Vor dem Löschen einer Konfiguration wird keine Bestätigung ausgegeben. Stellen Sie sicher, dass es sich um die Aktion handelt, die Sie ausführen möchten, bevor Sie **Löschen** auswählen.


## <a name="next-steps"></a>Nächste Schritte 

- [Was ist die Identitätsbereitstellung?](what-is-provisioning.md)
- [Was ist die Azure AD Connect-Cloudbereitstellung?](what-is-cloud-provisioning.md)
