---
title: 'Azure AD: Aktivieren der Einstellungen für externe B2B-Zusammenarbeit'
description: Hier erfahren Sie, wie Sie die externe B2B-Zusammenarbeit in Azure Active Directory aktivieren und steuern, wer Gastbenutzer einladen darf. Verwenden Sie die Rolle „Gasteinladender“, um Einladungen zu delegieren.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: fa2b5f17af466d5ea2b6e3ba942fafc24cc94713
ms.sourcegitcommit: 0fa52a34a6274dc872832560cd690be58ae3d0ca
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2020
ms.locfileid: "84204258"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Aktivieren der externen B2B-Zusammenarbeit und Steuern, wer Gäste einladen kann

In diesem Artikel wird beschrieben, wie die B2B-Zusammenarbeit in Azure Active Directory (Azure AD) aktiviert wird und wie Sie festlegen, wer Gäste einladen darf. Per Standardeinstellung können alle Benutzer und Gäste in Ihrem Verzeichnis Gäste einladen, auch denn, wenn ihnen keine Administratorrolle zugewiesen ist. In den Einstellungen für die externe Zusammenarbeit können Sie die Möglichkeit, Gäste einzuladen, für verschiedene Benutzertypen in Ihrer Organisation aktivieren oder deaktivieren. Diese Möglichkeit können Sie auch an einzelne Benutzer delegieren, indem Sie diesen Benutzern Rollen zuweisen, die es ihnen ermöglichen, Gäste einzuladen.

## <a name="configure-b2b-external-collaboration-settings"></a>Konfigurieren der Einstellungen für externe B2B-Zusammenarbeit

In der Azure AD B2B-Zusammenarbeit kann ein Mandantenadministrator folgende Einladungsrichtlinien festlegen:

- Einladungen deaktivieren.
- Nur Administratoren und Benutzer in der Rolle „Gasteinladender“ dürfen einladen.
- Administratoren, Benutzer in der Rolle „Gasteinladender“ und Mitglieder dürfen einladen.
- Alle Benutzer, einschließlich Gästen, dürfen einladen.

Per Standardeinstellung können alle Benutzer, einschließlich Gäste, Gastbenutzer einladen.

### <a name="to-configure-external-collaboration-settings"></a>So können Sie die Einstellungen für externe Zusammenarbeit konfigurieren:

1. Melden Sie sich als Mandantenadministrator im [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie **Azure Active Directory** aus.
3. Wählen Sie **Externe Identitäten** > **Einstellungen für externe Zusammenarbeit** aus.
6. Wählen Sie auf der Seite **Einstellungen für externe Zusammenarbeit** die Richtlinien aus, die Sie aktivieren möchten.

   ![Einstellungen für externe Zusammenarbeit](./media/delegate-invitations/control-who-to-invite.png)

  - **Berechtigungen für Gastbenutzer sind eingeschränkt:** Diese Richtlinie legt Berechtigungen für Gäste in Ihrem Verzeichnis fest. Wenn Sie verhindern möchten, dass Gäste bestimmte Verzeichnisaufgaben ausführen können, z. B. Benutzer, Gruppen oder andere Verzeichnisressourcen aufzulisten, legen Sie diese Richtlinie auf **Ja** fest. Wenn Sie Gästen denselben Zugriff auf Verzeichnisdaten gewähren möchten wie regulären Benutzern in Ihrem Verzeichnis, legen Sie diese Richtlinie auf **Nein** fest.
   - **Administratoren und Benutzer mit der Rolle „Gasteinladender“ können einladen:** Wenn Sie Administratoren und Gästen, die die Rolle „Gasteinladender“ innehaben, erlauben möchten, Gäste einzuladen, legen Sie diese Richtlinie auf **Ja** fest.
   - **Mitglieder können einladen:** Wenn Sie Mitgliedern in Ihrem Verzeichnis, die keine Administratoren sind, erlauben möchten, Gäste einzuladen, legen Sie diese Richtlinie auf **Ja** fest.
   - **Gäste können einladen:** Wenn Sie Gästen erlauben möchten, andere Gäste einzuladen, legen Sie diese Richtlinie auf **Ja** fest.
   - **Einmalkennung per E-Mail für Gastbenutzer aktivieren (Vorschau):** Weitere Informationen zum Einmalkennungsfeature finden Sie unter [Authentifizierung mit Einmalkennung per E-Mail (Vorschauversion)](one-time-passcode.md).
   - **Einschränkungen für die Zusammenarbeit:** Weitere Informationen dazu, wie Sie Einladungen für bestimmte Domains erlauben oder verbieten können, finden Sie unter [Zulassen oder Blockieren von Einladungen für B2B-Benutzer von bestimmten Organisationen](allow-deny-list.md).
   
   > [!NOTE]
   > Wenn **Mitglieder können einladen** auf **Nein** festgelegt ist und **Administratoren und Benutzer mit der Rolle „Gasteinladender“ können einladen** auf **Ja** festgelegt ist, können Benutzer mit der Rolle **Gasteinladender** weiterhin Gäste einladen.

## <a name="assign-the-guest-inviter-role-to-a-user"></a>Zuweisen der Rolle „Gasteinladender“ an einen Benutzer

Über die Rolle „Gasteinladender“ können Sie einzelnen Benutzern die Möglichkeit geben, Gäste einzuladen, ohne ihnen dafür eine globale Administratorrolle oder eine andere Administratorrolle zuweisen zu müssen. Weisen Sie die Rolle „Gasteinladender“ einzelnen Benutzern zu. Stellen Sie dann sicher, dass die Richtlinie **Administratoren und Benutzer mit der Rolle „Gasteinladender“ können einladen** auf **Ja** festgelegt ist.

Hier ist ein Beispiel, das zeigt, wie PowerShell verwendet wird, um einen Benutzer der Rolle „Gasteinladender“ hinzuzufügen:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Artikel zur Azure AD B2B-Zusammenarbeit:

- [Was ist die Azure AD B2B-Zusammenarbeit?](what-is-b2b.md)
- [Hinzufügen von Gastbenutzern der B2B-Zusammenarbeit ohne Einladung](add-user-without-invite.md)
- [Hinzufügen eines B2B-Zusammenarbeitsbenutzers zu einer Rolle](add-guest-to-role.md)

