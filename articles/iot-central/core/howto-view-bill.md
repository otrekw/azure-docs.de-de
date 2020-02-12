---
title: Verwalten Ihrer Rechnung und Umstellen vom kostenlosen Testtarif in der Azure IoT Central-Anwendung | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie als Administrator Ihre Rechnung verwalten und in Ihrer Azure IoT Central-Anwendung vom kostenlosen Tarif zu einem Standardtarif wechseln.
author: v-krghan
ms.author: v-krghan
ms.date: 12/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 58503784f28a3ba0a6290a2209ce9fdccc14b4e4
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2020
ms.locfileid: "77023768"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>Verwalten Ihrer Rechnung in einer IoT Central-Anwendung

In diesem Artikel wird beschrieben, wie Sie als Administrator Ihre Rechnung in der Azure IoT Central-Anwendung im Verwaltungsbereich verwalten können. Sie erfahren, wie Sie Ihre Testversion vom kostenlosen Tarif auf einen Standardtarif umstellen und wie Sie Ihren Tarif herauf- oder herabstufen können.

Um auf den Abschnitt **Verwaltung** zuzugreifen und ihn zu verwenden, muss Ihnen die Rolle *Administrator* zugewiesen sein, oder Sie müssen eine *benutzerdefinierte Benutzerrolle* haben, die Ihnen das Anzeigen der Rechnung für eine Azure IoT Central-Anwendung erlaubt. Wenn Sie eine Azure IoT Central-Anwendung erstellen, wird Ihnen automatisch die Rolle **Administrator** für die jeweilige Anwendung zugewiesen.

## <a name="move-from-free-to-standard-pricing-plan"></a>Wechseln vom Tarif „Free“ zu „Standard“

- Anwendungen, die den kostenlosen Tarif nutzen, sind sieben Tage lang kostenlos und laufen dann ab. Um Datenverluste zu vermeiden, können Sie sie jederzeit auf einen Standardtarif umstellen, bevor sie ablaufen.
- Anwendungen, die einen Standardtarif nutzen, werden pro Gerät abgerechnet. Dabei sind die ersten beiden Geräte pro Anwendung kostenlos.

Weitere Informationen zu den Preisen finden Sie unter [Azure IoT Central – Preise](https://azure.microsoft.com/pricing/details/iot-central/).

Im Preisabschnitt können Sie Ihre Anwendung vom Tarif „Free“ auf den Tarif „Standard“ umstellen.

Um diesen Self-Service-Prozess durchzuführen, gehen Sie folgendermaßen vor:

1. Wechseln Sie zur Seite **Preise** im Abschnitt **Verwaltung**.

    ![Status der Testversion](media/howto-view-bill/freetrialbilling.png)

1. Wählen Sie **In kostenpflichtigen Plan konvertieren** aus.

    ![Umwandeln der Testversion](media/howto-view-bill/convert.png)

1. Wählen Sie die entsprechende Azure Active Directory-Instanz und dann das Azure-Abonnement aus, das für Ihre Anwendung mit kostenpflichtigem Tarif verwendet werden soll.

1. Nach dem Auswählen von **Konvertieren** wird Ihre Anwendung auf einen kostenpflichtigen Tarif umgestellt, und Sie erhalten ab diesem Zeitpunkt Rechnungen.

> [!Note]
> Standardmäßig werden Sie auf den Tarif *Standard 2* umgestellt.

## <a name="how-to-change-your-application-pricing-plan"></a>Ändern des Tarifs für Ihre Anwendung

Anwendungen, die einen Standardtarif nutzen, werden pro Gerät abgerechnet. Dabei sind die ersten beiden Geräte pro Anwendung kostenlos.

Im Preisabschnitt können Sie jederzeit ein Upgrade oder Downgrade Ihres Azure IoT-Tarifs durchführen.

1. Wechseln Sie zur Seite **Preise** im Abschnitt **Verwaltung**.

    ![Status der Testversion](media/howto-view-bill/pricing.png)

1. Wählen Sie für das Upgrade oder Downgrade den **Tarif** aus, und klicken Sie auf **Speichern**.

## <a name="view-your-bill"></a>Anzeigen Ihrer Rechnung

1. Wählen Sie die entsprechende Azure Active Directory-Instanz und dann das Azure-Abonnement aus, das für Ihre Anwendung mit kostenpflichtigem Tarif verwendet werden soll.

1. Nach dem Auswählen von **Konvertieren** wird Ihre Anwendung auf einen kostenpflichtigen Tarif umgestellt, und Sie erhalten ab diesem Zeitpunkt Rechnungen.

> [!Note]
> Standardmäßig werden Sie auf den Tarif *Standard 2* umgestellt.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie etwas über das Verwalten Ihrer Rechnung in der Azure IoT Central-Anwendung erfahren haben, wird als nächster Schritt empfohlen, dass Sie sich mit dem [Anpassen der Anwendungsbenutzeroberfläche](howto-customize-ui.md) in Azure IoT Central vertraut machen.