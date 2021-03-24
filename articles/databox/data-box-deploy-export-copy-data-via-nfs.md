---
title: Tutorial zum Kopieren von Daten aus Azure Data Box über NFS | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Daten über NFS aus Ihrer Azure Data Box kopieren.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 12/18/2020
ms.author: alkohli
ms.openlocfilehash: 64bb5e94c4b18626d1f85d7e61252aae74202eb9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97680616"
---
# <a name="tutorial-copy-data-from-azure-data-box-via-nfs"></a>Tutorial: Kopieren von Daten aus Azure Data Box über NFS

In diesem Tutorial wird beschrieben, wie Sie über NFS eine Verbindung mit der lokalen Webbenutzeroberfläche Ihrer Data Box herstellen und Daten von dieser auf einen lokalen Datenserver kopieren. Die Daten auf Ihrer Data Box werden aus Ihrem Azure Storage-Konto exportiert.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Herstellen einer Verbindung mit der Data Box
> * Kopieren von Daten aus Data Box

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie Folgendes sicher, bevor Sie beginnen:

1. Sie haben die Bestellung für Azure Data Box aufgegeben.
    - Weitere Informationen zu Importbestellungen finden Sie unter [Tutorial: Bestellen von Azure Data Box](data-box-deploy-ordered.md).
    - Weitere Informationen zu Exportbestellungen finden Sie unter [Tutorial: Bestellen von Azure Data Box](data-box-deploy-export-ordered.md).
2. Sie haben Ihre Data Box erhalten, und die Bestellung wird im Portal mit dem Status **Übermittelt** angezeigt.
3. Sie verfügen über einen Hostcomputer, auf den Sie die Daten von Ihrer Data Box kopieren möchten. Für Ihren Hostcomputer müssen die folgenden Bedingungen erfüllt sein:
   * Es muss ein [unterstütztes Betriebssystem](data-box-system-requirements.md) ausgeführt werden.
   * Er muss mit einem Hochgeschwindigkeitsnetzwerk verbunden sein. Mindestens eine 10-GbE-Verbindung wird dringend empfohlen. Falls keine 10-GbE-Verbindung verfügbar ist, verwenden Sie eine 1-GbE-Datenverbindung, die Geschwindigkeit der Kopiervorgänge wird dadurch jedoch beeinträchtigt.

## <a name="connect-to-data-box"></a>Herstellen einer Verbindung mit der Data Box

[!INCLUDE [data-box-shares](../../includes/data-box-shares.md)]

Wenn Sie einen Linux-Hostcomputer verwenden, führen Sie die folgenden Schritte aus, um Data Box zum Zulassen des Zugriffs auf NFS-Clients zu konfigurieren. Data Box kann gleichzeitig so viele Verbindungen wie fünf NFS-Clients herstellen.

1. Geben Sie die IP-Adressen der zulässigen Clients an, die auf die Freigabe zugreifen können:

    1.  Wechseln Sie auf der lokalen Webbenutzeroberfläche zur Seite **Verbindung herstellen und Daten kopieren**. Klicken Sie unter **NFS-Einstellungen** auf **NFS-Clientzugriff**. 

        ![Öffnen des NFS-Clientzugriffs](media/data-box-deploy-export-copy-data/nfs-client-access-1.png)

    1. Geben Sie die IP-Adresse des Clients an, und klicken Sie auf **Hinzufügen**, um einen NFS-Client hinzuzufügen. Data Box kann gleichzeitig so viele Verbindungen wie fünf NFS-Clients herstellen. Klicken Sie abschließend auf **OK**.

         ![Hinzufügen eines NFS-Clients](media/data-box-deploy-export-copy-data/nfs-client-access-2.png)

2. Stellen Sie sicher, dass auf dem Linux-Hostcomputer eine [unterstützte Version](data-box-system-requirements.md) des NFS-Clients installiert ist. Verwenden Sie die jeweilige Version für Ihre Linux-Distribution. 

