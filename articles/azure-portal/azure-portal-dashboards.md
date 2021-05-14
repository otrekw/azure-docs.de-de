---
title: Erstellen eines Dashboards im Azure-Portal
description: In diesem Artikel wird beschrieben, wie Dashboards im Azure-Portal erstellt und angepasst werden.
ms.assetid: ff422f36-47d2-409b-8a19-02e24b03ffe7
ms.topic: how-to
ms.date: 04/15/2021
ms.openlocfilehash: 0666a9f8ca9df2fa44a7eaa4045c9b5e9a724ff5
ms.sourcegitcommit: 6f1aa680588f5db41ed7fc78c934452d468ddb84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107726078"
---
# <a name="create-a-dashboard-in-the-azure-portal"></a>Erstellen eines Dashboards im Azure-Portal

Dashboards bieten eine fokussierte und organisierte Ansicht Ihrer Cloudressourcen im Azure-Portal. Verwenden Sie Dashboards als Arbeitsbereich, in dem Sie Ressourcen überwachen und schnell Aufgaben für alltägliche Vorgänge starten können. Erstellen Sie benutzerdefinierte Dashboards, die beispielsweise auf Projekten, Aufgaben oder Benutzerrollen basieren.

Das Azure-Portal stellt ein Standarddashboard als Ausgangspunkt bereit. Sie können das Standarddashboard bearbeiten und zusätzliche Dashboards erstellen und anpassen.

> [!NOTE]
> Jeder Benutzer kann bis zu 100 private Dashboards erstellen. Wenn Sie das [Dashboard veröffentlichen und freigeben](azure-portal-dashboard-share-access.md), wird es als Azure-Ressource in Ihrem Abonnement implementiert und nicht auf diesen Grenzwert angerechnet.

In diesem Artikel wird beschrieben, wie Sie ein neues Dashboard erstellen und anpassen. Weitere Informationen zum Freigeben von Dashboards finden Sie unter [Freigeben von Azure-Dashboards mithilfe der rollenbasierten Zugriffssteuerung](azure-portal-dashboard-share-access.md).

## <a name="create-a-new-dashboard"></a>Erstellen eines neuen Dashboards

In diesem Beispiel wird gezeigt, wie Sie ein neues privates Dashboard mit einem zugewiesenen Namen erstellen. Alle Dashboards sind bei ihrer Erstellung privat. Sie können Ihr Dashboard aber bei Bedarf veröffentlichen und für andere Benutzer in Ihrer Organisation freigeben.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

1. Wählen Sie im Menü des Azure-Portals die Option **Dashboard** aus. Die Standardansicht ist möglicherweise bereits auf das Dashboard festgelegt.

    ![Screenshot: Azure-Portal mit ausgewähltem Dashboard](./media/azure-portal-dashboards/portal-menu-dashboard.png)

1. Wählen Sie **Neues Dashboard** und dann **Leeres Dashboard** aus.

    ![Screenshot: Optionen für das neue Dashboard](./media/azure-portal-dashboards/create-new-dashboard.png)

    Mit dieser Aktion werden der **Kachelkatalog**, aus dem Sie Kacheln auswählen können, sowie ein leeres Raster geöffnet, in dem Sie die Kacheln anordnen.

1. Wählen Sie in der Dashboardbezeichnung den Text **Mein Dashboard** aus, und geben Sie einen Namen ein, mit dem Sie das benutzerdefinierte Dashboard leicht identifizieren können.

    :::image type="content" source="media/azure-portal-dashboards/dashboard-name.png" alt-text="Screenshot: Leeres Rasters mit dem Kachelkatalog":::

1. Um das Dashboard ohne Änderungen zu speichern, wählen Sie in der Seitenkopfzeile **Anpassung abgeschlossen** aus. Fahren Sie alternativ mit dem nächsten Abschnitt fort, um Kacheln hinzuzufügen und Ihr Dashboard zu speichern.

In der Dashboardansicht wird nun das neue Dashboard angezeigt. Wählen Sie den Pfeil neben dem Namen des Dashboards aus, um die für Sie verfügbaren Dashboards anzuzeigen. Die Liste kann Dashboards enthalten, die von anderen Benutzern erstellt und freigegeben wurden.

## <a name="edit-a-dashboard"></a>Bearbeiten eines Dashboards

