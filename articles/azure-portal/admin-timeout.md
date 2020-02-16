---
title: Festlegen des Inaktivitätstimeouts auf Verzeichnisebene für Benutzer des Azure-Portals | Microsoft-Dokumentation
description: Administratoren können die maximale Leerlaufzeit erzwingen, nach der eine Sitzung abgemeldet wird. Die Richtlinie für das Inaktivitätstimeout wird auf Verzeichnisebene festgelegt.
services: azure-portal
keywords: Einstellungen, Timeout
author: mgblythe
ms.author: mblythe
ms.date: 02/10/2020
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 950580048f0496fd8436901938a5b6768c61bab6
ms.sourcegitcommit: f718b98dfe37fc6599d3a2de3d70c168e29d5156
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2020
ms.locfileid: "77132076"
---
# <a name="set-directory-level-inactivity-timeout"></a>Festlegen des Inaktivitätstimeouts auf Verzeichnisebene

Die Einstellung für das Inaktivitätstimeout schützt Ihre Ressourcen vor nicht autorisiertem Zugriff, wenn die Benutzer vergessen, ihre Arbeitsstation abzusichern. Wenn ein Benutzer sich für längere Zeit keine Aktionen ausführt, wird seine Sitzung im Azure-Portal automatisch abgemeldet. Administratoren in der [globalen Administratorrolle](../active-directory/users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator) können die maximale Leerlaufzeit erzwingen, nach der eine Sitzung abgemeldet wird. Die Einstellung für das Inaktivitätstimeout wird auf Verzeichnisebene angewandt. Weitere Informationen zu Verzeichnissen finden Sie unter [Übersicht über Active Directory Domain Services](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

## <a name="configure-the-inactive-timeout-setting"></a>Konfigurieren der Einstellung für das Inaktivitätstimeout

Wenn Sie ein globaler Administrator sind und eine Einstellung für das Leerlauftimeout für alle Benutzer des Azure-Portals erzwingen möchten, führen Sie die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie in der globalen Kopfzeile **Einstellungen** aus.
3. Wählen Sie den Linktext **Timeout auf Verzeichnisebene konfigurieren** aus.

    ![Screenshot der Portaleinstellungen mit hervorgehobenem Linktext](./media/admin-timeout/settings.png)

4. Eine neue Seite wird geöffnet. Wählen Sie auf der Seite **Timeout für Inaktivität auf Verzeichnisebene konfigurieren** die Option **Leerlauftimeout auf Verzeichnisebene für Azure-Portal aktivieren** aus, um die Einstellung zu aktivieren.
5. Geben Sie als Nächstes die **Stunden** und die **Minuten** für die maximale Zeit ein, die sich ein Benutzer im Leerlauf befinden darf, bevor die Sitzung automatisch abgemeldet wird.
6. Wählen Sie **Übernehmen**.

    ![Screenshot der Seite zum Festlegen des Inaktivitätstimeouts auf Verzeichnisebene](./media/admin-timeout/configure.png)

Um zu überprüfen, ob die Richtlinie für das Inaktivitätstimeout ordnungsgemäß festgelegt ist, wählen Sie in der globalen Kopfzeile **Benachrichtigungen** aus. Vergewissern Sie sich, dass eine Erfolgsbenachrichtigung aufgeführt ist.

  ![Screenshot mit Meldung zur erfolgreichen Benachrichtigung für Inaktivitätstimeout auf Verzeichnisebene](./media/admin-timeout/confirmation.png)

Diese Einstellung ist für neue Sitzungen wirksam. Für Benutzer, die bereits angemeldet sind, wird sie nicht sofort angewandt.

> [!NOTE]
> Wenn ein Administrator eine Timeouteinstellung auf Verzeichnisebene konfiguriert hat, können Benutzer die Richtlinie außer Kraft setzen und ihre eigene Dauer bis zur Abmeldung bei Inaktivität festlegen. Der Benutzer muss jedoch ein kleineres Zeitintervall als das auf Verzeichnisebene festgelegte auswählen.
>

## <a name="next-steps"></a>Nächste Schritte

* [Festlegen Ihrer Azure-Portaleinstellungen](set-preferences.md)
* [Exportieren oder Löschen von Benutzereinstellungen](azure-portal-export-delete-settings.md)
* [Aktivieren des hohen Kontrasts oder Ändern des Designs](azure-portal-change-theme-high-contrast.md)
