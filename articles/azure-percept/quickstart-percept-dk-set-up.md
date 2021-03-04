---
title: Einrichten Ihres Azure Percept DK
description: Verbinden Ihres DevKits mit Azure und Bereitstellen Ihres ersten KI-Modells
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/15/2021
ms.custom: template-quickstart
ms.openlocfilehash: 3540204d66bb589c567514f92a9a8acb2159e343
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101664318"
---
# <a name="set-up-your-azure-percept-dk-and-deploy-your-first-ai-model"></a>Einrichten Ihres Azure Percept DK und Bereitstellen Ihres ersten KI-Modells

Erste Schritte mit Azure Percept DK und Azure Percept Studio mithilfe der Azure Percept DK-Einrichtungserfahrung, um Ihr Gerät mit Azure zu verbinden und Ihr erstes KI-Modell bereitzustellen. Nachdem Sie sich vergewissert haben, dass Ihr Azure-Konto mit Azure Percept Studio kompatibel ist, führen Sie die Einrichtung durch, um sicherzustellen, dass Ihr Azure Percept DK für das Erstellen von Proof-of Concepts mit Edge-KI konfiguriert ist.

Wenn während dieses Schnellstarts Probleme auftreten, finden Sie im Leitfaden zur [Problembehandlung](./troubleshoot-dev-kit.md) Informationen zu möglichen Lösungen.

## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure Percept DK.
- Ein Windows-, Linux- oder OS X-basierter Hostcomputer mit WLAN-Funktion und einem Webbrowser.
- Ein Azure-Konto mit einem aktiven Abonnement. [Kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Das Azure-Konto muss über die Rolle „Besitzer“ oder „Mitwirkender“ für das Abonnement verfügen. Weitere Informationen zu Azure-Rollendefinitionen

### <a name="prerequisite-check"></a>Prüfen der Voraussetzungen

Gehen Sie folgendermaßen vor, um zu überprüfen, ob Ihr Azure-Konto „Besitzer“ oder „Mitwirkender“ des Abonnements ist.

1. Wechseln Sie zum Azure-Portal, und melden Sie sich mit demselben Azure-Konto an, das Sie mit Azure Percept Studio verwenden werden. 

    > [!NOTE]
    > Wenn Sie über mehrere Azure-Konten verfügen, speichert Ihr Browser Anmeldeinformationen von einem anderen Konto möglicherweise zwischen. Weitere Informationen dazu, wie Sie sicherstellen können, dass Sie mit dem richtigen Konto angemeldet sind, finden Sie im Handbuch zur Problembehandlung.

1. Erweitern Sie das Hauptmenü in der oberen linken Ecke des Bildschirms, und klicken Sie auf „Abonnements“, oder wählen Sie im Menü der Symbole auf der Startseite „Abonnements“ aus. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-01-subscription.png" alt-text="supscription icon in Azure portal.":::
    --->
1. Wählen Sie Ihr Abonnement in der Liste aus. Wenn Ihr Abonnement nicht in der Liste angezeigt wird, stellen Sie sicher, dass Sie mit dem richtigen Azure-Konto angemeldet sind. 
    <!---
    :::image type="content" source="./media/quickstart-percept-dk-setup/prereq-02-sub-list.png" alt-text="supscription list in Azure portal.":::
    --->
Wenn Sie ein neues Abonnement erstellen möchten, befolgen Sie [diese Schritte](https://docs.microsoft.com/azure/cost-management-billing/manage/create-subscription).

1. Wählen Sie im Menü „Abonnement“ die Option „Zugriffssteuerung (IAM)“ aus.
1. Klicken Sie auf die Schaltfläche „Meinen Zugriff anzeigen“.
1. Überprüfen der Rolle
    - Wenn die Rolle „Leser“ angezeigt wird, oder wenn Sie eine Meldung erhalten, die besagt, dass Sie nicht über die Berechtigung zum Anzeigen von Rollen verfügen, müssen Sie den erforderlichen Prozess in Ihrer Organisation befolgen, damit die Rolle Ihres Kontos erhöht wird.
    - Wenn die Rolle als „Besitzer“ oder „Mitwirkender“ angezeigt wird, funktioniert Ihr Konto mit Azure Percept Studio. 

## <a name="launch-the-azure-percept-dk-setup-experience"></a>Starten der Azure Percept DK-Einrichtungserfahrung

<!---
> [!NOTE]
> Connecting over ethernet? See [this how-to guide](<link needed>) for detailed instructions.
--->
1. Verbinden Sie Ihren Hostcomputer direkt mit dem WLAN-Zugriffspunkt des DevKits. Dies erfolgt wie das Herstellen einer Verbindung mit einem beliebigen anderen WLAN.
    - **Netzwerkname**: scz-xxxx (wobei „xxxx“ die letzten vier Ziffern der MAC-Netzwerkadresse des DevKits sind)
    - **Kennwort**: Sie finden es auf der Begrüßungskarte, die im Lieferumfang des DevKits enthalten war.

    > [!WARNING]
    > Während Sie eine Verbindung mit dem WLAN-Zugriffspunkt des Azure Percept DK herstellen, verliert Ihr Hostcomputer vorübergehend seine Internetverbindung. Aktive Videokonferenzanrufe, Webstreaming oder andere netzwerkbasierte Erfahrungen werden unterbrochen, bis Schritt 3 der Azure Percept DK-Einrichtungserfahrung abgeschlossen ist.

1. Nachdem die Verbindung mit dem WLAN-Zugriffspunkt des DevKits hergestellt wurde, startet das Hostgerät automatisch die Azure Percept DK-Einrichtungserfahrung in einem neuen Browserfenster. Wenn sie nicht automatisch geöffnet wird, können Sie sie manuell starten, indem Sie ein Browserfenster öffnen und zu [http://10.1.1.1](http://10.1.1.1) navigieren. 

1. Nachdem Sie nun die Azure Percept-Einrichtungserfahrung gestartet haben, sind Sie bereit die Einrichtungserfahrung fortsetzen. 

    > [!NOTE]
    > Der Webdienst der Azure Percept DK-Einrichtungserfahrung wird nach 30 Minuten der Nichtverwendung und nach Beendigung der Einrichtungserfahrung heruntergefahren. In diesem Fall empfiehlt es sich, das DevKit neu zu starten, um Konnektivitätsprobleme mit dem WLAN-Zugriffspunkt des DevKits zu vermeiden.

## <a name="complete-the-azure-percept-dk-setup-experience"></a>Abschließen der Azure Percept DK-Einrichtungserfahrung

1. Erste Schritte: Klicken Sie auf dem Begrüßungsbildschirm auf **Weiter**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-01-welcome.png" alt-text="Begrüßungsseite.":::

1. Herstellen einer WLAN-Verbindung: Klicken Sie auf der Seite „Netzwerkverbindung“ auf **Mit neuem WLAN-Netzwerk verbinden**, um Ihr DevKit mit Ihrem WLAN-Netzwerk zu verbinden.

    Wenn Sie dieses DevKit schon einmal mit Ihrem WLAN verbunden haben oder bereits mit der Azure Percept DK-Einrichtungserfahrung über Ihr WLAN-Netzwerk verbunden sind, klicken Sie auf den Link **Überspringen**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-02-connect-to-wi-fi.png" alt-text="Herstellen einer WLAN-Verbindung.":::

1. Wählen Sie Ihr WLAN-Netzwerk aus den verfügbaren Verbindungen aus, und verbinden Sie sich damit. (Erfordert Ihr lokales WLAN-Kennwort.)

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-03-select-wi-fi.png" alt-text="WLAN-Netzwerk auswählen."::: 

1. Kopieren Ihrer IP-Adresse: Nachdem Ihr DevKit erfolgreich eine Verbindung mit dem Netzwerk Ihrer Wahl hergestellt hat, notieren Sie sich die **IPv4-Adresse**, die auf der Seite angezeigt wird. **Sie benötigen diese IP-Adresse später in dieser Schnellstartanleitung**. 

    > [!NOTE]
    > Die IP-Adresse kann sich nach jedem Gerätestart ändern.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-04-success-wi-fi.png" alt-text="Kopieren der IP-Adresse.":::

1. Lesen und Akzeptieren des Lizenzvertrags: Lesen Sie den Lizenzvertrag, und wählen Sie **Ich habe den Lizenzvertrag gelesen und stimme ihm zu** aus (Sie müssen bis zum Ende des Vertrags scrollen), und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-05-eula.png" alt-text="Akzeptieren des Lizenzvertrags.":::

1. Erstellen Ihres SSH-Anmeldekontos: Richten Sie SSH für den Remotezugriff auf Ihr DevKit ein. Erstellen Sie einen SSH-Benutzernamen und ein Kennwort. 

    > [!NOTE]
    > SSH (Secure Shell) ist ein Netzwerkprotokoll, das für die sichere Kommunikation zwischen dem Hostgerät und dem DevKit verwendet wird. Weitere Informationen zu SSH finden Sie [in diesem Artikel](https://en.wikipedia.org/wiki/SSH_(Secure_Shell)).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-06-ssh-login.png" alt-text="Erstellen eines SSH-Kontos."::: 

1. Starten des DevKit-Verbindungsprozesses: Klicken Sie auf dem nächsten Bildschirm auf **Mit neuem Gerät verbinden**, um mit dem Herstellen der Verbindung Ihres DevKits mit Azure IoT Hub zu beginnen. 

    <!---
    Connecting with an existing IoT Edge device connection string? See this [how-to guide](<link needed>) for reference.
    --->
    :::image type="content" source="./media/quickstart-percept-dk-setup/main-07-connect-device.png" alt-text="Verbinden mit Azure."::: 

1. Kopieren des Gerätecodes: Klicken Sie auf den Link **Kopieren**, um Ihren Gerätecode zu kopieren. Klicken Sie dann auf **Bei Azure anmelden**. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-08-copy-code.png" alt-text="Kopieren des Gerätecodes."::: 

1. Einfügen des Gerätecodes: Eine neue Browserregisterkarte wird mit einem Fenster geöffnet, in dem der kopierte Gerätecode angefordert wird. Fügen Sie den Code in das Fenster ein, und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-09-enter-code.png" alt-text="Eingeben des Gerätecodes."::: 

1. Anmelden bei Azure: Im nächsten Fenster müssen Sie sich mit dem Azure-Konto anmelden, das Sie zu Beginn des Schnellstarts überprüft haben. Geben Sie diese Anmeldeinformationen ein, und klicken Sie auf **Weiter**. 

    > [!NOTE]
    > Es kann sein, dass Ihr Browser andere Anmeldeinformationen automatisch zwischenspeichert. Stellen Sie sicher, dass Sie sich mit dem richtigen Konto anmelden.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-10-azure-sign-in.png" alt-text="Anmelden bei Azure.":::
 
1. An dieser Stelle nicht die Browserregisterkarte der Einrichtungserfahrung schließen: Nachdem Sie sich angemeldet haben, wird ein Bildschirm angezeigt, der bestätigt, dass Sie sich angemeldet haben. Obwohl er besagt, dass Sie das Fenster schließen können, **empfehlen wir, dass Sie keine Fenster schließen**. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-11-sign-in-success.png" alt-text="Erfolgreiche Anmeldung."::: 

1. Kehren Sie zur Browserregisterkarte zurück, die die Einrichtungserfahrung hostet.
1. Auswählen Ihrer IoT Hub-Option
    - Wenn Sie bereits über einen IoT Hub verfügen, der auch auf dieser Seite aufgeführt wird, können Sie ihn auswählen und **zu Schritt 17 wechseln**.
    - Wenn Sie keinen IoT Hub besitzen oder einen neuen erstellen möchten, wechseln Sie zum Ende der Liste, und klicken Sie auf **Neuen Azure IoT Hub erstellen**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-13-iot-hub-select.png" alt-text="Auswählen eines IoT Hubs."::: 

1. Erstellen Ihres neuen IoT Hubs: Füllen Sie alle Felder auf dieser Seite aus. 
    - Wählen Sie das Azure-Abonnement aus, das Sie mit Azure Percept Studio verwenden werden.
    - Wählen Sie eine vorhandene Ressourcengruppe aus. Wenn keine vorhanden ist, klicken Sie auf „Neue erstellen“, und befolgen Sie die Aufforderungen. 
    - Wählen Sie die Azure-Region aus. 
    - Benennen Sie Ihren neuen IoT Hub.
    - Wählen Sie den Tarif aus (entspricht normalerweise dem Abonnement).

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-14-create-iot-hub.png" alt-text="Erstellen eines neuen IoT Hubs."::: 

1. Warten auf die Bereitstellung des IoT Hubs: Dies kann ein einige Minuten dauern.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-15-iot-hub-deploy.png" alt-text="Bereitstellen des IoT Hubs."::: 

1. Registrieren Ihres DevKits: Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf die Schaltfläche **Registrieren**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-16-iot-hub-success.png" alt-text="IoT Hub erfolgreich bereitgestellt."::: 

1. Benennen Ihres DevKits: Geben Sie einen Gerätenamen für Ihr DevKit ein, und klicken Sie auf **Weiter**.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-17-device-name.png" alt-text="Benennen des Geräts."::: 

1. Warten auf den Download der KI-Standardmodelle: Dieser Vorgang dauert einige Minuten.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-18-finalize.png" alt-text="Abschließen der Einrichtung."::: 

1. Vision KI in Aktion sehen: Ihr DevKit wurde erfolgreich mit Ihrem Azure IoT Hub verknüpft und hat das Standard-Objekterkennungsmodell von Vision KI empfangen. Die Azure Percept-Vision-Kamera kann nun ohne eine Cloudverbindung Objekterkennungsrückschlüsse ziehen. 

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-2-preview-video-output.png" alt-text="Auf „Vorschau der Videoausgabe anzeigen“ klicken."::: 
       
    > [!NOTE]
    > Der Onboardingprozess und die Verbindung mit dem WLAN-Zugriffspunkt des Geräts auf Ihren Hostcomputer werden an dieser Stelle heruntergefahren, aber Ihr DevKit bleibt mit dem Internet verbunden.   Sie können die Onboardingerfahrung mit einem Neustart des DevKits neu starten, was es Ihnen ermöglicht, das Onboarding nochmals zu durchlaufen, um das Gerät mit einem anderen IoT Hub erneut zu verbinden, der demselben oder einem anderen Azure-Abonnement zugeordnet ist.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-19-0-warning.png" alt-text="Warnung wegen Trennung der Verbindung der Einrichtungserfahrung."::: 

1. Fortfahren mit dem Azure-Portal: Wechseln Sie zurück zum Fenster mit der Einrichtungserfahrung, und klicken Sie auf die Schaltfläche **Zum Azure-Portal wechseln**, um mit dem Erstellen Ihrer benutzerdefinierten KI-Modelle in Azure Percept Studio zu beginnen.

    > [!NOTE]
    > Vergewissern Sie sich, dass Ihr Hostcomputer in Ihren WLAN-Einstellungen nicht mehr mit dem DevKit-Zugriffspunkt verbunden ist und nun wieder mit Ihrem lokalen WLAN verbunden ist.

    :::image type="content" source="./media/quickstart-percept-dk-setup/main-20-azure-portal-continue.png" alt-text="Wechseln zu Azure Percept Studio."::: 

## <a name="view-your-device-in-the-azure-percept-studio-and-deploy-common-prebuilt-sample-apps"></a>Anzeigen Ihres Geräts in Azure Percept Studio und Bereitstellen gängiger vorkonfigurierter Beispiel-Apps

1. Zeigen Sie Ihre Liste der „Geräte“ auf der Seite „Übersicht über Azure Percept“ an. Die Seite „Übersicht über Azure Percept“ ist Ihr Ausgangspunkt für den Zugriff auf viele verschiedene Workflows, sowohl für die erste als auch für fortgeschrittenere Entwicklung von erweiterten KI-Edge-Modellen und -Lösungen.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-01-get-device-list.png" alt-text="Anzeigen Ihrer Geräteliste.":::
    
1. Vergewissern Sie sich, dass das soeben von Ihnen erstellte Gerät verbunden ist, und klicken Sie darauf.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-02-select-device.png" alt-text="Wählen Sie Ihr Gerät aus.":::

1. Zeigen Sie Ihren Gerätestream an, um zu sehen, was Ihre DevKit-Kamera anzeigt. Ein Standard-Objekterkennungsmodell wird vorgefertigt bereitgestellt und erkennt eine Reihe gängiger Objekte.

    > [!NOTE]
    > Wenn Sie dies zum ersten Mal auf einem neuen Gerät durchführen, erhalten Sie in der oberen rechten Ecke eine Benachrichtigung, dass ein neues Modul bereitgestellt wird.  Sobald dieser Vorgang abgeschlossen ist, können Sie das Fenster mit dem Kameravideostream starten. 
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-1-start-video-stream.png" alt-text="Anzeigen Ihres Videostreams.":::
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-03-2-object-detection.png" alt-text="Objekterkennung anzeigen.":::

1. Erkunden Sie vorgefertigte KI-Beispielmodelle.   Azure Percept Studio verfügt über eine Reihe allgemeiner, vorgefertigter Beispiele, die mit einem einzigen Mausklick bereitgestellt werden können.   Wählen Sie „Beispielmodell bereitstellen“ aus, um diese anzuzeigen und bereitzustellen.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-04-explore-prebuilt.png" alt-text="Erkunden vorgefertigter Modelle.":::
    
1. Stellen Sie ein neues vorgefertigtes Beispiel auf Ihrem verbundenen Gerät bereit. Wählen Sie ein Beispiel aus der Bibliothek aus, und klicken Sie auf „Auf Gerät bereitstellen“.

    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-1-select-prebuilt.png" alt-text="Auswählen eines vorgefertigten Beispiels.":::
    
    :::image type="content" source="./media/quickstart-percept-dk-setup/portal-05-2-select-journey.png" alt-text="Objekterkennung in Aktion sehen.":::

## <a name="next-steps"></a>Nächste Schritte

Sie können einem ähnlichen Ablauf folgen, um [vorgefertigte Sprachmodelle](./tutorial-no-code-speech.md) auszuprobieren.