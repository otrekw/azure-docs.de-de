---
title: Abrufen von Wetterdaten von Partnern für Wetterdaten
description: In diesem Artikel wird beschrieben, wie Sie Wetterdaten von Partnern abrufen.
author: sunasing
ms.topic: article
ms.date: 03/31/2020
ms.author: sunasing
ms.openlocfilehash: 66fa4e7d3edf839ab1e497e86362bcfc979dc279
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "81265070"
---
# <a name="get-weather-data-from-weather-partners"></a>Abrufen von Wetterdaten von Partnern für Wetterdaten

Azure FarmBeats ermöglicht Ihnen das Abrufen der Wetterdaten von Ihren entsprechenden Anbietern, indem Sie ein Docker-basiertes Connector Framework nutzen. Bei Verwendung dieses Frameworks wird von den Anbietern der Wetterdaten eine Docker-Instanz implementiert, die in FarmBeats integriert werden kann. Derzeit werden die folgenden Anbieter von Wetterdaten unterstützt:

[NOAA-Daten über Azure Open Datasets](https://azure.microsoft.com/services/open-datasets/)

Die Wetterdaten können verwendet werden, um verwertbare Erkenntnisse zu generieren und KI/ML-Modelle für FarmBeats zu entwickeln.

## <a name="before-you-start"></a>Vorbereitung

Stellen Sie zum Abrufen von Wetterdaten sicher, dass Sie FarmBeats installiert haben. **Die Integration von Wetterdaten wird in Version 1.2.11 oder höher unterstützt**. Informationen zur Installation von Azure FarmBeats finden Sie unter [Installieren von Azure FarmBeats](https://aka.ms/farmbeatsinstalldocumentation).

## <a name="enable-weather-integration-with-farmbeats"></a>Aktivieren der Integration von Wetterdaten für FarmBeats

Führen Sie die folgenden Schritte aus, um mit dem Abrufen von Wetterdaten auf Ihrem FarmBeats-Datenhub zu beginnen:

1. Navigieren Sie zu Ihrem FarmBeats-Datenhub-Swagger (https://yourdatahub.azurewebsites.net/swagger).

2. Navigieren Sie zur Partner-API, und senden Sie eine POST-Anforderung mit der folgenden Eingabenutzlast:

   ```json
   {  
 
     "dockerDetails": {  
       "credentials": { 
         "username": "<credentials to access private docker - not required for public docker>", 
         "password": "<credentials to access private docker – not required for public docker>"  
       },  
       "imageName" : "<docker image name. Default is azurefarmbeats/fambeats-noaa>",
       "imageTag" : "<docker image tag, default:latest>",
       "azureBatchVMDetails": {  
         "batchVMSKU" : "<VM SKU. Default is standard_d2_v2>",  
         "dedicatedComputerNodes" : 1,
         "nodeAgentSKUID": "<Node SKU. Default is batch.node.ubuntu 18.04>"  
       }
     },
     "partnerCredentials": {  
       "key1": "value1",  
       "key2": "value2"  
     },  
     "partnerType": "Weather",  
     "name": "<Name of the partner>",  
     "description": "<Description>",
     "properties": {  }  
   }  
   ```

   Verwenden Sie beispielsweise die unten angegebene Nutzlast, um NOAA-Wetterdaten über Azure Open Datasets abzurufen. Sie können den Namen und die Beschreibung wie gewünscht ändern.

   ```json
   {
 
     "dockerDetails": {
       "imageName": "azurefarmbeats/farmbeats-noaa",
       "imageTag": "latest",
       "azureBatchVMDetails": {
         "batchVMSKU": "standard_d2_v2",
         "dedicatedComputerNodes": 1,
         "nodeAgentSKUID": "batch.node.ubuntu 18.04"
       }
     },
     "partnerType": "Weather",
     "name": "ods-noaa",
     "description": "NOAA data from Azure Open Datasets registered as a Weather Partner"
   }  
   ```

   > [!NOTE]
   > Weitere Informationen zum Partner-Objekt finden Sie im [Anhang](get-weather-data-from-weather-partner.md#appendix).

   Mit dem vorherigen Schritt werden die Ressourcen bereitgestellt, um für die Docker-Instanz die Ausführung in der FarmBeats-Umgebung des Kunden zu ermöglichen.  

   Es dauert ca. 10 bis 15 Minuten, um die obigen Ressourcen bereitzustellen.

3. Überprüfen Sie den Status des Partner-Objekts, das Sie in Schritt 2 erstellt haben. Führen Sie hierzu eine GET-Anforderung für die Partner-API durch, und überprüfen Sie den **Status** des Partner-Objekts. Nachdem der Partner von FarmBeats erfolgreich bereitgestellt wurde, wird der Status auf **Aktiv** festgelegt.

4. Navigieren Sie zur JobType-API, und führen Sie dafür eine GET-Anforderung durch. Suchen Sie nach den Wetteraufträgen, die in Schritt 2 beim Hinzufügen des Partners erstellt wurden. Das Feld **pipelineName** in den Wetteraufträgen hat das folgende Format: „partner-name_partner-type_job-name“.

5. Nun verfügt Ihre FarmBeats-Instanz über einen aktiven Partner für Wetterdaten und kann Aufträge ausführen, um Wetterdaten für einen bestimmten Ort (Breiten-/Längengrad) und einen Datumsbereich anzufordern. Die Auftragstypen (JobTypes) enthalten Details dazu, welche Parameter für die Ausführung von Wetteraufträgen benötigt werden.

   Für NOAA-Daten über Azure Open Datasets werden beispielsweise die folgenden Auftragstypen erstellt:

   - get_weather_data (ISD/Wetterdatenverlauf abrufen)
   - get_weather_forecast_data (GFS/Wetterdatenvorhersage abrufen)

6. Notieren Sie sich die **ID** und die Parameter der Auftragstypen.

7. Navigieren Sie zur Jobs-API, und senden Sie für „Jobs“ eine POST-Anforderung mit der folgenden Eingabenutzlast:

   ```json
    {
         "typeId": "<id of the JobType>",
         "arguments": {
           "additionalProp1": {},
           "additionalProp2": {},
           "additionalProp3": {}
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
       }
   ```

   Verwenden Sie beispielsweise die folgende Nutzlast, um **get_weather_data** auszuführen:

   ```json
   {
 
         "typeId": "<id of the JobType>",
         "arguments": {
               "latitude": 47.620422,
               "longitude": -122.349358,
               "start_date": "yyyy-mm-dd",
               "end_date": "yyyy-mm-dd"
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
   }
   ```

8. Im obigen Schritt werden die Wetteraufträge gemäß der Definition auf der Docker-Instanz des Partners ausgeführt, und die Wetterdaten werden in FarmBeats erfasst. Sie können den Status des Auftrags überprüfen, indem Sie eine GET-Anforderung für „Jobs“ senden und in der Antwort nach **currentState** suchen. Nach Abschluss des Vorgangs wird „currentState“ auf **Succeeded** (Erfolgreich) festgelegt.

## <a name="query-ingested-weather-data"></a>Abfragen von erfassten Wetterdaten

Nach Abschluss der Wetteraufträge können Sie die erfassten Wetterdaten abfragen, um Modelle zu entwickeln oder verwertbare Erkenntnisse zu gewinnen. Es gibt zwei Möglichkeiten, auf die Wetterdaten in FarmBeats zuzugreifen und diese abzufragen:

- API und
- Time Series Insights (TSI).

### <a name="query-using-rest-api"></a>Abfragen über die REST-API

Führen Sie die folgenden Schritte aus, um Wetterdaten mit der FarmBeats-REST-API abzufragen:

1. Navigieren Sie in Ihrem FarmBeats-Datenhub-Swagger (https://yourdatahub.azurewebsites.net/swagger) zur WeatherDataLocation-API, und führen Sie eine GET-Anforderung durch. Die Antwort enthält WeatherDataLocation-Objekte, die für den Standort (Breiten-/Längengrad) erstellt wurden, der bei der Ausführung des Auftrags angegeben wurde. Notieren Sie sich die **ID** und die **weatherDataModelId** der Objekte.

2. Führen Sie wie in Schritt 1 beschrieben mit der WeatherDataModel-API für die **weatherDataModelId** einen „GET/{id}“-Vorgang durch. Das „Wetterdatenmodell“ enthält alle Metadaten und Details zu den erfassten Wetterdaten. **Weather Measure** (Wettermessgröße) im Objekt **Weather Data Model** (Wetterdatenmodell) enthält beispielsweise Details dazu, welche Wetterinformationen unterstützt und welche Typen und Einheiten jeweils genutzt werden. Beispiel:

   ```json
   {
       "name": "Temperature <name of the weather measure - this is what we will receive as part of the queried weather data>",
       "dataType": "Double <Data Type - eg. Double, Enum>",
       "type": "AmbientTemperature <Type of measure eg. AmbientTemperature, Wind speed etc.>",
       "unit": "Celsius <Unit of measure eg. Celsius, Percentage etc.>",
       "aggregationType": "None <either of None, Average, Maximum, Minimum, StandardDeviation, Sum, Total>",
       "description": "<Description of the measure>"
   }
   ```

   Notieren Sie sich die Antwort auf den Aufruf von „GET/{id}“ für „Weather Data Model“ (Wetterdatenmodell).

3. Navigieren Sie zur **Telemetry**-API, und senden Sie eine POST-Anforderung mit der folgenden Eingabenutzlast:

   ```json
   {
       "weatherDataLocationId": "<id from step 1 above>",
     "searchSpan": {
       "from": "2020-XX-XXT07:30:00Z",
       "to": "2020-XX-XXT07:45:00Z"
     }
   }
   ```

4. Die Antwort mit den Wetterdaten, die für den angegebenen Zeitbereich verfügbar sind, sieht wie folgt aus:

   ```json
   {
     "timestamps": [
       "2020-XX-XXT07:30:00Z",
       "2020-XX-XXT07:45:00Z"
     ],
     "properties": [
       {
         "values": [
           "<id of the weatherDataLocation>",
           "<id of the weatherDataLocation>"
         ],
         "name": "Id",
         "type": "String"
       },
       {
         "values": [
           29.1,
           30.2
         ],
         "name": "Temperature <name of the WeatherMeasure as defined in the WeatherDataModel object>",
         "type": "Double <Data Type of the value - eg. Double>"
       }
     ]
   }
   ```

Im obigen Beispiel hat die Antwort Daten für zwei Zeitstempel sowie die Messgrößenbezeichnung („Temperature“) und die Werte der gemeldeten Wetterdaten in den beiden Zeitstempeln enthalten. Sie müssen auf das zugehörige Wetterdatenmodell verweisen (wie oben in Schritt 2 beschrieben), um den Typ und die Einheit der gemeldeten Werte zu interpretieren.

### <a name="query-using-azure-time-series-insights-tsi"></a>Abfragen über Azure Time Series Insights (TSI)

FarmBeats nutzt [Azure Time Series Insights (TSI)](https://azure.microsoft.com/services/time-series-insights/) zum Erfassen, Speichern, Abfragen und Visualisieren von Daten im IoT-Bereich. Diese Daten sind hochgradig kontextualisiert und für Zeitreihen optimiert.

Wetterdaten werden in einem Event Hub empfangen und per Pushübertragung an eine TSI-Umgebung in der FarmBeats-Ressourcengruppe gesendet. Die Daten können dann direkt aus TSI abgefragt werden. Weitere Informationen finden Sie in der [TSI-Dokumentation](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).

Führen Sie die Schritte aus, um Daten in TSI visuell darzustellen:

1. Wechseln Sie zu **Azure Portal** > **FarmBeats-DataHub-Ressourcengruppe**. Wählen Sie die **Time Series Insights**-Umgebung (tsi-xxxx) und dann **Datenzugriffsrichtlinien** aus. Fügen Sie einen Benutzer mit den Zugriffberechtigungen „Leser“ oder „Mitwirkender“ hinzu.

2. Wechseln Sie zur Seite **Übersicht** der **Time Series Insights**-Umgebung (tsi-xxxx), und wählen Sie die **Time Series Insights-Explorer-URL** aus. Sie können die erfassten Wetterdaten nun visualisieren.

Neben dem Speichern, Abfragen und Visualisieren von Wetterdaten ermöglicht TSI auch die Integration in ein Power BI-Dashboard. Weitere Informationen finden Sie unter [Visualisieren der Daten von Time Series Insights in Power BI](https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi).

## <a name="appendix"></a>Anhang

|        Partner   |  Details   |
| ------- | -------             |
|     DockerDetails – imageName         |          Name des Docker-Images. Beispielsweise „docker.io/azurefarmbeats/farmbeats-noaa“ (Image unter „hub.docker.com“) ODER „myazureacr.azurecr.io/mydockerimage“ (Image in Azure Container Registry) usw. Wenn keine Registrierung angegeben wird, wird standardmäßig „hub.docker.com“ verwendet.      |
|          DockerDetails – imageTag             |         Der Tagname des Docker-Images. Die Standardeinstellung ist „latest“.     |
|  DockerDetails – credentials      |  Die Anmeldeinformationen zum Zugreifen auf die private Docker-Instanz. Wird vom Partner für den Kunden bereitgestellt.   |
|  DockerDetails – azureBatchVMDetails – batchVMSKU     |    Azure Batch-VM-SKU. Informationen zu allen virtuellen Linux-Computern finden Sie [hier](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Gültige Werte sind: 'Small', 'ExtraLarge', 'Large', 'A8', 'A9', 'Medium', 'A5', 'A6', 'A7', 'STANDARD_D1', 'STANDARD_D2', 'STANDARD_D3', 'STANDARD_D4', 'STANDARD_D11', 'STANDARD_D12', 'STANDARD_D13', 'STANDARD_D14', 'A10', 'A11', 'STANDARD_D1_V2', 'STANDARD_D2_V2', 'STANDARD_D3_V2', 'STANDARD_D4_V2', 'STANDARD_D11_V2', 'STANDARD_D12_V2', 'STANDARD_D13_V2', 'STANDARD_D14_V2', 'STANDARD_G1', 'STANDARD_G2', 'STANDARD_G3', 'STANDARD_G4', 'STANDARD_G5', 'STANDARD_D5_V2', 'BASIC_A1', 'BASIC_A2', 'BASIC_A3', 'BASIC_A4', 'STANDARD_A1', 'STANDARD_A2', 'STANDARD_A3', 'STANDARD_A4', 'STANDARD_A5', 'STANDARD_A6', 'STANDARD_A7', 'STANDARD_A8', 'STANDARD_A9', 'STANDARD_A10', 'STANDARD_A11', 'STANDARD_D15_V2', 'STANDARD_F1', 'STANDARD_F2', 'STANDARD_F4', 'STANDARD_F8', 'STANDARD_F16', 'STANDARD_NV6', 'STANDARD_NV12', 'STANDARD_NV24', 'STANDARD_NC6', 'STANDARD_NC12', 'STANDARD_NC24', 'STANDARD_NC24r', 'STANDARD_H8', 'STANDARD_H8m', 'STANDARD_H16', 'STANDARD_H16m', 'STANDARD_H16mr', 'STANDARD_H16r', 'STANDARD_A1_V2', 'STANDARD_A2_V2', 'STANDARD_A4_V2', 'STANDARD_A8_V2', 'STANDARD_A2m_V2', 'STANDARD_A4m_V2', 'STANDARD_A8m_V2', 'STANDARD_M64ms', 'STANDARD_M128s', 'STANDARD_D2_V3'. **Die Standardeinstellung ist „standard_d2_v2“.**  |
|    DockerDetails – azureBatchVMDetails – dedicatedComputerNodes   |  Anzahl von dedizierten Computerknoten für den Batchpool. Der Standardwert ist 1. |
|    DockerDetails – azureBatchVMDetails – nodeAgentSKUID          |    SKU-ID des Azure Batch-Knoten-Agents. Derzeit wird nur der Batch-Knoten-Agent „batch.node.ubuntu 18.04“ unterstützt.    |
| DockerDetails – partnerCredentials | Anmeldeinformationen für das Aufrufen der Partner-API in Docker. Der Partner muss diese Informationen basierend auf dem unterstützten Authentifizierungsmechanismus an die Kunden weitergeben. Beispiel: Benutzername/Kennwort oder API-Schlüssel. |
| partnerType | „Weather“ (andere Partnertypen in FarmBeats sind „Sensor“ und „Imagery“)  |
|  name   |   Gewünschter Name des Partners im FarmBeats-System.   |
|  description |  BESCHREIBUNG   |

## <a name="next-steps"></a>Nächste Schritte

Sie haben nun Sensordaten von Ihrer Azure FarmBeats-Instanz abgefragt. Als Nächstes können Sie sich damit vertraut machen, wie Sie [Karten für Ihre landwirtschaftlichen Betriebe generieren](generate-maps-in-azure-farmbeats.md#generate-maps).
