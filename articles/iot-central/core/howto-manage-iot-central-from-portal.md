---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendungen über das Azure-Portal erstellen und verwalten.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: e7efda8efa27044168386e3ebbc557bf7fb74e8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157924"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie Ihre Anwendungen auch über das [Azure-Portal](https://portal.azure.com) verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

Zum Erstellen einer Anwendung navigieren Sie zum [Azure-Portal](https://ms.portal.azure.com), und wählen Sie **Ressource erstellen** aus.

Geben Sie in der Leiste **Marketplace durchsuchen** den Suchbegriff *IoT Central* ein:

![Verwaltungsportal: Suchen](media/howto-manage-iot-central-from-portal/image0a1.png)

Wählen Sie in den Suchergebnissen die Kachel **IoT Central-Anwendung** aus:

![Verwaltungsportal: Suchergebnisse](media/howto-manage-iot-central-from-portal/image0b1.png)

Wählen Sie jetzt **Erstellen** aus:

![Verwaltungsportal: IoT Central-Ressource](media/howto-manage-iot-central-from-portal/image0c1.png)

Füllen Sie alle Felder im Formular aus. Dieses Formular ähnelt dem Formular, das Sie ausfüllen müssen, um Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen. Weitere Informationen finden Sie unter dem Schnellstart [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

![Erstellen eines IoT Central-Formulars](media/howto-manage-iot-central-from-portal/image6a.png)

**Standort** beschreibt die [geografische Region](https://azure.microsoft.com/global-infrastructure/geographies/), in der Sie Ihre Anwendung erstellen möchten. Normalerweise wählen Sie den Standort aus, der Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Azure IoT Central ist derzeit in den geografischen Regionen **Australien**, **Asien-Pazifik**, **Europa**, **USA**, **Vereinigtes Königreich** und **Japan** verfügbar. Nachdem Sie einen Standort ausgewählt haben, können Sie Ihre Anwendung nicht zu einem späteren Zeitpunkt an einen anderen Standort verschieben.

Wählen Sie **Erstellen** aus, wenn Sie alle Felder ausgefüllt haben.

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen

Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben.

> [!NOTE]
> Im kostenlosen Tarif erstellte Anwendungen werden im Azure-Portal nicht angezeigt, da sie nicht mit Ihrem Abonnement verknüpft sind.

Wählen Sie im Portal zunächst **Alle Ressourcen** aus. Wählen Sie **Ausgeblendete Typen anzeigen** aus, und geben Sie in **Nach Name filtern** den Namen Ihrer Anwendung ein, um danach zu suchen. Wählen Sie dann die IoT Central-Anwendung aus, die Sie verwalten möchten.

Wählen Sie die **URL der IoT Central-Anwendung** aus, um zur Anwendung zu navigieren:

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, wählen Sie neben der Ressourcengruppe **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** die Ressourcengruppe aus, in die Sie diese Anwendung verschieben möchten:

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image4a.png)

Wenn Sie die Anwendung in ein anderes Abonnement verschieben möchten, wählen Sie neben dem Abonnement **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** das Abonnement aus, in das Sie diese Anwendung verschieben möchten:

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)