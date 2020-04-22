---
title: Leitfaden zum Steuern des Verhaltens beim Herunterfahren von Windows in Azure Lab Services | Microsoft-Dokumentation
description: Schritte zum automatischen Herunterfahren eines virtuellen Windows-Computers, der sich im Leerlauf befindet, und zum Entfernen des Windows-Befehls zum Herunterfahren.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.topic: article
ms.date: 3/30/2020
ms.author: spelluru
ms.openlocfilehash: 7b839df5940ab26e5c1a99a1bda1fbd2545f8cc4
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113120"
---
# <a name="guide-to-controlling-windows-shutdown-behavior"></a>Leitfaden zum Steuern des Verhaltens beim Herunterfahren von Windows

Azure Lab Services bietet mehrere Kostenkontrollen, um zu verhindern, dass virtuelle Windows-Computer (VMs) unerwartet ausgeführt werden:
 - [Zeitplan festlegen](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
 - [Festlegen von Kontingenten für Benutzer](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users)
 - [Aktivieren von automatischem Herunterfahren beim Trennen der Verbindung](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

Selbst bei Verwendung dieser Kostenkontrollen können Situationen eintreten, in denen eine Windows-VM unerwartet weiter ausgeführt und folglich vom Kontingent des Studenten abgezogen wird:

- **Das RDP-Fenster bleibt geöffnet.**
  
    Wenn ein Kursteilnehmer per RDP eine Verbindung mit dem virtuellen Computer herstellt, lässt er möglicherweise das RDP-Fenster versehentlich geöffnet.  Solange das RDP-Fenster geöffnet bleibt, wird die Einstellung zum **automatischen Herunterfahren beim Trennen der Verbindung** nicht angewendet, da sie erst ausgelöst wird, nachdem die RDP-Sitzung getrennt wurde.

- **Die VM wird mit dem Windows-Befehl zum Herunterfahren ausgeschaltet.**
  
    Ein Kursteilnehmer kann zum Ausschalten der VM den Windows-Befehl zum Herunterfahren oder andere in Windows bereitgestellte Mechanismen zum Herunterfahren statt der [Stopp-Schaltfläche in Azure Lab Services](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-classroom-lab#start-or-stop-the-vm) verwenden.  In diesem Fall ist aus Sicht von Azure Lab Services die VM noch in Verwendung.
    
Um diese Situationen zu verhindern, werden im Leitfaden die Schritte zum automatischen Herunterfahren einer Windows-VM im Leerlauf und zum Entfernen des Windows-Befehls zum Herunterfahren aus dem **Startmenü** beschrieben.  

> [!NOTE]
> Eine VM kann auch unerwartet vom Kontingent abgezogen werden, wenn der Kursteilnehmer die VM startet, aber nie eine RDP-Verbindung mit ihr herstellt.  Dieses Szenario wird im Leitfaden derzeit *nicht* behandelt.  Stattdessen sollten die Kursteilnehmer aufgefordert werden, sofort nach dem Starten der VM eine RDP-Verbindung mit ihr herzustellen, oder die VM zu beenden.

## <a name="automatic-rdp-disconnect-and-shutdown-for-idle-vm"></a>Automatisches Trennen der RDP-Verbindung und Herunterfahren der VM im Leerlauf

Windows bietet Einstellungen für **Lokale Gruppenrichtlinien**, mit denen Sie ein Zeitlimit für das automatische Trennen einer RDP-Sitzung festlegen können, wenn sie in den Leerlauf wechselt.  Eine Sitzung befindet sich im Leerlauf, wenn *keine* Eingabe mit der Maus/Tastatur erfolgt.  Alle Aktivitäten mit langer Ausführungszeit ohne Maus-/Tastatureingabe bewirken den Leerlauf der VM.  Dies umfasst auch das Ausführen einer Abfrage mit langer Ausführungszeit, das Streamen von Videos, das Kompilieren u. ä.  Abhängig von den Anforderungen der Klasse können Sie ein Leerlaufzeitlimit festlegen, dessen Größe für die Behandlung dieser Typen von Aktivitäten ausreicht.  Sie können beispielsweise ggf. ein Leerlaufzeitlimit von 1 Stunde oder mehr festlegen.

Wenn Sie das **Leerlaufsitzungslimit** zusammen mit der Einstellung für [**automatisches Herunterfahren beim Trennen der Verbindung**](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect) konfigurieren, bewirkt dies für den Kursteilnehmer Folgendes:
 1. Der Kursteilnehmer stellt mithilfe von RDP eine Verbindung mit der Windows-VM her.
 2. Wenn der Kursteilnehmer das RDP-Fenster geöffnet lässt und sich die VM für die Dauer des von Ihnen festgelegten **Leerlaufsitzungslimits** (z. B. 5 Minuten) im Leerlauf befindet, wird das folgende Dialogfeld angezeigt:

    ![Dialogfeld für abgelaufenes Leerlaufzeitlimit](../media/how-to-windows-shutdown/idle-time-expired.png)

1. Wenn der Kursteilnehmer *nicht* auf **OK** klickt, wird die Verbindung mit RDP-Sitzung nach zwei Minuten automatisch getrennt.
2. Nachdem die Verbindung mit der RDP-Sitzung getrennt wurde, wird der virtuelle Computer automatisch durch Azure Lab Services heruntergefahren, sobald die angegebene Zeitspanne der Einstellung für **automatisches Herunterfahren beim Trennen der Verbindung** erreicht wurde.

### <a name="set-rdp-idle-session-time-limit-on-the-template-vm"></a>Festlegen des Leerlaufzeitlimits für die RDP-Sitzung in der Vorlagen-VM

Um das Leerlaufzeitlimit für RDP-Sitzungen festzulegen, können Sie eine Verbindung mit der Vorlagen-VM herstellen und das folgende PowerShell-Skript ausführen.

```powershell
# The MaxIdleTime is in milliseconds; by default, this script sets MaxIdleTime to 15 minutes.
$maxIdleTime = 15 * 60 * 1000

Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -Name "MaxIdleTime" -Value $maxIdleTime -Force
```
Oder Sie können mithilfe der Vorlagen-VM die folgenden manuellen Schritte ausführen:

1. Drücken Sie die Windows-Taste, geben Sie **gpedit** ein, und wählen Sie dann **Gruppenrichtlinie bearbeiten (Systemsteuerung)** aus.

1. Wechseln Sie zu **Computerkonfiguration > Administrative Vorlagen > Windows-Komponenten > Remotedesktopdienste > Remotedesktop-Sitzungshost > Sitzungszeitlimits**.  

    ![Editor für lokale Gruppenrichtlinien](../media/how-to-windows-shutdown/group-policy-idle.png)
   
1. Klicken Sie mit der rechten Maustaste auf **Zeitlimit für aktive, aber im Leerlauf befindliche Remotedesktopdienste-Sitzungen festlegen**, und klicken Sie auf **Bearbeiten**.

1. Legen Sie die folgenden Einstellungen fest, und klicken Sie dann auf **OK**:
   1. Wählen Sie **Aktiviert**.
   1. Geben Sie unter **Optionen** das **Leerlaufsitzungslimit** an.

    ![Leerlaufsitzungslimit](../media/how-to-windows-shutdown/edit-idle-time-limit.png)

1. Um schließlich dieses Verhalten mit der Einstellung für **automatisches Herunterfahren beim Trennen der Verbindung** zu kombinieren, führen Sie die Schritte in der folgenden Anleitung aus: [Aktivieren des automatischen Herunterfahrens von VMs beim Trennen](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect).

> [!WARNING]
> Nachdem Sie diese Einstellung entweder mit PowerShell zur direkten Änderung der Registrierungseinstellung oder manuell mit dem Gruppenrichtlinien-Editor konfiguriert haben, müssen Sie die VM zunächst neu starten, damit die Einstellungen wirksam werden.  Wenn Sie die Einstellung mithilfe der Registrierung konfigurieren, wird der Gruppenrichtlinien-Editor nicht immer aktualisiert, um Änderungen an der Registrierungseinstellung widerzuspiegeln. Die Registrierungseinstellung wird jedoch weiterhin wie erwartet wirksam, und die RDP-Sitzung wird nach einem Leerlauf für die von Ihnen angegebene Zeitspanne getrennt.

## <a name="remove-windows-shutdown-command-from-start-menu"></a>Entfernen des Windows-Befehls zum Herunterfahren aus dem Startmenü

Sie können den Befehl zum Herunterfahren auch mit den Windows-Einstellungen für **Lokale Gruppenrichtlinien** aus dem **Startmenü** entfernen.

Um den Befehl zum Herunterfahren zu entfernen, können Sie eine Verbindung mit der Vorlagen-VM herstellen und das folgende PowerShell-Skript ausführen.

```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" -Name "HidePowerOptions" -Value 1 -Force
```

Oder Sie können mithilfe der Vorlagen-VM die folgenden manuellen Schritte ausführen:

1. Drücken Sie die Windows-Taste, geben Sie **gpedit** ein, und wählen Sie dann **Gruppenrichtlinie bearbeiten (Systemsteuerung)** aus.

1. Wechseln Sie zu **Computerkonfiguration > Administrative Vorlagen > Startmenü und Taskleiste**.  

    ![Editor für lokale Gruppenrichtlinien](../media/how-to-windows-shutdown/group-policy-shutdown.png)

1. Klicken Sie mit der rechten Maustaste auf **Befehle „Herunterfahren“, „Neu starten“, „Energie sparen“ und „Ruhezustand“ entfernen und Zugriff darauf verweigern**, und klicken Sie auf **Bearbeiten**.

1. Aktivieren Sie die Einstellung **Aktiviert**, und klicken Sie dann auf **OK**:
 
   ![Einstellung für Herunterfahren](../media/how-to-windows-shutdown/edit-shutdown.png)

1. Beachten Sie, dass der Befehl zum Herunterfahren nicht mehr im **Startmenü** von Windows angezeigt wird. Es wird nur noch der Befehl **Trennen** angezeigt.

    ![Befehl „Herunterfahren“](../media/how-to-windows-shutdown/start-menu.png)

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie den Artikel zum Erstellen einer Windows-Vorlagen-VM: [Leitfaden zum Einrichten eines Windows-Vorlagencomputers in Azure Lab Services](how-to-prepare-windows-template.md)