---
title: Problembehandlung in Azure FarmBeats
description: In diesem Artikel wird beschrieben, wie Sie Probleme in Azure FarmBeats behandeln.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: fb4b06eca0d6df6848e2e215d8890569701f7596
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705614"
---
# <a name="troubleshoot"></a>Problembehandlung

In diesem Artikel finden Sie Lösungen für häufige Probleme mit Azure FarmBeats.

Wenn Sie weitere Hilfe benötigen, wenden Sie sich an uns unter farmbeatssupport@microsoft.com. Stellen Sie sicher, dass Sie die Datei **deployer.log** in Ihre E-Mail einschließen.

Gehen Sie zum Herunterladen der Datei **deployer.log** wie folgt vor:

1. Melden Sie sich beim **Azure-Portal** an, und wählen Sie Ihr Abonnement und den Azure AD-Mandanten aus.
2. Starten Sie Cloud Shell über den oberen Navigationsbereich im Azure-Portal.
3. Wählen Sie **Bash** als bevorzugte Cloud Shell-Erfahrung aus.
4. Wählen Sie das hervorgehobene Symbol aus, und wählen Sie dann in der Dropdownliste **Herunterladen** aus.

    ![FarmBeats-Projekt](./media/troubleshoot-azure-farmbeats/download-deployer-log-1.png)

5. Geben Sie im nächsten Bereich den Pfad zu Ihrer **deployer.log**-Datei ein. Geben Sie beispielsweise **farmbeats-deployer.log** ein.

## <a name="sensor-telemetry"></a>Sensortelemetrie

### <a name="cant-view-telemetry-data"></a>Telemetriedaten können nicht angezeigt werden

**Symptom**: Geräte oder Sensoren sind bereitgestellt, und Sie haben FarmBeats mit Ihrem Gerätepartner verknüpft, können aber keine Telemetriedaten aus FarmBeats abrufen oder anzeigen.

**Korrekturmaßnahme**:

1. Wechseln Sie zur Ressourcengruppe für Ihren FarmBeats-Datenhub.   
2. Wählen Sie den **Event Hub** (DatafeedEventHubNamespace) aus, und überprüfen Sie die Anzahl eingehender Nachrichten.
3. Führen Sie einen der folgenden Schritte aus:   
   - Wenn *keine eingehenden Nachrichten* vorhanden sind, wenden Sie sich an Ihren Gerätepartner.  
   - Wenn *eingehende Nachrichten* vorhanden sind, wenden Sie sich an farmbeatssupport@microsoft.com. Fügen Sie Ihre Datahub- und Accelerator-Protokolle sowie die erfassen Telemetriedaten an.

