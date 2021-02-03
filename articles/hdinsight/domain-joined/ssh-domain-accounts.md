---
title: Verwalten des SSH-Zugriffs für Domänenkonten in Azure HDInsight
description: Schritte zum Verwalten des SSH-Zugriffs für Azure AD-Konten in HDInsight
ms.service: hdinsight
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 3fab06f5e269f311f798c096a6465c8c6ce75fc2
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/28/2021
ms.locfileid: "98946750"
---
# <a name="manage-ssh-access-for-domain-accounts-in-azure-hdinsight"></a>Verwalten des SSH-Zugriffs für Domänenkonten in Azure HDInsight

Standardmäßig können sich alle Domänenbenutzer in [Azure AD DS](../../active-directory-domain-services/overview.md) per [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) auf Haupt- und Edgeknoten anmelden. Diese Benutzer sind nicht Teil der sudo-Gruppe und erhalten keinen Root-Zugriff. Der bei der Clustererstellung erstellte SSH-Benutzer hat Root-Zugriff.

## <a name="manage-access"></a>Verwalten des Zugriffs

Um den SSH-Zugriff auf bestimmte Benutzer oder Gruppen zu ändern, aktualisieren Sie `/etc/ssh/sshd_config` auf den einzelnen Knoten.

1. Verwenden Sie einen [ssh-Befehl](../hdinsight-hadoop-linux-use-ssh-unix.md) zum Herstellen der Verbindung mit dem Cluster. Bearbeiten Sie den folgenden Befehl, indem Sie CLUSTERNAME durch den Namen Ihres Clusters ersetzen, und geben Sie den Befehl dann ein:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Öffnen Sie die Datei `ssh_confi`g.

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

1. Ändern Sie die Datei `sshd_config` nach Bedarf. Wenn Sie Benutzer auf bestimmte Gruppen beschränken, können die lokalen Konten keine SSH-Anmeldung auf diesen Knoten durchführen. Dies ist lediglich ein Beispiel für die Syntax:

    ```bash
    AllowUsers useralias1 useralias2

    AllowGroups groupname1 groupname2
    ```

    Speichern Sie dann die Änderungen: **STRG + X**, **Y**, **EINGABETASTE**.

1. Starten Sie sshd neu.

    ```bash
    sudo systemctl restart sshd
    ```

1. Wiederholen Sie die obigen Schritte für jeden Knoten.

## <a name="ssh-authentication-log"></a>SSH-Authentifizierungsprotokoll

Das SSH-Authentifizierungsprotokoll wird in `/var/log/auth.log` geschrieben. Wenn Ihnen Anmeldefehler über SSH für lokale Konten oder Domänenkonten auffallen, müssen Sie das Protokoll durchgehen, um die Fehler zu debuggen. Häufig hängt das Problem mit bestimmten Benutzerkonten zusammen, und es hat sich bewährt, andere Benutzerkonten oder SSH über den SSH-Standardbenutzer (lokales Konto) auszuprobieren und dann kinit auszuführen.

## <a name="ssh-debug-log"></a>SSH-Debugprotokoll

Um die ausführliche Protokollierung zu aktivieren, müssen Sie `sshd` mit der Option `-d` neu starten. Beispiel: `/usr/sbin/sshd -d`. Sie können `sshd` auch an einem benutzerdefinierten Port (z. B. 2222) ausführen, damit Sie den SSH-Hauptdaemon nicht abbrechen müssen. Außerdem können Sie die Option `-v` mit dem SSH-Client verwenden, um weitere Protokolle (clientseitige Ansicht der Fehler) zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von HDInsight-Clustern mit dem Enterprise-Sicherheitspaket](./apache-domain-joined-manage.md)
* [Herstellen einer Verbindung mit HDInsight (Apache Hadoop) per SSH](../hdinsight-hadoop-linux-use-ssh-unix.md)
