---
title: Installieren von Updates auf Geräten der Azure Data Box Gateway-Serie | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie über das Azure-Portal und die lokale Webbenutzeroberfläche auf einem Gerät der Azure Data Box Gateway-Serie Updates anwenden.
services: databox
author: twooley
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: twooley
ms.openlocfilehash: 3972aecf70a664980e8279152eb001915ae1bb53
ms.sourcegitcommit: 9c3cfbe2bee467d0e6966c2bfdeddbe039cad029
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/24/2020
ms.locfileid: "88783874"
---
# <a name="update-your-azure-data-box-gateway"></a>Aktualisieren des Azure Data Box Gateway

In diesem Artikel sind die erforderlichen Schritte zum Installieren von Updates auf Ihrem Azure Data Box Gateway über die lokale Webbenutzeroberfläche oder das Azure-Portal beschrieben. Sie installieren Softwareupdates oder -hotfixes, um Ihr Data Box Gateway auf dem neuesten Stand zu halten.

> [!IMPORTANT]
>
> - Update **1911** entspricht der Softwareversion **1.6.1049.786** auf Ihrem Gerät. Informationen zu diesem Update finden Sie in den [Versionshinweisen](data-box-gateway-1911-release-notes.md).
>
> - Beachten Sie, dass das Gerät bei der Installation des Updates oder Hotfixes neu gestartet wird. Da es sich beim Data Box Gateway um ein Gerät mit einem Einzelknoten handelt, werden beim Update der Gerätesoftware alle laufenden E/A-Vorgänge unterbrochen, und es kommt zu einer Gerätedowntime von bis zu 30 Minuten.

Die einzelnen Schritte werden in den folgenden Abschnitten beschrieben.

## <a name="use-the-azure-portal"></a>Verwenden des Azure-Portals

Die Installation von Updates über das Azure-Portal wird empfohlen. Das Gerät sucht einmal täglich automatisch nach Updates. Sobald Updates verfügbar sind, wird im Portal eine Benachrichtigung angezeigt. Sie können die Updates dann herunterladen und installieren.

> [!NOTE]
> Stellen Sie sicher, dass das Gerät fehlerfrei ist und als Status **Online** angezeigt wird, bevor Sie mit der Installation der Updates fortfahren.

