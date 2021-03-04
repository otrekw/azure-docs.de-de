---
title: Erstellen eines Sprach-Assistenten mit Azure Percept DK und Azure Percept-Audio
description: Es wird beschrieben, wie Sie ohne Code eine Sprachlösung erstellen und für Ihr Azure Percept DK bereitstellen.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/17/2021
ms.custom: template-how-to
ms.openlocfilehash: de85c4f8cdcd9781345ee1488549aab23e38ec5c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678074"
---
# <a name="create-a-voice-assistant-with-azure-percept-dk-and-azure-percept-audio"></a>Erstellen eines Sprach-Assistenten mit Azure Percept DK und Azure Percept-Audio

In diesem Tutorial erstellen Sie einen Sprach-Assistenten aus einer Vorlage, den Sie für Ihr Azure Percept DK und für Azure Percept-Audio verwenden können. Die Demo für den Sprach-Assistenten wird in [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819) ausgeführt und enthält eine Auswahl virtueller Objekte, die per Sprache gesteuert werden. Sagen Sie Ihr Schlüsselwort, um ein Objekt zu steuern. Beim Schlüsselwort handelt es sich um ein Wort oder einen kurzen Ausdruck, mit dem Sie Ihr Gerät aufwecken, gefolgt von einem Befehl. Jede Vorlage reagiert auf verschiedene spezifische Befehle.

In diesem Leitfaden wird Schritt für Schritt der Prozess zum Einrichten Ihrer Geräte, Erstellen eines Sprach-Assistenten und der benötigten Ressourcen für [Speech-Dienste](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview), Testen des Sprach-Assistenten, Konfigurieren Ihres Schlüsselworts und Erstellen von benutzerdefinierten Schlüsselwörtern beschrieben.

## <a name="prerequisites"></a>Voraussetzungen

