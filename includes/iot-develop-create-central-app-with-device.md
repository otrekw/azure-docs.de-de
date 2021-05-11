---
title: include file
description: include file
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 04/28/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 981ebfece7bc12584adfdf1e565bd275d8e1b2fb
ms.sourcegitcommit: 49bd8e68bd1aff789766c24b91f957f6b4bf5a9b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2021
ms.locfileid: "108226689"
---
## <a name="create-an-application"></a>Erstellen einer Anwendung
Es gibt verschiedene Möglichkeiten zum Verbinden von Geräten mit Azure IoT. In diesem Abschnitt wird beschrieben, wie Sie Azure IoT Central verwenden, um die Verbindung für ein Gerät herzustellen. Bei IoT Central handelt es sich um eine IoT-Anwendungsplattform, mit der die Kosten und die Komplexität der Verwaltung von Geräten in einer IoT-Lösung reduziert werden.

Erstellen Sie wie folgt eine neue Anwendung:
1. Navigieren Sie zu [Azure IoT Central](https://apps.azureiotcentral.com/), und melden Sie sich mit einem persönlichen Microsoft-, Geschäfts- oder Schulkonto an.
1. Navigieren Sie zu **Erstellen**, und klicken Sie auf **Benutzerdefinierte Apps**.
   :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-build.png" alt-text="IoT Central-Startseite":::
1. Geben Sie unter **Anwendungsname** einen eindeutigen Namen ein, oder verwenden Sie den generierten Namen.
1. Geben Sie unter **URL** ein leicht zu behaltendes URL-Präfix ein, oder verwenden Sie das generierte URL-Präfix.
1. Behalten Sie für **Anwendungsvorlage** die Einstellung *Benutzerdefinierte Anwendung* bei. 
1. Wählen Sie eine Option für **Preisplan** aus. 
    - Wenn Sie die Anwendung sieben Tage lang kostenlos nutzen möchten, wählen Sie **Kostenlos** aus. Sie können eine kostenlose Anwendung in den Standardtarif konvertieren, bevor sie abläuft.
    - Optional können Sie einen Standardtarifplan auswählen. Wenn Sie den Standardtarif auswählen, werden weitere Optionen angezeigt, und Sie müssen ein **Verzeichnis**, ein **Azure-Abonnement** und einen **Speicherort** festlegen. Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central pricing](https://azure.microsoft.com/pricing/details/iot-central/). 
        - Bei **Verzeichnis** handelt es sich um die Azure Active Directory-Instanz, in der Sie Ihre Anwendung erstellen. Eine Azure Active Directory-Instanz enthält Benutzeridentitäten, Anmeldeinformationen und andere organisatorische Informationen. Wenn Sie keine Azure Active Directory-Instanz besitzen, wird beim Erstellen eines Azure-Abonnements eine Instanz angelegt.
        - Mit einem **Azure-Abonnement** können Sie Instanzen von Azure-Diensten erstellen. IoT Central stellt Ressourcen in Ihrem Abonnement bereit. Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses](https://aka.ms/createazuresubscription) Konto erstellen. Falls Sie über ein Abonnement verfügen, können Sie es in der Dropdownliste auswählen.
        - **Speicherort** ist die [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/), in der Sie eine Anwendung erstellen. Wählen Sie den Speicherort aus, der Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Nachdem Sie einen Speicherort ausgewählt haben, können Sie Ihre Anwendung nicht an einen anderen Speicherort verschieben.

    :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-pricing.png" alt-text="IoT Central: Dialogfeld „Neue Anwendung“":::
1. Klicken Sie auf **Erstellen**.
    
    Nachdem IoT Central die Anwendung erstellt hat, werden Sie zum Anwendungsdashboard umgeleitet.
    :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-created.png" alt-text="IoT Central: Dashboard „Neue Anwendung“":::

## <a name="add-a-device"></a>Hinzufügen eines Geräts
In diesem Abschnitt fügen Sie Ihrer IoT Central-Anwendung ein neues Gerät hinzu. Das Gerät ist eine Instanz einer Gerätevorlage, die ein reales oder simuliertes Gerät darstellt, das Sie mit der Anwendung verbinden werden. 

So erstellen Sie ein neues Gerät:
1. Wählen Sie im linken Bereich **Geräte** und dann **+ Neu** aus.
1. Behalten Sie für **Gerätevorlage** die Festlegung *Nicht zugewiesen* bei.

    > [!NOTE]
    > In diesem Schnellstart stellen Sie der Einfachheit halber eine Verbindung mit einem simulierten Gerät her, das eine nicht zugewiesene Vorlage verwendet. Wenn Sie IoT Central weiterhin zum Verwalten von Geräten verwenden, erfahren Sie mehr über die Verwendung von Gerätevorlagen. Eine Übersicht über das Arbeiten mit Gerätevorlagen finden Sie unter [Schnellstart: Hinzufügen eines simulierten Geräts zu ihrer IoT Central-Anwendung](../articles/iot-central/core/quick-create-simulated-device.md).
1. Legen Sie einen benutzerfreundlichen **Gerätenamen** und eine benutzerfreundliche **Geräte-ID** fest. Verwenden Sie optional die generierten Werte.
    :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-create-device.png" alt-text="IoT Central: Dialogfeld „Neues Gerät“":::
1. Klicken Sie auf **Erstellen**.

    Das erstellte Gerät wird in der Liste **Alle Geräte** angezeigt.
    :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-devices-list.png" alt-text="IoT Central: Liste „Alle Geräte“":::
    
So rufen Sie Verbindungsdetails für das neue Gerät ab:
1. Klicken Sie in der Liste **Alle Geräte** auf den Namen des verknüpften Geräts, um Details anzuzeigen. 
1. Klicken Sie im obersten Menü auf **Verbinden**.

    Im Dialogfeld **Geräteverbindung** werden die Verbindungsdetails angezeigt:  :::image type="content" source="media/iot-develop-create-central-app-with-device/iot-central-device-connect.png" alt-text="IoT Central: Geräteverbindungsdetails":::
1. Kopieren Sie die folgenden Werte aus dem Dialogfeld **Geräteverbindung** an einen sicheren Speicherort. Sie verwenden diese Werte, um Ihr Gerät mit IoT Central zu verbinden.
    * `ID scope`
    * `Device ID`
    * `Primary key`