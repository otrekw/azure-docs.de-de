---
title: 'Referenz: Bekannte Probleme und Problembehandlung'
titleSuffix: Azure Data Science Virtual  Machine
description: Hier finden Sie eine Liste mit bekannten Problemen, Problemumgehungen und Problembehandlungsinformationen für Azure Data Science Virtual Machine.
services: machine-learning
ms.service: data-science-vm
author: timoklimmer
ms.author: tklimmer
ms.topic: reference
ms.date: 05/27/2021
ms.openlocfilehash: 8e3393f236152e591b054ee2d7c4c2753c2ff670
ms.sourcegitcommit: c385af80989f6555ef3dadc17117a78764f83963
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2021
ms.locfileid: "111408747"
---
# <a name="known-issues-and-troubleshooting-the-azure-data-science-virtual-machine"></a>Bekannte Probleme und Problembehandlung für Azure Data Science Virtual Machine

Dieser Artikel hilft Ihnen, Fehler oder Ausfälle zu ermitteln und zu beheben, auf die Sie bei der Verwendung der Azure Data Science Virtual Machine stoßen könnten.

## <a name="prompted-for-password-when-running-sudo-command-ubuntu"></a>Aufforderung zur Eingabe eines Kennworts beim Ausführen eines sudo-Befehls (Ubuntu)

Wenn Sie einen `sudo`-Befehl auf einem Ubuntu-Computer ausführen, werden Sie möglicherweise aufgefordert, Ihr Kennwort erneut einzugeben, um zu bestätigen, dass Sie tatsächlich der angemeldete Benutzer sind. Dies ist das erwartete Standardverhalten auf Linux-Systemen wie Ubuntu.
In einigen Szenarien ist eine wiederholte Authentifizierung jedoch nicht erforderlich, sondern eher störend.

Um die erneute Authentifizierung in den meisten Situationen zu deaktivieren, können Sie den folgenden Befehl in einem Terminal ausführen.

`echo -e "\n$USER ALL=(ALL) NOPASSWD: ALL\n" | sudo tee -a /etc/sudoers`

Nach dem Neustart des Terminals wird für sudo keine weitere Anmeldung angefordert und stattdessen die Authentifizierung bei Ihrer Sitzungsanmeldung als ausreichend betrachtet.

## <a name="accessing-sql-server-windows"></a>Zugreifen auf SQL Server (Windows)

Wenn Sie versuchen, eine Verbindung mit der vorinstallierten SQL Server-Instanz herzustellen, tritt möglicherweise der Fehler „Fehler bei der Anmeldung“ auf. Um erfolgreich eine Verbindung mit der SQL Server-Instanz herzustellen, müssen Sie das Programm, mit dem Sie eine Verbindung herstellen, z. B. SQL Server Management Studio (SSMS), im Administratormodus ausführen. Der Administratormodus ist erforderlich, da gemäß DSVM-Standard nur Administratoren eine Verbindung herstellen dürfen.

## <a name="python-package-installation-issues"></a>Probleme bei der Installation von Python-Paketen

### <a name="installing-packages-with-pip-breaks-dependencies-on-linux"></a>Beschädigung von Abhängigkeiten unter Linux beim Installieren von Paketen mit pip

Verwenden Sie beim Installieren von Paketen `sudo pip install` anstelle von `pip install`.

## <a name="disk-encryption-issues"></a>Probleme mit der Datenträgerverschlüsselung

### <a name="disk-encryption-fails-on-the-ubuntu-dsvm"></a>Nicht erfolgreiche Datenträgerverschlüsselung in der Ubuntu-DSVM-Instanz

Azure Disk Encryption (ADE) wird aktuell für Ubuntu-DSVM-Instanzen nicht unterstützt. Als Problemumgehung sollten Sie die Konfiguration der [serverseitigen Verschlüsselung von verwalteten Azure-Datenträgern](../../virtual-machines/disk-encryption.md) in Erwägung ziehen.

## <a name="tool-appears-disabled"></a>Tool anscheinend deaktiviert

### <a name="hyper-v-does-not-work-on-the-windows-dsvm"></a>Hyper-V funktioniert in der Windows-DSVM-Instanz nicht

Dass Hyper-V unter Windows zunächst nicht funktioniert, ist ein erwartetes Verhalten. Für die Startleistung haben wir einige Dienste deaktiviert. So aktivieren Sie Hyper-V

1. Öffnen Sie die Suchleiste auf Ihrer Windows-DSVM.
1. Geben Sie „Dienste“ ein.
1. Legen Sie alle Hyper-V-Dienste auf „Manuell“ fest.
1. Legen Sie „Verwaltung virtueller Hyper-V-Computer“ auf „Automatisch“ fest.

Ihr Bildschirm sollte dann wie folgt aussehen:

   ![Aktivieren von Hyper-V](./media/workaround/hyperv-enable-dsvm.png)