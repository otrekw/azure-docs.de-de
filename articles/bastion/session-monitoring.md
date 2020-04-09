---
title: Überwachen und Verwalten von Azure Bastion-Sitzungen | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine laufende Sitzung auswählen und die Trennung erzwingen oder die Sitzung löschen.
services: bastion
author: charwen
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: 6bf80be4868295145fa79ae29d5322181b6131d1
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619195"
---
# <a name="session-monitoring-and-management-for-azure-bastion"></a>Überwachen und Verwalten von Azure Bastion-Sitzungen

Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich nahtlos mit jeder VM in diesem virtuellen Netzwerk zu verbinden. Wenn Benutzer eine Verbindung mit Workloads herstellen, kann Azure Bastion zur Überwachung von Remotesitzungen und für schnelle Verwaltungsaktionen verwendet werden. Bei der Überwachung von Azure Bastion-Sitzungen können Sie sehen, welche Benutzer mit welchen VMs verbunden sind. Neben der IP-Adresse, über die der Benutzer eine Verbindung hergestellt hat, sehen Sie auch, wie lange die Verbindung andauert und wann sie hergestellt wurde. Mit der Sitzungsverwaltung können Sie eine laufende Sitzung auswählen und die Trennung erzwingen bzw. eine Sitzung löschen, um den Benutzer von der laufenden Sitzung zu trennen.

## <a name="monitor-remote-sessions"></a><a name="monitor"></a>Überwachen von Remotesitzungen

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Azure Bastion-Ressource, und wählen Sie auf der Azure Bastion-Seite **Sitzungen** aus.

   ![Sitzungen](./media/session-monitoring/sessions.png)
2. Rechts auf der Seite **Sitzungen** sehen Sie die laufenden Remotesitzungen.

   ![Sitzung anzeigen](./media/session-monitoring/view-session.png)
3. Wählen Sie **Aktualisieren** aus, um die aktualisierte Liste der Remotesitzungen anzuzeigen. Wenn Sie „Aktualisieren“ auswählen, ruft Azure Bastion die neuesten Überwachungsinformationen ab und aktualisiert sie im Portal.

   ![Aktualisieren](./media/session-monitoring/refresh.png)

>[!IMPORTANT]
> Aktivieren Sie Port 4443 für eingehenden Datenverkehr vom Gateway-Manager, damit die Sitzungsüberwachung funktioniert.
>

## <a name="delete-or-force-disconnect-an-ongoing-remote-session"></a><a name="view"></a>Löschen oder Erzwingen der Trennung einer laufenden Remotesitzung

Sie können eine Reihe von Sitzungen auswählen und deren Trennung erzwingen. Die folgenden Schritte zeigen, wie Remotesitzungen gelöscht werden:

1. Navigieren Sie zu Ihrer Azure Bastion-Ressource, und wählen Sie auf der Azure Bastion-Seite **Sitzungen** aus.

   ![Navigieren](./media/session-monitoring/navigate.png)
2. Nachdem Sie „Sitzungen“ ausgewählt haben, wird eine Liste mit Remotesitzungen angezeigt.

   ![Sitzungen auflisten](./media/session-monitoring/list.png)
3. Wählen Sie eine bestimmte Remotesitzung, anschließend die Auslassungspunkte auf der rechten Seite der Sitzungszeile und zuletzt **Löschen** aus.

   ![delete](./media/session-monitoring/delete.png)
4. Wenn Sie „Löschen“ auswählen, wird die Remotesitzung getrennt, und der Benutzer erhält in der Remotesitzung eine Meldung, dass seine Verbindung getrennt wurde.

   ![Trennen](./media/session-monitoring/disconnect.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).