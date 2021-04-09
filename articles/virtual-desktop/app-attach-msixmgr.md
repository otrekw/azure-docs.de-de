---
title: Verwenden des MSIXMGR-Tools – Azure
description: Hier erfahren Sie, wie Sie das MSIXMGR-Tool für Windows Virtual Desktop verwenden.
author: Heidilohr
ms.topic: how-to
ms.date: 02/23/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4b34fb0d3bb2d49255007b9722a0a636c1441b8c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "101743342"
---
# <a name="using-the-msixmgr-tool"></a>Verwenden des MSIXMGR-Tools

Das MSIXMGR-Tool dient zum Erweitern von mit MSIX gepackten Anwendungen in MSIX-Images. Das Tool erweitert eine mit MSIX gepackte Anwendung (.MSIX) in eine VHD-, VHDx- oder CIM-Datei. Das resultierende MSIX-Image wird in dem Azure Storage-Konto gespeichert, das von Ihrer Windows Virtual Desktop-Bereitstellung verwendet wird. In diesem Artikel erfahren Sie, wie Sie das MSIXMGR-Tool verwenden.

>[!NOTE]
>Um die Kompatibilität sicherzustellen, vergewissern Sie sich, dass die CIMs, die Ihre MSIX-Images speichern, in der Betriebssystemversion generiert werden, die in Ihren Windows Virtual Desktop-Hostpools ausgeführt wird. MSIXMGR kann CIM-Dateien erstellen, aber Sie können diese Dateien nur in einem Hostpool verwenden, in dem Windows 10 20H2 ausgeführt wird.

## <a name="requirements"></a>Anforderungen

Bevor Sie die Anweisungen in diesem Artikel befolgen können, müssen Sie die folgenden Schritte ausführen:

- [Herunterladen des MSIXMGR-Tools](https://aka.ms/msixmgr)
- Abrufen einer mit MSIX gepackten Anwendung (MSIX-Datei)
- Erhalten von administrativen Berechtigungen auf dem Computer, auf dem Sie das MSIX-Image erstellen werden

## <a name="create-an-msix-image"></a>Erstellen eines MSIX-Images

Als Erweiterung wird der Vorgang bezeichnet, bei dem eine mit MSIX gepackte Anwendung (.MSIX) in ein MSIX-Image (.VHD(x)-Datei oder .CIM-Datei) entpackt wird.

So erweitern Sie eine MSIX-Datei:

1. [Laden Sie das MSIXMGR-Tool](https://aka.ms/msixmgr) herunter, falls Sie dies noch nicht getan haben.

2. Entpacken Sie die Datei MSIXMGR.zip in einem lokalen Ordner.

3. Öffnen Sie eine Eingabeaufforderung im Modus mit erhöhten Rechten.

4. Suchen Sie den Ordner aus Schritt 2.

5. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um ein MSIX-Image zu erstellen.

    ```cmd
    msixmgr.exe -Unpack -packagePath <path to package> -destination <output folder> [-applyacls] [-create] [-vhdSize <size in MB>] [-filetype <CIM | VHD | VHDX>] [-rootDirectory <rootDirectory>]
    ```

    Denken Sie daran, die Platzhalterwerte durch die jeweils relevanten Werte zu ersetzen. Beispiel:

    ```cmd
    msixmgr.exe -Unpack -packagePath "C:\Users\%username%\Desktop\packageName_3.51.1.0_x64__81q6ced8g4aa0.msix" -destination "c:\temp\packageName.vhdx" -applyacls -create -vhdSize 200 -filetype "vhdx" -rootDirectory apps
    ```

6. Wechseln Sie nach dem Erstellungsvorgang zum Zielordner, und vergewissern Sie sich, dass das MSIX-Image (.VHDX) erfolgreich erstellt wurde.

## <a name="create-an-msix-image-in-a-cim-file"></a>Erstellen eines MSIX-Images in einer CIM-Datei

Sie können auch den Befehl in [Schritt 5](#create-an-msix-image) verwenden, um CIM- und VHDX-Dateien zu erstellen. Ersetzen Sie dabei den Dateityp und den Zielpfad.

Um beispielsweise eine CIM-Datei zu erstellen, verwenden Sie den folgenden Befehl:

```cmd
msixmgr.exe -Unpack -packagePath "C:\Users\ssa\Desktop\packageName_3.51.1.0_x64__81q6ced8g4aa0.msix" -destination "c:\temp\packageName.cim" -applyacls -create -vhdSize 200 -filetype "cim" -rootDirectory apps
```

So erstellen Sie mit diesem Befehl eine VHDX-Datei:

```cmd
msixmgr.exe -Unpack -packagePath "C:\Users\ssa\Desktop\packageName_3.51.1.0_x64__81q6ced8g4aa0.msix" -destination "c:\temp\packageName.vhdx" -applyacls -create -vhdSize 200 -filetype "vhdx" -rootDirectory apps
```

## <a name="next-steps"></a>Nächste Schritte

Unter [Was ist das MSIX-Feature zum Anfügen von Apps?](what-is-app-attach.md) erfahren Sie mehr über das Anfügen von Apps mit MSIX.

Weitere Informationen zum Einrichten der App-Anfügung finden Sie in den folgenden Artikeln:

- [Einrichten des MSIX-Features zum Anfügen von Apps mit dem Azure-Portal](app-attach-azure-portal.md)
- [Einrichten des MSIX-Features zum Anfügen von Apps mit PowerShell](app-attach-powershell.md)
- [Erstellen von PowerShell-Skripts für das MSIX-Feature zum Anfügen von Apps](app-attach.md)
- [Vorbereiten eines MSIX-Images für Windows Virtual Desktop](app-attach-image-prep.md)
- [Einrichten einer Dateifreigabe für das MSIX-Feature zum Anfügen von Apps](app-attach-file-share.md)

Wenn Sie Fragen zum MSIX-Feature zum Anfügen von Apps haben, finden Sie Antworten unter [Häufig gestellte Fragen zum MSIX-Feature zum Anfügen von Apps](app-attach-faq.md) und im [Glossar des MSIX-Features zum Anfügen von Apps](app-attach-glossary.md).
