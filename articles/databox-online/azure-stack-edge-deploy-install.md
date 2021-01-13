---
title: 'Installationstutorial: Auspacken, Montieren und Verkabeln des physischen Azure Stack Edge Pro-Geräts | Microsoft-Dokumentation'
description: Das zweite Tutorial zur Installation von Azure Stack Edge Pro enthält Informationen zum Auspacken, zur Rackmontage und Verkabelung des physischen Geräts.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/17/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Azure Stack Edge Pro in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: 9aa02521d91d41380b1bdac3efe50ab3d196a856
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "90894208"
---
# <a name="tutorial-install-azure-stack-edge-pro"></a>Tutorial: Installieren von Azure Stack Edge Pro

In diesem Tutorial erfahren Sie, wie Sie ein physisches Azure Stack Edge Pro-Gerät installieren. Die Installation umfasst das Auspacken, die Rackmontage und die Verkabelung des Geräts. 

Die Installation dauert voraussichtlich etwa zwei Stunden.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

## <a name="prerequisites"></a>Voraussetzungen

Für die Installation eines physischen Geräts wird Folgendes vorausgesetzt:

### <a name="for-the-azure-stack-edge-resource"></a>Für die Azure Stack Edge-Ressource

Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie haben alle Schritte des Tutorials [Vorbereiten der Bereitstellung von Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md) ausgeführt.
    * Sie haben eine Azure Stack Edge-Ressource für die Bereitstellung Ihres Geräts erstellt.
    * Sie haben den Aktivierungsschlüssel zum Aktivieren Ihres Geräts mit der Azure Stack Edge-Ressource erstellt.

 
### <a name="for-the-azure-stack-edge-pro-physical-device"></a>Für das physische Azure Stack Edge Pro-Gerät

Führen Sie folgende Schritte aus, bevor Sie ein Gerät bereitstellen:

- Achten Sie darauf, dass das Gerät sicher auf einer flachen, stabilen und ebenen Arbeitsfläche liegt.
- Vergewissern Sie sich, dass der geplante Aufstellungsort über Folgendes verfügt:
    - Standardmäßige unabhängige Wechselstromversorgung

        ODER
    - Rack-PDU (Power Distribution Unit) mit unterbrechungsfreier Stromversorgung (USV)
    - Freier Einschub mit 1 HE in dem Rack, in das Sie das Gerät einbauen möchten

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter

Vorbereitungen

