---
title: Direktes Upgrade von Red Hat Enterprise Linux-Images in Azure
description: Hier finden Sie Schritte zur Durchführung eines direkten Upgrades von Red Hat Enterprise 7.x-Images auf die neueste Version 8.x.
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: beede74134affeb3ee0d4bdd20d5da3b4c5e6eda
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566621"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Direkte Upgrades für Red Hat Enterprise Linux

Dieser Artikel enthält schrittweise Anweisungen zur Durchführung eines direkten Upgrades von Red Hat Enterprise Linux 7 auf Red Hat Enterprise Linux 8 mithilfe des Hilfsprogramms „Leapp“ in Azure. Während des direkten Upgrades wird das bestehende Betriebssystem RHEL 7 durch die Version RHEL 8 ersetzt.

>[!Note] 
> Angebote von SQL Server auf Red Hat Enterprise Linux unterstützen kein direktes Upgrade auf Azure.

## <a name="what-to-expect-during-the-upgrade"></a>Erwartungen während des Upgrades
Das System wird während des Upgrades einige Male neu gestartet, und das ist normal. Beim letzten Neustart wird ein Upgrade der VM auf die neueste Nebenversion von RHEL 8 durchgeführt. 

Der Upgradevorgang kann zwischen 20 Minuten und einigen Stunden dauern. Dies hängt von mehreren Faktoren ab, z. B. von der VM-Größe und der Anzahl der auf dem System installierten Pakete.

## <a name="preparations-for-the-upgrade"></a>Vorbereitungen für das Upgrade
Direkte Upgrades sind die offiziell empfohlene Methode von Red Hat und Azure, um Kunden ein Upgrade ihres Systems auf die nächste Hauptversion zu ermöglichen. Bevor Sie das Upgrade durchführen, sollten Sie einige Aspekte beachten und in Betracht ziehen. 

>[!Important] 
> Erstellen Sie eine Momentaufnahme des Images, bevor Sie das Upgrade durchführen.

>[!NOTE]
> Die Befehle in diesem Artikel müssen unter Verwendung des Root-Kontos ausgeführt werden.

* Stellen Sie sicher, dass Sie die neueste RHEL 7-Version verwenden, die derzeit RHEL 7.9 ist. Wenn Sie eine gesperrte Version verwenden und kein Upgrade auf RHEL 7.9 durchführen können, können Sie die [Schritte hier verwenden, um zu einem Nicht-EUS-Repository zu wechseln](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock).

* Führen Sie den nachstehenden Befehl aus, um den Status Ihres Upgrades zu erfahren und ob es abgeschlossen wird. Der Befehl sollte unter „/var/log/leapp/leapp/leapp-report.txt“ eine Datei erstellen, in der der Prozess und die ausgeführten Schritte sowie die Information erläutert werden, ob das Upgrade möglich ist.
    ```bash
    leapp preupgrade --no-rhsm
    ```
* Vergewissern Sie sich, dass die serielle Konsole funktioniert, da dies eine Überwachung während des Upgradeprozesses ermöglicht.

* Aktivieren des SSH-Root-Zugriffs in `/etc/ssh/sshd_config`
    1. Öffnen Sie die Datei `/etc/ssh/sshd_config`.
    1. Suchen Sie nach „#PermitRootLogin yes“.
    1. Entfernen Sie das „#“ zum Aufheben der Auskommentierung.

## <a name="steps-for-performing-the-upgrade"></a>Schritte zum Durchführen des Upgrades

Führen Sie diese Schritte sorgfältig aus. Es wird auf jeden Fall empfohlen, das Upgrade auf einem Testcomputer zu testen, bevor es auf Instanzen der Produktionsumgebung übernommen wird.

1. Führen Sie ein yum-Update durch, um die neuesten Clientpakete abzurufen.
    ```bash
    yum update -y
    ```

1. Installieren Sie das leapp-Clientpaket.
    ```bash
    yum install leapp-rhui-azure
    ```
    
