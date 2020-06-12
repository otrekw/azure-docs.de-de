---
title: Festlegen Ihrer Azure-Portaleinstellungen | Microsoft-Dokumentation
description: Sie können die Standardeinstellungen des Azure-Portals ändern, um sie an Ihre eigenen Vorlieben anzupassen. Zu den Einstellungen gehören das Timeout inaktiver Sitzungen, die Standardansicht, der Menümodus, der Kontrast, das Design, Benachrichtigungen sowie Sprache und regionale Formate.
services: azure-portal
keywords: Einstellungen, Timeout, Sprache, regional
author: mgblythe
ms.author: mblythe
ms.date: 12/19/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: a3f51c356f4476782bb830b2702a8fe87c79235c
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/03/2020
ms.locfileid: "84308063"
---
# <a name="set-your-azure-portal-preferences"></a>Festlegen Ihrer Azure-Portaleinstellungen

Sie können die Standardeinstellungen des Azure-Portals ändern, um sie an Ihre eigenen Vorlieben anzupassen. Alle unten aufgeführten Einstellungen können geändert werden:

* [Timeout inaktiver Sitzungen](#change-the-idle-duration-for-inactive-sign-out)
* [Standardansicht](#choose-your-default-view)
* [Portalmenümodus](#choose-a-portal-menu-mode)
* [Farbe und Design mit hohem Kontrast](#choose-a-theme)
* [Popupbenachrichtigungen](#enable-or-disable-pop-up-notifications)
* [Sprache und regionales Format](#change-language-and-regional-settings)

## <a name="change-general-portal-settings"></a>Ändern allgemeiner Portaleinstellungen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie in der globalen Kopfzeile **Einstellungen** aus.

    ![Screenshot, der globale Kopfzeilensymbole mit hervorgehobenen Einstellungen zeigt](./media/set-preferences/header-settings.png)

### <a name="change-the-idle-duration-for-inactive-sign-out"></a>Ändern der Leerlaufdauer für Abmeldung bei Inaktivität

Die Einstellung für das Inaktivitätstimeout schützt Ihre Ressourcen vor nicht autorisiertem Zugriff, wenn Sie vergessen, ihre Arbeitsstation abzusichern. Nachdem Sie eine Weile inaktiv waren, werden Sie automatisch von Ihrer Azure-Portalsitzung abgemeldet.

Wählen Sie die Dropdownliste unter **Bei Inaktivität abmelden** aus. Wählen Sie die Dauer aus, nach der die Azure-Portalsitzung abgemeldet wird, wenn Sie inaktiv sind.

   ![Screenshot der Portaleinstellungen mit hervorgehobenen Timeouteinstellungen bei Inaktivität](./media/set-preferences/inactive-signout-user.png)

Die Änderung wird automatisch gespeichert. Wenn Sie inaktiv sind, meldet sich Ihre Azure-Portalsitzung nach der von Ihnen festgelegten Dauer ab.

Diese Einstellung kann auch von einem Administrator auf Verzeichnisebene vorgenommen werden, um eine maximale Leerlaufzeit zu erzwingen. Wenn ein Administrator eine Timeouteinstellung auf Verzeichnisebene festgelegt hat, können Sie trotzdem Ihre eigene Abmeldedauer bei Inaktivität festlegen. Wählen Sie eine Zeiteinstellung aus, die kleiner als die auf Verzeichnisebene festgelegte ist.

Wenn Ihr Administrator eine Richtlinie für das Inaktivitätstimeout aktiviert hat, aktivieren Sie das Kontrollkästchen **Timeoutrichtlinie bei Verzeichnisinaktivität außer Kraft setzen**. Legen Sie ein Zeitintervall fest, das kleiner als die Richtlinieneinstellung ist.

   ![Screenshot, der Portaleinstellungen mit hervorgehobener Option „Timeoutrichtlinie bei Verzeichnisinaktivität außer Kraft setzen“ zeigt](./media/set-preferences/inactive-signout-override.png)


> [!NOTE]
> Wenn Sie Administrator sind und eine Timeouteinstellung bei Inaktivität für alle Benutzer des Azure-Portals erzwingen möchten, finden Sie weitere Informationen unter [Festlegen des Inaktivitätstimeouts auf Verzeichnisebene für Benutzer des Azure-Portals](admin-timeout.md).
>

### <a name="choose-your-default-view"></a>Auswählen der Standardansicht 

Sie können die Seite ändern, die standardmäßig geöffnet wird, wenn Sie sich am Azure-Portal anmelden.

   ![Screenshot mit Azure-Portaleinstellungen mit hervorgehobener Standardansicht](./media/set-preferences/default-view.png)

Die Einstellung für die Standardansicht steuert, welche Azure-Portalansicht angezeigt wird, wenn Sie sich anmelden. Sie können standardmäßig die Azure-Startseite oder die Dashboardansicht öffnen.

* Die **Startseite** kann nicht angepasst werden.  Sie zeigt Verknüpfungen mit beliebten Azure-Diensten an und listet die Ressourcen auf, die Sie zuletzt verwendet haben. Außerdem werden nützliche Links zu Ressourcen wie Microsoft Learn und der Azure-Roadmap bereitgestellt.
* Dashboards können angepasst werden, um einen Arbeitsbereich zu erstellen, der nur für Sie konzipiert wurde. Beispielsweise können Sie ein Dashboard erstellen, das sich auf Projekte, Aufgaben oder Rollen konzentriert. Wenn Sie **Dashboard** auswählen, verwendet Ihre Standardansicht das zuletzt verwendete Dashboard.

### <a name="choose-a-portal-menu-mode"></a>Auswählen eines Portalmenümodus

Der Standardmodus für das Portalmenü steuert, wie viel Platz das Portalmenü auf der Seite einnimmt.

* Wenn sich das Portalmenü im **Flyoutmodus** befindet, wird es ausgeblendet, bis Sie es benötigen. Wählen Sie das Menüsymbol aus, um das Menü zu öffnen oder zu schließen.
* Wenn Sie für das Portalmenü den **angedockten** Modus auswählen, wird es immer angezeigt. Sie können das Menü reduzieren, um den Arbeitsbereich zu vergrößern. 

### <a name="choose-a-theme"></a>Auswählen eines Designs

Das Design, das Sie auswählen, wirkt sich auf die Hintergrund- und Schriftartfarben aus, die im Azure-Portal angezeigt werden. Sie können eines der vier vordefinierten Farbdesigns auswählen. Wählen Sie die einzelnen Miniaturansichten aus, um das Design zu ermitteln, das Ihnen am besten gefällt.

   ![Screenshot mit Azure-Portaleinstellungen mit hervorgehobenen Designs](./media/set-preferences/theme.png)

Sie können stattdessen eines der Designs mit hohem Kontrast auswählen. Die Einstellungen für hohen Kontrast erleichtern die Lesbarkeit des Azure-Portals für Benutzer mit einer Sehbehinderung und überschreiben alle anderen Designs. Weitere Informationen finden Sie unter [Aktivieren von hohem Kontrast oder Ändern des Designs](azure-portal-change-theme-high-contrast.md).

### <a name="enable-or-disable-pop-up-notifications"></a>Aktivieren oder Deaktivieren von Popupbenachrichtigungen

Benachrichtigungen sind Systemmeldungen, die sich auf Ihre aktuelle Sitzung beziehen. Sie enthalten Informationen wie Ihr aktuelles Guthaben, wann die von Ihnen aktuell erstellten Ressourcen verfügbar werden oder bestätigen beispielsweise Ihre letzte Aktion. Wenn Popupbenachrichtigungen aktiviert sind, werden die Meldungen kurzzeitig in der oberen Ecke des Bildschirms angezeigt. 

Aktivieren bzw. deaktivieren Sie das Kontrollkästchen **Popupbenachrichtigungen aktivieren**, um Popupbenachrichtigungen zu aktivieren oder zu deaktivieren.

   ![Screenshot mit Azure-Portaleinstellungen mit hervorgehobenen Popupbenachrichtigungen](./media/set-preferences/popup-notifications.png)

Wenn Sie alle während der aktuellen Sitzung empfangenen Benachrichtigungen lesen möchten, wählen Sie **Benachrichtigungen** aus der globalen Kopfzeile aus.

   ![Screenshot, der die globale Kopfzeile des Azure-Portals mit hervorgehobenen Benachrichtigungen zeigt](./media/set-preferences/read-notifications.png)

Wenn Sie Benachrichtigungen aus vorherigen Sitzungen lesen möchten, suchen Sie nach Ereignissen im Aktivitätsprotokoll. Weitere Informationen finden Sie unter [Anzeigen und Abrufen von Azure-Aktivitätsprotokollereignissen](/azure/azure-monitor/platform/activity-log-view).

### <a name="settings-under-useful-links"></a>Einstellungen unter nützliche Links

Wenn Sie Änderungen an den Azure-Portaleinstellungen vorgenommen haben und diese verwerfen möchten, wählen Sie **Standardeinstellungen wiederherstellen** aus. Alle Änderungen, die Sie an Portaleinstellungen vorgenommen haben, gehen verloren. Diese Option wirkt sich nicht auf Dashboardanpassungen aus.

Weitere Informationen zum **Exportieren aller Einstellungen** oder **Löschen aller Einstellungen und privaten Dashboards** finden Sie unter [Exportieren oder Löschen von Benutzereinstellungen](azure-portal-export-delete-settings.md).

## <a name="change-language-and-regional-settings"></a>Ändern der Sprache und der regionalen Einstellungen

Es gibt zwei Einstellungen, mit denen gesteuert wird, wie der Text im Azure-Portal angezeigt wird. Mit der Einstellung **Sprache** wird die Sprache festgelegt, in der Text im Azure-Portal angezeigt wird. **Regionales Format** steuert, wie Datumsangaben, Uhrzeiten, Ziffern und Währungen angezeigt werden.

Zum Ändern der Sprache, die im Azure-Portal verwendet wird, verwenden Sie die Dropdownliste, um aus den verfügbaren Sprachen auszuwählen.

Die Auswahl des regionalen Formats ändert sich, um regionale Optionen nur für die Sprache anzuzeigen, die Sie ausgewählt haben. Um diese automatische Auswahl zu ändern, wählen Sie das gewünschte regionale Format mithilfe der Dropdownliste aus.

Wenn Sie z.B. Englisch als Sprache und dann USA als regionales Format auswählen, wird die Währung in US-Dollar angezeigt. Wenn Sie Englisch als Sprache und dann Europa als regionales Format auswählen, wird die Währung in Euro angezeigt.

Wählen Sie **Anwenden** aus, um Ihre Sprache und die Einstellungen für das regionale Format zu aktualisieren.

   ![Screenshot mit Einstellungen für Sprache und regionales Format](./media/set-preferences/language.png)

>[!NOTE]
>Diese Einstellungen für Sprache und regionales Format wirken sich nur auf das Azure-Portal aus. Dokumentationslinks, die in einer neuen Registerkarte oder in einem neuen Fenster geöffnet werden, verwenden die Spracheinstellungen Ihres Browsers, um die anzuzeigende Sprache zu ermitteln.
>

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Freigeben von benutzerdefinierten Dashboards](azure-portal-dashboards.md)
* [Anleitungsvideos für das Azure-Portal](azure-portal-video-series.md)