- Lesen Sie die Netzwerkanforderungen für die Bereitstellung eines Azure Stack Edge Pro-Geräts, und konfigurieren Sie das Netzwerk des Rechenzentrums gemäß den Anforderungen. Weitere Informationen finden Sie im Abschnitt mit den [Azure Stack Edge Pro-Netzwerkanforderungen](azure-stack-edge-system-requirements.md#networking-port-requirements).

- Vergewissern Sie sich, dass die Internetbandbreite mindestens 20 MBit/s beträgt, damit das Gerät optimal funktioniert.


## <a name="unpack-the-device"></a>Auspacken des Geräts

Dieses Gerät wird in einem einzelnen Karton ausgeliefert. Führen Sie die folgenden Schritte aus, um das Gerät auszupacken. 

1. Legen Sie den Karton auf einen flachen, ebenen Untergrund.
2. Überprüfen Sie den Karton und das Verpackungsmaterial auf Risse, Schnitte, Wasserschäden oder anderweitige offensichtliche Beschädigungen. Sollte der Karton oder die Verpackung stark beschädigt sein, öffnen Sie den Karton bzw. die Verpackung nicht. Wenden Sie sich an den Microsoft-Support, um zu ermitteln, ob das Gerät funktionstüchtig ist.
3. Packen Sie den Karton aus. Stellen Sie nach dem Auspacken des Kartons sicher, dass Folgendes vorhanden ist:
    - Ein Azure Stack Edge Pro-Gerät (einzelnes Gehäuse)
    - Zwei Netzkabel
    - Eine Schienenbaugruppe
    - Ein Booklet mit Informationen zu Sicherheit, Umwelt und rechtlichen Aspekten

Sollten Sie nicht alle oben aufgeführten Teile erhalten haben, wenden Sie sich an den Azure Stack Edge Pro-Support. Im nächsten Schritt wird das Gerät in ein Rack eingebaut.


## <a name="rack-the-device"></a>Montieren des Geräts in einem Rack

Das Gerät muss in einem standardmäßigen 19-Zoll-Rack installiert werden. Gehen Sie wie folgt vor, um Ihr Gerät in einem 19-Zoll-Standardrack zu montieren.

> [!IMPORTANT]
> Azure Stack Edge Pro-Geräte müssen für den ordnungsgemäßen Betrieb in ein Rack eingebaut werden.


### <a name="prerequisites"></a>Voraussetzungen

- Machen Sie sich anhand des Booklets mit Informationen zu Sicherheit, Umwelt und rechtlichen Aspekten zunächst mit den Sicherheitsanweisungen vertraut. Dieses Booklet liegt dem Gerät bei.
- Installieren Sie die Schienen möglichst weit unten im Rack am dafür vorgesehenen Ort.
- Für die Schienenmontage mit Werkzeug benötigen Sie Folgendes:
    -  Acht Schrauben (#10-32, #12-24, #M5 oder #M6). Der Durchmesser des Schraubenkopfs muss weniger als 10 mm (0,4") betragen.
    -  Einen Schlitzschraubendreher.

### <a name="identify-the-rail-kit-contents"></a>Überprüfen des Inhalts des Schienensatzes

Für die Installation der Schienenbaugruppe benötigen Sie folgende Komponenten:
1. Zwei Gleitschienen vom Typ „A7 Dell ReadyRails II“
2. Zwei Klettbänder

    ![Überprüfen des Inhalts des Schienensatzes](./media/azure-stack-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Installieren und Entfernen der Schienen ohne Werkzeug (Racks mit Vierkant- oder Rundlöchern)

> [!TIP]
> Bei dieser Option können die Schienen unter Verwendung der gewindelosen Vierkant- oder Rundlöcher des Racks ohne Werkzeug installiert und entfernt werden.

1. Positionieren Sie das mit **FRONT** beschriftete Ende der linken und rechten Schiene an der Innenseite, und richten Sie es jeweils an den vorderseitigen Löchern der vertikalen Rackflansche aus.
2. Richten Sie die Endstücke jeweils an den unteren und oberen Löchern der gewünschten Position im Rack aus.
3. Schieben Sie das hintere Ende der Schiene in Richtung des vertikalen Rackflanschs, bis es einrastet. Wiederholen Sie die gleiche Prozedur, um das vordere Endstück am vertikalen Rackflansch anzubringen.
4. Wenn Sie die Schienen entfernen möchten, betätigen Sie jeweils die Entriegelung in der Mitte des Endstücks, und entnehmen Sie die Schiene.

    ![Installieren und Entfernen der Schienen ohne Werkzeug](./media/azure-stack-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Installieren und Entfernen der Schienen mit Werkzeug (Racks mit Gewindelöchern)

> [!TIP]
> Bei dieser Option wird ein Schlitzschraubendreher_benötigt_, um die Schienen unter Verwendung der runden Gewindelöcher des Racks zu installieren und zu entfernen.

1. Entfernen Sie die Stifte an der vorderen und hinteren Befestigungsklammer mithilfe eines Schlitzschraubendrehers.
2. Ziehen und drehen Sie die Schienenarretierungen, um sie von den Befestigungsklammern zu entfernen.
3. Befestigen Sie die linke und die rechte Schiene mit jeweils zwei Schrauben an den vorderen vertikalen Rackflanschen.
4. Schieben Sie die linke und die rechte hintere Klammer von hinten gegen die hinteren vertikalen Rackflansche, und befestigen Sie sie ebenfalls mit jeweils zwei Schrauben.

    ![Installieren und Entfernen der Schienen mit Werkzeug](./media/azure-stack-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Installieren des Systems in einem Rack

1. Ziehen Sie die inneren Gleitschienen aus dem Rack, bis sie einrasten.
2. Platzieren Sie den hintere Schienenabstandhalter auf beiden Seiten des Systems von oben in den hinteren J-förmigen Schlitzen der Schienen. Drehen Sie das System nach unten, bis alle Schienenabstandhalter in den J-förmigen Schlitzen platziert sind.
3. Drücken Sie das System nach innen, bis die Arretierhebel einrasten.
4. Drücken Sie auf den Schienenentriegelungsmechanismus an beiden Schienen, und schieben Sie das System in das Rack.

    ![Installieren des Systems in einem Rack](./media/azure-stack-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Entfernen des Systems aus dem Rack

1. Suchen Sie nach den Arretierhebeln an der Seite der inneren Schienen.
2. Entriegeln Sie den Mechanismus, indem Sie die Hebel jeweils nach oben in die geöffnete Position drehen.
3. Halten Sie beide Seiten des Systems gut fest, und ziehen Sie das System nach vorn, bis sich die Schienenabstandhalter an der Vorderseite der J-förmigen Schlitze befinden. Heben Sie das System nach oben aus dem Rack heraus, und legen Sie es auf einer ebenen Fläche ab.

    ![Entfernen des Systems aus dem Rack](./media/azure-stack-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Verwenden des Schnappmechanismus (Einrasten und Entriegeln)

> [!NOTE]
> Bei Systemen ohne Schnappmechanismus muss das System wie in Schritt 3 beschrieben mit Schrauben befestigt werden.

1. Suchen Sie an der Vorderseite auf beiden Seiten des Systems nach dem Schnappmechanismus.
2. Der Mechanismus rastet automatisch ein, wenn das System in das Rack geschoben wird. Zur Entriegelung müssen die beiden Riegel nach oben gezogen werden.
3. Falls Sie das Rack transportieren möchten, während sich das System darin befindet, oder es in einer instabilen Umgebung verwenden möchten, können Sie die unter den beiden Riegeln befindlichen Befestigungsschrauben mit einem passenden Kreuzschlitzschraubendreher (#2) anziehen.

    ![Verwenden des Schnappmechanismus (Einrasten und Lösen)](./media/azure-stack-edge-deploy-install/engaging-releasing-slam-latch.png)


## <a name="cable-the-device"></a>Verkabeln des Geräts

Verlegen Sie die Kabel, und verkabeln Sie Ihr Gerät. In diesem Abschnitt erfahren Sie, wie Sie Ihr Azure Stack Edge Pro-Gerät an die Stromversorgung und an das Netzwerk anschließen.

Bevor Sie mit der Verkabelung Ihres Geräts beginnen, benötigen Sie Folgendes:

- Das physische Azure Stack Edge Pro-Gerät (ausgepackt und im Rack montiert)
- Zwei Netzkabel
- Mindestens ein Netzwerkkabel (1 GbE, RJ45) zum Anschließen an die Verwaltungsschnittstelle. Das Gerät verfügt über zwei 1-GbE-Netzwerkschnittstellen: eine für die Verwaltung und eine für Daten.
- Ein SFP+-Kupferkabel (25 GbE) für jede zu konfigurierende Datennetzwerkschnittstelle. Mindestens eine Datennetzwerkschnittstelle („PORT 2“, „PORT 3“, „PORT 4“, „PORT 5“ oder PORT 6“) muss mit dem Internet verbunden sein und über Azure-Konnektivität verfügen.  
- Zugang zu zwei PDUs (Power Distribution Units) (empfohlen)

> [!NOTE]
> - Wenn Sie nur eine einzelne Datennetzwerkschnittstelle nutzen, sollten Sie eine 25/10-GbE-Netzwerkschnittstelle wie „PORT 3“, „PORT 4“, „PORT 5“ oder „PORT 6“ verwenden, um Daten an Azure zu senden. 
> - Aus Leistungsgründen und für die Verarbeitung großer Datenmengen empfiehlt es sich, alle Datenports zu verwenden.
> - Das Azure Stack Edge Pro-Gerät sollte mit dem Rechenzentrumsnetzwerk verbunden sein, um Daten von Datenquellservern erfassen zu können.

Auf dem Azure Stack Edge Pro-Gerät:

- Auf der Vorderseite befinden sich Laufwerke und ein Netzschalter.

    - Es stehen zehn Datenträgerslots zur Verfügung.
    - In Slot 0 befindet sich ein SATA-Laufwerk mit 240 GB, das als Betriebssystem-Datenträger fungiert. Slot 1 ist leer, und die Slots 2 bis 9 enthalten NVMe-SSDs, die als reguläre Datenträger fungieren.
- Auf der Rückseite befinden sich redundante Netzteile (Power Supply Units, PSUs).
- Auf der Rückseite stehen sechs Netzwerkschnittstellen zur Verfügung:

    - Zwei Schnittstellen mit jeweils 1 GBit/s
    - Vier Schnittstellen mit jeweils 25 GBit/s, die auch als Schnittstellen mit 10 GBit/s fungieren können
    - Ein Baseboard-Verwaltungscontroller (Baseboard Management Controller, BMC)

- Auf der Rückseite befinden sich zwei Netzwerkkarten für die sechs Ports:

    - QLogic FastLinQ 41264
    - QLogic FastLinQ 41262

Eine vollständige Liste der unterstützten Kabel, Switches und Transceiver für diese Netzwerkkarten finden Sie in der [Interoperabilitätsmatrix für die Cavium FastlinQ 41000-Reihe](https://www.marvell.com/documents/xalflardzafh32cfvi0z/).
 
Gehen Sie wie folgt vor, um Ihr Gerät an die Stromversorgung und an das Netzwerk anzuschließen:

1. Machen Sie sich mit den verschiedenen Anschlüssen auf der Geräterückseite vertraut.

    ![Rückseite eines verkabelten Geräts](./media/azure-stack-edge-deploy-install/backplane-cabled.png)

2. Machen Sie die Datenträgerslots und den Netzschalter auf der Vorderseite des Geräts ausfindig.

    ![Vorderseite eines Geräts](./media/azure-stack-edge-deploy-install/device-front-plane-labeled-1.png)

3. Schließen Sie die Netzkabel an die PSUs des Gehäuses an. Zur Gewährleistung hoher Verfügbarkeit müssen beide PSUs installiert und an unterschiedliche Stromquellen angeschlossen werden.
4. Schließen Sie die Netzkabel an die PDUs des Racks an. Stellen Sie sicher, dass die beiden PSUs separate Stromquellen verwenden.
5. Drücken Sie den Netzschalter, um das Gerät einzuschalten.
6. Verbinden Sie die 1-GbE-Netzwerkschnittstelle „PORT 1“ mit dem Computer, der zum Konfigurieren des physischen Geräts verwendet wird. „PORT 1“ ist die dedizierte Verwaltungsschnittstelle.
7. Verbinden Sie mindestens einen der Ports 2, 3, 4, 5 oder 6 mit dem Datencenternetzwerk/Internet.

    - Wenn Sie sich für „PORT 2“ entscheiden, verwenden Sie das RJ45-Netzwerkkabel.
    - Verwenden Sie für die 10/25-GbE-Netzwerkschnittstellen die SFP+-Kupferkabel.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurden unter anderem folgende Azure Stack Edge Pro-Themen behandelt:

> [!div class="checklist"]
> * Auspacken des Geräts
> * Montieren des Geräts in einem Rack
> * Verkabeln des Geräts

Im nächsten Tutorial erfahren Sie, wie Sie Ihr Gerät verbinden, einrichten und aktivieren.

> [!div class="nextstepaction"]
> [Verbinden und Einrichten von Azure Stack Edge Pro](./azure-stack-edge-deploy-connect-setup-activate.md)
