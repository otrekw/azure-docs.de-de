---
title: Integration von Partnern für Wetterdaten
description: In diesem Artikel wird die Vorgehensweise zur Integration eines Anbieters von Wetterdaten mit FarmBeats beschrieben.
author: sunasing
ms.topic: article
ms.date: 07/09/2020
ms.author: sunasing
ms.openlocfilehash: a2677b5343b2d65a39e7c9f6d5006db599c1ac73
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496994"
---
# <a name="weather-partner-integration"></a>Integration von Partnern für Wetterdaten

In diesem Artikel erfahren Sie mehr über die Docker-Komponente **Connector** von Azure FarmBeats, die von Anbietern von Wetterdaten zur Integration mit FarmBeats entwickelt werden kann. Dabei werden die Wetterdaten über APIs an FarmBeats gesendet. Sobald die Daten in FarmBeats verfügbar sind, können sie zur Datenfusion und zum Erstellen von Modellen für maschinelles Lernen/künstliche Intelligenz verwendet werden.

 > [!NOTE]
 > Im Rahmen dieser Dokumentation wird eine Referenzimplementierung verwendet, die auf NOAA in Azure Open Datasets basiert. Sie ist verfügbar unter [https://github.com/azurefarmbeats/noaa_docker](https://github.com/azurefarmbeats/noaa_docker).
 > Das entsprechende Docker-Image finden Sie unter [https://hub.docker.com/r/azurefarmbeats/farmbeats-noaa](https://hub.docker.com/r/azurefarmbeats/farmbeats-noaa).

Ein Partner für Wetterdaten muss ein Docker-Image/-Programm (mit den unten aufgeführten Spezifikationen) bereitstellen und dieses in einer Containerregistrierung hosten, auf die die Kunden zugreifen können. Partner für Wetterdaten müssen ihren Kunden die folgenden Informationen bereitstellen:

- URL des Docker-Images
- Docker-Imagetag
- Schlüssel/Anmeldeinformationen für den Zugriff auf das Docker-Image
- Kundenspezifische API-Schlüssel/-Anmeldeinformationen für den Zugriff auf die Wetterdaten des Partnersystems
- SKU-Details zu virtuellen Computern (können von den Partnern bereitgestellt werden, falls für die Docker-Instanz bestimmte VM-Anforderungen gelten, andernfalls können die Kunden aus den in Azure unterstützten VM-SKUs wählen)

Unter Verwendung der aufgelisteten Docker-Informationen können sich die Kunden bei einem Partner für Wetterdaten in ihrer FarmBeats-Instanz registrieren. Weitere Informationen zur Erfassung von Wetterdaten in FarmBeats mithilfe von Docker finden Sie im Leitfaden [Abrufen von Wetterdaten](https://docs.microsoft.com/azure/industry/agriculture/get-weather-data-from-weather-partner).

## <a name="connector-docker-development"></a>Entwickeln der Docker-Komponente „Connector“

**REST-API-basierte Integration**

Die FarmBeats-APIs enthalten eine technische Swagger-Dokumentation. Informationen zu allen APIs und den entsprechenden Anforderungen oder Antworten finden Sie unter [FarmBeats-Swagger](https://aka.ms/farmbeatsswagger). 

Wenn Sie FarmBeats installiert haben können Sie unter `https://yourfarmbeatswebsitename-api.azurewebsites.net/swagger` auf Ihren FarmBeats-Swagger zugreifen.

Beachten Sie, dass dem Namen Ihrer FarmBeats-Website die Endung „-api“ angehängt wird.
Der API-Endpunkt lautet `https://yourfarmbeatswebsitename-api.azurewebsites.net`.

### <a name="datahub-lib"></a>Datenhub-Bibliothek

In FarmBeats wird eine Bibliothek bereitgestellt, die von den Partnern für Wetterdaten genutzt werden kann. Diese Bibliothek steht derzeit als Teil der [Referenzimplementierung](https://github.com/azurefarmbeats/noaa_docker/tree/master/datahub_lib) zur Verfügung. Später wird sie als SDK in mehreren Sprachen verfügbar sein.

### <a name="authentication"></a>Authentifizierung

**Authentifizierung über FarmBeats-APIs**

Für die in FarmBeats verwendete Bearerauthentifizierung erfolgt der Zugriff auf die APIs durch Bereitstellen eines Zugriffstokens im Headerabschnitt der Anforderung:

```
headers = *{"Authorization": "Bearer " + access_token, …}*
```

Das Zugriffstoken wird von einer Azure-Funktion angefordert, die in der FarmBeats-Instanz des Kunden ausgeführt wird. Die URL der Azure-Funktion wird dem Docker-Programm als Argument bereitgestellt, und das Zugriffstoken kann über eine GET-Anforderung für die URL abgerufen werden. Die Antwort der URL enthält dann das Zugriffstoken. Die Datenhub-Bibliothek enthält Hilfsfunktionen für den Abruf des Zugriffstokens durch die Partner. Ausführlichere Informationen finden Sie [hier](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_auth_helper.py).

Das Zugriffstoken ist nur einige Stunden gültig und muss nach Ablauf erneut angefordert werden.

**Authentifizierung über APIs auf Partnerseite**

Damit sich Kunden während der Docker-Ausführung über die APIs auf Partnerseite authentifizieren können, müssen sie während der Partnerregistrierung die Anmeldeinformationen auf folgende Weise bereitstellen:

```json
{
 "partnerCredentials": {
   "key1": "value1",
   "key2": "value2"
   }
}
```
Dieses Wörterbuch wird vom API-Dienst serialisiert und in einem [Schlüsseltresor](https://docs.microsoft.com/azure/key-vault/basic-concepts) gespeichert.

Mithilfe von [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) werden Wetteraufträge orchestriert und die Ressourcen für die Ausführung des Docker-Codes gestartet. Zudem enthält Azure Data Factory einen Mechanismus, um die Daten sicher per Push auf den virtuellen Computer zu übertragen, auf dem der Docker-Auftrag ausgeführt wird. Die in einem Schlüsseltresor gespeicherten API-Anmeldeinformationen werden als sichere Zeichenfolgen aus dem Schlüsseltresor gelesen und als erweiterte Eigenschaften im Arbeitsverzeichnis des Docker-Containers über die Datei „activity.json“ verfügbar gemacht (der Dateipfad lautet „/mnt/working_dir/activity.json“). Die Anmeldeinformationen können vom Docker-Code während der Laufzeit aus dieser Datei gelesen werden, um im Namen des Kunden auf die APIs auf Partnerseite zuzugreifen. Die Anmeldeinformationen sind in der Datei in der folgenden Form verfügbar:

```json
{ 
   "typeProperties" : {
      "extendedProperties" : { 
         "partnerCredentials": "" } 
   } 
}
```
Beachten Sie, dass die Anmeldeinformationen „partnerCredentials“ genau so angezeigt werden, wie sie vom Kunden bei der Partnerregistrierung angegeben wurden.

Die FarmBeats-Bibliothek enthält Hilfsfunktionen für die Partner zum Lesen der Anmeldeinformationen aus den Aktivitätseigenschaften. Ausführlichere Informationen finden Sie [hier](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/auth/partner_adf_helper.py).

Die Lebensdauer der Datei erstreckt sich nur über die Zeit der Ausführung des Docker-Codes. Nach Abschluss der Docker-Ausführung wird die Datei gelöscht.

Weitere Informationen zur Funktionsweise von ADF-Pipelines und -Aktivitäten finden Sie unter [https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping](https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping).

**HTTP-Anforderungsheader**

Die folgenden gängigsten Anforderungsheader müssen für einen API-Aufruf an die FarmBeats-Instanz angegeben werden:

**Kopfzeile** | **Beschreibung und Beispiel**
--- | ---
Content-Type | Das Anforderungsformat (Content-Type: application/<format>). Für FarmBeats-Datenhub-APIs wird das Format „JSON“ verwendet. Content-Type: application/json
Authorization | Gibt das Zugriffstoken an, das zum Ausführen eines API-Aufrufs erforderlich ist. Autorisierung: Bearer <Access-Token>
Akzeptieren | Das Antwortformat. Für FarmBeats-Datenhub-APIs wird das Format „JSON“ verwendet. Accept: application/json

## <a name="data-format"></a>Datenformat

JSON ist ein gängiges sprachunabhängiges Datenformat, bei dem eine einfache Textdarstellung beliebiger Datenstrukturen bereitgestellt wird. Weitere Informationen finden Sie unter [json.org](http://json.org).

## <a name="docker-specifications"></a>Docker-Spezifikationen

Das Docker-Programm muss zwei Komponenten aufweisen: **Bootstrap** und **Aufträge**. Es können auch mehrere Aufträge vorhanden sein.

### <a name="bootstrap"></a>Bootstrap

Diese Komponente wird beim Initiieren der Docker-Registrierung durch den Kunden in FarmBeats ausgeführt. Dabei werden dem Programm die folgenden Argumente (arg1, arg2) übergeben:

- Endpunkt der FarmBeats-API: FarmBeats-API-Endpunkt für API-Anforderungen: Dies ist der Endpunkt für API-Aufrufe an die FarmBeats-Bereitstellung.
- URL der Azure-Funktion: Dies ist Ihr persönlicher Endpunkt, über den Sie das Zugriffstoken für die FarmBeats-APIs erhalten. Durch Aufrufen einer GET-Anforderung für die URL erhalten Sie das Zugriffstoken als Antwort.

Bootstrap ist für das Erstellen der erforderlichen Metadaten zuständig, damit die Benutzer Ihre Aufträge ausführen können, um die Wetterdaten abzurufen. Weitere Informationen finden Sie in der [Referenzimplementierung](https://github.com/azurefarmbeats/noaa_docker). Sie können die Datei „bootstrap_manifest.json“ Ihren Wünschen entsprechend aktualisieren, und das Bootstrap-Referenzprogramm erstellt anschließend die erforderlichen Metadaten.

Die folgenden Metadaten werden im Rahmen dieses Vorgangs erstellt. 

 > [!NOTE]
 > **Beachten Sie**: Wenn Sie die Datei „bootstrap_manifest.json“ wie in der [Referenzimplementierung](https://github.com/azurefarmbeats/noaa_docker) beschrieben aktualisieren, müssen Sie die unten aufgeführten Metadaten nicht erstellen. Diese werden von Bootstrap auf der Grundlage der Manifestdatei erstellt.

- /**WeatherDataModel**:  Das Modell „WeatherDataModel“ dient der Darstellung von Wetterdaten und entspricht verschiedenen Datasets, die von der Quelle bereitgestellt werden. Das Modell „DailyForecastSimpleModel“ stellt beispielsweise Informationen zur durchschnittlichen Temperatur, Luftfeuchtigkeit und Niederschlagsmenge bereit, während das Modell „DailyForecastAdvancedModel“ einige weitere Informationen in Stundenintervallen umfasst. Sie können beliebig viele WeatherDataModels-Modelle erstellen.
- /**JobType**: FarmBeats verfügt über ein erweiterbares Auftragsverwaltungssystem. Als Anbieter von Wetterdaten können Sie Ihre verschiedenen Datasets/APIs (z. B. „GetDailyForecasts“) in FarmBeats als Aufträge vom Typ „JobType“ aktivieren. Kunden können einen Auftrag vom Typ „JobType“, den Sie erstellt haben, auslösen, um Wetterdaten für einen bestimmten Standort/landwirtschaftlichen Betrieb abzurufen (weitere Informationen hierzu finden Sie unter[FarmBeats-Swagger](https://aka.ms/farmbeatsswagger) in den APIs „JobType“ und „Job“).

### <a name="jobs"></a>Aufträge

Diese Komponente wird aufgerufen, wenn ein FarmBeats-Benutzer einen Auftrag vom Typ „/JobType“ ausführt, den Sie als Teil des Bootstrap-Vorgangs erstellt haben. Der Befehl zum Ausführen von Docker für den Auftrag wird als Teil von **/JobType** definiert, den Sie erstellt haben.
- Der Auftrag umfasst das Abrufen von Daten aus der Quelle und deren Übertragung per Push an FarmBeats. Die für das Abrufen der Daten erforderlichen Parameter sollten im Bootstrap-Vorgang als Teil von „/JobType“ definiert werden.
- Im Rahmen des Auftrags muss das Programm den Standort **/WeatherDataLocation** basierend auf dem Modell „/WeatherDataLocation“ erstellen, das während des Bootstrap-Vorgangs erstellt wurde. Dabei entspricht **/WeatherDataLocation** dem Standort (Längen-/Breitengrad), der vom Benutzer als Parameter für den Auftrag angegeben wurde.

### <a name="details-of-the-objects"></a>Details der Objekte

  WeatherDataModel | Beschreibung |
  --- | ---
  Name  | Name des Wetterdatenmodells |
  BESCHREIBUNG  | Aussagekräftige Beschreibung des Modells |
  Eigenschaften  | Zusätzliche Eigenschaften, die vom Datenanbieter definiert werden |
  weatherMeasures > Name  | Name der Wettermessgröße, z. B. „humidity_max“ |
  weatherMeasures > DataType  | „Double“ oder „Enum“. Für den Enum-Typ ist „measureEnumDefinition“ erforderlich. |
  weatherMeasures > measureEnumDefinition  | Nur erforderlich, wenn „DataType“ vom Typ „Enum“ ist, z. B. { „NoRain“: 0, „Snow“: 1, „Drizzle“: 2, „Rain“: 3 } |
  weatherMeasures > Type  | Typ der Wettertelemetriedaten, z. B. „RelativeHumidity“ Systemdefinierte Typen: AmbientTemperature, NoUnit, CO2, Depth, ElectricalConductivity, LeafWetness, Length, LiquidLevel, Nitrate, O2, PH, Phosphate, PointInTime, Potassium, Pressure, RainGauge, RelativeHumidity, Salinity, SoilMoisture, SoilTemperature, SolarRadiation, State, TimeDuration, UVRadiation, UVIndex, Volume, WindDirection, WindRun, WindSpeed, Evapotranspiration, PAR. Informationen zum Hinzufügen weiterer Typen finden Sie in der API „/ExtendedType“ oder im Abschnitt [Hinzufügen von Typen und Einheiten](weather-partner-integration-in-azure-farmbeats.md#add-extendedtype).
  weatherMeasures > Unit | Einheit der Wettertelemetriedaten Systemdefinierte Einheiten: NoUnit, Celsius, Fahrenheit, Kelvin, Rankine, Pascal, Mercury, PSI, MilliMeter, CentiMeter, Meter, Inch, Feet, Mile, KiloMeter, MilesPerHour, MilesPerSecond, KMPerHour, KMPerSecond, MetersPerHour, MetersPerSecond, Degree, WattsPerSquareMeter, KiloWattsPerSquareMeter, MilliWattsPerSquareCentiMeter, MilliJoulesPerSquareCentiMeter, VolumetricWaterContent, Percentage, PartsPerMillion, MicroMol, MicroMolesPerLiter, SiemensPerSquareMeterPerMole, MilliSiemensPerCentiMeter, Centibar, DeciSiemensPerMeter, KiloPascal, VolumetricIonContent, Liter, MilliLiter, Seconds, UnixTimestamp, MicroMolPerMeterSquaredPerSecond und InchesPerHour. Informationen zum Hinzufügen weiterer Einheiten finden Sie in der API „/ExtendedType“ oder im Abschnitt [Hinzufügen von Typen und Einheiten](weather-partner-integration-in-azure-farmbeats.md#add-extendedtype).
  weatherMeasures > AggregationType  | „None“, „Average“, „Maximum“, „Minimum“, „StandardDeviation“, „Sum“ oder „Total“
  weatherMeasures > Depth  | Die Tiefe des Sensors in Zentimetern. Beispielsweise die Messung der Feuchtigkeit 10 cm unter der Erde.
  weatherMeasures > Description  | Geben Sie eine aussagekräftige Beschreibung der Messung an. |
  **JobType** | **Beschreibung** |
  Name  | Name des Auftrags, z. B. „Get_Daily_Forecast“, den der Kunde zum Abrufen der Wetterdaten ausführt|
  pipelineDetails > parameters > name  | Name des Parameters |
  pipelineDetails > parameters > type | „String“, „Int“, „Float“, „Bool“ oder „Array“ |
  pipelineDetails > parameters > isRequired | Boolescher Wert: TRUE (Standardwert) für erforderliche Parameter, FALSE für nicht erforderliche Parameter |
  pipelineDetails > parameters > defaultValue | Standardwert des Parameters |
  pipelineDetails > parameters > description | Beschreibung des Parameters |
  Eigenschaften  | Zusätzliche Eigenschaften des Herstellers
  Properties > **programRunCommand** | Befehl zum Ausführen von Docker: Dieser Befehl wird beim Ausführen des Wetterauftrags durch den Kunden ausgeführt. |
  **WeatherDataLocation** | **Beschreibung** |
  weatherDataModelId  | ID des entsprechenden Modells vom Typ „WeatherDataModel“, das während des Bootstrap-Vorgangs erstellt wurde|
  location  | Breitengrad, Längengrad und Höhe |
  Name | Name des Objekts |
  BESCHREIBUNG | BESCHREIBUNG |
  farmId | **Optional**: ID des landwirtschaftlichen Betriebs, wird vom Kunden als Parameter für den Auftrag bereitgestellt. |
  Eigenschaften  | Zusätzliche Eigenschaften des Herstellers

 Informationen zu den einzelnen Objekten und ihren Eigenschaften finden Sie unter [Swagger](https://aka.ms/FarmBeatsSwagger).

 > [!NOTE]
 > Die APIs geben eindeutige IDs für jede erstellte Instanz zurück. Diese ID muss vom Translator für die Geräteverwaltung und die Synchronisierung von Metadaten beibehalten werden.

**Synchronisierung von Metadaten**

Die Docker-Komponente „Connector“ sollte Aktualisierungen für die Metadaten senden können. Beispiele für Updateszenarios sind das Hinzufügen neuer Wetterparameter im Dataset des Anbieters von Wetterdaten oder das Hinzufügen von Funktionen (z. B. 30-Tage-Vorhersage).

> [!NOTE]
> Das Löschen von Metadaten, z. B. von Wetterdatenmodellen, wird nicht unterstützt.
>
> Zum Aktualisieren der Metadaten ist es zwingend erforderlich, „/Get/{ID}“ für das Wetterdatenmodell aufzurufen, die geänderten Eigenschaften zu aktualisieren und anschließend „/Put/{ID}“ zu senden, damit keine vom Benutzer festgelegten Eigenschaften verloren gehen.

## <a name="weather-data-telemetry-specifications"></a>Spezifikationen für Wetterdaten (Telemetriedaten)

Die Wetterdaten werden einer kanonischen Nachricht zugeordnet, die zur Verarbeitung per Push in eine Azure Event Hub-Instanz übertragen wird. Der Azure Event Hub-Dienst ermöglicht die Erfassung von Echtzeitdaten (Telemetrie) von verbundenen Geräten und Anwendungen. Wenn Sie Wetterdaten an FarmBeats senden möchten, müssen Sie einen Client erstellen, der Nachrichten an eine Event Hub-Instanz in FarmBeats sendet. Weitere Informationen zum Senden von Telemetriedaten finden Sie unter [Senden von Telemetriedaten an eine Event Hub-Instanz](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send).

Im Folgenden ein Beispiel für Python-Code, mit dem Telemetriedaten als Client an einen angegebenen Event Hub gesendet werden.

```python
import azure
from azure.eventhub import EventHubClient, Sender, EventData, Receiver, Offset
EVENTHUBCONNECTIONSTRING = "<EventHub Connection String provided by customer>"
EVENTHUBNAME = "<EventHub Name provided by customer>"

write_client = EventHubClient.from_connection_string(EVENTHUBCONNECTIONSTRING, eventhub=EVENTHUBNAME, debug=False)
sender = write_client.add_sender(partition="0")
write_client.run()
for i in range(5):
    telemetry = "<Canonical Telemetry message>"
    print("Sending telemetry: " + telemetry)
    sender.send(EventData(telemetry))
write_client.stop()

```

Das kanonische Nachrichtenformat lautet wie folgt:

```json
{
   "weatherstations": [
   {
   "id": "id of the WeatherDataLocation",
   "weatherdata": [
   {
     "timestamp": "timestamp of the data. For historical, this is the time for which the observations are sent. For forecast this is the time for which data is forecasted. Format is ISO 8601. Default time-zone is UTC",
     "predictiontimestamp": "timestamp on which the forecast data is predicted i.e time of prediction. Required only for forecast data. Format is ISO 8601. Default timezone is UTC ",
     "weathermeasurename1": <value>,
     "weathermeasurename2": <value>
     }
     ]
    }
   ]
}
```

Dies wäre beispielsweise eine Telemetrienachricht:

```json
{
   "weatherstations": [
   {
     "id": "5e4b34e7-bf9e-4f39-xxxx-f3c06d0366ea",
     "weatherdata": [
     {
      "timestamp": "2019-07-10T00:00:00Z",
      "predictiontimestamp": "2019-07-05T00:00:00Z",
      "PrecipitationTotalLiquidEquivalent": 0,
      "AvgPressure": 0,
      "AvgRelativeHumidity": 72
     }
    ] 
  }
 ]
}

```

## <a name="troubleshooting-and-error-management"></a>Problembehandlung und Fehlerverwaltung

**Fehlerprotokollierung**

Da der Partnerauftrag im vorhandenen Auftragsframework ausgeführt wird, werden Fehler auf die gleiche Weise protokolliert wie Fehler in bereits vorhandenen Aufträgen in FarmBeats (z. B. „GetFarmData“, „SensorPlacement“ usw.). Die in der ADF-Pipeline ausgeführte ADF-Aktivität protokolliert sowohl STDERR- als auch STDOUT-Dateien. Beide Dateien sind in der FarmBeats-Ressourcengruppe im Speicherkonto „datahublogs-xxx“ verfügbar.

Die Datenhub-Bibliothek enthält Hilfsfunktionen zur Protokollerstellung als Teil des Datenhub-Gesamtprotokolls. Ausführlichere Informationen finden Sie [hier](https://github.com/azurefarmbeats/noaa_docker/blob/master/datahub_lib/framework/logger.py).

**Problembehandlungsoption oder Support**

Falls der Kunde keine Wetterdaten in der angegebenen FarmBeats-Instanz empfangen kann, muss der Partner für Wetterdaten Support sowie einen Mechanismus zur Problembehandlung bereitstellen.

## <a name="add-extendedtype"></a>Hinzufügen von „ExtendedType“

FarmBeats unterstützt das Hinzufügen neuer Messungstypen und Maßeinheiten für Sensoren. Beachten Sie, dass Partner für Wetterdaten neue Einheiten/Typen durch Aktualisieren der Datei „bootstrap_manifest.json“ in der [Referenzimplementierung](https://github.com/azurefarmbeats/noaa_docker) hinzufügen können.

Führen Sie die folgenden Schritte aus, um eine neue Messgröße vom Typ „WeatherMeasure“ (z. B. „PrecipitationDepth“) hinzuzufügen:

1. Senden Sie für „/ExtendedType“ eine GET-Anforderung, und verwenden Sie als Abfragefilter den Schlüssel „WeatherMeasureType“.
2. Notieren Sie sich die ID des zurückgegebenen Objekts.
3. Fügen Sie der Liste im zurückgegebenen Objekt den neuen Typ hinzu, und senden Sie für „/ExtendedType{ID}“ eine PUT-Anforderung mit der neuen Liste. Die Eingabenutzlast sollte mit der zuvor empfangenen Antwort übereinstimmen, der am Ende der Liste mit den Werten die neue Einheit angehängt wurde.

Weitere Informationen zur API „/ExtendedType“ finden Sie unter [Swagger](https://aka.ms/FarmBeatsSwagger).

## <a name="next-steps"></a>Nächste Schritte

Sie verfügen nun über die Docker-Komponente „Connector“, die mit FarmBeats integriert ist. Als Nächstes erfahren Sie, wie Sie Wetterdaten mithilfe von Docker in FarmBeats abrufen. Fahren Sie hierzu mit dem Artikel [Abrufen von Wetterdaten](get-weather-data-from-weather-partner.md) fort.
