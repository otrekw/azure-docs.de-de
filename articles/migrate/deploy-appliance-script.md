---
title: Einrichten einer Azure Migrate-Appliance mit einem Skript
description: Erfahren Sie, wie Sie eine Azure Migrate-Appliance mit einem Skript einrichten.
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: bf8d7148f685d4ac6a5f33603020a0503b0c62e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337368"
---
# <a name="set-up-an-appliance-with-a-script"></a>Einrichten einer Appliance mit einem Skript

In diesem Artikel wird beschrieben, wie Sie die [Azure Migrate-Appliance](deploy-appliance.md) mithilfe eines PowerShell-Installationsskripts einrichten.

Das Skript bietet Folgendes:
- Eine Alternative zum Einrichten der Appliance mithilfe einer OVA-Vorlage für die Bewertung und die Migration ohne Agents von VMware-VMS.
- Eine Alternative zum Einrichten der Appliance mithilfe einer VHD-Vorlage für die Bewertung und die Migration von Hyper-V-VMs.
- Das Skript ist die einzige Methode, die Appliance zur Bewertung physischer Server (oder VMs, die Sie als physische Server migrieren möchten) einzurichten.

## <a name="prerequisites"></a>Voraussetzungen

Das Skript richtet die Azure Migrate-Appliance auf einem vorhandenen physischen Computer oder virtuellen Computer ein.

- Auf dem Computer, der als Appliance fungieren soll, müssen Windows Server 2016 mit 32 GB Arbeitsspeicher, acht vCPUs, ca. 80 GB Festplattenspeicher und ein externer virtueller Switch ausgeführt werden. Er benötigt eine statische oder dynamische IP-Adresse und Internetzugriff.
- Überprüfen Sie vor der Bereitstellung der Appliance ihre detaillierten Anforderungen für [VMware-VMs](migrate-appliance.md#appliance---vmware), [Hyper-V-VMs](migrate-appliance.md#appliance---hyper-v) und [physische Server](migrate-appliance.md#appliance---physical).
- Führen Sie das Skript nicht auf einer vorhandenen Azure Migrate-Appliance aus.


## <a name="download-the-script"></a>Herunterladen des Skripts

1. Suchen Sie den Computer/die VM, der/die als Azure Migrate-Appliance fungieren soll.
2. Gehen Sie auf dem Computer wie folgt vor:

    - Um die Appliance mit VMware-VMs oder Hyper-V-VMs zu verwenden, [laden Sie den ZIP-Ordner herunter](https://go.microsoft.com/fwlink/?linkid=2105112), der das Installationsskript und die MSIs enthält.
    - Um die Appliance mit physischen Servern zu verwenden, laden Sie das Skript aus dem Azure Migrate-Portal herunter, wie in diesem [Tutorial](tutorial-assess-physical.md#set-up-the-appliance)beschrieben.

## <a name="verify-file-security"></a>Überprüfen der Dateisicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die gezippte Datei zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\Users\administrator\Desktop\AzureMigrateInstaller.zip SHA256```

3. Überprüfen Sie, ob die generierten Hashwerte diesen Einstellungen (für die aktuelle Applianceversion) entsprechen:

    **Algorithmus** | **Hashwert**
      --- | ---
      MD5 | 1e92ede3e87c03bd148e56a708cdd33f
      SHA256 | a3fa78edc8ff8aff9ab5ae66be1b64e66de7b9f475b6542beef114b20bfdac3c

## <a name="run-the-script"></a>Führen Sie das Skript aus.

Funktion des Skripts:

- Installation von Agents und einer Webanwendung.
- Installation von Windows-Rollen, darunter beispielsweise Windows-Aktivierungsdienst, IIS und PowerShell ISE.
- Download und Installation eines wiederbeschreibbaren IIS-Moduls. [Weitere Informationen](https://www.microsoft.com/download/details.aspx?id=7435)
- Aktualisierung eines Registrierungsschlüssels (HKLM) mit dauerhaften Einstellungen für Azure Migrate.
- Erstellen von Protokoll- und Konfigurationsdateien wie folgt:
    - **Konfigurationsdateien**: %ProgramData%\Microsoft Azure\Config
    - **Protokolldateien**: %ProgramData%\Microsoft Azure\Logs

So führen Sie das Skript aus

1. Extrahieren Sie die gezippte Datei in einem Ordner auf dem Computer, der die Appliance hostet.
2. Starten Sie PowerShell auf dem Computer mit Administratorberechtigungen (erhöhten Rechten).
3. Ändern Sie das PowerShell-Verzeichnis in den Ordner mit den Inhalten, die aus der gezippten Datei extrahiert wurden, die Sie heruntergeladen haben.
4. Führen Sie das Skript **AzureMigrateInstaller.ps1** wie folgt aus:

    - Für VMware: 
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario VMware
        ```
    - Für Hyper-V:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1 -scenario Hyperv
        ```
    - Für physische Server:
        ```
        PS C:\Users\administrator\Desktop\AzureMigrateInstaller> AzureMigrateInstaller.ps1
        ```      
5. Nachdem das Skript erfolgreich ausgeführt wurde, wird die Appliancewebanwendung gestartet, damit Sie das Gerät einrichten können. Bei Problemen können Sie unter „C:\ProgramData\Microsoft Azure\Logs\AzureMigrateScenarioInstaller_<em>Timestamp</em>.log“ auf die Skriptprotokolle zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Appliance mithilfe des Skripts eingerichtet haben, befolgen Sie diese Anweisungen:

- Richten Sie die Appliance für [VMware](how-to-set-up-appliance-vmware.md#configure-the-appliance) ein.
- Richten Sie die Appliance für [Hyper-V](how-to-set-up-appliance-hyper-v.md#configure-the-appliance) ein.
- Richten Sie die Appliance für [physische Server](how-to-set-up-appliance-physical.md) ein.