---
title: Rollen von X.509-Zertifikaten in Azure IoT Central
description: Informationen zum Rollen von X.509-Zertifikaten bei Ihrer IoT Central-Anwendung
author: dominicbetts
ms.author: dobett
ms.date: 07/31/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: a9e35c7d4d64279c65971dd512bcd2107dad6594
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "92000063"
---
# <a name="how-to-roll-x509-device-certificates-in-iot-central-application"></a>Informationen zum Rollen von X.509-Gerätezertifikaten in einer IoT Central-Anwendung

Während des Lebenszyklus Ihrer IoT-Lösung müssen Sie Zertifikate „rollen“ (festlegen, dass ein neues Zertifikat ab einem bestimmten Zeitpunkt zum aktuellen Zertifikat wird). Zwei der wichtigsten Gründe für das Rollen von Zertifikaten sind Sicherheitsverletzungen und der Ablauf von Zertifikaten.

Bei einer Sicherheitsverletzung ist das Rollen von Zertifikaten eine bewährte Sicherheitsmethode zum Schutz Ihres Systems. Als Teil der [Assume Breach-Methodik](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf) (von einer Sicherheitsverletzung ausgehen) betont Microsoft neben vorbeugenden Maßnahmen die Notwendigkeit von reaktiven Sicherheitsprozessen. Das Rollen Ihrer Gerätezertifikate sollte ein Bestandteil dieser Sicherheitsprozesse sein. Die Häufigkeit, mit der Sie die Zertifikate rollen, hängt von den Sicherheitsanforderungen Ihrer Lösung ab. Kunden, deren Lösungen sensible Daten beinhalten, rollen Zertifikate möglicherweise täglich, während andere ihre Zertifikate alle zwei Jahre rollen.


## <a name="obtain-new-x509-certificates"></a>Abrufen von neuen X.509-Zertifikaten

Sie können Ihre eigenen X.509-Zertifikate mit einem Tool wie OpenSSL erstellen. Dieser Ansatz eignet sich hervorragend zum Testen von X.509-Zertifikaten, bietet jedoch wenig Garantien im Hinblick auf die Sicherheit. Verwenden Sie nur diesen Ansatz für Tests – außer wenn Sie als Ihr eigener ZS-Anbieter fungieren können.

## <a name="enrollment-groups-and-security-breaches"></a>Registrierungsgruppen und Sicherheitsverletzungen

Zum Aktualisieren einer Gruppenregistrierung als Reaktion auf eine Sicherheitsverletzung sollten Sie den folgenden Ansatz verwenden, bei dem das aktuelle Zertifikat sofort aktualisiert wird:

1. Navigieren Sie im linken Bereich zu **Verwaltung**, und wählen Sie **Geräteverbindung** aus.

2. Wählen Sie **Registrierungsgruppen** und dann in der Liste den Gruppennamen aus.

3. Wählen Sie zum Aktualisieren des Zertifikats **Primäres verwalten** oder **Sekundäres verwalten** aus.

4. Fügen Sie das X.509-Stammzertifikat in der Registrierungsgruppe hinzu, und überprüfen Sie es.

   Führen Sie diese Schritte für das primäre und das sekundäre Zertifikat aus, wenn beide kompromittiert wurden.

## <a name="enrollment-groups-and-certificate-expiration"></a>Registrierungsgruppen und Zertifikatablauf

Wenn Sie Zertifikate rollen, weil sie ablaufen, verwenden Sie den folgenden Ansatz zum sofortigen Aktualisieren des aktuellen Zertifikats:

1. Navigieren Sie im linken Bereich zu **Verwaltung**, und wählen Sie **Geräteverbindung** aus.

2. Wählen Sie **Registrierungsgruppen** und dann in der Liste den Gruppennamen aus.

3. Wählen Sie zum Aktualisieren des Zertifikats **Primäres verwalten** aus.

4. Fügen Sie das X.509-Stammzertifikat in der Registrierungsgruppe hinzu, und überprüfen Sie es.

5. Wenn das sekundäre Zertifikat zu einem späteren Zeitpunkt abgelaufen ist, kehren Sie zurück, und aktualisieren Sie dieses Zertifikat.

## <a name="individual-enrollments-and-security-breaches"></a>Individuelle Registrierungen und Sicherheitsverletzungen

Wenn Sie Zertifikate als Reaktion auf eine Sicherheitsverletzung rollen, verwenden Sie den folgenden Ansatz zum sofortigen Aktualisieren des aktuellen Zertifikats:

1. Wählen Sie **Geräte** und dann das gewünschte Gerät aus.

2. Wählen Sie **Verbinden** und dann als **Individuelle Registrierung** die Methode „Connect“ aus.

3. Wählen Sie als Mechanismus **Zertifikate (X.509)** aus.

    ![Verwalten individueller Registrierungen](./media/how-to-roll-x509-certificates/certificate-update.png)

4. Wählen Sie für die Zertifikataktualisierung das Ordnersymbol und dann das neue Zertifikat aus, das für den Registrierungseintrag hochgeladen werden soll. Wählen Sie **Speichern**.

    Führen Sie diese Schritte für das primäre und das sekundäre Zertifikat aus, wenn beide kompromittiert wurden.

## <a name="individual-enrollments-and-certificate-expiration"></a>Individuelle Registrierungen und Zertifikatablauf

Wenn Sie Zertifikate rollen, weil Zertifikate ablaufen, sollten Sie die Konfiguration des sekundären Zertifikats wie folgt verwenden, um die Downtime für Geräte zu reduzieren, die sich bereitzustellen versuchen.

Wenn das sekundäre Zertifikat bald abläuft und gerollt werden muss, können Sie zur Verwendung der primären Konfiguration wechseln. Ein solcher Wechsel zwischen primärem und sekundärem Zertifikat reduziert die Downtime für Geräte, die sich bereitzustellen versuchen.

1. Wählen Sie **Geräte** und dann das gewünschte Gerät aus.

2. Wählen Sie **Verbinden** und dann als **Individuelle Registrierung** die Methode „Connect“ aus.

3. Wählen Sie als Mechanismus **Zertifikate (X.509)** aus.

    ![Verwalten individueller Registrierungen](./media/how-to-roll-x509-certificates/certificate-update.png)

4. Wählen Sie für die Aktualisierung des sekundären Zertifikats das Ordnersymbol und dann das neue Zertifikat aus, das für den Registrierungseintrag hochgeladen werden soll. Wählen Sie **Speichern**.

5. Wenn das primäre Zertifikat zu einem späteren Zeitpunkt abgelaufen ist, kehren Sie zurück, und aktualisieren Sie dieses Zertifikat.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie X.509-Zertifikate in ihrer Azure IoT Central-Anwendung rollen, können Sie jetzt [eine Verbindung mit Azure IoT Central herstellen](concepts-get-connected.md).