- Azure Percept DK (DevKit)
- Azure Percept-Audio
- Lautsprecher oder Kopfhörer (optional)
- [Azure-Abonnement](https://azure.microsoft.com/free/)
- [Azure Percept DK-Setup](./quickstart-percept-dk-set-up.md): Sie haben Ihr DevKit mit einem WLAN verbunden, eine IoT Hub-Instanz erstellt und das DevKit mit der IoT Hub-Instanz verbunden.

## <a name="device-setup"></a>Geräteeinrichtung

1. (Optional) Schließen Sie Ihre Lautsprecher oder Kopfhörer an der Kopfhörerbuchse des Audio-SoM an. Dieser Anschluss hat die Bezeichnung „Line Out“. Dies ermöglicht es Ihnen, die Audioantworten Ihres Sprach-Assistenten zu hören. Wenn Sie keine Lautsprecher oder Kopfhörer anschließen, können Sie die Antworten aber trotzdem verfolgen, da sie im Demofenster auch in Textform angezeigt werden.

1. Schließen Sie das Audio-SoM mit dem beigefügten USB-A-zu-Micro-B-Kabel an das Board Ihres DevKit an.

1. Schalten Sie das DevKit ein.

    - Die LED L01 des Audio-SoM leuchtet nun grün und gibt damit an, dass das Gerät eingeschaltet wurde.
    - Die LED L02 blinkt grün und gibt damit an, dass das Audio-SoM authentifiziert wird.

1. Warten Sie, bis der Authentifizierungsvorgang abgeschlossen ist. Dies kann bis zu drei Minuten dauern.

1. Fahren Sie mit dem nächsten Abschnitt fort, wenn einer der folgenden Fälle eintritt:

    - LED L01 ist aus, und LED L02 leuchtet weiß. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit noch nicht mit einem Schlüsselwort konfiguriert wurde.
    - Alle drei LEDs leuchten blau. Dadurch wird angegeben, dass die Authentifizierung abgeschlossen ist und das DevKit mit einem Schlüsselwort konfiguriert wurde.

    > [!NOTE]
    > Wenden Sie sich an den Support, wenn das DevKit nicht authentifiziert wird.

## <a name="create-a-voice-assistant-using-an-available-template"></a>Erstellen eines Sprach-Assistenten mit einer verfügbaren Vorlage

1. Navigieren Sie zu [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).

1. Öffnen Sie die Registerkarte **Demos und Tutorials**.

    :::image type="content" source="./media/tutorial-no-code-speech/portal-overview.png" alt-text="Screenshot der Startseite des Azure-Portals":::

1. Klicken Sie unter **Sprachtutorials und -demos** auf **Sprach-Assistenten-Vorlagen ausprobieren**. Auf der rechten Seite des Bildschirms wird ein Fenster geöffnet.

1. Gehen Sie im Fenster wie folgt vor:

    1. Wählen Sie im Dropdownmenü **IoT Hub** den IoT-Hub aus, mit dem Ihr DevKit verbunden ist.

    1. Wählen Sie im Dropdownmenü **Gerät** Ihr DevKit aus.

    1. Wählen Sie eine der verfügbaren Sprach-Assistenten-Vorlagen aus.

    1. Klicken Sie auf das Kontrollkästchen **Ich stimme den Bestimmungen für dieses Projekt zu**.

    1. Klicken Sie auf **Erstellen**.

    :::image type="content" source="./media/tutorial-no-code-speech/template-creation.png" alt-text="Screenshot der Erstellung einer Vorlage für den Sprach-Assistenten":::

1. Nach dem Klicken auf **Erstellen** wird im Portal ein neues Fenster geöffnet, in dem Sie Ihre Sprachdesignressource erstellen können. Gehen Sie im Fenster wie folgt vor:

    1. Wählen Sie im Feld **Abonnement** Ihr Azure-Abonnement aus.

    1. Wählen Sie im Dropdownmenü **Ressourcengruppe** Ihre bevorzugte Ressourcengruppe aus. Klicken Sie unter dem Dropdownmenü auf **Erstellen**, und befolgen Sie die Aufforderungen, wenn Sie eine neue Ressourcengruppe für Ihren Sprach-Assistenten erstellen möchten.

    1. Geben Sie unter **Anwendungspräfix** einen Namen ein. Dies ist das Präfix für Ihr Projekt und den Namen Ihres benutzerdefinierten Befehls.

    1. Wählen Sie unter **Region** die Region aus, in der Ressourcen bereitgestellt werden sollen.

    1. Wählen Sie unter **Tarif für LUIS-Vorhersagen** die Option **Standard** aus (im Free-Tarif werden Sprachanforderungen nicht unterstützt).

    1. Klicken Sie auf die Schaltfläche **Erstellen**. Ressourcen für die Sprach-Assistenten-Anwendung werden unter Ihrem Abonnement bereitgestellt.

        > [!WARNING]
        > Schließen Sie das Fenster **NICHT**, bevor die Bereitstellung der Ressource im Portal abgeschlossen ist. Das vorzeitige Schließen des Fensters kann für den Sprach-Assistenten zu unerwartetem Verhalten führen. Nachdem Ihre Ressource bereitgestellt wurde, wird die Demo angezeigt.

    :::image type="content" source="./media/tutorial-no-code-speech/resource-group.png" alt-text="Screenshot des Fensters zur Auswahl von Abonnement und Ressourcengruppe":::

## <a name="test-out-your-voice-assistant"></a>Testen Ihres Sprach-Assistenten

Sagen Sie zum Interagieren mit Ihrem Sprach-Assistenten das Schlüsselwort, und nennen Sie dann einen Befehl. Wenn das akustische SoM Ihr Schlüsselwort erkennt, gibt das Gerät einen Glockenton aus (den Sie hören, wenn Lautsprecher oder Kopfhörer angeschlossen sind), und die LEDs blinken blau. Die LEDs blinken schnell nacheinander blau auf, während Ihr Befehl verarbeitet wird. Die Antwort des Sprach-Assistenten auf Ihren Befehl wird in Textform im Demofenster und als Ton über Ihre Lautsprecher bzw. Kopfhörer ausgegeben. Als Standardschlüsselwort (neben **Benutzerdefiniertes Schlüsselwort**) ist „Computer“ festgelegt. Jede Vorlage verfügt über verschiedene kompatible Befehle, mit denen Sie mit virtuellen Objekten im Demofenster interagieren können. Wenn Sie beispielsweise die Demo für das Hotel- und Gaststättengewerbe oder das Gesundheitswesen verwenden möchten, sagen Sie „Computer, turn on TV“ (Computer, Fernseher einschalten), um den virtuellen Fernseher einzuschalten.

:::image type="content" source="./media/tutorial-no-code-speech/hospitality-demo.png" alt-text="Screenshot des Fensters mit der Demo für das Hotel- und Gaststättengewerbe":::

### <a name="hospitality-and-healthcare-demo-commands"></a>Demo für Hotel- und Gaststättengewerbe und Gesundheitswesen: Befehle

Die Demos für „Gesundheitswesen“ und für „Hotel- und Gaststättengewerbe“ verfügen jeweils über virtuelle Fernseher, Beleuchtung, Jalousien und Thermostate, mit denen Sie interagieren können. Die folgenden Befehle (und zusätzliche Varianten) werden unterstützt:

* „Turn on/off the lights.“ (Licht ein-/ausschalten.)
* „Turn on/off the TV.“ (Fernseher ein-/ausschalten.)
* „Turn on/off the AC.“ (Klimaanlage ein-/ausschalten.)
* „Open/close the blinds.“ (Jalousien öffnen/schließen.)
* „Set temperature to X degrees.“ (Temperatur auf X Grad einstellen.) (X ist die gewünschte Temperatur, z. B. 75 Grad Fahrenheit/24 Grad Celsius.)

:::image type="content" source="./media/tutorial-no-code-speech/healthcare-demo.png" alt-text="Screenshot des Fensters mit der Demo für das Gesundheitswesen":::

### <a name="automotive-demo-commands"></a>Demo für Automobilindustrie: Befehle

Die Demo für „Automobilindustrie“ umfasst eine Sitzheizung, eine Scheibenheizung und einen Thermostat als virtuelle Elemente, mit denen Sie interagieren können. Die folgenden Befehle (und zusätzliche Varianten) werden unterstützt:

* „Turn on/off the defroster.“ (Scheibenheizung ein-/ausschalten.)
* „Turn on/off the seat warmer.“ (Sitzheizung ein-/ausschalten.)
* „Set temperature to X degrees.“ (Temperatur auf X Grad einstellen.) (X ist die gewünschte Temperatur, z. B. 75 Grad Fahrenheit/24 Grad Celsius.)
* „Increase/decrease the temperature by Y degrees.“ (Temperatur um Y Grad erhöhen/verringern.)

:::image type="content" source="./media/tutorial-no-code-speech/auto-demo.png" alt-text="Screenshot des Fensters mit der Demo für die Automobilindustrie":::

### <a name="inventory-demo-commands"></a>Demo für Bestandsverwaltung: Befehle

Die Demo für die Bestandsverwaltung verfügt über verschiedene virtuelle Kisten in blauer, gelber und grüner Farbe, mit denen Sie über eine App für die virtuelle Bestandsverwaltung interagieren können. Die folgenden Befehle (und zusätzliche Varianten) werden unterstützt:

* „Add/remove X boxes.“ (X Kisten hinzufügen/entfernen.) (X ist die Anzahl von Kisten, z. B. 4.)
* „Order/ship X boxes.“ (X Kisten bestellen/versenden.)
* „How many boxes are in stock?“ (Wie viele Kisten sind auf Lager?)
* „Count Y boxes.“ (Y Kisten zählen.) (Y ist die Farbe der Kisten, z. B. Gelb.)
* „Ship everything in stock.“ (Gesamten Bestand versenden.)

:::image type="content" source="./media/tutorial-no-code-speech/inventory-demo.png" alt-text="Screenshot des Fensters mit der Demo für die Bestandsverwaltung":::

## <a name="configure-your-keyword"></a>Konfigurieren des Schlüsselworts

Klicken Sie zum Ändern Ihres Schlüsselworts im Demofenster neben **Benutzerdefiniertes Schlüsselwort** auf **Ändern**. Wählen Sie eins der verfügbaren Schlüsselwörter aus, und klicken Sie auf **Speichern**. Sie können eine Auswahl aus den vordefinierten Schlüsselwörtern und den von Ihnen erstellten benutzerdefinierten Schlüsselwörtern treffen.

:::image type="content" source="./media/tutorial-no-code-speech/change-keyword.png" alt-text="Screenshot der Auswahl verfügbarer Schlüsselwörter":::

### <a name="create-a-custom-keyword"></a>Erstellen eines benutzerdefinierten Schlüsselworts

Klicken Sie zum Erstellen eines benutzerdefinierten Schlüsselworts oben im Demofenster auf **+ Benutzerdefiniertes Schlüsselwort erstellen**. Geben Sie Ihr gewünschtes Schlüsselwort ein. Hierbei kann es sich um ein einzelnes Wort oder um eine kurze Wortfolge handeln. Wählen Sie Ihre **Sprachressource** aus (wird im Demofenster neben **Benutzerdefinierter Befehl** angezeigt und enthält Ihr Anwendungspräfix), und klicken Sie auf **Speichern**. Das Training für Ihr benutzerdefiniertes Schlüsselwort dauert ggf. nur wenige Sekunden.

:::image type="content" source="./media/tutorial-no-code-speech/custom-keyword.png" alt-text="Screenshot des Fensters zum Erstellen eines benutzerdefinierten Schlüsselworts":::

## <a name="create-a-custom-command"></a>Erstellen eines benutzerdefinierten Befehls

Das Portal enthält auch Funktionen zum Erstellen von benutzerdefinierten Befehlen mit vorhandenen Sprachressourcen. Der Begriff „Benutzerdefinierter Befehl“ bezieht sich auf den Sprach-Assistenten selbst, und nicht auf einen bestimmten Befehl in der vorhandenen Anwendung. Mit der Erstellung eines benutzerdefinierten Befehls erstellen Sie auch ein neues Sprachprojekt, das Sie dann in [Speech Studio](https://speech.microsoft.com/) weiterentwickeln müssen.

Klicken Sie zum Erstellen eines neuen benutzerdefinierten Befehls im Demofenster oben auf der Seite auf **+ Benutzerdefinierten Befehl erstellen**, und gehen Sie dann wie folgt vor:

1. Geben Sie einen Namen für Ihren benutzerdefinierten Befehl ein.

1. Geben Sie eine Beschreibung Ihres Projekts ein (optional).

1. Wählen Sie Ihre bevorzugte Sprache aus.

1. Wählen Sie Ihre Sprachressource aus.

1. Wählen Sie Ihre LUIS-Ressource aus.

1. Wählen Sie Ihre LUIS-Erstellungsressource aus, oder erstellen Sie eine neue.

1. Klicken Sie auf **Erstellen**.

:::image type="content" source="./media/tutorial-no-code-speech/custom-commands.png" alt-text="Screenshot des Fensters für die Erstellung benutzerdefinierter Befehle":::

Nachdem Sie einen benutzerdefinierten Befehl erstellt haben, müssen Sie für die weitere Entwicklung zu [Speech Studio](https://speech.microsoft.com/) wechseln. Führen Sie die folgenden Schritte aus, falls Sie Speech Studio öffnen und Ihr benutzerdefinierter Befehl nicht angezeigt wird:

1. Klicken Sie in Azure Percept Studio im linken Bereich unter **KI-Projekte** auf **Sprache**.

1. Wählen Sie die Registerkarte **Befehle** aus.

    :::image type="content" source="./media/tutorial-no-code-speech/ai-projects.png" alt-text="Screenshot der Liste mit benutzerdefinierten Befehlen für die Bearbeitung":::

1. Wählen Sie den benutzerdefinierten Befehl aus, den Sie weiterentwickeln möchten. Das Projekt wird in Speech Studio geöffnet.

    :::image type="content" source="./media/tutorial-no-code-speech/speech-studio.png" alt-text="Screenshot der Startseite von Speech Studio":::

Weitere Informationen zur Entwicklung von benutzerdefinierten Befehlen finden Sie in der [Dokumentation zum Speech-Dienst](https://docs.microsoft.com/azure/cognitive-services/speech-service/custom-commands).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="voice-assistant-was-created-but-does-not-respond-to-commands"></a>Sprach-Assistent wurde erstellt, reagiert aber nicht auf Befehle

Überprüfen Sie die LED-Leuchten am Audio-SoM:

* Mit drei dauerhaft blau leuchtenden LEDs wird angezeigt, dass der Sprach-Assistent bereit ist und auf das Schlüsselwort wartet.
* Wenn die mittlere LED (L02) weiß leuchtet, wurde die Initialisierung vom DevKit abgeschlossen, und die Konfiguration mit einem Schlüsselwort ist erforderlich.
* Mit einer beliebigen Kombination aus grün leuchtenden LEDs wird angegeben, dass die Initialisierung für das Audio-SoM noch nicht abgeschlossen wurde. Der Initialisierungsvorgang kann einige Minuten dauern.

Weitere Informationen zu den LED-Anzeigen des Audio-SoM finden Sie im Artikel zu den LEDs.

### <a name="voice-assistant-does-not-respond-to-a-custom-keyword-created-in-speech-studio"></a>Sprach-Assistent reagiert nicht auf ein benutzerdefiniertes Schlüsselwort, das in Speech Studio erstellt wurde

Dies kann passieren, wenn das Sprachmodul veraltet ist. Führen Sie die folgenden Schritte aus, um das Sprachmodul auf die neueste Version zu aktualisieren:

1. Klicken Sie auf der Startseite von Azure Percept Studio im Menü auf der linken Seite auf **Geräte**.

1. Suchen Sie nach Ihrem Gerät, und wählen Sie es aus.

    :::image type="content" source="./media/tutorial-no-code-speech/devices.png" alt-text="Screenshot der Geräteliste in Azure Percept Studio":::

1. Wählen Sie im Gerätefenster die Registerkarte **Sprache** aus.

1. Überprüfen Sie die Version des Sprachmoduls. Wenn ein Update verfügbar ist, wird neben der Versionsnummer die Schaltfläche **Update** angezeigt.

    :::image type="content" source="./media/tutorial-no-code-speech/devkit.png" alt-text="Screenshot des Fensters mit DevKit-Spracheinstellungen":::

1. Klicken Sie auf **Update**, um das Update für das Sprachmodul bereitzustellen. Es dauert normalerweise zwei bis drei Minuten, bis der Updatevorgang abgeschlossen ist.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie mit der Arbeit an Ihrer Sprach-Assistenten-Anwendung fertig sind, ist es ratsam, die im Rahmen dieses Tutorials bereitgestellten Sprachressourcen mit den folgenden Schritten zu bereinigen:

1. Wählen Sie im [Azure-Portal](https://ms.portal.azure.com/#home) im Menü auf der linken Seite die Option **Ressourcengruppen** aus, oder geben Sie ihren Namen in der Suchleiste ein.

    :::image type="content" source="./media/tutorial-no-code-speech/azure-portal.png" alt-text="Screenshot der Startseite des Azure-Portals mit linkem Menübereich und Ressourcengruppen":::

1. Wählen Sie Ihre Ressourcengruppe aus.

1. Wählen Sie alle sechs Ressourcen aus, die Ihr Anwendungspräfix enthalten, und klicken Sie im oberen Menübereich auf das Symbol **Löschen**.

    :::image type="content" source="./media/tutorial-no-code-speech/select-resources.png" alt-text="Screenshot mit zum Löschen ausgewählten Sprachressourcen":::

1. Geben Sie zum Bestätigen des Löschvorgangs im entsprechenden Feld **Ja** ein, vergewissern Sie sich, dass Sie die richtigen Ressourcen ausgewählt haben, und klicken Sie auf **Löschen**.

    :::image type="content" source="./media/tutorial-no-code-speech/delete-confirmation.png" alt-text="Screenshot des Bestätigungsfensters für den Löschvorgang":::

> [!WARNING]
> Hierbei werden alle benutzerdefinierten Schlüsselwörter entfernt, die mit den von Ihnen gelöschten Sprachressourcen erstellt wurden, und die Demo für den Sprach-Assistenten funktioniert nicht mehr.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ohne Code eine Sprachlösung erstellt haben, können Sie versuchen, eine [Vision-Lösung ohne Code](./tutorial-nocode-vision.md) für Ihr Azure Percept DK zu erstellen.
