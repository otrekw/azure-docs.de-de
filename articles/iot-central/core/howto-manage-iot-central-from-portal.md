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
ms.openlocfilehash: 2af97206db00d683ab409710bc71a3b5048bf6ae
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "104658464"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Statt IoT Central-Anwendungen auf der Website des [Azure IoT Central-Anwendungs-Managers](https://aka.ms/iotcentral) zu erstellen und zu verwalten, können Sie Ihre Anwendungen auch über das [Azure-Portal](https://portal.azure.com) verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]


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
> Da für Anwendungen, die mit dem Tarif *Free* erstellt wurden, keine Azure-Abonnements erforderlich sind, werden sie auch nicht in Ihrem Azure-Abonnement im Azure-Portal aufgeführt. Kostenlose Apps können nur über das IoT Central-Portal angezeigt und verwaltet werden.

Wählen Sie im Portal zunächst **Alle Ressourcen** aus. Wählen Sie **Ausgeblendete Typen anzeigen** aus, und geben Sie in **Nach Name filtern** den Namen Ihrer Anwendung ein, um danach zu suchen. Wählen Sie dann die IoT Central-Anwendung aus, die Sie verwalten möchten.

Wählen Sie die **URL der IoT Central-Anwendung** aus, um zur Anwendung zu navigieren:

![Der Screenshot zeigt die Seite „Übersicht“ mit hervorgehobenem „URL der IoT Central-Anwendung“.](media/howto-manage-iot-central-from-portal/image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, wählen Sie neben der Ressourcengruppe **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** die Ressourcengruppe aus, in die Sie diese Anwendung verschieben möchten:

![Der Screenshot zeigt die Seite „Übersicht“ mit hervorgehobenem „Ressourcengruppe (ändern)“.](media/howto-manage-iot-central-from-portal/image4a.png)

Wenn Sie die Anwendung in ein anderes Abonnement verschieben möchten, wählen Sie neben dem Abonnement **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** das Abonnement aus, in das Sie diese Anwendung verschieben möchten:

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)