---
title: Aktualisieren des Microsoft Azure Recovery Services-Agents (MARS)
description: Erfahren Sie, wie Sie den Microsoft Azure Recovery Services-Agent (MARS) aktualisieren.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: a1ee26db962781643e9599069282647658301bac
ms.sourcegitcommit: 3fb5e772f8f4068cc6d91d9cde253065a7f265d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89181475"
---
# <a name="upgrade-the-microsoft-azure-recovery-services-mars-agent"></a>Aktualisieren des Microsoft Azure Recovery Services-Agents (MARS)

In diesem Artikel lernen Sie Folgendes:

* Identifizieren von Servern mit früheren Versionen des MARS-Agents
* Aktualisieren von MARS-Installationen auf diesen Servern

## <a name="identify-servers-with-earlier-versions-of-the-mars-agent"></a>Identifizieren von Servern mit früheren Versionen des MARS-Agents

Für Installationen von Azure Backup-Agent und Azure Backup-Server:

1. Navigieren Sie zum Recovery Services-Tresor, bei dem Sie Server registriert haben, die möglicherweise von älteren Versionen des Agents gesichert werden. Eine repräsentative Liste von Tresoren mit älteren Azure Backup-Agents finden Sie in den Azure Backup-Updatebenachrichtigungen von Azure.
1. Wählen Sie auf der linken Seite im Abschnitt **Einstellungen** des Recovery Services-Tresor unter dem Abschnitt **Verwalten** die Option **Sicherungsinfrastruktur**.
1. Um Azure Backup-Agents zu ermitteln, die als Teil von Azure Backup-Serverinstallationen installiert wurden, navigieren Sie zu **Sicherungsverwaltungsserver** unter **Verwaltungsserver**. Dadurch werden die Server aufgelistet, auf denen Azure Backup-Server installiert sind, zusammen mit der Versionsnummer für den zugehörigen Azure Backup-Agent.

    ![Liste der MARS-Agents, die als Teil von Azure Backup-Serverinstallationen installiert wurden](./media/upgrade-mars-agent/backup-management-servers.png)

1. Um Agent-Versionen für Installationen des Microsoft Azure Recovery Services (MARS)-Agents oder des Azure Backup-Agents zu überprüfen, navigieren Sie zu **Geschützte Server** unter **Verwaltungsserver**. Wählen Sie dann **Azure Backup-Agent** unter „Sicherungsverwaltungstyp“. Dadurch werden die Server aufgelistet, auf denen Azure Backup-Agents installiert sind, zusammen mit der Versionsnummer der Installation.

    ![Liste der Server, auf denen der MARS-Agent installiert ist](./media/upgrade-mars-agent/protected-servers.png)

1. Sortieren Sie die Spalte mit der Version des Azure Backup-Agents, indem Sie für MARS Agent-Installationen die Spalte **Agent-Version** oder für Azure Backup-Serverinstallationen die Spalte **Azure Backup-Agent-Version** auswählen.

1. Im vorherigen Schritt erhalten Sie eine Liste der Server mit Azure Backup-Agent-Versionen kleiner als 2.0.9083.0, oder deren Feld für die Agent-Version leer ist. Dies sind die Server, auf denen Azure Backup-Agents aktualisiert werden müssen.

## <a name="update-the-mars-agent-installation-on-the-server"></a>Aktualisieren der MARS-Agent-Installation auf dem Server