1. Sobald die Updates für Ihr Gerät verfügbar sind, wird eine Benachrichtigung angezeigt. Wählen Sie die Benachrichtigung aus, oder klicken Sie auf der oberen Befehlsleiste auf **Gerät aktualisieren**. Auf diese Weise können Sie Updates der Gerätesoftware anwenden.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. Bestätigen Sie auf dem Blatt **Geräteupdates**, dass Sie die Lizenzbedingungen für neue Features in den Versionshinweisen gelesen haben.

    Sie können auswählen, ob Sie die Updates **herunterladen und installieren** oder nur **herunterladen** möchten. Sie können diese Updates dann später installieren.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Wenn Sie die Updates herunterladen und installieren möchten, aktivieren Sie die Option zum automatischen Installieren der Updates nach Abschluss des Downloads.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Der Download der Updates wird gestartet. Es wird eine Benachrichtigung angezeigt, dass der Download läuft.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    Außerdem wird im Azure-Portal ein Benachrichtigungsbanner angezeigt. Dies zeigt den Downloadstatus an.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    Sie können diese Benachrichtigung oder **Gerät aktualisieren** auswählen, um den ausführlichen Status des Updates anzuzeigen.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. Nachdem der Download abgeschlossen ist, wird das Benachrichtigungsbanner aktualisiert, um den Abschluss anzuzeigen. Wenn Sie ausgewählt haben, dass die Updates heruntergeladen und installiert werden sollen, wird die Installation automatisch gestartet.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Wenn Sie Updates nur herunterladen möchten, wählen Sie die Benachrichtigung aus, um das Blatt **Geräteupdates** zu öffnen. Wählen Sie **Installieren** aus.
  
    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. Es wird eine Benachrichtigung angezeigt, dass die Installation läuft.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    Im Portal wird außerdem eine Informationsmeldung angezeigt, um darauf hinzuweisen, dass die Installation ausgeführt wird. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Da es sich um ein Gerät mit Einzelknoten handelt, wird das Gerät nach der Installation der Updates neu gestartet. Die kritische Warnung während des Neustarts weist darauf hin, dass das Gerät keinen Heartbeat mehr ausgibt.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    Wählen Sie die Warnung aus, um das entsprechende Geräteereignis anzuzeigen.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. Nachdem die Updates installiert wurden, ändert sich der Gerätestatus in **Online**.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    Wählen Sie auf der oberen Befehlsleiste **Geräteupdates** aus. Vergewissern Sie sich, dass das Update erfolgreich installiert wurde und die entsprechende Version der Gerätesoftwareversion angezeigt wird.

    ![Softwareversion nach dem Update](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>Verwenden der lokalen Webbenutzeroberfläche

Die Verwendung der lokalen Webbenutzeroberfläche umfasst zwei Schritte:

- Herunterladen des Updates oder Hotfixes
- Installieren des Updates oder Hotfixes

Jeder dieser Schritte wird in den folgenden Abschnitten ausführlich erläutert.

### <a name="download-the-update-or-the-hotfix"></a>Herunterladen des Updates oder Hotfixes

Führen Sie die folgenden Schritte aus, um das Update herunterzuladen. Sie können das Update von einem von Microsoft bereitgestellten Speicherort oder aus dem Microsoft Update-Katalog herunterladen.

Führen Sie die folgenden Schritte aus, um das Softwareupdate aus dem Microsoft Update-Katalog herunterzuladen.

1. Starten Sie einen Browser, und navigieren Sie zu [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com).

   ![Katalog durchsuchen](./media/data-box-gateway-apply-updates/download-update-1.png)

2. Geben Sie im Suchfeld des Microsoft Update-Katalogs die KB-Nummer (Knowledge Base) des Hotfixes oder der Nutzungsbedingungen des Updates ein, das Sie herunterladen möchten. Geben Sie z. B. **Azure Data Box Gateway** ein, und klicken Sie dann auf **Suchen**.

   Der Updateeintrag lautet **Azure Data Box Gateway 1911**.

   ![Katalog durchsuchen](./media/data-box-gateway-apply-updates/download-update-2.png)

3. Wählen Sie **Herunterladen** aus. Als Download wird eine Datei mit dem Namen *SoftwareUpdatePackage.exe* angeboten, die dem Gerätesoftwareupdate entspricht. Laden Sie die Datei in einen Ordner auf dem lokalen System herunter. Sie können den Ordner auch in eine Netzwerkfreigabe kopieren, auf die vom Gerät aus zugegriffen werden kann.

   ![Katalog durchsuchen](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>Installieren des Updates oder Hotfixes

Vergewissern Sie sich vor dem Installieren des Updates oder Hotfixes, dass Folgendes erfüllt ist:

- Das Update oder der Hotfix wurde lokal auf Ihren Host heruntergeladen, oder auf das Update oder den Hotfix kann über eine Netzwerkfreigabe zugegriffen werden.
- Der Gerätestatus ist fehlerfrei, wie auf der Seite **Übersicht** der lokalen Webbenutzeroberfläche angezeigt wird.

   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Dieser Vorgang dauert etwa 20 Minuten. Führen Sie die folgenden Schritte aus, um Updates oder Hotfixes zu installieren.

1. Wechseln Sie auf der lokalen Webbenutzeroberfläche zu **Wartung** > **Softwareupdate**. Notieren Sie sich die Softwareversion, die Sie ausführen.

   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Geben Sie den Pfad zur Updatedatei an. Sie können auch zur Installationsdatei des Updates navigieren, sofern sie auf einer Netzwerkfreigabe platziert wurde. Wählen Sie die Softwareupdatedatei mit dem Suffix *SoftwareUpdatePackage.exe* aus.

   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. Wählen Sie **Übernehmen**.

   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. Wenn Sie zur Bestätigung aufgefordert werden, klicken Sie auf **Ja**, um fortzufahren. Da es sich um ein Gerät mit einem Einzelknoten handelt, wird das Gerät neu gestartet, und es kommt zu einer Downtime.
   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. Das Update wird gestartet. Nachdem das Gerät erfolgreich aktualisiert wurde, wird es neu gestartet. Auf die lokale Benutzeroberfläche kann währenddessen nicht zugegriffen werden.

6. Nach dem Neustart wird die Seite **Anmelden** angezeigt. Um sicherzustellen, dass die Gerätesoftware aktualisiert wurde, wechseln Sie auf der lokalen Webbenutzeroberfläche zu **Wartung** >  **Softwareupdate**. Als Softwareversion sollte **1.6.1049.786** angezeigt werden.

   ![Gerät aktualisieren](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über die [Verwaltung des Azure Data Box Gateway](data-box-gateway-manage-users.md)
