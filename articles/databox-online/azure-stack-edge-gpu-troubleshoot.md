---
title: Ausführen von Diagnosen und Sammeln von Protokollen zum Behandeln von Problemen bei Azure Stack Edge-Geräten | Microsoft-Dokumentation
description: Beschrieben wird, wie Diagnosen ausgeführt und Protokolle verwendet werden, um Probleme bei Azure Stack Edge Pro GPU-Geräten zu beheben.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 06/10/2021
ms.author: alkohli
ms.custom: contperf-fy21q4
ms.openlocfilehash: 82f8fe0574ec98c71ace2aaddda2d0bc2bc6e99f
ms.sourcegitcommit: 190658142b592db528c631a672fdde4692872fd8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2021
ms.locfileid: "112006345"
---
# <a name="run-diagnostics-collect-logs-to-troubleshoot-azure-stack-edge-device-issues"></a>Ausführen von Diagnosen und Sammeln von Protokollen zum Behandeln von Problemen bei Azure Stack Edge-Geräten

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

In diesem Artikel wird beschrieben, wie Sie Diagnosen ausführen, ein Supportpaket erfassen, erweiterte Sicherheitsprotokolle sammeln und Protokolle überprüfen, um Probleme beim Geräteupload und Aktualisieren auf Ihrem Azure Stack Edge-Gerät zu beheben.


## <a name="run-diagnostics"></a>Ausführen von Diagnosen

Zum Diagnostizieren und Behandeln von Gerätefehlern können Sie die Diagnosetests ausführen. Gehen Sie auf der lokalen Webbenutzeroberfläche Ihres Geräts wie folgt vor, um Diagnosetests auszuführen:

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche zu **Problembehandlung > Diagnosetests**. Wählen Sie den gewünschten Test aus, und klicken Sie auf **Test ausführen**. Mit dem Test werden mögliche Probleme mit Ihren Netzwerk-, Geräte-, Webproxy-, Zeit- oder Cloudeinstellungen diagnostiziert. Sie werden benachrichtigt, dass auf dem Gerät Tests ausgeführt werden.

    ![Auswählen von Tests ](media/azure-stack-edge-gpu-troubleshoot/run-diag-1.png)
 
2. Nach Abschluss der Tests werden die Ergebnisse angezeigt. 

    ![Testergebnisse anzeigen](media/azure-stack-edge-gpu-troubleshoot/run-diag-2.png)

    Sollte ein Test nicht erfolgreich sein, wird eine URL für die empfohlene Maßnahme angezeigt. Wählen Sie die URL aus, um die empfohlene Maßnahme anzuzeigen.
 
    ![Überprüfen von Warnungen für Tests mit Fehlern](media/azure-stack-edge-j-series-troubleshoot/run-diag-3.png)


## <a name="collect-support-package"></a>Zusammenstellen des Unterstützungspaket

Ein Protokollpaket enthält alle relevanten Protokolle, die das Supportteam von Microsoft bei der Behandlung von Geräteproblemen unterstützen. Sie können ein Unterstützungspaket über die lokale Webbenutzeroberfläche generieren.

Gehen Sie wie folgt vor, um ein Unterstützungspaket zusammenzustellen: 

1. Navigieren Sie auf der lokalen Webbenutzeroberfläche zu **Problembehandlung > Support**. Wählen Sie **Supportpaket erstellen** aus. Daraufhin wird das Unterstützungspaket zusammengestellt. Dieser Vorgang kann mehrere Minuten dauern.

    ![Auswählen von „Benutzer hinzufügen“](media/azure-stack-edge-gpu-troubleshoot/collect-logs-1.png)
 
2. Nachdem das Supportpaket erstellt wurde, wählen Sie **Supportpaket herunterladen** aus. Ein ZIP-Paket wird an den gewünschten Pfad heruntergeladen. Sie können das Paket entpacken und sich die Systemprotokolldateien ansehen.

    ![Auswählen von „Benutzer 2 hinzufügen“](media/azure-stack-edge-gpu-troubleshoot/collect-logs-2.png)

## <a name="gather-advanced-security-logs"></a>Sammeln erweiterter Sicherheitsprotokolle

Bei den erweiterten Sicherheitsprotokollen kann es sich um software- oder hardwarebezogene Protokolle zu Angriffen auf Ihr Azure Stack Edge Pro-Gerät handeln.

### <a name="software-intrusion-logs"></a>Protokolle zu softwarebezogenen Angriffen

Die Protokolle zu softwarebezogenen Angriffen oder standardmäßigen Firewallprotokolle werden für ein- und ausgehenden Datenverkehr gesammelt. 

- Wenn für das Gerät ab Werk ein Image erstellt wird, ist die standardmäßige Firewallprotokollierung aktiviert. Diese Protokolle werden standardmäßig im Supportpaket gebündelt, wenn Sie ein Supportpaket über die lokale Benutzeroberfläche oder die Windows PowerShell-Schnittstelle des Geräts erstellen.

