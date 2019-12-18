---
title: Abrufen der Sensordaten von Partnern
description: In diesem Artikel erfahren Sie, wie Sie Sensordaten von Partnern abrufen.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: ece310a248140b7913ffcc9f7146d382ee44fb5d
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851297"
---
# <a name="get-sensor-data-from-sensor-partners"></a>Abrufen der Sensordaten von Sensorpartnern

Mit Azure FarmBeats können Sie Streamingdaten von Ihren IoT-Geräten und -Sensoren an den Datenhub übertragen. Derzeit werden folgende Sensorgerätepartner unterstützt:

  ![FarmBeats-Partner](./media/get-sensor-data-from-sensor-partner/partner-information-1.png)

Durch die Integration von Gerätedaten in Azure FarmBeats können Sie Bodendaten von den IoT-Sensoren Ihres landwirtschaftlichen Betriebs an Ihren Datenhub übertragen. Die verfügbaren Daten können dann über den FarmBeats-Accelerator visualisiert werden. Die Daten können für die Datenfusion und für die Erstellung von ML-/KI-Modellen (Machine Learning/künstliche Intelligenz) mit FarmBeats verwendet werden.

Stellen Sie Folgendes sicher, bevor Sie mit dem Streamen von Sensordaten beginnen:

-  Sie haben FarmBeats aus dem Azure Marketplace installiert.
-  Sie haben die Sensoren und Geräte ausgewählt, die Sie in Ihrem landwirtschaftlichen Betrieb installieren möchten.
-  Falls Sie Bodenfeuchtigkeitssensoren verwenden möchten, können Sie die FarmBeats-Karte zur Platzierung von Bodenfeuchtigkeitssensoren verwenden, um eine Empfehlung hinsichtlich der Anzahl und Platzierung der Sensoren zu erhalten. Weitere Informationen finden Sie unter [Generieren von Karten](generate-maps.md).
- Geräte und Sensoren müssen bei Ihrem Gerätepartner erworben und in Ihrem landwirtschaftlichen Betrieb installiert werden. Vergewissern Sie sich, dass Sie mit der Lösung Ihres Gerätepartners auf die Sensordaten zugreifen können.

## <a name="enable-device-integration-with-farmbeats"></a>Ermöglichen der Geräteintegration mit FarmBeats 

Nachdem Sie das Streamen von Sensordaten gestartet haben, können Sie mit dem Abrufen der Daten für Ihr FarmBeats-System beginnen. Geben Sie die folgenden Informationen an Ihren Geräteanbieter weiter, um die Integration in FarmBeats zu ermöglichen:

 - API-Endpunkt
 - Mandanten-ID
 - Client-ID
 - Geheimer Clientschlüssel
 - EventHub-Verbindungszeichenfolge

Die obigen Informationen erhalten Sie von Ihrem Systemintegrator. Wenden Sie sich an Ihren Systemintegrator, falls Sie Probleme beim Aktivieren der Geräteintegrationen haben.

Alternativ können Sie die Anmeldeinformationen auch generieren, indem Sie dieses Skript über Azure Cloud Shell ausführen. Führen Sie folgende Schritte durch:

