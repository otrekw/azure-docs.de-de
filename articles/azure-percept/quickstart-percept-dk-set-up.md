---
title: Einrichten Ihres Azure Percept DK
description: Verbinden Ihres DevKits mit Azure und Bereitstellen Ihres ersten KI-Modells
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 03/17/2021
ms.custom: template-quickstart
ms.openlocfilehash: 7056027965bd5e0f41c07835d21b454ab772a55a
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108132249"
---
# <a name="set-up-your-azure-percept-dk-and-deploy-your-first-ai-model"></a>Einrichten Ihres Azure Percept DK und Bereitstellen Ihres ersten KI-Modells

Durchlaufen Sie die Einrichtungserfahrung für das Azure Percept DK, um Ihr Development Kit zu konfigurieren und Ihr erstes KI-Modell bereitzustellen. Nachdem Sie sich vergewissert haben, dass Ihr Azure-Konto mit Azure Percept kompatibel ist, führen Sie die folgenden Schritte aus:

- Herstellen einer WLAN-Verbindung für Ihr Development Kit
- Einrichten einer SSH-Anmeldung für Remotezugriff auf Ihr Development Kit
- Erstellen einer neuen IoT Hub-Instanz für Azure Percept
- Verbinden Ihres Development Kits mit Ihrer IoT Hub-Instanz und Ihrem Azure-Konto

