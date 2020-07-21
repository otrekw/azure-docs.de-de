---
title: 'Schnellstart: Erstellen eines Sprachassistenten mithilfe von benutzerdefinierten Befehlen'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erstellen und testen Sie eine einfache Anwendung für benutzerdefinierte Befehle mithilfe von Speech Studio.
services: cognitive-services
author: singhsaumya
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: sausin
ms.openlocfilehash: 2ea850acf0ef581ea66733b0293ba044c87bb3a0
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2020
ms.locfileid: "85362141"
---
# <a name="create-a-voice-assistant-using-custom-commands"></a>Erstellen eines Sprachassistenten mithilfe von benutzerdefinierten Befehlen

In dieser Schnellstartanleitung erstellen und testen Sie eine einfache Anwendung für benutzerdefinierte Befehle mithilfe von Speech Studio. Sie können auf diese Anwendung auch von einer Windows-Client-App aus zugreifen. Mit **benutzerdefinierten Befehlen** ist es einfach, funktionsreiche Apps mit Sprachsteuerung zu erstellen, die für Voice-First-Interaktionserfahrungen optimiert sind. Sie bieten eine einheitliche Benutzeroberfläche für die Erstellung, ein automatisches Hostingmodell und eine relativ geringe Komplexität, sodass Sie sich auf die Entwicklung der besten Lösung für Ihre Szenarien mit Sprachsteuerung konzentrieren können.