1. Laden Sie die [ZIP-Datei](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie sie auf Ihrem lokalen Laufwerk. In der ZIP-Datei befinden sich zwei Dateien.
2. Melden Sie sich bei https://portal.azure.com/ an, und öffnen Sie Cloud Shell. Diese Option ist auf der Symbolleiste in der rechten oberen Ecke des Azure-Portals verfügbar.

    ![Symbolleiste im Azure-Portal](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Vergewissern Sie sich, dass die Umgebung auf **PowerShell** festgelegt ist. Standardmäßig ist sie auf „Bash“ festgelegt.

    ![Einstellung „PowerShell“ auf der Symbolleiste](./media/get-sensor-data-from-sensor-partner/power-shell-new-1.png)

4. Laden Sie die beiden Dateien aus dem ersten Schritt in Ihre Cloud Shell-Instanz hoch.

    ![Uploadschaltfläche auf der Symbolleiste](./media/get-sensor-data-from-sensor-partner/power-shell-two-1.png)

5. Navigieren Sie zum Uploadverzeichnis der Dateien. Die Dateien werden standardmäßig unter dem Benutzernamen in das Basisverzeichnis hochgeladen.
6. Führen Sie das folgende Skript aus:

    ```azurepowershell-interactive 

    ./generateCredentials.ps1   

    ```
7. Folgen Sie den Anweisungen auf dem Bildschirm, um die Werte für **API-Endpunkt**, **Mandanten-ID**, **Client-ID**, **Geheimer Clientschlüssel** und **EventHub-Verbindungszeichenfolge** zu erfassen. Die EventHub-Verbindungszeichenfolge ist in der API-Antwort in Swagger enthalten.

### <a name="integrate-device-data-by-using-the-generated-credentials"></a>Integrieren von Gerätedaten unter Verwendung der generierten Anmeldeinformationen

Navigieren Sie zum Portal des Gerätepartners, um unter Verwendung der Anmeldeinformationen, die Sie im vorherigen Abschnitt generiert haben, eine Verknüpfung mit FarmBeats einzurichten:

 - API-Endpunkt
 - EventHub-Verbindungszeichenfolge
 - Client-ID
 - Geheimer Clientschlüssel
 - Mandanten-ID

 Die erfolgreiche Integration wird vom Geräteanbieter bestätigt. Nach Erhalt der Bestätigung können Sie in Azure FarmBeats alle Geräte und Sensoren anzeigen.

## <a name="view-devices-and-sensors"></a>Anzeigen von Geräten und Sensoren

Verwenden Sie den folgenden Abschnitt, um die Geräte und Sensoren Ihres landwirtschaftlichen Betriebs anzuzeigen.

### <a name="view-devices"></a>Anzeigen von Geräten

Für FarmBeats werden derzeit folgende Geräte unterstützt:

- **Knoten:** Ein Gerät, das mit mindestens einem Sensor verknüpft ist.
- **Gateway**: Ein Gerät, das mit mindestens einem Knoten verknüpft ist.

Führen Sie folgende Schritte durch:

1. Wählen Sie auf der Startseite im Menü die Option **Geräte** aus.
  Auf der Seite **Geräte** werden Typ, Modell und Status des Geräts, der zugehörige landwirtschaftliche Betrieb und das letzte Aktualisierungsdatum der Metadaten angezeigt. Die Spalte für den landwirtschaftlichen Betrieb ist standardmäßig auf *NULL* festgelegt. Sie können angeben, dass ein Gerät einem landwirtschaftlichen Betrieb zugewiesen werden soll. Weitere Informationen finden Sie unter [Zuweisen von Geräten](#assign-devices).
2. Wählen Sie das Gerät aus, um die Geräteeigenschaften, Telemetriedaten und untergeordneten Geräte anzuzeigen, die mit dem Gerät verbunden sind.

    ![Seite "Geräte"](./media/get-sensor-data-from-sensor-partner/view-devices-1.png)

### <a name="view-sensors"></a>Anzeigen von Sensoren

Führen Sie folgende Schritte durch:

1. Wählen Sie auf der Startseite im Menü die Option **Sensoren** aus.
  Auf der Seite **Sensoren** werden Details wie Sensortyp, zugehöriger landwirtschaftlicher Betrieb, übergeordnetes Gerät, Portname, Porttyp und letzter aktualisierter Status angezeigt.
2. Wählen Sie den Sensor aus, um Sensoreigenschaften, aktive Warnungen und Telemetriedaten für den Sensor anzuzeigen.

    ![Seite „Sensoren“](./media/get-sensor-data-from-sensor-partner/view-sensors-1.png)

## <a name="assign-devices"></a>Zuweisen von Geräten  

Die eingehenden Sensordaten können dem landwirtschaftlichen Betrieb zugewiesen werden, für den Sie die Sensoren bereitgestellt haben.

1. Wählen Sie im Menü auf der Startseite die Option **Farms** (Landwirtschaftliche Betriebe) aus. Die Seite **Farms** (Landwirtschaftliche Betriebe) wird angezeigt.
2. Wählen Sie den landwirtschaftlichen Betrieb aus, dem Sie das Gerät zuweisen möchten, und wählen Sie **Geräte hinzufügen** aus.
3. Das Fenster **Geräte hinzufügen** wird angezeigt. Wählen Sie das Gerät aus, das Sie dem landwirtschaftlichen Betrieb zuweisen möchten.

    ![Fenster „Geräte hinzufügen“](./media/get-sensor-data-from-sensor-partner/add-devices-1.png)

4. Wählen Sie **Geräte hinzufügen** aus. Alternativ können Sie zum Menü **Geräte** navigieren, die Geräte auswählen, die einem landwirtschaftlichen Betrieb zugewiesen werden sollen, und dann **Geräte zuordnen** auswählen.
5. Wählen Sie im Fenster **Geräte zuordnen** in der Dropdownliste den landwirtschaftlichen Betrieb und dann **Auf alle anwenden** aus, um den landwirtschaftlichen Betrieb allen ausgewählten Geräten zuzuordnen.

    ![Fenster „Geräte zuordnen“](./media/get-sensor-data-from-sensor-partner/associate-devices-1.png)

6. Wenn Sie die einzelnen Geräte jeweils einem anderen landwirtschaftlichen Betrieb zuordnen möchten, wählen Sie in der Spalte **Assign to Farm** (Landwirtschaftlichem Betrieb zuweisen) den Dropdownpfeil und anschließend einen landwirtschaftlichen Betrieb für jede Gerätezeile aus.
7. Wählen Sie **Zuweisen** aus, um die Gerätezuweisung abzuschließen.

### <a name="visualize-sensor-data"></a>Visualisieren von Sensordaten

Führen Sie folgende Schritte durch:

1. Wählen Sie auf der Startseite im Menü die Option **Farms** (Landwirtschaftliche Betriebe) aus, um die Seite **Farms** (Landwirtschaftliche Betriebe) anzuzeigen.
2. Wählen Sie den **landwirtschaftlichen Betrieb** aus, für den Sie die Sensordaten anzeigen möchten.
3. Im Dashboard **Farm** (Landwirtschaftlicher Betrieb) können Sie die Telemetriedaten anzeigen. Sie können auch Livetelemetriedaten anzeigen oder **Benutzerdefinierter Bereich** verwenden, um einen bestimmten Datumsbereich für die Anzeige anzugeben.

    ![Dashboard für den landwirtschaftlichen Betrieb](./media/get-sensor-data-from-sensor-partner/telemetry-data-1.png)

## <a name="delete-a-sensor"></a>Löschen eines Sensors

Führen Sie folgende Schritte durch:

1. Wählen Sie auf der Startseite im Menü die Option **Sensoren** aus, um die Seite **Sensoren** zu öffnen.
2. Wählen Sie das zu löschende Gerät und anschließend im Bestätigungsfenster die Option **Löschen** aus.

    ![Schaltfläche „Löschen“](./media/get-sensor-data-from-sensor-partner/delete-sensors-1.png)

Eine Bestätigungsmeldung mit dem Hinweis, dass das Löschen des Sensors erfolgreich war, wird angezeigt.

## <a name="delete-devices"></a>Löschen von Geräten

Führen Sie folgende Schritte durch:

1. Wählen Sie im Menü auf der Startseite die Option **Geräte** aus, um die Seite **Geräte** zu öffnen.
2. Wählen Sie das zu löschende Gerät und anschließend im Bestätigungsfenster die Option **Löschen** aus.

    ![Schaltfläche „Löschen“](./media/get-sensor-data-from-sensor-partner/delete-device-1.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun die Übertragung von Sensordaten an Ihre Azure FarmBeats-Instanz eingerichtet. Als Nächstes können Sie sich damit vertraut machen, wie Sie [Karten für Ihre landwirtschaftlichen Betriebe generieren](generate-maps.md#generate-maps).
