---
title: Herstellen einer seriellen Verbindung mit Ihrem Azure Percept DK
description: Erfahren Sie, wie Sie mit PuTTY und einem seriellen USB-zu-TTL-Kabel eine serielle Verbindung mit Ihrem Azure Percept DK einrichten.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/03/2021
ms.custom: template-how-to
ms.openlocfilehash: 93b8ab0ce53202402e86b059abe3c600590d549e
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660368"
---
# <a name="connect-to-your-azure-percept-dk-over-serial"></a>Herstellen einer seriellen Verbindung mit Ihrem Azure Percept DK

Führen Sie die folgenden Schritte aus, um über [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)eine serielle Verbindung mit Ihrem Azure Percept DK einzurichten.

> [!WARNING]
> Versuchen Sie **NICHT**, Ihr DevKit über eine serielle Verbindung zu verbinden, es sei denn, ein schwerwiegender Fehler liegt vor (z. B. wenn Sie Ihr Gerät gebrickt haben). Das Zerlegen des Trägerplatinengehäuses zum Anschließen des seriellen Kabels ist sehr schwierig und führt zur Beschädigung der WLAN-Antennenkabel.

## <a name="prerequisites"></a>Voraussetzungen

- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Host-PC
- Azure Percept DK
- [Serielles USB-zu-TTL-Kabel](https://www.adafruit.com/product/954)

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/usb-serial-cable.png" alt-text="Serielles USB-zu-TTL-Kabel.":::

## <a name="initiate-the-serial-connection"></a>Initiieren der seriellen Verbindung

1. Wenn Ihre Trägerplatine an eine 80/20-Schiene angebracht, entfernen Sie sie mit dem Inbusschlüssel (in der DevKit-Begrüßungskarte enthalten) von der Schiene.

1. Entfernen Sie die Schrauben an der Unterseite des Trägerplatinengehäuses, und ziehen Sie die Hauptplatine heraus.

    > [!WARNING]
    > Beim Entfernen der Hauptplatine werden die WLAN-Antennenkabel beschädigt. Führen Sie die serielle Verbindung **NICHT** durch, es sei denn, es ist die letzte Möglichkeit, Ihr Gerät wiederherzustellen.

1. Entfernen Sie den Kühlkörper.

1. Entfernen Sie den Rangierverteiler von den GPIO-Stiften.

    > [!TIP]
    > Beachten Sie die Ausrichtung des Rangierverteilers, bevor Sie ihn entfernen. Zeichnen Sie als Referenz z. B. einen Pfeil auf oder kleben Sie einen Aufkleber auf den Rangierverteiler, der in Richtung der Schaltung zeigt. Der Rangierverteiler ist nicht formschlüssig und kann beim Zusammenbau der Trägerplatine versehentlich verkehrt herum angeschlossen werden.

1. Verbinden Sie wie unten gezeigt das [serielle USB-zu-TTL-Kabel](https://www.adafruit.com/product/954) mit den GPIO-Stiften auf der Hauptplatine. Beachten Sie, dass das rote Kabel nicht verbunden wird.

    - Verbinden Sie das schwarze Kabel (GND) mit Stift 6.
    - Verbinden Sie das weiße Kabel (RX) mit Stift 8.
    - Verbinden Sie das grüne Kabel (TX) mit Stift 10.

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/serial-connection-carrier-board.png" alt-text="Serielle Stiftverbindungen auf der Trägerplatine.":::

1. Schalten Sie Ihr DevKit ein, und verbinden Sie das USB-Ende des seriellen Kabels mit Ihrem PC.

1. Rufen Sie unter Windows **Start** -> **Windows Update-Einstellungen** -> **Optionale Updates anzeigen** -> **Treiberupdates** auf. Suchen Sie in der Liste nach einem Update für „Seriell zu USB“, aktivieren Sie das Kontrollkästchen neben dem Update, und klicken Sie auf **Herunterladen und installieren**.  

1. Öffnen Sie als nächstes den Windows-Geräte-Manager (**Start** -> **Geräte-Manager**). Wechseln Sie zu **Ports**, und klicken Sie auf **USB zu UART**, um die **Eigenschaften** zu öffnen. Beachten Sie, mit welchem COM-Port Ihr Gerät verbunden ist.

1. Klicken Sie auf die Registerkarte **Porteinstellungen**. Stellen Sie sicher, dass **Bits pro Sekunde** auf 115200 eingestellt ist.

1. Öffnen Sie PuTTY. Geben Sie Folgendes ein, und klicken Sie auf **Öffnen**, um eine serielle Verbindung mit Ihrem DevKit herzustellen:

    1. Serielle Leitung: COM[port #]
    1. Geschwindigkeit: 115200
    1. Verbindungstyp: Seriell

    :::image type="content" source="./media/how-to-connect-to-percept-dk-over-serial/putty-serial-session.png" alt-text="PuTTY-Sitzungsfenster mit ausgewählten seriellen Parametern.":::

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Aktualisieren eines nicht startbaren Geräts mit dem [seriellen USB-zu-TTL-Kabel](https://www.adafruit.com/product/954) finden Sie im USB-Updatehandbuch für nicht standardmäßige Situationen.

[comment]: # (Fügen Sie den Link zum USB-Updatehandbuch hinzu, falls verfügbar.)