Nun bearbeiten wir das Dashboard zum Hinzufügen, Ändern der Größe und Anordnen von Kacheln, die Ihre Azure-Ressourcen darstellen.

### <a name="add-tiles-from-the-tile-gallery"></a>Hinzufügen von Kacheln aus dem Kachelkatalog

Führen Sie die folgenden Schritte aus, um einem Dashboard Kacheln hinzuzufügen:

1. Wählen Sie in der Seitenkopfzeile ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** aus.

    ![Screenshot: Dashboard mit hervorgehobener Bearbeitungsoption](./media/azure-portal-dashboards/dashboard-edit.png)

1. Durchsuchen Sie den **Kachelkatalog**, oder verwenden Sie das Suchfeld, um nach einer bestimmten Kachel zu suchen. Wählen Sie die Kachel aus, die Sie Ihrem Dashboard hinzufügen möchten.

   :::image type="content" source="media/azure-portal-dashboards/dashboard-tile-gallery.png" alt-text="Screenshot: Kachelkatalog":::

1. Wählen Sie **Hinzufügen** aus, um die Kachel dem Dashboard mit einer Standardgröße und einer Standardposition hinzuzufügen. Sie können die Kachel auf das Raster ziehen und dann an der gewünschten Position platzieren. Fügen Sie alle gewünschten Kacheln hinzu. Hier einige Empfehlungen:

    - Fügen Sie **Alle Ressourcen** hinzu, um alle Ressourcen anzuzeigen, die Sie bereits erstellt haben.

    - Wenn Sie mit mehreren Organisationen arbeiten, fügen Sie dem Dashboard die Kachel **Organisationsidentität** hinzu, um die Organisation eindeutig anzuzeigen, zu der die Ressourcen gehören.

1. Ändern Sie bei Bedarf die Größe der Kachel, indem Sie die untere rechte Ecke der Kachel ziehen und loslassen.

1. Wählen Sie zum Speichern der Änderungen in der Seitenkopfzeile **Speichern** aus. Sie können auch eine Vorschau der Änderungen anzeigen, ohne sie zu speichern, indem Sie in der Seitenkopfzeile **Vorschau** auswählen. Auf dem Vorschaubildschirm können Sie **Speichern** auswählen, um die Änderungen beizubehalten, **Verwerfen**, um sie zu entfernen, oder **Bearbeiten**, um zu den Bearbeitungsoptionen zurückzukehren und weitere Änderungen vorzunehmen.

   :::image type="content" source="media/azure-portal-dashboards/dashboard-save.png" alt-text="Screenshot: Optionen „Vorschau“, „Speichern“ und „Verwerfen“":::

### <a name="pin-content-from-a-resource-page"></a>Anheften von Inhalten von einer Ressourcenseite

Sie können Kacheln auch direkt über eine Ressourcenseite zu Ihrem Dashboard hinzufügen.

Viele Ressourcenseiten enthalten ein Stecknadelsymbol in der Befehlsleiste. Wenn Sie dieses Symbol auswählen, können Sie eine Kachel, die die Quellseite darstellt, an ein vorhandenes Dashboard oder an ein von Ihnen erstelltes neues Dashboard anheften.

![Screenshot der Befehlsleiste der Seite mit Reißzweckensymbol](./media/azure-portal-dashboards/dashboard-pin-blade.png)

Manchmal wird auch ein Stecknadelsymbol für einen bestimmten Inhalt auf einer Seite angezeigt. Das bedeutet, dass Sie eine Kachel für diesen bestimmten Inhalt anstatt für die gesamte Seite anheften können.

### <a name="resize-or-rearrange-tiles"></a>Ändern der Größe oder Neuanordnung von Kacheln

Führen Sie die folgenden Schritte aus, um die Größe einer Kachel zu ändern oder die Kacheln auf einem Dashboard neu anzuordnen:

1. Wählen Sie ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** in der Kopfzeile der Seite aus.

1. Wählen Sie in der oberen rechten Ecke einer Kachel das Kontextmenü aus. Wählen Sie dann eine Kachelgröße aus. Kacheln, die eine beliebige Größe unterstützen, enthalten auch einen „Ziehpunkt“ in der unteren rechten Ecke, mit dem Sie die Kachel auf die gewünschte Größe ziehen können.

    :::image type="content" source="media/azure-portal-dashboards/dashboard-tile-resize.png" alt-text="Screenshot: Dashboard mit geöffnetem Menü „Kachelgröße“":::

