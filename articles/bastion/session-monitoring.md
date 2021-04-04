---
title: Überwachen und Verwalten von Azure Bastion-Sitzungen | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine laufende Sitzung auswählen und die Trennung erzwingen oder die Sitzung löschen.
services: bastion
author: charwen
ms.service: bastion
ms.topic: how-to
ms.date: 05/21/2020
ms.author: charwen
ms.openlocfilehash: beb4f8fc5b9e22bc3e2bdd4732c8321dfcd70780
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "90980730"
---
# <a name="session-monitoring-and-management-for-azure-bastion"></a>Überwachen und Verwalten von Azure Bastion-Sitzungen

Sobald der Bastion-Dienst provisioniert und in Ihrem virtuellen Netzwerk bereitgestellt wurde, können Sie ihn verwenden, um sich nahtlos mit jeder VM in diesem virtuellen Netzwerk zu verbinden. Wenn Benutzer eine Verbindung mit Workloads herstellen, kann Azure Bastion zur Überwachung von Remotesitzungen und für schnelle Verwaltungsaktionen verwendet werden. Bei der Überwachung von Azure Bastion-Sitzungen können Sie sehen, welche Benutzer mit welchen VMs verbunden sind. Neben der IP-Adresse, über die der Benutzer eine Verbindung hergestellt hat, sehen Sie auch, wie lange die Verbindung andauert und wann sie hergestellt wurde. Mit der Sitzungsverwaltung können Sie eine laufende Sitzung auswählen und die Trennung erzwingen bzw. eine Sitzung löschen, um den Benutzer von der laufenden Sitzung zu trennen.

## <a name="monitor-remote-sessions"></a><a name="monitor"></a>Überwachen von Remotesitzungen

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Azure Bastion-Ressource, und wählen Sie auf der Azure Bastion-Seite **Sitzungen** aus.

   ![Screenshot des Menüs „Einstellungen“ im Azure-Portal mit Auswahl von „Sitzungen“.](./media/session-monitoring/sessions.png)
2. Rechts auf der Seite **Sitzungen** sehen Sie die laufenden Remotesitzungen.

   ![Sitzung anzeigen](./media/session-monitoring/view-session.png)
3. Wählen Sie **Aktualisieren** aus, um die aktualisierte Liste der Remotesitzungen anzuzeigen. Wenn Sie „Aktualisieren“ auswählen, ruft Azure Bastion die neuesten Überwachungsinformationen ab und aktualisiert sie im Portal.

   ![Screenshot einer Sitzung mit Azure Bastion und Hervorhebung der Option „Aktualisieren“.](./media/session-monitoring/refresh.png)


## <a name="delete-or-force-disconnect-an-ongoing-remote-session"></a><a name="view"></a>Löschen oder Erzwingen der Trennung einer laufenden Remotesitzung

Sie können eine Reihe von Sitzungen auswählen und deren Trennung erzwingen. Die folgenden Schritte zeigen, wie Remotesitzungen gelöscht werden:

1. Navigieren Sie zu Ihrer Azure Bastion-Ressource, und wählen Sie auf der Azure Bastion-Seite **Sitzungen** aus.

   ![Screenshot des Azure-Portals mit Auswahl von „Sitzungen“ unter „Einstellungen“.](./media/session-monitoring/navigate.png)
2. Nachdem Sie „Sitzungen“ ausgewählt haben, wird eine Liste mit Remotesitzungen angezeigt.

   ![Sitzungen auflisten](./media/session-monitoring/list.png)
3. Wählen Sie eine bestimmte Remotesitzung, anschließend die Auslassungspunkte auf der rechten Seite der Sitzungszeile und zuletzt **Löschen** aus.

   ![Screenshot des Azure-Portals mit angezeigter Sitzung und ausgewähltem Symbol „Löschen“.](./media/session-monitoring/delete.png)
4. Wenn Sie „Löschen“ auswählen, wird die Remotesitzung getrennt, und der Benutzer erhält in der Remotesitzung eine Meldung, dass seine Verbindung getrennt wurde.

   ![Screenshot mit einer Meldung, die bestätigt, dass Ihre Verbindung getrennt wurde.](./media/session-monitoring/disconnect.png)

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die [häufig gestellten Fragen zu Bastion](bastion-faq.md).
