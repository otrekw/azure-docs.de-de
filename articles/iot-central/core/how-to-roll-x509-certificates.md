---
title: Rollen von X.509-Zertifikaten in Azure IoT Central
description: Informationen zum Rollen von X.509-Zertifikaten bei Ihrer IoT Central-Anwendung
author: v-krghan
ms.author: v-krghan
ms.date: 07/31/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 76e2b9542d20b5788a2875dec89d447ce38276a0
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121952"
---
# <a name="how-to-roll-x509-device-certificates-in-iot-central-application"></a>Informationen zum Rollen von X.509-Gerätezertifikaten in einer IoT Central-Anwendung

Während des Lebenszyklus Ihrer IoT-Lösung müssen Sie Zertifikate „rollen“ (festlegen, dass ein neues Zertifikat ab einem bestimmten Zeitpunkt zum aktuellen Zertifikat wird). Zwei der wichtigsten Gründe für das Rollen von Zertifikaten sind Sicherheitsverletzungen und der Ablauf von Zertifikaten. 

Das Rollen von Zertifikaten ist eine bewährte Sicherheitsmethode, um Ihr System im Falle einer Sicherheitsverletzung zu schützen. Als Teil der [Assume Breach-Methodik](https://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf) (von einer Sicherheitsverletzung ausgehen) betont Microsoft neben vorbeugenden Maßnahmen die Notwendigkeit von reaktiven Sicherheitsprozessen. Das Rollen Ihrer Gerätezertifikate sollte ein Bestandteil dieser Sicherheitsprozesse sein. Die Häufigkeit, mit der Sie die Zertifikate rollen, hängt von den Sicherheitsanforderungen Ihrer Lösung ab. Kunden, deren Lösungen sensible Daten beinhalten, rollen Zertifikate möglicherweise täglich, während andere ihre Zertifikate alle zwei Jahre rollen.


## <a name="obtain-new-x509-certificates"></a>Abrufen von neuen X.509-Zertifikaten

Sie können Ihre eigenen X.509-Zertifikate mit einem Tool wie OpenSSL erstellen. Dieser Ansatz eignet sich hervorragend zum Testen von X.509-Zertifikaten, bietet jedoch wenig Garantien im Hinblick auf die Sicherheit. Verwenden Sie nur diesen Ansatz für Tests – außer wenn Sie als Ihr eigener ZS-Anbieter fungieren können.


## <a name="enrollment-groups-and-security-breaches"></a>Registrierungsgruppen und Sicherheitsverletzungen

Zum Aktualisieren einer Gruppenregistrierung als Reaktion auf eine Sicherheitsverletzung sollten Sie den folgenden Ansatz verwenden, bei dem das aktuelle Zertifikat sofort aktualisiert wird:

1. Navigieren Sie im linken Bereich zu **Verwaltung**, und klicken Sie auf **Geräteverbindung**.

2. Klicken Sie auf **Registrierungsgruppen**, und klicken Sie dann in der Liste auf den Gruppennamen.

    ![Geräteverbindung](./media/how-to-roll-x509-certificates/device-connection.png)


3. Klicken Sie zum Aktualisieren des Zertifikats auf **Primäres verwalten** oder **Sekundäres verwalten**.

    ![Verwalten von Zertifikaten](./media/how-to-roll-x509-certificates/certificates.png)


4. Fügen Sie das X.509-Stammzertifikat in der Registrierungsgruppe hinzu, und überprüfen Sie es.

   Führen Sie diese Schritte für das primäre und das sekundäre Zertifikat aus, wenn beide kompromittiert wurden.



## <a name="enrollment-groups-and-certificate-expiration"></a>Registrierungsgruppen und Zertifikatablauf

Wenn Sie Zertifikate rollen, weil sie ablaufen, verwenden Sie den folgenden Ansatz zum sofortigen Aktualisieren des aktuellen Zertifikats:

1. Navigieren Sie im linken Bereich zu **Verwaltung**, und klicken Sie auf **Geräteverbindung**. 

2. Klicken Sie auf **Registrierungsgruppen**, und klicken Sie dann in der Liste auf den Gruppennamen.

    ![Geräteverbindung](./media/how-to-roll-x509-certificates/device-connection.png)


3. Klicken Sie zum Aktualisieren des Zertifikats auf **Primäres verwalten**.

    ![Geräteverbindung](./media/how-to-roll-x509-certificates/manage-certs.png)

4. Fügen Sie das X.509-Stammzertifikat in der Registrierungsgruppe hinzu, und überprüfen Sie es.

5. Wenn das sekundäre Zertifikat zu einem späteren Zeitpunkt abgelaufen ist, kehren Sie zurück, und aktualisieren Sie dieses Zertifikat.



## <a name="individual-enrollments-and-security-breaches"></a>Individuelle Registrierungen und Sicherheitsverletzungen

Wenn Sie Zertifikate als Reaktion auf eine Sicherheitsverletzung rollen, verwenden Sie den folgenden Ansatz zum sofortigen Aktualisieren des aktuellen Zertifikats:


1. Klicken Sie auf **Geräte**, und wählen Sie das gewünschte Gerät aus. 

2. Klicken Sie auf **Verbinden**, und wählen Sie als **Individuelle Registrierung** die Methode „Connect“ aus.

3. Wählen Sie als Mechanismus **Zertifikate (X.509)** aus.

    ![Verwalten individueller Registrierungen](./media/how-to-roll-x509-certificates/certificate-update.png)

4. Klicken Sie für die Zertifikataktualisierung auf das Ordnersymbol, um das neue Zertifikat auszuwählen, das für den Registrierungseintrag hochgeladen werden soll. Klicken Sie auf **Speichern**.

    Führen Sie diese Schritte für das primäre und das sekundäre Zertifikat aus, wenn beide kompromittiert wurden.



## <a name="individual-enrollments-and-certificate-expiration"></a>Individuelle Registrierungen und Zertifikatablauf

Wenn Sie Zertifikate rollen, weil Zertifikate ablaufen, sollten Sie die Konfiguration des sekundären Zertifikats wie folgt verwenden, um die Downtime für Geräte zu reduzieren, die sich bereitzustellen versuchen.

Wenn später auch das sekundäre Zertifikat bald abläuft und gerollt werden muss, können Sie zur Verwendung der primären Konfiguration wechseln. Ein solcher Wechsel zwischen primärem und sekundärem Zertifikat reduziert die Downtime für Geräte, die sich bereitzustellen versuchen.

1. Klicken Sie auf **Geräte**, und wählen Sie das gewünschte Gerät aus.

2. Klicken Sie auf **Verbinden**, und wählen Sie als **Individuelle Registrierung** die Methode „Connect“ aus.

3. Wählen Sie als Mechanismus **Zertifikate (X.509)** aus.

    ![Verwalten individueller Registrierungen](./media/how-to-roll-x509-certificates/certificate-update.png)

4. Klicken Sie für die Aktualisierung des sekundären Zertifikats auf das Ordnersymbol, um das neue Zertifikat auszuwählen, das für den Registrierungseintrag hochgeladen werden soll. Klicken Sie auf **Speichern**.


5. Wenn das primäre Zertifikat zu einem späteren Zeitpunkt abgelaufen ist, kehren Sie zurück, und aktualisieren Sie dieses Zertifikat.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie X.509-Zertifikate in ihrer Azure IoT Central-Anwendung rollen, können Sie jetzt [eine Verbindung mit Azure IoT Central herstellen](concepts-get-connected.md).