1. Wählen Sie eine Kachel aus, und ziehen Sie sie an eine neue Position im Raster, um das Dashboard anzuordnen.

### <a name="additional-tile-configuration"></a>Zusätzliche Kachelkonfiguration

Einige Kacheln erfordern möglicherweise weitere Konfiguration, um die gewünschten Informationen anzuzeigen. Beispielsweise muss die Kachel **Metrikdiagramm** so eingerichtet werden, dass eine Metrik aus Azure Monitor angezeigt wird. Sie können Kacheldaten auch anpassen, um die Standardzeiteinstellungen des Dashboards außer Kraft zu setzen.

Für alle Kacheln, die eingerichtet werden müssen, wird ein Banner angezeigt, bis Sie die Kachel anpassen. Beim **Metrikdiagramm** ist das Banner **In Metriken bearbeiten**. So passen Sie die Kachel an

1. Wählen Sie im Seitenkopf **Speichern** aus, um den Bearbeitungsmodus zu beenden.

1. Wählen Sie das Banner aus, und nehmen Sie dann die gewünschte Einrichtung vor.

    ![Screenshot einer Kachel, die konfiguriert werden muss](./media/azure-portal-dashboards/dashboard-configure-tile.png)

> [!NOTE]
> Mithilfe einer Markdownkachel können Sie benutzerdefinierte, statische Inhalte in Ihrem Dashboard anzeigen. Dabei kann es sich um grundlegende Anweisungen, ein Bild, eine Reihe von Links oder sogar um Kontaktinformationen handeln. Weitere Informationen zum Verwenden einer Markdownkachel finden Sie unter [Verwenden einer Markdownkachel in Azure-Dashboards zum Anzeigen benutzerdefinierter Inhalte](azure-portal-markdown-tile.md).

### <a name="customize-tile-data"></a>Anpassen von Kacheldaten

Die Daten im Dashboard zeigen automatisch Aktivitäten der letzten 24 Stunden an. Gehen Sie folgendermaßen vor, um eine andere Zeitspanne nur für diese Kachel anzuzeigen:

1. Wählen Sie im Kontextmenü oder über den Filter ![Filtersymbol](./media/azure-portal-dashboards/dashboard-filter.png) in der linken oberen Ecke der Kachel die Option **Kacheldaten anpassen** aus.

    ![Screenshot: Kontextmenü einer Kachel](./media/azure-portal-dashboards/dashboard-customize-tile-data.png)

1. Aktivieren Sie das Kontrollkästchen zum **Außerkraftsetzen der Zeiteinstellungen für das Dashboard auf Kachelebene**.

    ![Screenshot: Dialogfeld zum Konfigurieren der Zeiteinstellungen der Kachel](./media/azure-portal-dashboards/dashboard-override-time-settings.png)

1. Wählen Sie die Zeitspanne aus, die für diese Kachel angezeigt werden soll. Sie können zwischen den letzten 30 Minuten bis zu den letzten 30 Tagen wählen oder einen benutzerdefinierten Bereich definieren.

1. Wählen Sie die anzuzeigende Zeitgranularität aus. Sie können ein beliebiges Inkrement zwischen einer Minute und einem Monat anzeigen.

1. Wählen Sie **Übernehmen**.

## <a name="delete-a-tile"></a>Löschen einer Kachel

Führen Sie zum Entfernen einer Kachel von einem Dashboard eine der folgenden Aktionen aus:

- Wählen Sie das Kontextmenü in der oberen rechten Ecke der Kachel aus, und wählen Sie dann **Aus Dashboard entfernen** aus.

- Wählen Sie ![Bearbeitungssymbol](./media/azure-portal-dashboards/dashboard-edit-icon.png) **Bearbeiten** aus, um den Anpassungsmodus einzugeben. Platzieren Sie den Mauszeiger in der oberen rechten Ecke der Kachel, und wählen Sie dann das ![Löschsymbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) Löschsymbol aus, um die Kachel aus dem Dashboard zu entfernen.

   ![Screenshot: Entfernen einer Kachel vom Dashboard](./media/azure-portal-dashboards/dashboard-delete-tile.png)

## <a name="clone-a-dashboard"></a>Klonen eines Dashboards

