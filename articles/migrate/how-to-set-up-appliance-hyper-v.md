---
title: Einrichten einer Azure Migrate-Appliance für Hyper-V
description: Erfahren Sie, wie Sie eine Azure Migrate-Appliance einrichten, um virtuelle Hyper-V-Computer zu bewerten und zu migrieren.
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: 56b034709309a3afe9d18df7af9ababc74a24cee
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109704"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>Einrichten einer Appliance für Hyper-V-VMs

In diesem Artikel finden Sie Informationen zum Einrichten der Azure Migrate-Appliance für die Bewertung von Hyper-V-VMs mit dem Tool [Azure Migrate-Serverbewertung](migrate-services-overview.md#azure-migrate-server-assessment-tool).

Die [Azure Migrate-Appliance](migrate-appliance.md) ist eine einfache Appliance, die von der Azure Migrate-Serverbewertung/-Migration verwendet wird, um lokale Hyper-V-VMs zu entdecken und VM-Metadaten/-Leistungsdaten an Azure zu senden.

Sie können die Appliance mithilfe verschiedener Methoden bereitstellen:

- Einrichten der Appliance auf einem virtuellen Hyper-V-Computers mithilfe einer heruntergeladenen VHD. Diese Methode wird in diesem Artikel beschrieben.
- Einrichten der Appliance auf einem virtuellen Hyper-V-Computer oder physischen Computer mithilfe eines PowerShell-Installationsskripts. [Diese Methode](deploy-appliance-script.md) sollte verwendet werden, wenn Sie keinen virtuellen Computer mithilfe einer VHD-Vorlage einrichten können oder in Azure Government arbeiten.

Überprüfen Sie nach der Erstellung der Appliance, ob diese eine Verbindung mit der Azure Migrate-Serverbewertung herstellen kann. Führen Sie dann die erstmalige Konfiguration durch, und registrieren Sie sie für das Azure Migrate-Projekt.

## <a name="appliance-deployment-vhd"></a>Bereitstellung der Appliance (VHD)

So richten Sie die Appliance mithilfe einer VHD-Vorlage ein

- Herunterladen einer komprimierten Hyper-V-VHD über das Azure-Portal
- Erstellen der Appliance und Überprüfen der Verbindungsherstellung mit der Azure Migrate-Serverbewertung
- Durchführen der Erstkonfiguration für die Appliance und Registrieren der Appliance beim Azure Migrate-Projekt

## <a name="download-the-vhd"></a>Herunterladen der VHD

Laden Sie die gezippte VHD-Vorlage für die Appliance herunter.

1. Klicken Sie unter **Migrationsziele** > **Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf **Ermitteln**.
2. Klicken Sie unter **Computer ermitteln** > **Sind Ihre Computer virtualisiert?** auf **Ja, mit Hyper-V**.
3. Klicken Sie auf **Herunterladen**, um die VHD-Datei herunterzuladen.

    ![Herunterladen der VM](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>Überprüfen der Sicherheit

Vergewissern Sie sich vor der Bereitstellung, dass die gezippte Datei sicher ist.

1. Öffnen Sie auf dem Computer, auf den Sie die Datei heruntergeladen haben, ein Administratorbefehlsfenster.
2. Führen Sie den folgenden Befehl aus, um den Hash für die VHD zu generieren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Beispielverwendung: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.vhd SHA256```
3.  Für die Applianceversion 2.19.11.12 muss der generierte Hash den folgenden [Einstellungen](./tutorial-assess-hyper-v.md#verify-security) entsprechen.




## <a name="create-the-appliance-vm"></a>Erstellen der Appliance-VM

Importieren Sie die heruntergeladene Datei, und erstellen Sie die VM.

1. Extrahieren Sie die gezippte VHD-Datei in einem Ordner auf dem Hyper-V-Host, der die Appliance-VM hostet. Drei Ordner werden extrahiert.
2. Öffnen Sie den Hyper-V-Manager. Klicken Sie unter **Aktionen** auf **Virtuellen Computer importieren**.

    ![Bereitstellen der VHD](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

2. Klicken Sie im Assistenten zum Importieren virtueller Computer unter **Vorbereitung** auf **Weiter**.
3. Geben Sie unter **Ordner suchen** den Ordner an, der die extrahierte VHD enthält. Klicken Sie dann auf **Weiter**.
1. Klicken Sie unter **Virtuellen Computer auswählen** auf **Weiter**.
2. Klicken Sie unter **Importtyp auswählen** auf **Virtuellen Computer kopieren (neue eindeutige ID erstellen)** . Klicken Sie dann auf **Weiter**.
3. Behalten Sie unter **Ziel auswählen** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
4. Behalten Sie unter **Speicherordner** die Standardeinstellung bei. Klicken Sie auf **Weiter**.
5. Geben Sie unter **Netzwerk auswählen** den virtuellen Switch an, der von der VM verwendet wird. Der Switch benötigt Internetkonnektivität, um Daten an Azure senden zu können.
6. Überprüfen Sie die Einstellungen unter **Zusammenfassung**. Klicken Sie auf **Fertig stellen**.
7. Starten Sie die VM im Hyper-V-Manager unter **Virtuelle Computer**.


### <a name="verify-appliance-access-to-azure"></a>Überprüfen des Appliancezugriffs auf Azure

Stellen Sie sicher, dass die Appliance-VM eine Verbindung mit Azure-URLs für [öffentliche](migrate-appliance.md#public-cloud-urls) und [Azure Government](migrate-appliance.md#government-cloud-urls)-Clouds herstellen kann.

## <a name="configure-the-appliance"></a>Konfigurieren der Appliance

Führen Sie die Ersteinrichtung der Appliance durch. Wenn Sie die Appliance mithilfe eines Skripts anstelle einer VHD bereitstellen, gelten die ersten beiden Schritte im Verfahren nicht.

1. Klicken Sie im Hyper-V-Manager unter **Virtuelle Computer** mit der rechten Maustaste auf die VM, und klicken Sie anschließend auf **Verbinden**.
2. Geben Sie die Sprache, die Zeitzone und das Kennwort für die Appliance an.
3. Öffnen Sie in einem Browser auf einem beliebigen Computer, der eine Verbindung mit der VM herstellen kann, und öffnen Sie die URL der Appliance-Web-App: **https://*Appliancename oder IP-Adresse*: 44368**.

   Alternativ können Sie auch auf dem Appliancedesktop auf die App-Verknüpfung klicken, um die App zu öffnen.
1. Gehen Sie in der Web-App unter **Erforderliche Komponenten einrichten** wie folgt vor:
    - **Lizenz**: Akzeptieren Sie die Lizenzbedingungen, und lesen Sie die Drittanbieterinformationen.
    - **Konnektivität**: Die App überprüft, ob die VM über Internetzugriff verfügt. Falls die VM einen Proxy verwendet, gehen Sie wie folgt vor:
        - Klicken Sie auf **Proxyeinstellungen**, und geben Sie die Proxyadresse und den Lauschport an (im Format http://ProxyIPAddress oder http://ProxyFQDN ).
        - Geben Sie die Anmeldeinformationen an, wenn der Proxy eine Authentifizierung erfordert.
        - Es werden nur HTTP-Proxys unterstützt.
    - **Uhrzeitsynchronisierung**: Die Uhrzeit wird überprüft. Die Uhrzeit der Appliance muss mit der Internetzeit synchronisiert werden, damit die VM-Ermittlung ordnungsgemäß funktioniert.
    - **Updates installieren**: Die Azure Migrate-Serverbewertung überprüft, ob auf der Appliance die neuesten Updates installiert sind.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrieren der Appliance bei Azure Migrate

1. Klicken Sie auf **Anmelden**. Sollte keine Anmeldung angezeigt werden, vergewissern Sie sich, dass Sie den Popupblocker im Browser deaktiviert haben.
2. Melden Sie sich auf der neuen Registerkarte mit Ihren Azure-Anmeldeinformationen an.
    - Melden Sie sich mit Ihrem Benutzernamen und Ihrem Kennwort an.
    - Die Anmeldung mit einer PIN wird nicht unterstützt.
3. Kehren Sie nach erfolgreicher Anmeldung zur Web-App zurück.
4. Wählen Sie das Abonnement aus, in dem das Azure Migrate-Projekt erstellt wurde. Wählen Sie anschließend das Projekt aus.
5. Geben Sie einen Namen für die Appliance an. Für den Namen können bis zu 14 alphanumerische Zeichen angegeben werden.
6. Klicken Sie auf **Registrieren**.


### <a name="delegate-credentials-for-smb-vhds"></a>Delegieren von Anmeldeinformationen für SMB-VHDs

Wenn Sie VHDs in SMBs ausführen, müssen Sie die Delegierung von Anmeldeinformationen von der Appliance an die Hyper-V-Hosts aktivieren. Gehen Sie dazu in der Appliance wie folgt vor:

1. Führen Sie auf der Appliance-VM den folgenden Befehl aus. „HyperVHost1“ und „HyperVHost2“ sind Beispielhostnamen.

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. Gehen Sie alternativ im Editor für lokale Gruppenrichtlinien auf der Appliance wie folgt vor:
    - Klicken Sie unter **Richtlinie für "Lokaler Computer"**  > **Computerkonfiguration** auf **Administrative Vorlagen** > **System** > **Delegierung von Anmeldeinformationen**.
    - Doppelklicken Sie auf **Delegierung von aktuellen Anmeldeinformationen zulassen**, und wählen Sie **Aktiviert** aus.
    - Klicken Sie unter **Optionen** auf **Anzeigen**, und fügen Sie jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.
    - Doppelklicken Sie unter **Delegierung von Anmeldeinformationen** auf **Delegierung von aktuellen Anmeldeinformationen mit reiner NTLM-Serverauthentifizierung zulassen**. Fügen Sie auch hier wieder jeden Hyper-V-Host, den Sie ermitteln möchten, mit dem Präfix **wsman/** der Liste hinzu.

## <a name="start-continuous-discovery"></a>Starten der kontinuierlichen Ermittlung

Stellen Sie von der Appliance aus eine Verbindung mit Hyper-V-Hosts oder -Clustern her, und starten Sie VM-Ermittlung.

1. Geben Sie unter **Benutzername** und **Kennwort** die Kontoanmeldeinformationen an, die die Appliance für die VM-Ermittlung verwendet. Geben Sie einen Anzeigenamen für die Anmeldeinformationen an, und klicken Sie auf **Details speichern**.
2. Klicken Sie auf **Host hinzufügen**, und geben Sie Details zum Hyper-V-Host/-Cluster an.
3. Klicken Sie auf **Überprüfen**. Nach der Überprüfung wird die Anzahl von VMs angezeigt, die auf dem jeweiligen Host/im jeweiligen Cluster ermittelt werden können.
    - Sollte bei der Überprüfung eines Hosts ein Fehler auftreten, sehen Sie sich den Fehler an, indem Sie mit dem Mauszeiger auf das Symbol in der Spalte **Status** zeigen. Beheben Sie mögliche Probleme, und wiederholen Sie die Überprüfung.
    - Wenn Sie Hosts oder Cluster entfernen möchten, wählen Sie **Löschen** aus.
    - Es ist nicht möglich, einen bestimmten Host aus einem Cluster zu entfernen. Sie können nur den gesamten Cluster entfernen.
    - Ein Cluster kann auch dann hinzugefügt werden, wenn Probleme mit bestimmten, in dem Cluster enthaltenen Hosts vorliegen.
4. Klicken Sie nach der Überprüfung auf **Speichern und Ermittlung starten**, um mit der Ermittlung zu beginnen.

Daraufhin wird die Ermittlung gestartet. Es dauert etwa 15 Minuten, bis Metadaten von ermittelten VMs im Azure-Portal angezeigt werden.

## <a name="verify-vms-in-the-portal"></a>Überprüfen virtueller Computer im Portal

Nach Abschluss der Ermittlung können Sie überprüfen, ob die VMs im Portal angezeigt werden.

1. Öffnen Sie das Azure Migrate-Dashboard.
2. Klicken Sie unter **Azure Migrate – Server** > **Azure Migrate: Server Assessment** (Azure Migrate-Serverbewertung) auf das Symbol mit der Anzahl für **Ermittelte Server**.


## <a name="next-steps"></a>Nächste Schritte

Testen Sie die [Hyper-V-Bewertung](tutorial-assess-hyper-v.md) mit der Azure Migrate-Serverbewertung.