- Wenn im Supportpaket nur die Firewallprotokolle benötigt werden, um jegliche Angriffe auf die Software (NW) auf dem Gerät zu überprüfen, verwenden Sie bei der Erstellung des Supportpakets die Option `-Include FirewallLog`. 

- Wenn keine spezielle Include-Option angegeben wird, ist das Firewallprotokoll standardmäßig im Supportpaket enthalten.

- Im Supportpaket befindet sich das Firewallprotokoll in der Datei `pfirewall.log` im Stammordner. Nachfolgend finden Sie ein Beispiel des Softwareangriffsprotokolls für das Azure Stack Edge Pro-Gerät. 

    ```
    #Version: 1.5
    #Software: Microsoft Windows Firewall
    #Time Format: Local
    #Fields: date time action protocol src-ip dst-ip src-port dst-port size tcpflags tcpsyn tcpack tcpwin icmptype icmpcode info path
    
    2019-11-06 12:35:19 DROP UDP 5.5.3.197 224.0.0.251 5353 5353 59 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e88 ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e88 ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e88 ff02::fb 5353 5353 89 - - - - - - 
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9d87 ff02::fb 5353 5353 79 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP 5.5.3.193 224.0.0.251 5353 5353 59 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe08:20d5 ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe08:20d5 ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e8b ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e8b ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP 5.5.3.33 224.0.0.251 5353 5353 59 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e8b ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e8a ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    2019-11-06 12:35:19 DROP UDP fe80::3680:dff:fe01:9e8b ff02::fb 5353 5353 89 - - - - - - - RECEIVE
    ```

### <a name="hardware-intrusion-logs"></a>Protokolle zu hardwarebezogenen Angriffen

Um jegliche Angriffe auf die Hardware des Geräts zu erkennen, werden derzeit alle Ereignisse am Gehäuse wie Öffnen und Schließen protokolliert. 

- Das Systemereignisprotokoll des Geräts wird mit dem Cmdlet `racadm` ausgelesen. Diese Ereignisse werden dann in einer `HWIntrusion.txt`-Datei nach gehäusebezogenen Ereignissen gefiltert.

- Um nur das Hardwareangriffsprotokoll in das Supportpaket aufzunehmen, verwenden Sie beim Erstellen des Supportpakets die Option `-Include HWSelLog`. 

- Wenn keine spezielle Include-Option angegeben wird, ist das Hardwareangriffsprotokoll standardmäßig im Supportpaket enthalten.

- Im Supportpaket befindet sich das Hardwareangriffsprotokoll in der Datei `HWIntrusion.txt` im Stammordner. Nachfolgend finden Sie ein Beispiel des Hardwareangriffsprotokolls für das Azure Stack Edge Pro-Gerät. 

    ```
    09/04/2019 15:51:23 system Critical The chassis is open while the power is off.
    09/04/2019 15:51:30 system Ok The chassis is closed while the power is off.
    ```

## <a name="troubleshoot-device-upload-and-refresh-errors"></a>Behandeln von Problemen und Fehlern beim Geräteupload und Aktualisieren von Geräten

Alle Fehler, die während der Upload- und Aktualisierungsprozesse aufgetreten sind, sind in den entsprechenden Fehlerdateien enthalten.

1. Navigieren Sie zum Anzeigen der Fehlerdateien zu Ihrer Freigabe, und wählen Sie die Freigabe aus, um den Inhalt anzuzeigen. 


2. Wählen Sie den Ordner _Microsoft Data Box Edge_ aus. Dieser Ordner enthält zwei Unterordner:

    - Der Ordner „Upload“ enthält Protokolldateien für Uploadfehler.
    - Der Ordner „Refresh“ ist für Fehler bei der Aktualisierung vorgesehen.

    Hier sehen Sie eine exemplarische Protokolldatei für die Aktualisierung.

    ```
    <root container="test1" machine="VM15BS020663" timestamp="03/18/2019 00:11:10" />
    <file item="test.txt" local="False" remote="True" error="16001" />
    <summary runtime="00:00:00.0945320" errors="1" creates="2" deletes="0" insync="3" replaces="0" pending="9" />
    ``` 

3. Wenn diese Datei einen Fehler enthält (im Beispiel hervorgehoben), notieren Sie sich den Fehlercode (in diesem Fall: 16001). Suchen Sie anhand dieses Fehlercodes in der folgenden Fehlerreferenz nach der entsprechenden Fehlerbeschreibung.

    [!INCLUDE [data-box-edge-edge-upload-error-reference](../../includes/data-box-edge-gateway-upload-error-reference.md)]


## <a name="next-steps"></a>Nächste Schritte

- [Behandeln von Problemen bei der Geräteaktivierung](azure-stack-edge-gpu-troubleshoot-activation.md).
- [Problembehandlung für Azure Resource Manager.](azure-stack-edge-gpu-troubleshoot-azure-resource-manager.md)
- [Behandeln von Blob Storage-Problemen.](azure-stack-edge-gpu-troubleshoot-blob-storage.md)
- [Behandeln von Computeproblemen in IoT Edge](azure-stack-edge-gpu-troubleshoot-iot-edge.md).