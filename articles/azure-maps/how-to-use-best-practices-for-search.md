---
title: Effizientes Suchen mit dem Suchdienst von Azure Maps | Microsoft Azure Maps
description: Erfahren Sie, wie Sie mit Microsoft Azure Maps bewährte Verfahren für den Suchdienst anwenden.
author: walsehgal
ms.author: v-musehg
ms.date: 01/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 68c7408f13027ded7beaabf46fb663217a90c52b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845755"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Bewährte Methoden zum Verwenden des Suchdiensts von Azure Maps

Azure Maps [Search Service](https://docs.microsoft.com/rest/api/maps/search) enthält APIs mit verschiedenen Funktionen. Die API für die Adresssuche wird beispielsweise für die Suche nach Points of Interest (POI) oder Daten rund um einen bestimmten Ort verwendet. In diesem Artikel werden bewährte Vorgehensweisen zum Aufrufen von Daten aus Suchdiensten von Azure Maps beschrieben. Sie lernen Folgendes:

* Erstellen von Abfragen zum Zurückgeben relevanter Übereinstimmungen
* Einschränken von Suchergebnissen
* Kennenlernen der Unterschiede zwischen verschiedenen Ergebnistypen
* Lesen der Antwortstruktur der Adresssuche


## <a name="prerequisites"></a>Voraussetzungen

Um die Maps-Dienst-APIs aufrufen zu können, benötigen Sie ein Azure Maps-Konto mit zugehörigem Schlüssel. Falls erforderlich, befolgen Sie die Anweisungen unter [Erstellen eines Kontos](quick-demo-map-app.md#create-an-account-with-azure-maps) und [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account). Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

> [!Tip]
> Um den Suchdienst abzufragen, können Sie die [Postman-App](https://www.getpostman.com/apps) verwenden, um REST-Aufrufe zu erstellen, oder Sie können eine beliebige API-Entwicklungsumgebung verwenden, die Sie bevorzugen.


## <a name="best-practices-for-geocoding-address-search"></a>Bewährte Methoden für die Geocodierung (Adresssuche)

Wenn Sie mit dem Suchdienst von Azure Maps nach einer vollständigen oder unvollständigen Adresse suchen, liest die API Schlüsselwörter aus Ihrer Suchabfrage und gibt den Längen- und Breitengrad der Adresse zurück. Dieser Vorgang wird als „Geocodierung“ bezeichnet. Die Möglichkeit der Geocodierung in einem Land hängt von der Abdeckung der Straßendaten und der Geocodierungsgenauigkeit des Geocodierungsdiensts ab.

Weitere Informationen zu den Geocodierungsfunktionen von Azure Maps nach Ländern/Regionen finden Sie unter [Abdeckung durch Geocodierung](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage).

### <a name="limit-search-results"></a>Einschränken von Suchergebnissen

 Mit der Azure Maps-Such-API können Sie die Suchergebnisse wie gewünscht einschränken, um Ihren Benutzern relevante Daten anzuzeigen.

   > [!Note]
   > Unten sind nicht alle unterstützten Parameter für die Such-APIs aufgeführt.

   **Einschränken von Suchergebnissen auf geografische Räume**

   Um Ihre Ergebnisse in den für Ihren Benutzer relevanten geografischen Raum zu lenken, sollten Sie immer die maximal mögliche Detailstufe für die Standorteingabe hinzufügen. Sie können die Suchergebnisse einschränken, indem Sie die folgenden Eingabetypen hinzufügen:

   1. Legen Sie die `countrySet`-Parameter fest, z.B. „US,FR“. Das Standardsuchverhalten besteht darin, die ganze Welt zu durchsuchen und möglicherweise unerwünschte Ergebnisse zu erhalten. Wenn Ihre Suchanfrage den `countrySet`-Parameter enthält, kann die Suche ungenaue Ergebnisse liefern. Die Suche nach einer Stadt namens **Bellevue** liefert z. B. Ergebnisse aus den USA und Frankreich, da es sowohl in Frankreich als auch in den USA Städte namens **Bellevue** gibt.

   2. Sie können die Parameter `btmRight` und `topleft` verwenden, um den Begrenzungsrahmen so festzulegen, damit die Suche auf einen bestimmten Bereich auf der Karte eingeschränkt wird.

   3. Sie können die Koordinatenparameter `lat` und `lon` definieren und den Radius des Suchbereichs mit dem Parameter `radius` festlegen, um den für die Ergebnisse relevanten Bereich zu beeinflussen.


   **Parameter der Fuzzysuche**
   
  Die Azure Maps-[API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ist der Dienst, den Sie verwenden sollten, wenn Sie die Benutzereingaben für eine Suchabfrage nicht kennen. Die API kombiniert POI-Suche (Point of Interest) und Geocodierung zu einer kanonischen *einzeiligen Suche*.

   1. `minFuzzyLevel` und `maxFuzzyLevel` helfen dabei, relevante Treffer zurückzugeben, auch wenn die Abfrageparameter nicht genau den gewünschten Informationen entsprechen. Zur Leistungssteigerung und Verringerung ungewöhnlicher Ergebnisse sind Suchabfragen sind standardmäßig auf `minFuzzyLevel=1` und `maxFuzzyLevel=2` festgelegt. Nehmen Sie z. B. den Suchbegriff „restrant“, der mit „restaurant“ verglichen wird, wenn `maxFuzzyLevel` auf 2 festgelegt ist. Die standardmäßigen Fuzzyebenen können je nach Bedarf außer Kraft gesetzt werden.  

   2. Sie können auch den genauen Satz der zurückzugebenden Ergebnistypen priorisieren, indem Sie den Parameter `idxSet` verwenden. Zu diesem Zweck können Sie eine durch Trennzeichen getrennte Liste von Indizes übermitteln, wobei die Reihenfolge der Elemente unerheblich ist. Die folgenden Indizes werden unterstützt:

       * `Addr` - **Adressbereiche:** Für einige Straßen gibt es Adresspunkte, die über Anfang und Ende der Straße interpoliert und als Adressbereiche dargestellt werden.
       * `Geo` - **Geografische Regionen:** Bereiche auf einer Karte, die die Verwaltungseinheiten eines Landes darstellen, d. h. Land, Bundesland, Stadt.
       * `PAD` - **Punktadresse:**  Punkte auf einer Karte, an denen eine bestimmte Adresse mit Straßenname und Nummer in einem Index zu finden ist, z. B. Soquel Dr 2501. Dieser idxSet-Wert ist die höchste verfügbare Genauigkeit für Adressen.  
       * `POI` - **Points of Interest:** Punkte auf einer Karte, die beachtenswert sind und interessant sein können.  [Suchadresse abrufen](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) gibt keine POIs zurück.  
       * `Str` - **Straßen:** Darstellung von Straßen auf der Karte.
       * `XStr` - **Querstraßen/Kreuzungen:**  Darstellung von Kreuzungen, d. h. Orte, an denen sich zwei Straßen kreuzen.


       **Anwendungsbeispiele**:

       * idxSet=POI (nur nach Points of Interest suchen) 

       * idxSet=PAD,Addr (nur Adressen suchen, PAD= Punktadresse, Addr= Adressbereich)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Inverser Geocode und Filter für Geografieentitätstyp

Wenn Sie eine inverse Geocodesuche mit [API für die umgekehrte Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) durchführen, kann der Dienst Polygone für die Verwaltungsbereiche zurückgeben. Durch die Angabe des Parameters `entityType` in der Anforderung können Sie die Suche auf bestimmte Geografieentitätstypen einschränken. Die resultierende Antwort enthält die Geografie-ID und den passenden Entitätstyp. Wenn Sie mehr als eine Entität angeben, gibt der Endpunkt die **kleinste verfügbare Entität** zurück. Die zurückgegebene Geometrie-ID kann dazu verwendet werden, die Geometrie dieser Geografie über [Polygondienst abrufen](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) abzurufen.

**Beispiel für eine Anforderung:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Antwort:**

```JSON
{
    "summary": {
        "queryTime": 14,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                },
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="search-results-language"></a>Sprache der Suchergebnisse

Mit dem Parameter `language` können Sie die Sprache der von der API zurückgegebenen Ergebnisse auswählen. Wenn die Sprache in der Anforderung nicht festgelegt ist, wird der Suchdienst automatisch auf die am häufigsten verwendete Sprache in dem Land/der Region voreingestellt. Wenn Daten in der angegebenen Sprache nicht verfügbar sind, wird ebenfalls die Standardsprache verwendet. Eine Liste der unterstützten Sprachen aus den Azure Maps-Diensten nach Ländern/Regionen finden Sie unter [Unterstützte Sprachen](https://docs.microsoft.com/azure/azure-maps/supported-languages).


### <a name="predictive-mode-autosuggest"></a>Vorhersagemodus (Vorschlagssuche)

Der Parameter `typeahead` sollte auf „true“ festgelegt werden, um weitere Übereinstimmungen für Teilabfragen zu finden. Die Abfrage wird als Teileingabe interpretiert und die Suche wechselt in den Vorhersagemodus. Andernfalls geht der Dienst davon aus, dass alle relevanten Informationen übermittelt wurden.

In der folgenden Beispielabfrage können Sie sehen, dass der Suchadressdienst nach „Microso“ abgefragt wird, wobei der Parameter `typeahead` auf **true** festgelegt ist. Sie können anhand der Antwort sehen, dass der Suchdienst die Abfrage als Teilabfrage interpretiert hat. Die Antwort enthält Ergebnisse für die automatisch vorgeschlagene Abfrage.

**Beispielabfrage:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

**Antwort:**

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 18,
        "numResults": 7,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/9438784",
            "score": 2.594099998474121,
            "dist": 314.0590106663596,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
            },
            "position": {
                "lat": 47.63988,
                "lon": -122.12438
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64223,
                    "lon": -122.1256,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1756074",
            "score": 2.592679977416992,
            "dist": 876.0272035824189,
            "address": {
                "streetName": "Microsoft Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64032,
                "lon": -122.1344
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64253,
                    "lon": -122.13535,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63816,
                    "lon": -122.13305,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1470668",
            "score": 2.5290400981903076,
            "dist": 2735.4883918101486,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Bellevue, WA 98007"
            },
            "position": {
                "lat": 47.65784,
                "lon": -122.14335
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.65785,
                    "lon": -122.14335,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65784,
                    "lon": -122.14325,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/12812615",
            "score": 2.527509927749634,
            "dist": 2870.9579016916873,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.66034,
                "lon": -122.1404
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.66039,
                    "lon": -122.14325,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65778,
                    "lon": -122.13749,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/197588",
            "score": 2.4630401134490967,
            "dist": 878.1404663812472,
            "address": {
                "streetName": "157th Avenue Northeast",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "extendedPostalCode": "980525344, 980525398, 980525399",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "157th Avenue Northeast, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64351,
                "lon": -122.13056
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64473,
                    "lon": -122.13058,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.6425,
                    "lon": -122.13016,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/3033991",
            "score": 2.0754499435424805,
            "dist": 3655467.8844475765,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
            },
            "position": {
                "lat": 35.14267,
                "lon": -80.91824
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14287,
                    "lon": -80.91839,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8395877",
            "score": 2.0754499435424805,
            "dist": 3655437.0037482483,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "countrySubdivisionName": "North Carolina",
                "postalCode": "28273",
                "extendedPostalCode": "282738105, 282738106, 282738108, 2827382, 282738200",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28273"
            },
            "position": {
                "lat": 35.14134,
                "lon": -80.9198
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14274,
                    "lon": -80.92159,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14002,
                    "lon": -80.91824,
                    "valid": true
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>URI-Codierung zum Verarbeiten von Sonderzeichen 

Um Querstraßenadressen zu finden, müssen Sie den URI codieren, um Sonderzeichen in der Adresse zu behandeln. Sehen Sie sich folgendes Adressbeispiel an: „1st Avenue & Union Street, Seattle“. Das Sonderzeichen „&“ muss vor dem Senden der Anforderung codiert werden. Es wird empfohlen, Zeichendaten in einem URI zu codieren, wobei alle Zeichen mit einem „%“-Zeichen und einem zweistelligen hexadezimalen Wert entsprechend ihrem UTF-8-Zeichen codiert werden.

**Anwendungsbeispiele**:

Suchadresse abrufen:

```
query=1st Avenue & E 111th St, New York
```

 muss wie folgt codiert werden:

```
query=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Hier folgen die verschiedenen Methoden, die für verschiedene Sprachen verwendet werden können: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#/VB:
```csharp
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

PHP:
```PHP
urlencode(query)
```

Ruby:
```Ruby
CGI::escape(query) 
```

Swift:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Go:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>Bewährte Methoden für die POI-Suche

Die POI-Suche (Points of Interest) ermöglicht es Ihnen, POI-Ergebnisse nach Namen anzufordern, z. B. Unternehmen anhand ihres Namens suchen. Es wird dringend empfohlen, den Parameter `countrySet` zu verwenden, um Länder anzugeben, die in der Anwendung abgedeckt werden sollen. Beim Standardverhalten wird auf der ganzen Welt gesucht, sodass potenziell unnötige Ergebnisse zurückgegeben werden und/oder dies zu längeren Suchzeiten führt.

### <a name="brand-search"></a>Markensuche

Um die Relevanz der Ergebnisse und der Informationen in der Antwort zu verbessern, umfasst die POI-Suchantwort (Point of Interest) die Markeninformationen, mit denen die Antwort weiter analysiert werden kann.

Sie können auch eine durch Trennzeichen getrennte Liste von Markennamen in der Anforderung übermitteln. Mithilfe der Liste können Sie die Ergebnisse auf bestimmte Marken beschränken, indem Sie den Parameter `brandSet` verwenden. Die Reihenfolge der Elemente ist unerheblich. Wenn mehrere Marken bereitgestellt werden, werden nur Ergebnisse zurückgegeben, die (mindestens) einer der bereitgestellten Listen angehören.

Lassen Sie uns eine [POI-Kategoriesuche](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) für Tankstellen in der Nähe des Microsoft Campus (Redmond, WA) anfordern. Wenn Sie die Antwort betrachten, können Sie Markeninformationen für jeden zurückgegebenen POI sehen.


**Beispielabfrage:**

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

**Antwort:**

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 276,
        "numResults": 3,
        "offset": 0,
        "totalResults": 762680,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/8831765",
            "score": 5.6631999015808105,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.chevron.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63222,
                        "lon": -122.13312,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831752",
            "score": 5.662710189819336,
            "dist": 1330.1278248163273,
            "info": "search:ta:840539001100326-US",
            "poi": {
                "name": "76",
                "phone": "+(1)-(425)-7472126",
                "brands": [
                    {
                        "name": "76"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.76.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2421",
                "streetName": "148Th Ave Ne",
                "municipalitySubdivision": "Redmond, Bridle Trails, Bellevue",
                "municipality": "Redmond, Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "2421 148Th Ave Ne, Bellevue, WA 98007",
                "localName": "Bellevue"
            },
            "position": {
                "lat": 47.63187,
                "lon": -122.14365
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63277,
                    "lon": -122.14498,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63097,
                    "lon": -122.14232,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "minor",
                    "position": {
                        "lat": 47.63187,
                        "lon": -122.14374,
                        "valid": true
                    }
                },
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63186,
                        "lon": -122.14313,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831764",
            "score": 5.662449836730957,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.texaco.com/",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62827,
                        "lon": -122.13628,
                        "valid": true
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Flughafensuche

Die POI-Suche unterstützt die Suche nach Flughäfen anhand der offiziellen Flughafencodes. Beispiel: **SEA** (Seattle-Tacoma International Airport). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Umgebungssuche

Damit nur POI-Ergebnisse in der Nähe eines bestimmten Ortes abgerufen werden, ist die [API für die Umgebungssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) möglicherweise die richtige Wahl. Dieser Endpunkt liefert nur POI-Ergebnisse und übernimmt keinen Suchabfrageparameter. Es wird empfohlen, den Radius festzulegen, um die Ergebnisse einzuschränken.

## <a name="understanding-the-responses"></a>Grundlegendes zu den Antworten

Lassen Sie uns eine Adresssuchanforderung an den Azure Maps-[Suchdienst](https://docs.microsoft.com/rest/api/maps/search) für eine Adresse in Seattle richten. Wenn Sie sich die nachfolgende Anforderungs-URL genau ansehen, wurde der Parameter `countrySet` auf **US** festgelegt, um nach der Adresse in den Vereinigten Staaten von Amerika zu suchen.

**Beispielabfrage:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

Lassen Sie uns ferner einen Blick auf die folgende Antwortstruktur werfen. Die Ergebnistypen der Ergebnisobjekte in der Antwort sind unterschiedlich. Bei genauer Betrachtung erkennen Sie, dass drei verschiedene Arten von Ergebnisobjekten vorliegen: „Punktadresse“, „Straße“ und „Querstraße“. Beachten Sie, dass die Adresssuche keine POIs zurückgibt. Der Parameter `Score` für jedes Antwortobjekt gibt die Bewertung zur relativen Übereinstimmung im Vergleich zu den Bewertungen anderer Objekte in derselben Antwort an. Weitere Informationen zu den Parametern des Antwortobjekts finden Sie unter [Suchadresse abrufen](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress).

**Unterstützte Ergebnistypen:**

* **Punktadresse:** Punkte auf einer Karte mit einer bestimmten Adresse, die Straßenname und Hausnummer umfasst. Dies ist die höchste verfügbare Genauigkeit für Adressen. 

* **Adressbereich:**  Für einige Straßen gibt es Adresspunkte, die über Anfang und Ende der Straße interpoliert und als Adressbereiche dargestellt werden. 

* **Geografie:** Bereiche auf einer Karte, die die Verwaltungseinheiten eines Landes darstellen, d. h. Land, Bundesland, Stadt. 

* **POI (Points of Interest):** Punkte auf einer Karte, die beachtenswert sind und interessant sein können.

* **Straße:** Darstellung von Straßen auf der Karte. Adressen werden in die Längen- und Breitenkoordinate der Straße aufgelöst, die diese Adresse enthält. Die Hausnummer wird möglicherweise nicht verarbeitet. 

* **Querstraße:** Kreuzungen. Darstellung von Kreuzungen, d. h. Orte, an denen sich zwei Straßen kreuzen.

**Antwort:**

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 146,
        "numResults": 6,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/28725082",
            "score": 9.893799781799316,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/6700384",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61724,
                "lon": -122.35207
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61825,
                    "lon": -122.35336,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61626,
                    "lon": -122.35078,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/9701953",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61965,
                "lon": -122.349
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62066,
                    "lon": -122.35041,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.34761,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/11721297",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Downtown Seattle, Denny Regrade, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98121",
                "extendedPostalCode": "981211237",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98121"
            },
            "position": {
                "lat": 47.61825,
                "lon": -122.35078
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61857,
                    "lon": -122.35078,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61825,
                    "lon": -122.35041,
                    "valid": true
                }
            }
        },
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/232144",
            "score": 6.754479885101318,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
            },
            "position": {
                "lat": 47.62545,
                "lon": -122.33974
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62635,
                    "lon": -122.34107,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62455,
                    "lon": -122.33841,
                    "valid": true
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometrie

Wenn der Antworttyp **Geometrie** lautet, kann er die Geometrie-ID enthalten, die im **dataSources**-Objekt unter „geometry“ und „id“ zurückgegeben wird. Mit [Polygondienst abrufen](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) können Sie z. B. die Geometriedaten im GeoJSON-Format anfordern, z. B. einen Stadt- oder Flughafenplan für eine Reihe von Entitäten. Sie können diese Begrenzungsdaten für [Geofencing](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) oder die [Suche von POIs innerhalb der Geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) verwenden.


Die API-Antworten für [Search Address](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) (Adresse suchen) oder [Search Fuzzy](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) (Fuzzy suchen) können die **Geometrie-ID** einbeziehen, die im „dataSources“-Objekt unter „geometry“ und „id“ zurückgegeben wird.


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, [wie Sie Suchanforderungen für die Azure Maps-Suche erstellen](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address).
* Machen Sie sich mit der [API-Dokumentation zum Suchdienst](https://docs.microsoft.com/rest/api/maps/search) von Azure Maps vertraut. 
