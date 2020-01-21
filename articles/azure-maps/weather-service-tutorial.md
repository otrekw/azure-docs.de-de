---
title: 'Tutorial: Verknüpfen von Sensordaten mit Wettervorhersagedaten unter Verwendung von Azure Notebooks (Python) | Microsoft Azure Maps'
description: In diesem Tutorial erfahren Sie, wie Sie Sensordaten unter Verwendung von Azure Notebooks (Python) mit Wettervorhersagedaten des Microsoft Azure Maps-Wetterdiensts verknüpfen.
author: walsehgal
ms.author: v-musehg
ms.date: 12/09/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 1a1493033717b18bef5d80b28d06004c901ffb29
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/12/2020
ms.locfileid: "75910794"
---
# <a name="tutorial-join-sensor-data-with-weather-forecast-data-by-using-azure-notebooks-python"></a>Tutorial: Verknüpfen von Sensordaten mit Wettervorhersagedaten unter Verwendung von Azure Notebooks (Python)

Windenergie ist eine alternative Energiequelle zu fossilen Brennstoffen im Kampf gegen den Klimawandel. Da Wind naturgemäß nicht konsistent weht, müssen Betreiber von Windkraftanlagen ML-Modelle (Machine Learning) zur Vorhersage der Windenergiekapazität erstellen, um den Strombedarf zu decken und die Stabilität des Stromnetzes zu gewährleisten. In diesem Tutorial erfahren Sie Schritt für Schritt, wie Azure Maps-Wettervorhersagedaten mit Demo-Datasets von Sensorstandorten mit Wetterdaten kombiniert werden können. Wettervorhersagedaten werden durch Aufrufen des Azure Maps-Wetterdiensts angefordert.

