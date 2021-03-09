---
title: 'Übersicht: Azure Percept Dev Tools-Paketinstallationsprogramm'
description: Hier erfahren Sie, wie Sie die erweiterte Entwicklung mit Azure Percept mithilfe des Dev Tools-Paketinstallationsprogramms beschleunigen können.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: d684311ef959ac13f3be8bac7ffbbb06a741962a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097702"
---
# <a name="dev-tools-pack-installer-overview"></a>Übersicht: Dev Tools-Paketinstallationsprogramm

Das Dev Tools-Paketinstallationsprogramm ist eine Komplettlösung, die alle erforderlichen Tools für die Entwicklung einer Intelligent Edge-Lösung installiert und konfiguriert. Wenn Sie die unten aufgeführten Softwarepakete bereits installiert haben, werden diese vom Dev Tools-Paketinstallationsprogramm neu installiert, damit Ihre Tools mit den Softwareversionen des Installationsprogramms übereinstimmen.

## <a name="mandatory-tools-installed"></a>Erforderliche Tools, die installiert werden

* [Visual Studio Code](https://code.visualstudio.com/)
* [Python 3.6 (Windows) oder 3.5 (Linux)](https://www.python.org/)
* [Docker 19.03](https://www.docker.com/)
* [PIP3](https://pip.pypa.io/en/stable/user_guide/)
* [TensorFlow 1.13](https://www.tensorflow.org/)
* [Azure Machine Learning SDK 1.1](https://docs.microsoft.com/python/api/overview/azure/ml/?view=azure-ml-py)

## <a name="optional-tools-available-for-installation"></a>Optionale Tools, die installiert werden können

* [Nvidia DeepStream SDK 5](https://developer.nvidia.com/deepstream-sdk) (Toolkit für die Entwicklung von Lösungen für NVIDIA-Beschleuniger)
* [Intel OpenVino Toolkit 2020.2](https://docs.openvinotoolkit.org/) (Toolkit für die Entwicklung von Lösungen für Intel-Beschleuniger)
* [Lobe.ai](https://lobe.ai/)  
* [Streamlit](https://www.streamlit.io/)
* [Pytorch 1.4.0 (Windows) oder 1.2.0 (Linux)](https://pytorch.org/)
* [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
* [Chainer 5.2](https://chainer.org/)
* [Caffe](https://caffe.berkeleyvision.org/)
* [CUDA Toolkit 10.0.130](https://developer.nvidia.com/cuda-toolkit)
* [Microsoft Cognitive Toolkit 2.5.1](https://www.microsoft.com/research/product/cognitive-toolkit/?lang=fr_ca)

## <a name="known-issues"></a>Bekannte Probleme

- Die optionale Caffe-Installation kann fehlschlagen, wenn Docker nicht korrekt auf dem System ausgeführt wird. Wenn Sie Caffe installieren möchten, stellen Sie sicher, dass Docker installiert ist und ausgeführt wird, bevor Sie die Caffe-Installation über das Dev Tools-Paketinstallationsprogramm durchführen. 

- Auf inkompatiblen Systemen tritt bei der optionalen CUDA-Installation ein Fehler auf. Überprüfen Sie die Systemkompatibilität, bevor Sie das [CUDA Toolkit 10.0.130](https://developer.nvidia.com/cuda-toolkit) über das Dev Tools-Paketinstallationsprogramm installieren.

## <a name="minimum-requirements"></a>Mindestanforderungen

* Mindestanforderungen für Docker:

    * Windows:
        * https://docs.docker.com/docker-for-windows/install/#system-requirements

        - Windows 10 64-Bit: Pro, Enterprise oder Education (Build 16299 oder höher).

             Informationen zu Windows 10 Home finden Sie unter „Installieren von Docker Desktop unter Windows Home“.
           - Die Windows-Features „Hyper-V“ und „Container“ müssen aktiviert sein.
           - Die folgende Hardware ist für die erfolgreiche Ausführung von Hyper-V für Clients unter Windows 10 erforderlich:

              - 64-Bit-Prozessor mit [Second Level Address Translation (SLAT)](https://en.wikipedia.org/wiki/Second_Level_Address_Translation)
              - 4 GB Systemarbeitsspeicher
              - Die Unterstützung für Hardwarevirtualisierung auf BIOS-Ebene muss in den BIOS-Einstellungen aktiviert sein. Weitere Informationen finden Sie unter „Virtualisierung“.

        > [!NOTE]
        > Docker unterstützt Docker Desktop unter Windows basierend auf dem Supportlebenszyklus von Microsoft für das Betriebssystem Windows 10. Weitere Informationen finden Sie im [Informationsblatt zum Lebenszyklus von Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

    * Mac:
        * https://docs.docker.com/docker-for-mac/install/#system-requirements
       
        Ihr Mac muss die folgenden Anforderungen erfüllen, damit Docker Desktop erfolgreich installiert werden kann:
         
         - **Mac-Hardware von 2010 oder ein neueres Modell mit Intel-Prozessor** mit Intel-Hardwareunterstützung für die Virtualisierung der Speicherverwaltungseinheit (Memory Management Unit, MMU), einschließlich Extended Page Tables (EPT) und uneingeschränktem Modus. Sie können überprüfen, ob Ihr Computer über diese Unterstützung verfügt, indem Sie den folgenden Befehl in einem Terminal ausführen: ```sysctl kern.hv_support```

        Wenn der Mac das Hypervisor-Framework unterstützt, gibt der Befehl ```kern.hv_support: 1``` aus.

         - **macOS-Version 10.14 oder höher** (d. h. Mojave, Catalina oder Big Sur). Wir empfehlen, ein Upgrade auf die neueste macOS-Version durchzuführen.

        Wenn nach dem Upgrade auf macOS-Version 10.15 Probleme auftreten, müssen Sie die neueste Version von Docker Desktop installieren, um die Kompatibilität mit dieser macOS-Version sicherzustellen.

        - Mindestens 4 GB RAM
        - VirtualBox-Versionen vor 4.3.30 sind nicht mit Docker Desktop kompatibel und dürfen daher nicht installiert sein.

        > [!NOTE]
        > Docker unterstützt Docker Desktop in den aktuellen macOS-Versionen, d. h. in der aktuellen Version von macOS und den vorherigen beiden Releases. Da neue Hauptversionen von macOS allgemein verfügbar gemacht werden, wird die älteste Version von Docker nicht mehr unterstützt. Stattdessen wird die neueste Version von macOS (zusätzlich zu den beiden vorherigen Releases) unterstützt. Docker Desktop unterstützt derzeit macOS Mojave, macOS Catalina und macOS Big Sur.
        > 
        - Das Installationsprogramm wird in Apple M1 nicht unterstützt.

## <a name="instructions"></a>Instructions

1. Laden Sie das Dev Tools-Paketinstallationsprogramm für [Windows](https://go.microsoft.com/fwlink/?linkid=2132187), [Linux](https://go.microsoft.com/fwlink/?linkid=2132186) und [Mac](https://go.microsoft.com/fwlink/?linkid=2132296) herunter.

1. Abhängig von Ihrer Plattform gibt es einige Unterschiede beim Starten des Installationsprogramms.

    1. Windows:
    
        1. Klicken Sie auf **Dev-Tools-Pack-Installer**, um den Installations-Assistenten zu öffnen.
        
    1. Für Mac:
    
        1. Verschieben Sie die Datei „Dev-Tools-Pack-Installer.app“ nach dem Herunterladen in den Ordner „Programme“.
        
        1. Klicken Sie auf **Dev-Tools-Pack-Installer.app**, um den Installations-Assistenten zu öffnen.
        
        1. Falls ein Sicherheitsdialogfeld „Nicht verifizierter Entwickler“ angezeigt wird, gehen Sie wie folgt vor:
        
            1. Wechseln Sie zu „Systemeinstellungen -> Sicherheit -> Allgemein“, und klicken Sie neben „Dev-Tools-Pack-Installer.app“ auf die Schaltfläche „Dennoch öffnen“.
        
            1. Klicken Sie erneut auf das Electron-Symbol im Dock.
        
            1. Klicken Sie im Sicherheitsdialogfeld auf die Schaltfläche „Öffnen“.
    
    1. Linux:
    
        1. Wenn Sie vom Browser dazu aufgefordert werden, klicken Sie auf „Speichern“, um den Download des Installationsprogramms abzuschließen.
        
        1. Fügen Sie der **.appimage**-Datei Ausführungsberechtigungen hinzu. Methode 1 (Befehlszeile):
            
            1. Öffnen Sie das Linux-Terminal.
            
            1. Geben Sie im Terminal Folgendes ein, um zum Ordner „Downloads“ zu wechseln.
            
                1. cd ~/Downloads/
                
            1. Geben Sie im Terminal Folgendes ein, um AppImage ausführbar zu machen.
            
                1. chmod +x **Dev-Tools-Pack-Installer.AppImage**
                
            1. Geben Sie im Terminal Folgendes ein, um das Installationsprogramm auszuführen.
            
                1. ./Dev-Tools-Pack-Installer.AppImage
        
        1. Fügen Sie der **.appimage**-Datei Ausführungsberechtigungen hinzu. Methode 2 (Benutzeroberfläche):
        
            1. Klicken Sie mit der rechten Maustaste auf die APPIMAGE-Datei, und wählen Sie „Eigenschaften“ aus.
            
            1. Öffnen Sie die Registerkarte „Berechtigungen“.
            
            1. Aktivieren Sie das Kontrollkästchen „Ausführen der Datei als Programm zulassen“.
            
            1. Schließen Sie die Eigenschaften, und öffnen Sie die APPIMAGE-Datei.

1. Klicken Sie auf der Seite zum **Installieren des Dev Tools-Paketinstallationsprogramms** auf **Lizenz anzeigen**, um die Lizenzverträge für die einzelnen Softwarepakete im Installationsprogramm anzuzeigen. Wenn Sie die Bedingungen der Lizenzverträge akzeptieren, aktivieren Sie das Kontrollkästchen, und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-license-agreements.png" alt-text="Bildschirm mit den Lizenzverträgen im Installationsprogramm.":::

1. Klicken Sie auf **Datenschutzbestimmungen**, um die Microsoft-Datenschutzbestimmungen anzuzeigen. Wenn Sie den Datenschutzbestimmungen zustimmen und Diagnosedaten an Microsoft senden möchten, wählen Sie **Ja** aus, und klicken Sie auf **Weiter**. Wählen Sie andernfalls **Nein** aus, und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-privacy-statement.png" alt-text="Bildschirm mit den Datenschutzbestimmungen im Installationsprogramm.":::

1. Wählen Sie auf der Seite **Komponenten konfigurieren** die optionalen Tools aus, die Sie installieren möchten (die erforderlichen Tools werden standardmäßig installiert).

    1. Wenn Sie mit dem Azure Percept-Audio-SOM (System On Module) arbeiten, das im Azure Percept DK enthalten ist, müssen Sie das Intel OpenVino Toolkit und Miniconda3 installieren.

    1. Klicken Sie auf **Installieren**, um die Installation fortzusetzen.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-configure-components.png" alt-text="Bildschirm mit den verfügbaren Softwarepaketen im Installationsprogramm.":::

1. Nach der erfolgreichen Installation aller ausgewählten Komponenten wird die Seite **Fertigstellen des Installations-Assistenten** angezeigt. Klicken Sie auf **Fertig stellen**, um das Installationsprogramm zu beenden.

    :::image type="content" source="./media/dev-tools-installer/dev-tools-finish.png" alt-text="Bildschirm zum Abschluss des Installationsprogramms.":::

## <a name="docker-status-check"></a>Überprüfen des Docker-Status

Wenn Sie vom Installationsprogramm aufgefordert werden, die korrekte Ausführung von Docker Desktop zu überprüfen, führen Sie die folgenden Schritte aus:

   1. Windows:
   
      1. Erweitern Sie ausgeblendete Symbole in der Taskleiste:
      
         1. Erweitern Sie die ausgeblendeten Symbole in der Taskleiste:

            :::image type="content" source="./media/dev-tools-installer/system-tray.png" alt-text="Taskleiste":::
         
         1. Überprüfen Sie, ob für das Docker Desktop-Symbol „Docker Desktop wird ausgeführt“ angezeigt wird:

            :::image type="content" source="./media/dev-tools-installer/docker-status-running.png" alt-text="Docker-Status":::
         
         1. Wenn das obige Symbol nicht in der Taskleiste angezeigt wird, starten Sie Docker Desktop über das Startmenü.
         
         1. Falls Sie von Docker zum Neustarten aufgefordert werden, können Sie das Installationsprogramm nach dem Neustart schließen und neu starten, wenn Docker ausgeführt wird. Alle erfolgreich installierten Anwendungen von Drittanbietern sollten erkannt und nicht automatisch neu installiert werden.

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten bei der erweiterten Entwicklung für Azure Percept DK finden Sie im [Repository zur erweiterten Entwicklung in Azure Percept](https://github.com/microsoft/azure-percept-advanced-development).