Sollten im Rahmen dieses Prozesses Probleme auftreten, finden Sie im [Leitfaden zur Problembehandlung](./how-to-troubleshoot-setup.md) Informationen zu möglichen Lösungen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Percept DK (Development Kit).
- Ein Windows-, Linux- oder OS X-basierter Hostcomputer mit WLAN und einem Webbrowser.
- Ein Azure-Konto mit einem aktiven Abonnement. [Kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Das Azure-Konto muss innerhalb des Abonnements über die Rolle **Besitzer** oder **Mitwirkender** verfügen. Führen Sie die im Anschluss angegebenen Schritte aus, um die Rolle Ihres Azure-Kontos zu prüfen. Weitere Informationen zu Azure-Rollendefinitionen finden Sie in der [Dokumentation zur rollenbasierten Zugriffssteuerung in Azure](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles).

    > [!CAUTION]
    > Wenn Sie über mehrere Azure-Konten verfügen, speichert Ihr Browser Anmeldeinformationen von einem anderen Konto möglicherweise zwischen. Zur besseren Übersichtlichkeit empfiehlt es sich, vor Beginn der Einrichtungserfahrung alle nicht verwendeten Browserfenster zu schließen und sich beim [Azure-Portal](https://portal.azure.com/) anzumelden. Weitere Informationen dazu, wie Sie sicherstellen können, dass Sie mit dem richtigen Konto angemeldet sind, finden Sie im [Leitfaden zur Problembehandlung](./how-to-troubleshoot-setup.md).

### <a name="check-your-azure-account-role"></a>Überprüfen der Rolle Ihres Azure-Kontos

Gehen Sie wie folgt vor, um sich zu vergewissern, dass Ihr Azure-Konto innerhalb des Abonnements über die Rolle „Besitzer“ oder „Mitwirkender“ verfügt:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) mit dem Azure-Konto an, das Sie mit Azure Percept verwenden möchten.

1. Klicken Sie auf das Symbol **Abonnements** (gelber Schlüssel).

1. Wählen Sie Ihr Abonnement in der Liste aus. Sollte Ihr Abonnement nicht angezeigt werden, vergewissern Sie sich, dass Sie mit dem richtigen Azure-Konto angemeldet sind. Wenn Sie ein neues Abonnement erstellen möchten, befolgen Sie [diese Schritte](../cost-management-billing/manage/create-subscription.md).

1. Wählen Sie im Menü „Abonnement“ die Option **Zugriffssteuerung (IAM)** aus.
1. Klicken Sie auf **View my access** (Meinen Zugriff anzeigen).
1. Überprüfen Sie die Rolle:
    - Wenn Ihre Rolle **Leser** lautet oder eine Meldung mit dem Hinweis angezeigt wird, dass Sie nicht zum Anzeigen von Rollen berechtigt sind, muss die Rolle Ihres Kontos erhöht werden. Verwenden Sie hierzu den in Ihrer Organisation vorgesehenen Prozess.
    - Wenn Ihre Rolle **Besitzer** oder **Mitwirkender** lautet, kann Ihr Konto mit Azure Percept verwendet werden, und Sie können mit der Einrichtungserfahrung fortfahren.

## <a name="launch-the-azure-percept-dk-setup-experience"></a>Starten der Azure Percept DK-Einrichtungserfahrung

1. Verbinden Sie Ihren Hostcomputer direkt mit dem WLAN-Zugriffspunkt des Development Kits. Öffnen Sie dazu genau wie bei anderen WLAN-Verbindungen die Netzwerk- und Interneteinstellungen auf Ihrem Computer, klicken Sie auf das folgende Netzwerk, und geben Sie das Netzwerkkennwort ein, wenn Sie dazu aufgefordert werden:

    - **Netzwerkname:** Der Name des WLAN-Zugriffspunkts lautet abhängig von der Betriebssystemversion Ihres Development Kits entweder **scz-xxxx** oder **apd-xxxx** (wobei „xxxx“ für die letzten vier Ziffern der MAC-Adresse des Dev Kits steht).
    - **Kennwort:** Das Kennwort finden Sie auf der Begrüßungskarte, die im Lieferumfang des Development Kits enthalten war.

    > [!WARNING]
    > Während Sie eine Verbindung mit dem WLAN-Zugriffspunkt des Azure Percept DK herstellen, verliert Ihr Hostcomputer vorübergehend seine Internetverbindung. Aktive Videokonferenzanrufe, Webstreaming oder andere netzwerkbasierte Erfahrungen werden unterbrochen.

1. Nachdem die Verbindung mit dem WLAN-Zugriffspunkt des Development Kits hergestellt wurde, startet der Hostcomputer die Einrichtungserfahrung automatisch in einem neuen Browserfenster mit **your.new.device/** in der Adressleiste. Sollte die Registerkarte nicht automatisch geöffnet werden, starten Sie die Einrichtungserfahrung, indem Sie zu [http://10.1.1.1](http://10.1.1.1) navigieren. Vergewissern Sie sich, dass Ihr Browser mit den Azure-Kontoanmeldeinformationen angemeldet ist, die Sie mit Azure Percept verwenden möchten.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-01-welcome.png" alt-text="Begrüßungsseite.":::

    > [!CAUTION]
    > Der Webdienst der Einrichtungserfahrung wird nach 30-minütiger Inaktivität heruntergefahren. Starten Sie in diesem Fall das Development Kit neu, um Konnektivitätsprobleme mit dem WLAN-Zugriffspunkt des Development Kits zu vermeiden.

## <a name="complete-the-azure-percept-dk-setup-experience"></a>Abschließen der Azure Percept DK-Einrichtungserfahrung

1. Klicken Sie auf dem Bildschirm **Willkommen** auf **Weiter**.

1. Klicken Sie auf der Seite **Netzwerkverbindung** auf **Verbindung mit neuem WLAN herstellen**.

    Falls Sie Ihr Development Kit bereits mit Ihrem WLAN verbunden haben, klicken Sie auf **Überspringen**.

1. Wählen Sie in der Liste mit den verfügbaren Netzwerken Ihr WLAN aus, und klicken Sie auf **Verbinden**. Geben Sie Ihr Netzwerkkennwort ein, wenn Sie dazu aufgefordert werden.

1. Wenn das Development Kit erfolgreich mit Ihrem Netzwerk verbunden wurde, wird auf der Seite die IPv4-Adresse angezeigt, die Ihrem Development Kit zugewiesen ist. **Notieren Sie sich die auf der Seite angezeigte IPv4-Adresse.** Die IP-Adresse wird benötigt, wenn Sie eine SSH-Verbindung mit Ihrem Development Kit herstellen möchten, um Probleme zu behandeln oder Geräteupdates durchzuführen.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-04-success-wi-fi.png" alt-text="Kopieren der IP-Adresse.":::

    > [!NOTE]
    > Die IP-Adresse kann sich nach jedem Gerätestart ändern.

1. Lesen Sie den Lizenzvertrag, und wählen Sie **Ich habe den Lizenzvertrag gelesen und stimme ihm zu** aus. (Sie müssen bis zum Ende des Vertrags scrollen.) Klicken Sie anschließend auf **Weiter**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-05-eula.png" alt-text="Akzeptieren des Lizenzvertrags.":::

1. Geben Sie einen SSH-Kontonamen und ein Kennwort ein, und **notieren Sie sich Ihre Anmeldeinformationen für später**.

    > [!NOTE]
    > SSH (Secure Shell) ist ein Netzwerkprotokoll, das es Ihnen ermöglicht, über einen Hostcomputer eine Remoteverbindung mit dem Development Kit herzustellen.

1. Klicken Sie auf der nächsten Seite auf **Als neues Gerät einrichten**, um in Ihrem Azure-Konto ein neues Gerät zu erstellen.

1. Klicken Sie auf **Kopieren**, um Ihren Gerätecode zu kopieren. Klicken Sie anschließend auf **Bei Azure anmelden**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-08-copy-code.png" alt-text="Kopieren des Gerätecodes.":::

1. Daraufhin wird eine neue Browserregisterkarte mit einem Fenster für die **Codeeingabe** geöffnet. Fügen Sie den Code in das Fenster ein, und klicken Sie auf **Weiter**. Lassen Sie die Registerkarte **Willkommen** mit der Einrichtungserfahrung GEÖFFNET.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-09-enter-code.png" alt-text="Eingeben des Gerätecodes.":::

1. Melden Sie sich unter Verwendung der Azure-Kontoanmeldeinformationen für Ihr Development Kit bei Azure Percept an. Lassen Sie die Browserregisterkarte nach Abschluss des Vorgangs geöffnet.

    > [!CAUTION]
    > Es kann sein, dass Ihr Browser andere Anmeldeinformationen automatisch zwischenspeichert. Stellen Sie sicher, dass Sie sich mit dem richtigen Konto anmelden.

    Nachdem Sie sich erfolgreich bei Azure Percept auf dem Gerät angemeldet haben, kehren Sie zur Registerkarte **Willkommen** zurück, um die Einrichtung fortzusetzen.

1. Sollte auf der Registerkarte **Willkommen** die Seite **Gerät Ihrer Azure IoT Hub-Instanz zuweisen** angezeigt werden, führen Sie eine der folgenden Aktionen aus:

    - Falls Sie bereits über eine IoT Hub-Instanz verfügen, die Sie mit Azure Percept verwenden möchten, und diese Instanz auf dieser Seite aufgeführt ist, wählen Sie sie aus, und fahren Sie mit Schritt 15 fort.
    - Falls Sie über keine IoT Hub-Instanz verfügen oder eine neue Instanz erstellen möchten, klicken Sie auf **Neue Azure IoT Hub-Instanz erstellen**.

    > [!IMPORTANT]
    > Falls Sie über eine IoT Hub-Instanz verfügen, diese aber nicht in der Liste aufgeführt ist, haben Sie sich möglicherweise mit den falschen Anmeldeinformationen bei Azure Percept angemeldet. Weitere Informationen finden Sie im [Leitfaden zur Problembehandlung](./how-to-troubleshoot-setup.md).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-13-iot-hub-select.png" alt-text="Auswählen eines IoT Hubs.":::

1. Wenn Sie eine neue IoT Hub-Instanz erstellen möchten, füllen Sie die folgenden Felder aus:

    - Wählen Sie das Azure-Abonnement aus, das Sie mit Azure Percept verwenden möchten.
    - Wählen Sie eine vorhandene Ressourcengruppe aus. Sollte keine vorhanden sein, klicken Sie auf **Neu erstellen**, und folgen Sie den Anweisungen.
    - Wählen Sie die Azure-Region aus, die Ihrem physischen Standort am nächsten ist.
    - Benennen Sie Ihre neue IoT Hub-Instanz.
    - Wählen Sie den Tarif „S1“ (Standard) aus.

    > [!NOTE]
    > Wenn Sie für Ihre Edge-KI-Anwendungen einen höheren [Nachrichtendurchsatz](../iot-hub/iot-hub-scaling.md#message-throughput) benötigen, können Sie Ihre IoT Hub-Instanz im Azure-Portal jederzeit [auf einen höheren Standard-Tarif upgraden](../iot-hub/iot-hub-upgrade.md). In den Tarifen B und F wird Azure Percept NICHT unterstützt.

1. Die IoT Hub-Bereitstellung kann mehrere Minuten dauern. Klicken Sie nach Abschluss der Bereitstellung auf **Registrieren**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-16-iot-hub-success.png" alt-text="IoT Hub erfolgreich bereitgestellt.":::

1. Geben Sie einen Gerätenamen für Ihr Development Kit ein, und klicken Sie auf **Weiter**.

1. Warten Sie, bis die Gerätemodule heruntergeladen wurden. Dieser Vorgang dauert einige Minuten.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-18-finalize.png" alt-text="Abschließen der Einrichtung.":::

1. Wenn die Seite **Gerätesetup abgeschlossen!** angezeigt wird, wurde ihr Development Kit erfolgreich mit Ihrer IoT Hub-Instanz verknüpft und die erforderliche Software heruntergeladen. Die Verbindung mit den WLAN-Zugriffspunkt wird von Ihrem Development Kit automatisch getrennt. Dies hat zwei Benachrichtigungen zur Folge:

    <!---
    > [!NOTE]
    > The onboarding process and connection to the device Wifi access to your host computer shuts down at this point, but your dev kit will stay connected to the internet.   You can restart the onboarding experience with a dev kit reboot, which will allow you to go back through the onboarding and reconnect the device to a different IOT hub associated with the same or a different Azure Subscription..
    --->

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-0-warning.png" alt-text="Warnung wegen Trennung der Verbindung der Einrichtungserfahrung.":::

1. Verbinden Sie Ihren Hostcomputer mit dem WLAN, mit dem Ihr Development Kit in Schritt 2 eine Verbindung hergestellt hat.

1. Klicken Sie auf **Zum Azure-Portal wechseln**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-20-azure-portal-continue.png" alt-text="Wechseln zu Azure Percept Studio.":::

## <a name="view-your-dev-kit-video-stream-and-deploy-a-sample-model"></a>Anzeigen des Videostreams Ihres Development Kits und Bereitstellen eines Beispielmodells

1. Auf der [Übersichtsseite von Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) stehen viele verschiedene Workflows für Einsteiger und erfahrene Entwickler von Edge-KI-Lösungen zur Verfügung. Klicken Sie zunächst im Menü auf der linken Seite auf **Geräte**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-01-get-device-list.png" alt-text="Anzeigen Ihrer Geräteliste.":::

1. Vergewissern Sie sich, dass Ihr Development Kit als **Verbunden** aufgeführt ist, und klicken Sie darauf, um die Geräteseite anzuzeigen.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-02-select-device.png" alt-text="Wählen Sie Ihr Gerät aus.":::

1. Klicken Sie auf **View your device stream** (Gerätedatenstrom anzeigen). Falls Sie den Videostream Ihres Geräts zum ersten Mal anzeigen, werden Sie rechts oben in einer Benachrichtigung darauf hingewiesen, dass ein neues Modell bereitgestellt wird. Dies kann einige Minuten dauern.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-1-start-video-stream.png" alt-text="Anzeigen Ihres Videostreams.":::

    Nach Abschluss der Modellbereitstellung erhalten Sie eine weitere Benachrichtigung mit dem Link **Stream anzeigen**. Klicken Sie auf den Link, um den Videostream Ihrer Azure Percept Vision-Kamera in einem neuen Browserfenster anzuzeigen. Das Development Kit verfügt über ein im Voraus geladenes KI-Modell zur automatischen Erkennung zahlreicher gängiger Objekte.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-2-object-detection.png" alt-text="Objekterkennung anzeigen.":::

1. Azure Percept Studio verfügt auch über eine Reihe von KI-Beispielmodellen. Kehren Sie zum Bereitstellen eines Beispielmodells für das Development Kit zur Seite Ihres Geräts zurück, und klicken Sie auf **Beispielmodell bereitstellen**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-04-explore-prebuilt.png" alt-text="Erkunden vorgefertigter Modelle.":::

1. Wählen Sie in der Bibliothek ein Beispielmodell aus, und klicken Sie auf **Auf Gerät bereitstellen**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-2-select-journey.png" alt-text="Objekterkennung in Aktion sehen.":::

1. Nach erfolgreicher Modellbereitstellung wird rechts oben auf dem Bildschirm eine Benachrichtigung mit dem Link **Stream anzeigen** angezeigt. Klicken Sie auf den Link in der Benachrichtigung, oder kehren Sie zur Geräteseite zurück, und klicken Sie auf **Gerätestream anzeigen**, um Modellrückschlüsse in Aktion zu erleben. Alle zuvor auf dem Development Kit ausgeführten Modelle werden nun durch das neue Modell ersetzt.

## <a name="video-walkthrough"></a>Exemplarische Vorgehensweise per Video

Eine visuelle exemplarische Vorgehensweise für die oben beschriebenen Schritte finden Sie im folgenden Video. Die Einrichtungserfahrung beginnt bei 0:50:

</br>

> [!VIDEO https://www.youtube.com/embed/-dmcE2aQkDE]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Vision-Lösung ohne Code](./tutorial-nocode-vision.md)