Informationen zum Herunterladen von Protokollen finden Sie im Abschnitt [Manuelles Erfassen von Protokollen](#collect-logs-manually).  

### <a name="cant-view-telemetry-data-after-ingesting-historicalstreaming-data-from-your-sensors"></a>Sie können keine Telemetriedaten anzeigen, nachdem Sie historische/Streamingdaten von Ihren Sensoren erfasst haben.

**Symptom**: Geräte oder Sensoren werden bereitgestellt, und Sie haben die Geräte/Sensoren in FarmBeats erstellt und Telemetrie erfasst und an EventHub gesendet, aber Sie können keine Telemetriedaten in FarmBeats abrufen oder anzeigen.

**Korrekturmaßnahme**:

1. Stellen Sie sicher, dass Sie die Partnerregistrierung korrekt durchgeführt haben. Um dies zu überprüfen, wechseln Sie zu Ihrem Datenhub-Swagger, navigieren zur Partner-API, führen ein „Get“ aus und sehen nach, ob der Partner registriert ist. Falls nicht, befolgen Sie die [hier beschriebenen Schritte](get-sensor-data-from-sensor-partner.md#enable-device-integration-with-farmbeats), um den Partner hinzuzufügen.
2. Stellen Sie sicher, dass Sie das richtige Telemetriemeldungsformat verwendet haben:

```json
{
"deviceid": "<id of the Device created>",
"timestamp": "<timestamp in ISO 8601 format>",
"version" : "1",
"sensors": [
    {
      "id": "<id of the sensor created>",
      "sensordata": [
        {
          "timestamp": "< timestamp in ISO 8601 format >",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        },
        {
          "timestamp": "<timestamp in ISO 8601 format>",
          "<sensor measure name (as defined in the Sensor Model)>": <value>
        }
      ]
    }
 ]
}
```

### <a name="dont-have-the-azure-event-hubs-connection-string"></a>Die Azure Event Hubs-Verbindungszeichenfolge liegt nicht vor

**Korrekturmaßnahme**:

1. Wechseln Sie in Datahub Swagger zur Partner-API.
2. Wählen Sie **Abrufen** > **Testen** > **Ausführen** aus.
3. Notieren Sie sich die Partner-ID des Sensorpartners, an dem Sie interessiert sind.
4. Wechseln Sie zurück zur Partner-API, und wählen Sie **GET/\<ID>** aus.
5. Geben Sie die Partner-ID aus Schritt 3 an, und wählen Sie dann **Ausführen** aus.

   Die API-Antwort sollte die Event Hubs-Verbindungszeichenfolge enthalten.

### <a name="device-appears-offline"></a>Das Gerät wird als offline angezeigt

**Symptome:** Geräte sind installiert, und Sie haben FarmBeats mit Ihrem Gerätepartner verknüpft. Die Geräte sind online und senden Telemetriedaten, werden aber als offline angezeigt.

**Korrekturmaßnahme**: Für dieses Gerät ist kein Berichtsintervall konfiguriert. Um das Berichtsintervall festzulegen, wenden Sie sich an den Hersteller Ihres Geräts. 

### <a name="error-deleting-a-device"></a>Fehler beim Löschen eines Geräts

Während Sie ein Gerät löschen, tritt möglicherweise eines der folgenden häufigen Fehlerszenarios auf:  

**Meldung**: „Auf das Gerät wird in Sensoren verwiesen: Dem Gerät ist mindestens ein Sensor zugeordnet. Löschen Sie den Sensor bzw. die Sensoren und dann das Gerät.“  

**Bedeutung**: Dem Gerät sind mehrere in dem landwirtschaftlichen Betrieb bereitgestellte Sensoren zugeordnet.   

**Korrekturmaßnahme**:  

1. Löschen Sie die Sensoren, die dem Gerät über den Accelerator zugeordnet sind.  
2. Falls Sie die Sensoren einem anderen Gerät zuordnen möchten, bitten Sie Ihren Gerätepartner, dasselbe zu tun.  
3. Löschen Sie das Gerät mithilfe eines `DELETE API`-Aufrufs, und legen Sie den force-Parameter auf *true* fest.  

**Meldung**: „Auf das Gerät wird in Geräten als ParentDeviceId verwiesen: Diesem Gerät sind ein oder mehrere Geräte als untergeordnete Geräte zugeordnet. Löschen Sie diese Geräte, und löschen Sie dann das Gerät.“  

**Bedeutung**: Ihrem Gerät sind andere Geräte zugeordnet.  

**Korrekturmaßnahme**:

1. Löschen Sie die Geräte, die diesem spezifischen Gerät zugeordnet sind.  
2. Löschen Sie das spezifische Gerät.  

    > [!NOTE]
    > Sie können ein Gerät nicht löschen, solange diesem Sensoren zugeordnet sind. Weitere Informationen zum Löschen von zugeordneten Sensoren finden Sie im Abschnitt „Löschen von Sensoren“ von [Abrufen von Sensordaten von Sensorpartnern](get-sensor-data-from-sensor-partner.md).


## <a name="issues-with-jobs"></a>Probleme mit Aufträgen

### <a name="farmbeats-internal-error"></a>Interner FarmBeats-Fehler

**Meldung**: „Interner FarmBeats-Fehler. Weitere Informationen finden Sie im Leitfaden zur Problembehandlung.“

**Korrekturmaßnahme**: Dieses Problem könnte durch einen temporären Ausfall der Datenpipeline verursacht werden. Erstellen Sie den Auftrag erneut. Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum unter Erwähnung der Fehlermeldung, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

## <a name="accelerator-troubleshooting"></a>Problembehandlung bei Accelerators

### <a name="access-control"></a>Zugriffssteuerung

**Problem:** Beim Hinzufügen einer Rollenzuweisung erhalten Sie eine Fehlermeldung.

**Meldung**: „Es wurden keine übereinstimmenden Benutzer gefunden.“

**Korrekturmaßnahme**: Überprüfen Sie die E-Mail-ID, für die Sie eine Rollenzuweisung hinzufügen möchten. Die E-Mail-ID muss exakt mit der ID übereinstimmen, die für diesen Benutzer in Active Directory registriert ist. Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum unter Erwähnung der Fehlermeldung, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

### <a name="unable-to-log-in-to-accelerator"></a>Anmeldung beim Accelerator nicht möglich

**Meldung**: „Fehler: Sie sind nicht zum Aufrufen des Diensts berechtigt. Wenden Sie sich an den Administrator, um die entsprechende Autorisierung zu erhalten.“

**Korrekturmaßnahme**: Bitten Sie den Administrator, Sie für den Zugriff auf die FarmBeats-Bereitstellung zu autorisieren. Dies kann mit einer POST-Anweisung der RoleAssignment-APIs oder über die Zugriffssteuerung im Bereich **Einstellungen** im Accelerator erfolgen.  

Wenn Ihnen bereits der Zugriff gewährt wurde und dieser Fehler auftritt, versuchen Sie es erneut, indem Sie die Seite aktualisieren. Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum unter Erwähnung der Fehlermeldung, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

![FarmBeats-Projekt](./media/troubleshoot-azure-farmbeats/accelerator-troubleshooting-1.png)

### <a name="accelerator-issues"></a>Accelerator-Probleme  

**Problem:** Sie haben eine Accelerator-Fehlermeldung unbestimmter Ursache empfangen.

**Meldung**: „Fehler: Unbekannter Fehler.“

**Korrekturmaßnahme**: Dieser Fehler tritt auf, wenn die Seite zu lange im Leerlauf belassen wurde. Aktualisieren Sie die Seite.  

Wenn der Fehler weiterhin besteht, stellen Sie eine entsprechende Frage im FarmBeats-Forum unter Erwähnung der Fehlermeldung, oder wenden Sie sich an FarmBeatsSupport@microsoft.com.

**Problem:** Der FarmBeats-Accelerator zeigt auch nach einem Upgrade von FarmBeatsDeployment nicht die neueste Version an.

**Korrekturmaßnahme**: Dieser Fehler tritt auf, weil ein Dienstworker im Browser persistent gespeichert wurde. Gehen Sie folgendermaßen vor:
1. Schließen Sie alle Registerkarten des Browsers, in denen der Accelerator geöffnet ist, und schließen Sie das Browserfenster.
2. Starten Sie eine neue Instanz des Browsers, und laden Sie den Accelerator-URI neu. Mit dieser Aktion wird die neue Accelerator-Version geladen.

## <a name="sentinel-imagery-related-issues"></a>Sentinel: Probleme im Zusammenhang mit Bildern

### <a name="wrong-username-or-password"></a>Falscher Benutzername oder falsches Kennwort

**Auftragsfehlermeldung**: „Für den Zugriff auf diese Ressource ist eine vollständige Authentifizierung erforderlich.“

**Korrekturmaßnahme**:

Führen Sie eines der folgenden Verfahren aus:
- Führen Sie das Installationsprogramm erneut aus, um ein Upgrade des Datenhub mit dem richtigen Benutzernamen und Kennwort durchzuführen.
- Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie dann, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-hub-wrongurlor-site-not-accessible"></a>Sentinel-Hub: Falsche URL oder kein Zugriff auf die Site möglich 

**Auftragsfehlermeldung**: „Leider ist ein Problem aufgetreten. Die Seite, auf die Sie versucht haben zuzugreifen, ist (vorübergehend) nicht verfügbar.“ 

**Korrekturmaßnahme**:
1. Öffnen Sie [Sentinel](https://scihub.copernicus.eu/dhus/) in Ihrem Browser, um zu sehen, ob auf die Website zugegriffen werden kann. 
2. Wenn nicht auf die Website zugegriffen werden kann, überprüfen Sie, ob eine Firewall, ein Unternehmensnetzwerk oder eine andere blockierende Software den Zugriff auf die Website verhindert, und führen Sie dann die erforderlichen Schritte aus, um die Sentinel-URL zuzulassen. 
3. Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie dann, ob der Auftrag erfolgreich ausgeführt wird.  

### <a name="sentinel-server-down-for-maintenance"></a>Sentinel-Server: Zu Wartungszwecken heruntergefahren

**Auftragsfehlermeldung**: „Der Copernicus Open Access-Hub ist bald wieder verfügbar! Wir führen derzeit Wartungsarbeiten durch. Der Server ist in Kürze wieder online.“ 

**Korrekturmaßnahme**:

Dieses Problem kann auftreten, wenn auf dem Sentinel-Server Wartungsaktivitäten ausgeführt werden.

1. Wenn ein Auftrag oder eine Pipeline wegen Wartungsarbeiten fehlschlägt, übermitteln Sie den Auftrag nach einiger Zeit erneut. 

   Informationen zu geplanten oder ungeplanten Wartungsaktivitäten für Sentinel finden Sie auf der Site [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/).  
2. Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie dann, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-maximum-number-of-connections-reached"></a>Sentinel: Maximale Anzahl von Verbindungen erreicht

**Auftragsfehlermeldung**: „Die maximale Anzahl von zwei gleichzeitigen Flows wurde durch den Benutzer ‚\<Benutzername>‘ erreicht.“

**Bedeutung**: Wenn ein Auftrag fehlschlägt, weil die maximale Anzahl von Verbindungen erreicht wurde, wird dasselbe Sentinel-Konto in einer anderen Softwarebereitstellung verwendet.

**Korrekturmaßnahme**: Probieren Sie einen der folgenden Schritte aus:
* Erstellen Sie ein neues Sentinel-Konto, und führen Sie dann das Installationsprogramm erneut aus, um ein Upgrade des Datenhub mit einem neuen Benutzernamen und Kennwort für Sentinel vorzunehmen.  
* Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie dann, ob der Auftrag erfolgreich ausgeführt wird.

### <a name="sentinel-server-refused-connection"></a>Sentinel-Server: Verbindung verweigert 

**Auftragsfehlermeldung**: „Der Server hat die Verbindung unter http://172.30.175.69:8983/solr/dhus verweigert.“ 

**Korrekturmaßnahme**: Dieses Problem kann auftreten, wenn auf dem Sentinel-Server Wartungsaktivitäten ausgeführt werden. 
1. Wenn ein Auftrag oder eine Pipeline wegen Wartungsarbeiten fehlschlägt, übermitteln Sie den Auftrag nach einiger Zeit erneut. 

   Informationen zu geplanten oder ungeplanten Wartungsaktivitäten für Sentinel finden Sie auf der Site [Copernicus Open Access Hub News](https://scihub.copernicus.eu/news/).  
2. Führen Sie den fehlerhaften Auftrag erneut aus, oder führen Sie einen Satellitenauftrag zur Indizierung für einen Datumsbereich von 5–7 Tagen aus, und überprüfen Sie dann, ob der Auftrag erfolgreich ausgeführt wird.

## <a name="collect-logs-manually"></a>Manuelles Erfassen von Protokollen

[Installieren Sie Azure Storage-Explorer, und stellen Sie ihn bereit]( https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

### <a name="collect-azure-data-factory-job-logs-in-datahub"></a>Erfassen von Azure Data Factory-Auftragsprotokollen in Datahub
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie im Feld **Suche** nach der Ressourcengruppe für den FarmBeats-Datenhub.

    > [!NOTE]
    > Wählen Sie die Datenhub-Ressourcengruppe aus, die Sie bei der Installation von FarmBeats angegeben haben.

3. Suchen Sie im Dashboard **Ressourcengruppe** nach dem Speicherkonto *datahublogs\** . Suchen Sie beispielsweise nach **datahublogsmvxmq**.  
4. Wählen Sie in der Spalte **Name** das Speicherkonto aus, um das Dashboard **Speicherkonto** anzuzeigen.
5. Wählen Sie im Bereich **datahublogs\*** die Option **In Explorer öffnen** aus, um die Anwendung **Azure Storage-Explorer öffnen** anzuzeigen.
6. Wählen Sie im linken Bereich **Blobcontainer** aus und dann **job-logs**.
7. Wählen Sie im Bereich **job-logs** die Option **Herunterladen** aus.
8. Laden Sie die Protokolle in einen lokalen Ordner auf Ihrem Computer herunter.
9. Senden Sie die heruntergeladene ZIP-Datei per E-Mail an farmbeatssupport@microsoft.com.

    ![FarmBeats-Projekt](./media/troubleshoot-azure-farmbeats/collecting-logs-manually-1.png)

### <a name="collect-azure-data-factory-job-logs-in-accelerator"></a>Erfassen von Azure Data Factory-Auftragsprotokollen in Accelerator

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie im Feld **Suche** nach der Ressourcengruppe für den FarmBeats-Accelerator.

    > [!NOTE]
    > Wählen Sie die Accelerator-Ressourcengruppe aus, die Sie bei der Installation von FarmBeats angegeben haben.

3. Suchen Sie im Dashboard **Ressourcengruppe** nach dem Speicherkonto *storage\** . Suchen Sie beispielsweise nach **storagedop4k\*** .
4. Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
5. Wählen Sie im Bereich **storage\*** die Option **In Explorer öffnen** aus, um die Anwendung Azure Storage-Explorer zu öffnen.
6. Wählen Sie im linken Bereich **Blobcontainer** aus und dann **job-logs**.
7. Wählen Sie im Bereich **job-logs** die Option **Herunterladen** aus.
8. Laden Sie die Protokolle in einen lokalen Ordner auf Ihrem Computer herunter.
9. Senden Sie die heruntergeladene ZIP-Datei per E-Mail an farmbeatssupport@microsoft.com.


### <a name="collect-datahub-app-service-logs"></a>Erfassen von App-Dienstprotokollen für Datenhubs

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie im Feld **Suche** nach der Ressourcengruppe für den FarmBeats-Datenhub.

    > [!NOTE]
    > Wählen Sie die Datenhub-Ressourcengruppe aus, die Sie bei der Installation von FarmBeats angegeben haben.

3. Suchen Sie in der Ressourcengruppe nach dem Speicherkonto *datahublogs\** . Suchen Sie beispielsweise nach **fordatahublogsmvxmq\*** .
4. Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
5. Wählen Sie im Bereich **datahublogs\*** die Option **In Explorer öffnen** aus, um die Anwendung Azure Storage-Explorer anzuzeigen.
6. Wählen Sie im linken Bereich **Blobcontainer** aus und dann **appinsights-logs**.
7. Wählen Sie im Bereich **appinsights-logs** die Option **Herunterladen** aus.
8. Laden Sie die Protokolle in einen lokalen Ordner auf Ihrem Computer herunter.
9. Senden Sie die heruntergeladene ZIP-Datei per E-Mail an farmbeatssupport@microsoft.com.

### <a name="collect-accelerator-app-service-logs"></a>Erfassen von App-Dienstprotokollen für Accelerators

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie im Feld **Suche** nach der Ressourcengruppe für den FarmBeats-Accelerator.

    > [!NOTE]
    > Wählen Sie die FarmBeats-Accelerator-Ressourcengruppe aus, die bei der Installation von FarmBeats angegeben wurde.

3. Suchen Sie in der Ressourcengruppe nach dem Speicherkonto *storage\** . Suchen Sie beispielsweise nach **storagedop4k\*** .
4. Wählen Sie das Speicherkonto in der Spalte **Name** aus, um das Dashboard **Speicherkonto** anzuzeigen.
5. Wählen Sie im Bereich **storage\*** die Option **In Explorer öffnen** aus, um die Anwendung Azure Storage-Explorer zu öffnen.
6. Wählen Sie im linken Bereich **Blobcontainer** aus und dann **appinsights-logs**.
7. Wählen Sie im Bereich **appinsights-logs** die Option **Herunterladen** aus.
8. Laden Sie die Protokolle in einen lokalen Ordner auf Ihrem Computer herunter.
9. Senden Sie den heruntergeladenen Ordner per E-Mail an farmbeatssupport@microsoft.com.

## <a name="known-issues"></a>Bekannte Probleme

## <a name="batch-related-issues"></a>Probleme im Zusammenhang mit Batch

**Fehlermeldung**: „Das regionale Kontokontingent für Batch-Konten wurde für das angegebene Abonnement erreicht.“

**Korrekturmaßnahme**: Erhöhen Sie das Kontingent, oder löschen Sie die nicht verwendeten Batch-Konten, und führen Sie die Bereitstellung erneut aus.

### <a name="azure-active-directory-azure-ad-related-issues"></a>Probleme im Zusammenhang mit Azure Active Directory (Azure AD)

**Fehlermeldung**: „Die erforderlichen Einstellungen für die Azure AD-App ‚d41axx40-xx21-4fbd-8xxf-97xxx9e2xxc0‘ konnten nicht aktualisiert werden: Nicht genügend Berechtigungen zum Abschließen des Vorgangs. Stellen Sie sicher, dass die oben genannten Einstellungen ordnungsgemäß für die Azure AD-App konfiguriert wurden.“

**Bedeutung**: Die Konfiguration der Azure AD-App-Registrierung wurde nicht ordnungsgemäß abgeschlossen.  

**Korrekturmaßnahme**: Bitten Sie den IT-Administrator (die Person mit Lesezugriff auf den Mandanten), unser [Skript](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect/tree/master/AppCreationScripts) zu verwenden, um die Azure AD-App-Registrierung zu erstellen. Dieses Skript führt auch die Konfigurationsschritte automatisch aus.

**Fehlermeldung**: „Die neue Active Directory-Anwendung \<Anwendungsname\> konnte in diesem Mandanten nicht erstellt werden: Es ist bereits ein anderes Objekt mit dem gleichen Wert für die Eigenschaftenbezeichner-URIs vorhanden.“

**Bedeutung**: Es ist bereits eine Azure AD-App-Registrierung mit demselben Namen vorhanden.

**Korrekturmaßnahme**: Löschen Sie die vorhandene Azure AD-App-Registrierung, oder verwenden Sie sie erneut für die Installation. Wenn Sie die vorhandene Azure AD-App-Registrierung erneut verwenden, übergeben Sie die Anwendungs-ID und den geheimen Clientschlüssel an das Installationsprogramm, und führen Sie die Bereitstellung erneut aus.

## <a name="issues-with-the-inputjson-file"></a>Probleme mit der Datei „input.json“

**Fehler:** Fehler beim Lesen der Eingabe aus der Datei *input.json*.

**Korrekturmaßnahme**: Dieses Problem tritt in der Regel auf, weil bei der Angabe des richtigen Dateipfads oder Namens von *input.json* für das Installationsprogramm ein Fehler aufgetreten ist. Nehmen Sie die entsprechenden Änderungen vor, und versuchen Sie die Bereitstellung erneut.

**Fehler:** Fehler beim Analysieren von Werten in der Datei *input.json*.

**Korrekturmaßnahme**: Dieser Fehler tritt meistens aufgrund falscher Werte in der Datei *input.json* auf. Nehmen Sie die entsprechenden Änderungen vor, und versuchen Sie die Bereitstellung erneut.

## <a name="high-cpu-usage"></a>Hohe CPU-Auslastung

**Fehler:** Sie erhalten eine E-Mail-Benachrichtigung mit einer **Warnung zu einer hohen CPU-Auslastung**. 

**Korrekturmaßnahme**: 
1. Wechseln Sie zur Ressourcengruppe für Ihren FarmBeats-Datenhub.
2. Wählen Sie den **App-Dienst** aus.  
3. Wechseln Sie zur Seite [App Service-Preise](https://azure.microsoft.com/pricing/details/app-service/windows/) (zentrales Hochskalieren), und wählen Sie einen geeigneten Tarif aus.

## <a name="next-steps"></a>Nächste Schritte

Wenn weiterhin Probleme mit FamBeats auftreten, kontaktieren Sie unser [Supportforum](https://social.msdn.microsoft.com/Forums/home?forum=ProjectFarmBeats).