Sobald Sie die Server identifiziert haben, der Azure Backup-Agents aktualisiert werden müssen, führen Sie für jeden identifizierten Server die folgenden Schritte durch (unter Verwendung des Azure Backup-Server- oder MARS-Agents). [Laden Sie die neueste Version des Azure Backup-Agents herunter](https://aka.ms/azurebackup_agent), bevor Sie die folgenden Schritte ausführen.

1. Wählen Sie eine Zeile aus, die einen Azure Backup-Agent mit einer Version niedriger als 2.0.9083.0 enthält oder leer ist. Dadurch wird der Bildschirm mit den Serverdetails geöffnet.

    ![Geschützte Server mit veralteten Agent-Versionen](./media/upgrade-mars-agent/old-agent-version.png)

    ![Azure Backup-Server mit veralteten Agent-Versionen](./media/upgrade-mars-agent/backup-management-servers-old-versions.png)

1. Wählen Sie **Connect** aus, um eine Remotedesktop-Verbindungsdatei für die Verbindung mit dem Server zu erhalten, oder über die Remotedesktopverbindung auf Ihrem Server eine direkte Verbindung mit dem Server herzustellen.

    ![Verbindung zum Server über eine Remotedesktopverbindung](./media/upgrade-mars-agent/connect-to-server.png)

    >[!NOTE]
    > Wenn der aufgelistete Server nicht existiert oder außer Betrieb genommen wurde, können Sie die restlichen Schritte ignorieren und mit dem nächsten Server fortfahren.

1. Geben Sie Ihre administrativen Anmeldeinformationen ein, und melden Sie sich an.

1. Wenn Ihr Server oder der Proxy Ihres Servers über einen eingeschränkten Internetzugang verfügt, stellen Sie sicher, dass die Firewalleinstellungen auf dem Server/Proxy so konfiguriert sind, dass die für die von Ihnen verwendete Azure Cloud geeignete URL zugelassen wird:

    Azure Cloud | URL
    --- | ---
    Azure Cloud (Public) |   `https://login.windows.net`
    Azure China 21Vianet Cloud   | `https://login.chinacloudapi.cn`
    Azure US Government Cloud |   `https://login.microsoftonline.us`
    Azure German Cloud  |  `https://login.microsoftonline.de`

1. Kopieren Sie das Updateinstallationsprogramm des Azure Backup-Agents auf den Server.

    ![Kopieren des Updateinstallationsprogramms des Azure Backup-Agents auf den Server](./media/upgrade-mars-agent/copy-agent-installer.png)

1. Führen Sie das Installationsprogramm aus. Der Upgrade-Assistent für den Microsoft Azure Recovery Services-Agent wird angezeigt.

    ![Der Upgrade-Assistent für den Microsoft Azure Recovery Services-Agent](./media/upgrade-mars-agent/agent-upgrade-wizard.png)

1. Wählen Sie **Weiter** aus.

1. Wählen Sie **Upgrade** aus.

    ![Installation des Microsoft Azure Recovery Services-Agents](./media/upgrade-mars-agent/upgrade-installation.png)

1. Der letzte Bestätigungsbildschirm zeigt an, dass der Azure Backup-Agent erfolgreich aktualisiert wurde.

## <a name="for-system-center-data-protection-manager-sc-dpm-customers"></a>Für Kunden von System Center Data Protection Manager (SC DPM)

Wenn Sie Azure Backup-Agents auf den System Center Data Protection Manager (SC DPM)-Servern installiert haben, müssen Sie die folgenden Schritte ausführen, um zu ermitteln, ob ein Update des Azure Backup-Agents auf Ihren DPM-Servern erforderlich ist:

1. Melden Sie sich als Administrator bei Ihrem SC DPM-Server an.
2. Öffnen Sie die DPM-Konsole.
3. Wählen Sie in der Navigation unten links in der Konsole **Verwaltung** aus.
4. Suchen Sie in den Informationen, die in der linken Navigation angezeigt werden, nach den Versionsinformationen des Azure Backup-Agents.
5. Wenn die Version niedriger als 2.0.9083.0 ist, laden Sie das neueste Installationsprogramm für den Azure Backup-Agent herunter, und führen Sie das Installationsprogramm auf dem DPM-Server aus, um den Azure Backup-Agent zu aktualisieren.

Wiederholen Sie die oben stehenden Schritte für alle DPM-Server in Ihrer Umgebung.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum [Sichern von Windows-Computern mit dem Azure Backup-MARS-Agent](backup-windows-with-mars-agent.md)
