---
title: Vorbereiten eines MSIX App Attach-Images für Windows Virtual Desktop – Azure
description: Erfahren Sie, wie Sie ein MSIX App Attach-Image für einen Windows Virtual Desktop-Hostpool erstellen.
author: Heidilohr
ms.topic: how-to
ms.date: 04/13/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: 443f117907381862639564dfbf9752562f4a3564
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2021
ms.locfileid: "107363664"
---
# <a name="prepare-an-msix-image-for-windows-virtual-desktop"></a>Vorbereiten eines MSIX-Images für Windows Virtual Desktop

MSIX App Attach ist eine Lösung für Anwendungsschichten, mit der Sie Apps aus einem MSIX-Paket dynamisch an eine Benutzersitzung anfügen können. Das MSIX-Paketsystem trennt Apps vom Betriebssystem und vereinfacht so das Erstellen von Images für VMs. Mit MSIX-Paketen haben Sie auch mehr Kontrolle darüber, auf welche Apps Benutzer auf ihren VMs zugreifen können. Sie können sogar Apps vom Masterimage abtrennen und erst später an die Benutzer übergeben.

## <a name="create-a-vhd-or-vhdx-package-for-msix"></a>Erstellen von VHD- oder VHDX-Paketen für MSIX

MSIX-Pakete müssen das VHD- oder VHDX-Format aufweisen, damit sie ordnungsgemäß funktionieren. Dies bedeutet, dass Sie zunächst ein VHD- oder VHDX-Paket erstellen müssen.

>[!NOTE]
>Wenn Sie dies noch nicht getan haben, müssen Sie auch Hyper-V aktivieren. Befolgen Sie dazu die Anweisungen unter [Installieren von Hyper-V unter Windows 10](/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

So erstellen Sie ein VHD- oder VHDX-Paket für MSIX

1. Öffnen Sie zuerst PowerShell.
2. Führen Sie als Nächstes das folgende Cmdlet aus, um eine VHD zu erstellen:

    ```powershell
    New-VHD -SizeBytes <size>MB -Path c:\temp\<name>.vhd -Dynamic -Confirm:$false
    ```

    >[!NOTE]
    > Stellen Sie sicher, dass die VHD groß genug ist, um das erweiterte MSIX-Paket zu speichern.

3. Führen Sie das folgende Cmdlet aus, um die neu erstellte VHD einzubinden:

    ```powershell
    $vhdObject = Mount-VHD c:\temp\<name>.vhd -Passthru
    ```

4. Führen Sie danach dieses Cmdlet aus, um die eingebundene VHD zu initialisieren:

    ```powershell
    $disk = Initialize-Disk -Passthru -Number $vhdObject.Number
    ```

5. Führen Sie anschließend dieses Cmdlet aus, um eine neue Partition für die initialisierte VHD zu erstellen:

    ```powershell
    $partition = New-Partition -AssignDriveLetter -UseMaximumSize -DiskNumber $disk.Number
    ```

6. Führen Sie dieses Cmdlet aus, um die Partition zu formatieren:

    ```powershell
    Format-Volume -FileSystem NTFS -Confirm:$false -DriveLetter $partition.DriveLetter -Force
    ```

7. Erstellen Sie zum Schluss einen übergeordneten Ordner auf der eingebundenen VHD. Dieser Schritt ist erforderlich, da das MSIX-Paket für die ordnungsgemäße Funktion einen übergeordneten Ordner benötigt. Der Name des übergeordneten Ordners ist nicht wichtig, er muss lediglich vorhanden ist.

## <a name="expand-msix"></a>Erweitern von MSIX

Danach müssen Sie das MSIX-Image durch Entpacken der enthaltenen Dateien in die VHD erweitern.

So erweitern Sie das MSIX-Image

1. [Laden Sie das msixmgr-Tool herunter](https://aka.ms/msixmgr), und speichern Sie den ZIP-Ordner in einem Ordner auf der Sitzungshost-VM.

2. Entpacken Sie den ZIP-Ordner des msixmgr-Tools.

3. Speichern Sie das MSIX-Quellpaket im gleichen Ordner, in dem Sie das msixmgr-Tool entpackt haben.

4. Öffnen Sie eine Eingabeaufforderung als Administrator, und navigieren Sie zu dem Ordner, den Sie zum Herunterladen und Entzippen des msixmgr-Tools verwendet haben.

5. Führen Sie das folgende Cmdlet aus, um das MSIX-Paket in die VHD zu entpacken, die Sie im vorherigen Abschnitt erstellt haben.

    ```powershell
    msixmgr.exe -Unpack -packagePath <package>.msix -destination "f:\<name of folder you created earlier>" -applyacls
    ```

    Nachdem das Entpacken abgeschlossen ist, sollte die folgende Meldung angezeigt werden:

    > Die ACLs für das Paket wurden erfolgreich entpackt und angewandt: <package name>.msix

    >[!NOTE]
    > Bei der Verwendung von Paketen aus dem Microsoft Store for Business (oder Education) in Ihrem Netzwerk oder auf Geräten, die nicht mit dem Internet verbunden sind, müssen Sie die Paketlizenzen aus dem Microsoft Store abrufen und installieren, um die Apps ausführen zu können. Informationen zum Erhalt der Lizenzen finden Sie unter [Offlineverwendung von Paketen](app-attach.md#use-packages-offline).

6. Wechseln Sie zur eingebundenen VHD, und öffnen Sie den App-Ordner, um zu überprüfen, ob der Paketinhalt vorhanden ist.

7. Heben Sie die Einbindung der VHD auf.

## <a name="upload-msix-image-to-share"></a>Hochladen des MSIX-Images in eine Freigabe

Nachdem Sie das MSIX-Paket erstellt haben, müssen Sie die resultierende VHD-, VHDX- oder CIM-Datei in eine Freigabe hochladen, auf die die VMs Ihrer Benutzer Zugriff haben.

## <a name="next-steps"></a>Nächste Schritte

Stellen Sie unserer Community unter [Windows Virtual Desktop TechCommunity](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) Ihre Fragen zu dieser Funktion.

Sie können Ihr Feedback in Bezug auf Windows Virtual Desktop auch im [Windows Virtual Desktop-Feedback-Hub](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app) hochladen.

Folgende Artikel sind eventuell hilfreich:

- [Glossar des MSIX-Features zum Anfügen von Apps](app-attach-glossary.md)
- [Häufig gestellte Fragen zum MSIX-Feature zum Anfügen von Apps](app-attach-faq.md)