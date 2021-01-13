---
title: Einbinden einer Azure-Dateifreigabe über SMB mit macOS | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mit dem Finder oder Terminal unter macOS eine Azure-Dateifreigabe über SMB einbinden. Azure Files ist das benutzerfreundliche Clouddateisystem von Microsoft.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/23/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 119f4c0ea434bc431b40c905d9142e187b7d9474
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326064"
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>Einbinden einer Azure-Dateifreigabe über SMB mit macOS
[Azure Files](storage-files-introduction.md) ist das benutzerfreundliche Clouddateisystem von Microsoft. Azure-Dateifreigaben können von macOS High Sierra 10.13 oder höher mit dem SMB 3-Protokoll nach Branchenstandard eingebunden werden. In diesem Artikel werden zwei Methoden zum Einbinden einer Azure-Dateifreigabe unter macOS behandelt: über die Finder-Benutzeroberfläche und mithilfe des Terminals.

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>Voraussetzungen für das Einbinden von Azure-Dateifreigaben unter macOS
* **Speicherkontoname**: Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den Namen des Speicherkontos.

* **Speicherkontoschlüssel**: Zum Einbinden einer Azure-Dateifreigabe benötigen Sie den primären (oder sekundären) Speicherschlüssel. SAS-Schlüssel können derzeit nicht zum Einbinden verwendet werden.

* **Sicherstellen, dass Port 445 geöffnet ist:** SMB kommuniziert über den TCP-Port 445. Vergewissern Sie sich, dass die Firewall Ihres Clientcomputers (Mac) den TCP-Port 445 nicht blockiert.

## <a name="mount-an-azure-file-share-via-finder"></a>Einbinden einer Azure-Dateifreigabe über den Finder
1. **Öffnen Sie den Finder:** Der Finder ist unter macOS standardmäßig geöffnet. Wenn Sie den Finder als aktuelle Anwendung auswählen möchten, klicken Sie im Dock auf das macOS-Gesichtssymbol:  
    ![Das macOS-Gesichtssymbol](./media/storage-how-to-use-files-mac/mount-via-finder-1.png)

2. **Wählen Sie im Menü „Gehe zu“ die Option „Mit Server verbinden“ aus:** Wandeln Sie die beiden umgekehrten Schrägstriche am Anfang (`\\`) unter Verwendung des UNC-Pfads aus den Voraussetzungen in `smb://` und alle anderen umgekehrten Schrägstriche (`\`) in normale Schrägstriche (`/`) um. Der Link sollte wie folgt aussehen: ![Dialogfeld „Mit Server verbinden“](./media/storage-how-to-use-files-mac/mount-via-finder-2.png)

3. **Geben Sie als Benutzername und Kennwort den Speicherkontonamen und den Speicherkontoschlüssel an:** Wenn Sie im Dialogfeld „Mit Server verbinden“ auf „Verbinden“ klicken, werden Sie zur Angabe eines Benutzernamens und eines Kennworts aufgefordert. (Hier wird automatisch Ihr macOS-Benutzername eingefügt.) Sie können den Namen des Speicherkontos und den Speicherkontoschlüssel optional in Ihrem macOS-Schlüsselbund platzieren.

4. **Verwenden Sie die Azure-Dateifreigabe nach Bedarf:** Nach Angabe des Freigabenamens und des Speicherkontoschlüssels als Benutzername und Kennwort wird die Freigabe eingebunden. Die Freigabe kann wie ein normaler lokaler Ordner oder eine normale Dateifreigabe verwendet werden. So können Sie beispielsweise Dateien per Drag&Drop in der Dateifreigabe platzieren:

    ![Momentaufnahme einer eingebundenen Azure-Dateifreigabe](./media/storage-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Einbinden einer Azure-Dateifreigabe über das Terminal
1. Ersetzen Sie  `<storage-account-name>`, `<storage-account-key>` und `<share-name>` durch die entsprechenden Werte für Ihre Umgebung.

    ```
    open smb://<storage-account-name>:<storage-account-key>@<storage-account-name>.file.core.windows.net/<share-name>
    ```

2. **Verwenden Sie die Azure-Dateifreigabe nach Bedarf:** Die Azure-Dateifreigabe wird an dem Bereitstellungspunkt eingebunden, der durch den vorherigen Befehl angegeben wurde.  

    ![Momentaufnahme der eingebundenen Azure-Dateifreigabe](./media/storage-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Nächste Schritte
* [Herstellen einer Verbindung zwischen Ihrem Mac und freigegebenen Computern: Apple Support](https://support.apple.com/guide/mac-help/connect-mac-shared-computers-servers-mchlp1140/mac)