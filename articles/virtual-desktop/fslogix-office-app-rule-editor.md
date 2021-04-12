---
title: Installieren von FSLogix-Anwendungscontainern für Microsoft Office in Windows Virtual Desktop – Azure
description: Verwenden des Regel-Editors für Apps zum Erstellen eines FSLogix-Anwendungscontainers mit Office in Windows Virtual Desktop.
author: Heidilohr
ms.topic: conceptual
ms.date: 02/23/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: a144adcfbf6c7cefc6b946f95bdb734868de801f
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2021
ms.locfileid: "106446807"
---
# <a name="install-microsoft-office-using-fslogix-application-containers"></a>Installieren von Microsoft Office mithilfe von FSLogix-Anwendungscontainern

Sie können Microsoft Office schnell und effizient installieren, indem Sie einen FSLogix-Anwendungscontainer als Vorlage für die anderen virtuellen Computer (VMs) in Ihrem Hostpool verwenden.

Die Verwendung eines FSLogix-App-Containers kann aus folgenden Gründen die Installation beschleunigen:

- Das Auslagern der Office-Apps in einen App-Container reduziert die Anforderungen an die Größe des Laufwerks „C“.
- Momentaufnahmen oder VM-Sicherungen weniger Ressourcen.
- Mit einer automatisierten Pipeline durch das Aktualisieren eines einzelnen Images wird das Aktualisieren der VMs vereinfacht.
- Sie benötigen nur ein Image, um Office (und andere Anwendungen) auf allen VMs in der Bereitstellung von Windows Virtual Desktop zu installieren.

In diesem Artikel erfahren Sie, wie Sie einen FSLogix-Anwendungscontainer für Office einrichten.

## <a name="requirements"></a>Anforderungen

Zum Einrichten des Regel-Editors benötigen Sie Folgendes:

- Eine VM, auf der Office nicht installiert ist
- Eine Kopie von Office
- Eine in Ihrer Bereitstellung installierte Kopie von FSLogix
- Eine Netzwerkfreigabe, auf die alle VMs in Ihrem Hostpool schreibgeschützten Zugriff haben

## <a name="install-office"></a>Installieren von Office

Um Office auf dem VHD- oder VHDX-Image zu installieren, aktivieren Sie das Remotedesktopprotokoll auf Ihrer VM, und befolgen Sie dann die Anweisungen unter [Installieren von Office für ein VHD-Masterimage](install-office-on-wvd-master-image.md). Stellen Sie bei der Installation sicher, dass Sie [die richtigen Lizenzen](overview.md#requirements) verwenden.

>[!NOTE]
>Für Windows Virtual Desktop ist die Aktivierung gemeinsam genutzter Computer (Shared Computer Activation, SCA) erforderlich.

## <a name="install-fslogix"></a>Installieren von FSLogix

Befolgen Sie die Anweisungen unter [Download and Install FSLogix](/fslogix/install-ht) (Herunterladen und Installieren von FSLogix, in englischer Sprache), um FSLogix und den Regel-Editor zu installieren.

## <a name="create-and-prepare-a-vhd-to-store-office"></a>Erstellen und Vorbereiten eines VHD-Images zum Speichern von Office

Als Nächstes müssen Sie ein VHD-Image erstellen und vorbereiten, in dem Sie den Regel-Editor verwenden:

1. Öffnen Sie eine Eingabeaufforderung als Administrator. Führen Sie den folgenden Befehl aus:

    ```cmd
        taskkill /F /IM MicrosoftEdge.exe /T
    ```

    >[!NOTE]
    > Stellen Sie sicher, dass die Leerzeichen in diesem Befehl beibehalten werden.

2. Führen Sie dann den folgenden Befehl aus:

    ```cmd
    sc queryex type=service state=all | find /i "ClickToRunSvc"
    ```
    
   Wenn Sie den Dienst gefunden haben, starten Sie die VM neu, bevor Sie mit Schritt 3 fortfahren.

    ```cmd
    net stop ClickToRunSvc
    ```

3. Wechseln Sie anschließend zu **Programme** > **FSLogix** > **Apps**, und führen Sie den folgenden Befehl aus, um das VHD-Zielimage zu erstellen:

    ```cmd
    frx moveto-vhd -filename <path to network share>\office.vhdx -src "C:\Program Files\Microsoft Office" -size-mbs 5000 
    ```

    Das VHD-Image, das Sie mit diesem Befehl erstellen, muss den Ordner „C:\\Program Files\\Microsoft Office“ enthalten.

    >[!NOTE]
    >Wenn Fehler angezeigt werden, deinstallieren Sie Office, und beginnen Sie wieder mit Schritt 1.

## <a name="configure-the-rule-editor"></a>Konfigurieren des Regel-Editors

Nachdem Sie das Image vorbereitet haben, müssen Sie den Regel-Editor konfigurieren und eine Datei erstellen, in der die Regeln gespeichert werden.

1. Wechseln Sie zu **Programme** > **FSLogix** > **Apps**, und führen Sie **RuleEditor.exe** aus.

2. Wählen Sie **Datei** > **Neu** > **Erstellen** aus, um einen neuen Regelsatz zu erstellen. Speichern Sie dann den Regelsatz in einem lokalen Ordner.

3. Wählen Sie **Blank Rule Set** (Leerer Regelsatz) aus, und klicken Sie dann auf **OK**.

4. Klicken Sie auf die Schaltfläche **+** (Anwenden). Dadurch wird das Fenster **Regel hinzufügen** geöffnet. Das Dialogfeld **Regel hinzufügen** enthält jetzt andere Optionen.

5. Wählen Sie im Dropdownmenü die Option **App Container (VHD) Rule** (Regel für App-Container (VHD)) aus.

6. Geben Sie **C:\\Program Files\\Microsoft Office** im Feld **Ordner** ein.

7. Wählen Sie für das Feld **Disk file** (Datenträgerdatei) im Abschnitt **Create target VHD** (Ziel-VHD erstellen) die Option **\<path\>\\office.vhd** aus.

8. Klicken Sie auf **OK**.

9. Wechseln Sie zum Arbeitsordner unter **C:\\Benutzer\\\<username\>\\Dokumente\\FSLogix Rule Sets**, und suchen Sie nach den FRX- und FXA-Dateien. Damit die Regeln angewendet werden können, müssen Sie diese Dateien in den Ordner „Rules“ verschieben (**C:\\Program Files\\FSLogix\\Apps\\Rules**).

10. Wählen Sie **Apply Rules to System** (Regeln auf System anwenden) aus, damit die Regeln wirksam werden.

     >[!NOTE]
     > Sie müssen die App-Regeldateien auf alle Sitzungshosts anwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu FSLogix finden Sie in unserer [FSLogix-Dokumentation](/fslogix/).