---
title: Verwalten von IoT Central über das Azure-Portal | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie Ihre IoT Central-Anwendungen über das Azure-Portal erstellen und verwalten.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 04/17/2021
ms.topic: how-to
ms.openlocfilehash: 3e5e126815d0171a6c1627a08419b05b9a3c0c23
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2021
ms.locfileid: "107719203"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>Verwalten von IoT Central über das Azure-Portal

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

Sie können das [Azure-Portal](https://portal.azure.com) verwenden, um IoT Central-Anwendungen zu erstellen oder zu verwalten.

## <a name="create-iot-central-applications"></a>Erstellen von IoT Central-Anwendungen

[!INCLUDE [Warning About Access Required](../../../includes/iot-central-warning-contribitorrequireaccess.md)]

Navigieren Sie zum Erstellen einer Anwendung zur Seite [IoT Central-Anwendung](https://ms.portal.azure.com/#create/Microsoft.IoTCentral) im Azure-Portal:

![Erstellen eines IoT Central-Formulars](media/howto-manage-iot-central-from-portal/image6a.png)

* **Ressourcenname** ist ein eindeutiger Name, den Sie für Ihre IoT Central-Anwendung in Ihrer Azure-Ressourcengruppe auswählen können.

* **Anwendungs-URL** ist die URL, die Sie für den Zugriff auf Ihre Anwendung verwenden können.

* Der **Standort** beschreibt die [geografische Region](https://azure.microsoft.com/global-infrastructure/geographies/), in der Sie Ihre Anwendung erstellen möchten. Normalerweise wählen Sie den Standort aus, der Ihren Geräten physisch am nächsten liegt, um eine optimale Leistung zu erzielen. Azure IoT Central ist derzeit an den folgenden Standorten verfügbar:

  * Asien-Pazifik
  * Australien
  * Europa
  * Japan
  * Vereinigtes Königreich
  * USA

  Nachdem Sie einen Standort ausgewählt haben, können Sie Ihre Anwendung nicht zu einem späteren Zeitpunkt an einen anderen Standort verschieben.

Wählen Sie **Erstellen** aus, wenn Sie alle Felder ausgefüllt haben. Weitere Informationen finden Sie im Schnellstart [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central.md).

## <a name="manage-existing-iot-central-applications"></a>Verwalten vorhandener IoT Central-Anwendungen

Wenn Sie bereits über eine Azure IoT Central-Anwendung verfügen, können Sie sie löschen oder aber im Azure-Portal in ein anderes Abonnement oder eine andere Ressourcengruppe verschieben.

> [!NOTE]
> Da für Anwendungen, die mit dem Tarif *Free* erstellt wurden, keine Azure-Abonnements erforderlich sind, werden sie auch nicht in Ihrem Azure-Abonnement im Azure-Portal aufgeführt. Kostenlose Apps können nur über das IoT Central-Portal angezeigt und verwaltet werden.

Um zu beginnen, suchen Sie mithilfe der Suchleiste oben im Azure-Portal nach Ihrer Anwendung. Sie können auch sämtliche Ihrer Anwendungen anzeigen, indem Sie nach _IoT Central-Anwendungen_ suchen und den Dienst auswählen:

![Screenshot: Suchergebnisse für „IoT Central-Anwendungen“ mit ausgewähltem ersten Dienst](media/howto-manage-iot-central-from-portal/search-iot-central.png)

Wenn Sie eine Anwendung in den Suchergebnissen auswählen, wird im Azure-Portal die zugehörige Übersicht angezeigt. Sie können **URL der IoT Central-Anwendung** auswählen, um zur Anwendung zu navigieren:

![Der Screenshot zeigt die Seite „Übersicht“ mit hervorgehobenem „URL der IoT Central-Anwendung“.](media/howto-manage-iot-central-from-portal/image3.png)

Um die Anwendung in eine andere Ressourcengruppe zu verschieben, wählen Sie neben der Ressourcengruppe **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** die Ressourcengruppe aus, in die Sie diese Anwendung verschieben möchten:

![Der Screenshot zeigt die Seite „Übersicht“ mit hervorgehobenem „Ressourcengruppe (ändern)“.](media/howto-manage-iot-central-from-portal/image4a.png)

Wenn Sie die Anwendung in ein anderes Abonnement verschieben möchten, wählen Sie neben dem Abonnement **Ändern** aus. Wählen Sie auf der Seite **Ressourcen verschieben** das Abonnement aus, in das Sie diese Anwendung verschieben möchten:

![Verwaltungsportal: Ressourcenverwaltung](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun erfahren haben, wie Sie Azure IoT Central-Anwendungen über das Azure-Portal verwalten, wird der folgende nächste Schritt empfohlen:

> [!div class="nextstepaction"]
> [Verwalten Ihrer Anwendung](howto-administer.md)