1. Verwenden Sie die Datei „leapp-data.tar.gz“ mit „repomap.csv“ und „pes-events.json“, die im [RedHat-Portal](https://access.redhat.com/articles/3664871) verfügbar sind, und extrahieren Sie diese. 
    1. Laden Sie die Datei herunter.
    1. Extrahieren Sie den Inhalt, und entfernen Sie die Datei mithilfe des folgenden Befehls:
    ```bash
    tar -xzf leapp-data12.tar.gz -C /etc/leapp/files && rm leapp-data12.tar.gz
    ```

1. Fügen Sie die Datei „answers“ für Leapp hinzu.
    ```bash
    leapp answer --section remove_pam_pkcs11_module_check.confirm=True --add
    ``` 

1. Führen Sie das Upgrade für Leapp durch.
    ```bash
    leapp upgrade --no-rhsm
    ```
1.  Nachdem der `leapp upgrade`-Befehl erfolgreich ausgeführt wurde, starten Sie das System manuell neu, um den Vorgang abzuschließen. Das System wird mehrmals neu gestartet und steht währenddessen nicht zur Verfügung. Überwachen Sie den Prozess mithilfe der seriellen Konsole.

1.  Vergewissern Sie sich, dass das Upgrade erfolgreich abgeschlossen wurde.
    ```bash
    uname -a && cat /etc/redhat-release
    ```

1. Entfernen Sie den SSH-Root-Zugriff, nachdem das Upgrade ausgeführt wurde.
    1. Öffnen Sie die Datei `/etc/ssh/sshd_config`.
    1. Suchen Sie nach „#PermitRootLogin yes“.
    1. Fügen Sie das „#“ zum Kommentar hinzu.

1. Starten Sie den sshd-Dienst neu, damit die Änderungen wirksam werden.
    ```bash
    systemctl restart sshd
    ```

## <a name="common-issues"></a>Häufige Probleme
Im Folgenden finden Sie einige der häufigen Fälle, in denen beim Prozess `leapp preupgrade` oder `leapp upgrade` ein Fehler auftritt.

**Error: Keine Übereinstimmungen für die folgenden Muster für deaktivierte Plug-Ins gefunden**
```plaintext
STDERR:
No matches found for the following disabled plugin patterns: subscription-manager
Warning: Packages marked by Leapp for upgrade not found in repositories metadata: gpg-pubkey
```
**Lösung**\
Deaktivieren Sie das Abonnement-Manager-Plug-In, indem Sie die Datei `/etc/yum/pluginconf.d/subscription-manager.conf` bearbeiten und „enabled“ in `enabled=0` ändern.

Dieser Fehler tritt auf, weil das Abonnement-Manager-yum-Plug-In aktiviert ist, das für PAYG-VMs nicht verwendet wird.

**Error: Mögliche Probleme bei der Remoteanmeldung mithilfe von root** Bei `leapp preupgrade` kann der folgende Fehler auftreten:
```structured-text
============================================================
                     UPGRADE INHIBITED
============================================================

Upgrade has been inhibited due to the following problems:
    1. Inhibitor: Possible problems with remote login using root account
Consult the pre-upgrade report for details and possible remediation.

============================================================
                     UPGRADE INHIBITED
============================================================
```
**Lösung**\
Aktivieren Sie den Root-Zugriff in `/etc/sshd_conf`.
Dieser Fehler wird dadurch verursacht, dass der Root-SSH-Zugriff in `/etc/sshd_conf` nicht gemäß der Beschreibung unter [Vorbereitungen für das Upgrade](#preparations-for-the-upgrade) aktiviert wurde. 

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Red Hat-Images in Azure](./redhat-images.md).
* Erfahren Sie mehr über die [Red Hat-Updateinfrastruktur](./redhat-rhui.md).
* Erfahren Sie mehr über das [RHEL-BYOS-Angebot](./byos.md).
* Informationen über die Prozesse beim direkten Red Hat-Upgrade finden Sie in der Red Hat-Dokumentation unter [Upgrade von RHEL 7 auf RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index).
* Informationen zu Red Hat-Supportrichtlinien für alle RHEL-Versionen finden Sie auf der Seite [Red Hat Enterprise Linux Life Cycle (Red Hat Enterprise Linux-Lebenszyklus)](https://access.redhat.com/support/policy/updates/errata).