Führen Sie die folgenden Schritte aus, um ein vorhandenes Dashboard als Vorlage für ein neues Dashboard zu verwenden:

1. Stellen Sie sicher, dass in der Dashboardansicht das Dashboard angezeigt wird, das Sie kopieren möchten.

1. Wählen Sie in der Kopfzeile der Seite ![Klonsymbol](./media/azure-portal-dashboards/dashboard-clone.png) **Klonen** aus.

1. Eine Kopie des Dashboards mit dem Namen **Klon von** *Ihr Dashboardname* wird im Bearbeitungsmodus geöffnet. Verwenden Sie die weiter oben beschriebenen Schritte in diesem Artikel, um das Dashboard umzubenennen und anzupassen.

## <a name="publish-and-share-a-dashboard"></a>Veröffentlichen und Freigeben eines Dashboards

Wenn Sie ein Dashboard erstellen, ist es standardmäßig ein privates Dashboard. Dies bedeutet, dass Sie die einzige Person sind, die es anzeigen kann. Um Dashboards für andere Benutzer verfügbar zu machen, können Sie sie veröffentlichen und freigeben. Weitere Informationen finden Sie unter [Freigeben von Azure-Dashboards mithilfe der rollenbasierten Zugriffssteuerung](azure-portal-dashboard-share-access.md).

### <a name="open-a-shared-dashboard"></a>Öffnen eines freigegebenen Dashboards

Gehen Sie folgendermaßen vor, um ein freigegebenes Dashboard zu suchen und zu öffnen:

1. Wählen Sie den Pfeil neben dem Dashboardnamen aus.

1. Wählen Sie ein Dashboard in der angezeigten Liste der Dashboards aus. Gehen Sie wie folgt vor, wenn das Dashboard, das Sie öffnen möchten, nicht aufgeführt wird:

    1. Wählen Sie **Alle Dashboards durchsuchen** aus.

        ![Screenshot des Menüs „Dashboardauswahl“](./media/azure-portal-dashboards/dashboard-browse.png)

    1. Wählen Sie im Feld **Typ** die Option **Freigegebene Dashboards** aus.

        ![Screenshot des Auswahlmenüs „Alle Dashboards“](./media/azure-portal-dashboards/dashboard-browse-all.png)

    1. Wählen Sie mindestens ein Abonnement aus. Sie können auch Text eingeben, um Dashboards nach Namen zu filtern.

    1. Wählen Sie ein Dashboard aus der Liste der freigegebenen Dashboards aus.

## <a name="delete-a-dashboard"></a>Löschen eines Dashboards

Führen Sie die folgenden Schritte aus, um ein privates oder freigegebenes Dashboard dauerhaft zu löschen:

1. Wählen Sie das zu löschende Dashboard aus der Liste neben dem Namen des Dashboards aus.

1. Wählen Sie ![Löschsymbol](./media/azure-portal-dashboards/dashboard-delete-icon.png) **Löschen** in der Kopfzeile der Seite aus.

1. Wählen Sie für ein privates Dashboard im Bestätigungsdialogfeld **OK** aus, um das Dashboard zu entfernen. Aktivieren Sie für ein freigegebenes Dashboard im Bestätigungsdialogfeld das Kontrollkästchen, um zu bestätigen, dass das veröffentlichte Dashboard von anderen Benutzern nicht mehr angezeigt werden kann. Wählen Sie anschließend **OK** aus.

    ![Screenshot: Löschbestätigung](./media/azure-portal-dashboards/dashboard-delete-dash.png)

## <a name="recover-a-deleted-dashboard"></a>Wiederherstellen eines gelöschten Dashboards

Wenn Sie in der globalen Azure-Cloud ein _veröffentlichtes_ Dashboard im Azure-Portal löschen, können Sie das Dashboard innerhalb von 14 Tagen nach dem Löschvorgang wiederherstellen. Weitere Informationen finden Sie unter [Wiederherstellen eines gelöschten Dashboards im Azure-Portal](recover-shared-deleted-dashboard.md).

## <a name="next-steps"></a>Nächste Schritte

- [Freigeben von Azure-Dashboards mithilfe der rollenbasierten Zugriffssteuerung](azure-portal-dashboard-share-access.md)
- [Programmgesteuertes Erstellen von Azure-Dashboards](azure-portal-dashboards-create-programmatically.md)