3. Führen Sie nach der Installation des NFS-Clients den folgenden Befehl aus, um die NFS-Freigabe auf Ihrem Data Box-Gerät einzubinden:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    Das folgende Beispiel zeigt, wie Sie über NFS eine Verbindung mit einer Data Box-Freigabe herstellen. Die IP-Adresse des Data Box-Geräts lautet `10.161.23.130`, die Freigabe `Mystoracct_Blob` wird auf der Ubuntu-VM eingebunden, und der Bereitstellungspunkt ist `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`
    
    Für Mac-Clients müssen Sie eine zusätzliche Option wie folgt hinzufügen: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

    **Erstellen Sie immer einen Ordner für die Dateien, die Sie unter die Freigabe kopieren möchten, und kopieren Sie die Dateien dann in diesen Ordner**. Der Ordner, der unter der Blockblob- und der Seitenblob Freigabe erstellt wurde, entspricht einem Container, in den Daten als Blobs hochgeladen werden. Es ist nicht möglich, Dateien direkt in den *root*-Ordner im Speicherkonto zu kopieren.

## <a name="copy-data-from-data-box"></a>Kopieren von Daten aus Data Box

Nachdem Sie eine Verbindung mit den Data Box-Freigaben hergestellt haben, kopieren Sie im nächsten Schritt Ihre Daten.

[!INCLUDE [data-box-export-review-logs](../../includes/data-box-export-review-logs.md)]

 Sie können jetzt mit dem Kopieren der Daten beginnen. Wenn Sie einen Linux-Hostcomputer verwenden, verwenden Sie ein Kopierhilfsprogramm wie Robocopy. Einige der verfügbaren Alternativen in Linux sind [`rsync`](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) oder [Ultracopier](https://ultracopier.first-world.info/).  

Der `cp`-Befehl ist eine der besten Optionen zum Kopieren eines Verzeichnisses. Weitere Informationen zur Verwendung finden Sie auf den [Handbuchseiten zum cp-Befehl](http://man7.org/linux/man-pages/man1/cp.1.html).

Befolgen Sie die nachstehenden Richtlinien, wenn Sie die `rsync`-Option für einen Multithread-Kopiervorgang verwenden:

* Installieren Sie je nach Dateisystem, das Ihr Linux-Client verwendet, das **CIFS Utils**- oder **NFS Utils**-Paket.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

* Installieren Sie `rsync` und **Parallel** (variiert abhängig von der verteilten Linux-Version).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

* Erstellen Sie einen Bereitstellungspunkt.

    `sudo mkdir /mnt/databox`

* Binden Sie das Volume ein.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

* Spiegeln Sie die Verzeichnisstruktur des Ordners.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

* Kopieren Sie Dateien.

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     Hierbei gibt „j“ die Anzahl von Parallelisierungen und „X“ die Anzahl paralleler Kopien an.

     Wir empfehlen, mit 16 parallelen Kopien zu beginnen und die Anzahl von Threads je nach verfügbaren Ressourcen zu erhöhen.

> [!IMPORTANT]
> Folgende Linux-Dateitypen werden nicht unterstützt: symbolische Verknüpfungen, Zeichendateien, Blockdateien, Sockets und Pipes. Diese Dateitypen führen zu Fehlern bei der **Versandvorbereitung**.

Navigieren Sie nach Abschluss des Kopiervorgangs zum **Dashboard**, und überprüfen Sie den belegten Speicherplatz und den freien Speicherplatz auf Ihrem Gerät.

Sie können nun mit dem Versenden Ihrer Data Box an Microsoft fortfahren.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zu Azure Data Box-Themen erhalten, darunter die folgenden:

> [!div class="checklist"]
>
> * Voraussetzungen
> * Herstellen einer Verbindung mit der Data Box
> * Kopieren von Daten aus Data Box

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Ihre Data Box zurück an Microsoft senden.

> [!div class="nextstepaction"]
> [Zurücksenden der Azure Data Box an Microsoft](./data-box-deploy-export-picked-up.md)
