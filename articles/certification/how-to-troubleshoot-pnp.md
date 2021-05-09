---
title: Behandeln von Problemen im Zusammenhang mit Ihrem IoT Plug & Play-Gerät
description: Ein Leitfaden mit empfohlenen Problembehandlungsschritten für Partner, die ein IoT Plug & Play-Gerät zertifizieren möchten.
author: nkuntjoro
ms.author: nikuntjo
ms.service: certification
ms.topic: how-to
ms.date: 04/15/2021
ms.custom: template-how-to
ms.openlocfilehash: 591436de14916a919f0179978825b88dcc80117c
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/28/2021
ms.locfileid: "108129727"
---
# <a name="troubleshoot-your-iot-plug-and-play-certification-project"></a>Behandeln von Problemen im Zusammenhang mit Ihrem IoT Plug & Play-Zertifizierungsprojekt

Während der Phase „Verbinden und testen“ Ihres IoT Plug & Play-Zertifizierungsprojekts können bestimmte Szenarien dazu führen, dass Tests des IoT-Zertifizierungsdiensts (Azure IoT Certification Service, AICS) nicht bestanden werden.

## <a name="prerequisites"></a>Voraussetzungen

- Sie müssen angemeldet sein und über ein Projekt für Ihr Gerät im [Azure Certified Device-Portal](https://certify.azure.com) verfügen. Weitere Informationen finden Sie im [Tutorial](tutorial-01-creating-your-project.md).

## <a name="when-aics-tests-arent-passing"></a>Vorgehensweise für den Fall, dass AICS-Tests nicht bestanden werden

Nicht bestandene AICS-Tests können verschiedene Ursachen haben. Führen Sie die folgenden Schritte aus, um nach allgemeinen Problemen zu suchen und eine Problembehandlung für Ihr Gerät durchzuführen.

1. Vergewissern Sie sich, dass im Rahmen der DPS-Bereitstellung die Modell-ID-Nutzdaten durch Ihren Gerätecode festgelegt werden. Dies ist erforderlich, damit Ihr Gerät von AICS überprüft werden kann.
1. Sie können die Telemetrieprotokolle aus vorherigen Testläufen durch Auswählen von `View Logs` anzeigen, um zu ermitteln, warum der Test nicht bestanden wurde. Sowohl die Testnachrichten als auch die Rohdaten stehen zur Überprüfung zur Verfügung.  

    ![Überprüfen der Testdaten](./media/images/review-logs.png)

1. In Fällen, in denen in den Protokollen `Failed to get Digital Twin Model ID of device xx due to DeviceNotConnected` angegeben ist, können Sie versuchen, das Gerät und den Gerätebereitstellungsprozess neu zu starten.
1. Sind die automatisierten Tests weiterhin nicht erfolgreich, können Sie stattdessen eine manuelle Überprüfung der Ergebnisse anfordern (`request a manual review`). Dadurch wird eine Anforderung zur **manuellen Überprüfung** für das Azure Certified Device-Team ausgelöst.  

    ![Anfordern einer manuellen Überprüfung](./media/images/request-manual-review.png)

## <a name="when-you-see-passed-with-warnings"></a>Vorgehensweise bei Anzeige von „Passed with warnings“ (Mit Warnungen bestanden)

Wenn Sie beim Testen ein Ergebnis vom Typ `Passed with warnings` erhalten, wurden während des Testzeitraums einige Telemetriedaten nicht empfangen. Dies kann darauf zurückzuführen sein, dass die Telemetriedaten von längeren Zeitintervallen oder von externen Triggern abhängig sind, die nicht verfügbar waren. Sie können mit der Übermittlung Ihres Geräts zur Überprüfung fortfahren. Daraufhin wird vom Überprüfungsteam bestimmt, ob in Zukunft eine **manuelle Überprüfung** erforderlich ist.

## <a name="when-you-need-help-with-the-model-repository"></a>Vorgehensweise, wenn Sie Hilfe mit dem Modellrepository benötigen

Informationen zu IoT Plug & Play-Problemen im Zusammenhang mit dem Modellrepository finden Sie in unserer [Dokumentation zum Gerätemodellrepository](../iot-pnp/concepts-model-repository.md).

## <a name="next-steps"></a>Nächste Schritte

Wir hoffen, dieser Leitfaden hilft Ihnen bei Ihrer IoT Plug & Play-Zertifizierung. Nachdem Sie die AICS-Tests bestanden haben, können Sie mit unseren Tutorials zum Übermitteln und Veröffentlichen Ihres Geräts fortfahren.

- [Tutorial: Testen Ihres Geräts](tutorial-03-testing-your-device.md)