In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Verwenden von Datendateien in [Azure Notebooks](https://docs.microsoft.com/azure/notebooks) in der Cloud
> * Laden von Demodaten aus einer Datei
> * Aufrufen von Azure Maps-REST-APIs in Python
> * Rendern von Standortdaten auf der Karte
> * Anreichern der Demodaten mit Azure Maps-Wetterdaten vom Typ [Tägliche Vorhersage](https://aka.ms/AzureMapsWeatherDailyForecast)
> * Zeichnen von Vorhersagedaten in Diagrammen


## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial sind folgende Vorbereitungen erforderlich:

1. Gehen Sie wie unter [Erstellen eines Kontos und Abrufen Ihres Schlüssels](quick-demo-map-app.md#create-an-account-with-azure-maps) beschrieben vor, um ein Azure Maps-Kontoabonnement im Tarif S0 zu erstellen.
2. Gehen Sie wie unter [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) beschrieben vor, um den primären Abonnementschlüssel für Ihr Konto abzurufen.


Weitere Einzelheiten zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

Unter [Erstellen eines Azure-Notebooks](https://docs.microsoft.com/azure/azure-maps/tutorial-ev-routing#create-an-azure-notebook) können Sie sich mit Azure-Notebooks und mit den ersten Schritten vertraut machen.

> [!Note]
> Die Jupyter Notebook-Datei für dieses Projekt kann aus dem [Jupyter Notebook-Repository für Wetterkarten](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/tree/master/AzureMapsJupyterSamples/Tutorials/Analyze%20Weather%20Data) heruntergeladen werden.

## <a name="load-the-required-modules-and-frameworks"></a>Laden der erforderlichen Module und Frameworks

Führen Sie das folgende Skript aus, um alle erforderlichen Module und Frameworks zu laden:

```python
import aiohttp
import pandas as pd
import datetime
from IPython.display import Image, display
```

## <a name="import-weather-data"></a>Importieren von Wetterdaten

Im Rahmen dieses Tutorials verwenden wir Wetterdaten von Sensoren, die an vier verschiedenen Windkraftanlagen installiert sind. Bei den Beispieldaten handelt es sich um Wetterdaten für einen Zeitraum von 30 Tagen, die von Wetterdatencentern in der Nähe der einzelnen Windkraftanlagenstandorte gesammelt wurden. In den Demodaten sind Messwerte für Temperatur, Windgeschwindigkeit und Windrichtung enthalten. Die Demodaten können [hier](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook/tree/master/AzureMapsJupyterSamples/Tutorials/Analyze%20Weather%20Data/data) heruntergeladen werden. Das folgende Skript importiert Demodaten in das Azure-Notebook:

```python
df = pd.read_csv("./data/weather_dataset_demo.csv")
```

## <a name="request-daily-forecast-data"></a>Anfordern täglicher Vorhersagedaten

In unserem Beispielszenario möchten wir eine tägliche Vorhersage für jeden Sensorstandort anfordern. Das folgende Skript ruft die [API für tägliche Vorhersagen](https://aka.ms/AzureMapsWeatherDailyForecast) des Azure Maps-Wetterdiensts auf, um für jede Windkraftanlage tägliche Wettervorhersagen für die nächsten 15 Tage ab dem aktuellen Datum zu erhalten:


```python
subscription_key = "Your Azure Maps key"

# Get a lists of unique station IDs and their coordinates 
station_ids = pd.unique(df[['StationID']].values.ravel())
coords = pd.unique(df[['latitude','longitude']].values.ravel())

years,months,days = [],[],[]
dates_check=set()
wind_speeds, wind_direction = [], []

# Call azure maps weather service to get daily forecast data for 15 days from current date
session = aiohttp.ClientSession()
j=-1
for i in range(0, len(coords), 2):
    wind_speeds.append([])
    wind_direction.append([])
    
    query = str(coords[i])+', '+str(coords[i+1])
    forecast_response = await(await session.get("https://atlas.microsoft.com/weather/forecast/daily/json?query={}&api-version=1.0&subscription-key={}&duration=15".format(query, subscription_key))).json()
    j+=1
    for day in range(len(forecast_response['forecasts'])):
            date = forecast_response['forecasts'][day]['date'][:10]
            wind_speeds[j].append(forecast_response['forecasts'][day]['day']['wind']['speed']['value'])
            wind_direction[j].append(forecast_response['forecasts'][day]['day']['windGust']['direction']['degrees'])
            
            if date not in dates_check:
                year,month,day= date.split('-')
                years.append(year)
                months.append(month)
                days.append(day)
                dates_check.add(date)
            
await session.close()
```

Das folgende Skript ruft den [Dienst zum Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage) von Azure Maps auf, um die Anlagenstandorte auf der Karte zu rendern:

```python
# Render the turbine locations on the map by calling the Azure Maps Get Map Image service
session = aiohttp.ClientSession()

pins="default|la-25+60|ls12|lc003C62|co9B2F15||'Location A'{} {}|'Location B'{} {}|'Location C'{} {}|'Location D'{} {}".format(coords[1],coords[0],coords[3],coords[2],coords[5],coords[4], coords[7],coords[6])

image_response = "https://atlas.microsoft.com/map/static/png?subscription-key={}&api-version=1.0&layer=basic&style=main&zoom=6&center={},{}&pins={}".format(subscription_key,coords[7],coords[6],pins)

static_map_response = await session.get(image_response)

poi_range_map = await static_map_response.content.read()

await session.close()

display(Image(poi_range_map))
```

![Anlagenstandorte](./media/weather-service-tutorial/location-map.png)


Um die Demodaten mit den Vorhersagedaten anzureichern, gruppieren wir die Vorhersagedaten mit den Demodaten auf der Grundlage der Stations-ID für das Wetterdatencenter.

```python
# Group forecasted data for all locations
df = df.reset_index(drop=True)
forecast_data = pd.DataFrame(columns=['StationID','latitude','longitude','Year','Month','Day','DryBulbCelsius','WetBulbFarenheit','WetBulbCelsius','DewPointFarenheit','DewPointCelsius','RelativeHumidity','WindSpeed','WindDirection'])

for i in range(len(station_ids)):
    loc_forecast = pd.DataFrame({'StationID':station_ids[i], 'latitude':coords[0], 'longitude':coords[1], 'Year':years, 'Month':months, 'Day':days, 'WindSpeed':wind_speeds[i], 'WindDirection':wind_direction[i]})
    forecast_data = pd.concat([forecast_data,loc_forecast], axis=0, sort=False)
    
combined_weather_data = pd.concat([df,forecast_data])
grouped_weather_data = combined_weather_data.groupby(['StationID'])
```

Die folgende Tabelle enthält die kombinierten historischen und vorhergesagten Daten für einen der Anlagenstandorte:

```python
# Display data for first location
grouped_weather_data.get_group(station_ids[0]).reset_index()
```

<center>

![Gruppierte Daten](./media/weather-service-tutorial/grouped-data.png)</center>

## <a name="plot-forecast-data"></a>Zeichnen von Vorhersagedaten

Um zu sehen, wie sich Windgeschwindigkeit und -richtung im Laufe der nächsten 15 Tage verändern, zeichnen wir die vorhergesagten Werte für die jeweiligen Tage.

```python
# Plot wind speed
curr_date = datetime.datetime.now().date()
windsPlot_df = pd.DataFrame({ 'Location A': wind_speeds[0], 'Location B': wind_speeds[1], 'Location C': wind_speeds[2], 'Location D': wind_speeds[3]}, index=pd.date_range(curr_date,periods=15))
windsPlot = windsPlot_df.plot.line()
windsPlot.set_xlabel("Date")
windsPlot.set_ylabel("Wind speed")
```

```python
#Plot wind direction 
windsPlot_df = pd.DataFrame({ 'Location A': wind_direction[0], 'Location B': wind_direction[1], 'Location C': wind_direction[2], 'Location D': wind_direction[3]}, index=pd.date_range(curr_date,periods=15))
windsPlot = windsPlot_df.plot.line()
windsPlot.set_xlabel("Date")
windsPlot.set_ylabel("Wind direction")
```

Die folgenden Diagramme zeigen die vorhergesagten Daten für die Änderung der Windgeschwindigkeit (linkes Diagramm) und der Windrichtung (rechtes Diagramm) in den nächsten 15 Tagen ab dem Tag, an dem die Daten angefordert werden:

<center>

![Diagramm für die Windgeschwindigkeit](./media/weather-service-tutorial/speed-date-plot.png) ![Diagramm für die Windrichtung](./media/weather-service-tutorial/direction-date-plot.png)</center>


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie Azure Maps-REST-APIs aufrufen, um Wettervorhersagedaten abzurufen und die Daten in Diagrammen zu visualisieren.

Weitere Informationen zum Aufrufen von Azure Maps-REST-APIs in Azure Notebooks finden Sie unter [Tutorial: Routenplanung für Elektrofahrzeuge mit Azure Notebooks (Python)](https://docs.microsoft.com/azure/azure-maps/tutorial-ev-routing).

Informationen zu den in diesem Tutorial verwendeten Azure Maps-APIs finden Sie unter:

* [Wetter: Abrufen einer täglichen Vorhersage (Vorschau)](https://aka.ms/AzureMapsWeatherDailyForecast)
* [Rendern: Abrufen des Kartenbilds](https://docs.microsoft.com/rest/api/maps/render/getmapimage)

Eine vollständige Liste mit den Azure Maps-REST-APIs finden Sie unter [Azure Maps-Dokumentation](https://docs.microsoft.com/azure/azure-maps/consumption-model).

Weitere Informationen zu Azure Notebooks finden Sie unter [Azure Notebooks-Dokumentation](https://docs.microsoft.com/azure/notebooks).