## <a name="prerequisites"></a>Voraussetzungen

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Erstellen einer Azure Speech-Ressource in einer Region, die benutzerdefinierte Befehle unterstützt<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * Laden Sie die JSON-Beispieldatei für [Smart Room Lite](https://aka.ms/speech/cc-quickstart) herunter.
> * Laden Sie die neueste Version des [Windows-Sprachassistent-Clients](https://aka.ms/speech/va-samples-wvac) herunter.

> [!NOTE]
> Zum gegenwärtigen Zeitpunkt unterstützen die benutzerdefinierten Befehle nur Sprachabonnements in den Regionen „westus“, „westus2“ und „northeurope“.

## <a name="go-to-the-speech-studio-for-custom-commands"></a>Wechseln Sie zum Speech Studio für benutzerdefinierte Befehle.

1. Wechseln Sie in einem Webbrowser zu [Speech Studio](https://speech.microsoft.com/).
1. Geben Sie Ihre Anmeldeinformationen ein, um sich beim Portal anzumelden.

   Die Standardansicht ist Ihre Liste der Speech-Abonnements.
   > [!NOTE]
   > Wenn die Seite „Abonnement auswählen“ nicht angezeigt wird, können Sie dorthin navigieren, indem Sie „Speech-Ressourcen“ aus dem Einstellungsmenü in der oberen Leiste auswählen.

1. Wählen Sie Ihr Speech-Abonnement und dann **Zu Studio wechseln** aus.
1. Wählen Sie **Benutzerdefinierte Befehle** aus.

   Die Standardansicht ist eine Liste der Anwendungen für benutzerdefinierte Befehle, die in Ihrem ausgewählten Abonnement vorhanden sind.

## <a name="import-an-existing-application-as-a-new-custom-commands-project"></a>Importieren einer vorhandenen Anwendung als neues Projekt für benutzerdefinierte Befehle

1. Wählen Sie **Neues Projekt** aus, um ein Projekt zu erstellen.

1. Geben Sie im Feld **Name** den Projektnamen als `Smart-Room-Lite` (oder etwas ähnliches) ein.
1. Wählen Sie in der Liste **Sprache** die Option **Englisch (USA)** aus.
1. Wählen Sie **Dateien durchsuchen** und anschließend im Fenster „Durchsuchen“ die Datei **SmartRoomLite.json** aus.

    > [!div class="mx-imgBorder"]
    > ![Erstellen eines Projekts](media/custom-commands/import-project.png)

1.  Wählen Sie in der Liste **LUIS-Erstellungsressource** eine Erstellungsressource aus. Sind keine gültigen Erstellungsressourcen vorhanden, erstellen Sie eine, indem Sie **Neue LUIS-Erstellungsressource erstellen** auswählen.

    > [!div class="mx-imgBorder"]
    > ![Erstellen einer Ressource](media/custom-commands/create-new-luis-resource.png)
    
    
    1. Geben Sie im Feld **Ressourcenname** den Namen der Ressource ein.
    1. Wählen Sie in der Liste **Ressourcengruppe** eine Ressourcengruppe aus.
    1. Wählen Sie in der Liste **Standort** einen Standort aus.
    1. Wählen Sie in der Liste **Tarif** einen Tarif aus.
    
    
    > [!NOTE]
    > Sie können Ressourcengruppen erstellen, indem Sie den gewünschten Ressourcengruppennamen in das Feld „Ressourcengruppe“ eingeben. Die Ressourcengruppe wird erstellt, wenn **Erstellen** ausgewählt wird.


1. Wählen Sie als nächstes **Erstellen** aus, um das Projekt zu erstellen.
1. Sobald das Projekt erstellt ist, wählen Sie Ihr Projekt aus.
Sie sollten jetzt eine Übersicht über Ihre neue Anwendung für benutzerdefinierte Befehle sehen.

## <a name="try-out-some-voice-commands"></a>Ausprobieren einiger Sprachbefehle
1. Klicken Sie oben im rechten Bereich auf **Trainieren**.
1. Sobald das Training abgeschlossen ist, wählen Sie **Testen** aus. Anschließend probieren Sie die folgenden Äußerungen aus:
    - Turn on the tv (Schalte den Fernseher ein)
    - Set the temperature to 80 degrees (Stelle die Temperatur auf 27 Grad ein)
    - Turn it off (Schalte es aus)
    - The tv (Der Fernseher)
    - Set an alarm for 5 PM (Alarm für 17 Uhr festlegen)

## <a name="integrate-custom-commands-application-in-an-assistant"></a>Integrieren von Anwendungen für benutzerdefinierte Befehle in einen Assistenten
Bevor Sie außerhalb von Speech Studio auf diese Anwendung zugreifen können, müssen Sie die Anwendung veröffentlichen. Zum Veröffentlichen einer Anwendung müssen Sie die LUIS-Vorhersageressource konfigurieren.  

### <a name="update-prediction-luis-resource"></a>Aktualisieren der LUIS-Vorhersageressource


1. Wählen Sie im linken Bereich **Einstellungen** und dann im mittleren Bereich **LUIS-Ressourcen** aus.
1. Wählen Sie eine Vorhersageressource aus, oder erstellen Sie eine, indem Sie **Neue Ressource erstellen** auswählen.
1. Wählen Sie **Speichern** aus.
    
    > [!div class="mx-imgBorder"]
    > ![Festlegen von LUIS-Ressourcen](media/custom-commands/set-luis-resources.png)

> [!NOTE]
> Da die Erstellungsressource nur 1.000 Anforderungen von Vorhersage-Endpunkten pro Monat unterstützt, ist das Festlegen einer LUIS-Vorhersageressource vor dem Veröffentlichen Ihrer Anwendung für benutzerdefinierte Befehle obligatorisch.

### <a name="publish-the-application"></a>Veröffentlichen der Anwendung

Wählen Sie oben im rechten Bereich **Veröffentlichen** aus. Sobald die Veröffentlichung abgeschlossen ist, wird ein neues Fenster angezeigt. Notieren Sie daraus die Werte **Anwendungs-ID** und **Speech-Ressourcenschlüssel**. Sie benötigen diese beiden Werte, um von außerhalb von Speech Studio auf die Anwendung zugreifen zu können.

Alternativ können Sie diese Werte auch erhalten, indem Sie den Abschnitt **Einstellungen** > **Allgemein** auswählen.

### <a name="access-application-from-client"></a>Zugreifen auf die Anwendung über den Client

Im Rahmen dieses Artikels werden wir den Windows-Sprachassistent-Client verwenden, den Sie als Teil der Voraussetzungen heruntergeladen haben. Entzippen Sie den Ordner.
1. Starten Sie **VoiceAssistantClient.exe**.
1. Erstellen Sie ein neues Veröffentlichungsprofil, und geben Sie einen Wert für **Verbindungsprofil** ein. Geben Sie im Bereich **Allgemeine Einstellungen** die Werte **Abonnementschlüssel** (dieser Wert ist identisch mit dem Wert für den **Speech-Ressourcenschlüssel**, den Sie beim Veröffentlichen der Anwendung gespeichert haben), **Abonnementschlüsselregion** und **App-ID für benutzerdefinierte Befehle** ein.
    > [!div class="mx-imgBorder"]
    > ![WVAC: Profil erstellen](media/custom-commands/create-profile.png)
1. Wählen Sie **Profil speichern und anwenden** aus.
1. Probieren Sie nun die folgenden Eingaben per Sprache/Text aus.
    > [!div class="mx-imgBorder"]
    > ![WVAC: Profil erstellen](media/custom-commands/conversation.png)


> [!TIP]
> Sie können auf Einträge in **Aktivitätsprotokoll** klicken, um die vom Dienst für benutzerdefinierte Befehle gesendeten unformatierten Antworten zu prüfen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie eine vorhandene Anwendung verwendet. Als nächstes lernen Sie in den [Abschnitten zur Vorgehensweise](how-to-custom-commands-create-application-with-simple-commands.md) wie Sie eine Anwendung für benutzerdefinierte Befehle von Grund auf entwerfen, entwickeln, debuggen, testen und integrieren.
