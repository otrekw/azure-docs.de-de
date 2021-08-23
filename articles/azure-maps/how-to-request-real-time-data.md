---
title: Anfordern von Echtzeitdaten für den öffentlichen Nahverkehr mit Microsoft Azure Maps-Mobilitätsdiensten (Vorschau)
description: Erfahren Sie, wie Sie Echtzeitdaten des öffentlichen Nahverkehrs wie Ankünfte an einer Umsteigehaltestelle anfordern. Erfahren Sie, wie Sie die Azure Maps-Mobilitätsdienste (Vorschau) zu diesem Zweck verwenden.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/23/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
ms.custom: mvc
ms.openlocfilehash: 6e7358e76cdcf07f39a9b5a5fcab7ab0151ebc32
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2021
ms.locfileid: "122339850"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-services-preview"></a>Anfordern von Echtzeitdaten für den öffentlichen Nahverkehr mithilfe der Azure Maps-Mobilitätsdienste (Vorschau) 

> [!IMPORTANT]
> Die Vorschauversion von Azure Maps Mobility Services wurde eingestellt. Nach dem 5. Oktober 2021 ist sie nicht mehr verfügbar und wird nicht mehr unterstützt. Alle anderen Azure Maps-APIs und -Dienste sind von dieser Einstellungsankündigung nicht betroffen.
> Weitere Informationen finden Sie unter [Azure Maps Mobility – Einstellung der Vorschauversion](https://azure.microsoft.com/updates/azure-maps-mobility-services-preview-retirement/).


In diesem Artikel wird erläutert, wie Sie Azure Maps-[Mobilitätsdienste](/rest/api/maps/mobility) verwenden, um Echtzeitdaten für den öffentlichen Nahverkehr anzufordern.

In diesem Artikel erfahren Sie, wie Sie die nächsten Ankunftszeiten für alle Linien anfordern, die an der angegebenen Haltestelle ankommen (in Echtzeit).

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen zunächst ein Azure Maps-Konto und einen Abonnementschlüssel, um die Azure Maps-APIs für den öffentlichen Verkehr aufrufen zu können. Befolgen Sie zum Erstellen eines Azure Maps-Kontos die Anweisungen unter [Erstellen eines Kontos](quick-demo-map-app.md#create-an-azure-maps-account). Führen Sie die Schritte unter [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) aus, um den Primärschlüssel für Ihr Konto zu erhalten. Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

In diesem Artikel wird die [Postman-App](https://www.getpostman.com/apps) zum Erstellen von REST-Aufrufen verwendet. Sie können jedoch auch Ihre bevorzugte API-Entwicklungsumgebung verwenden.

## <a name="request-real-time-arrivals-for-a-stop"></a>Anfordern der Ankunftszeiten für eine Haltestelle in Echtzeit

Um für eine bestimmte Haltestelle von öffentlichen Verkehrsmitteln Ankunftszeiten in Echtzeit anzufordern, müssen Sie eine Anforderung an die [API für Ankunftszeiten in Echtzeit](/rest/api/maps/mobility/getrealtimearrivalspreview) des Azure Maps-[Mobilitätsdiensts (Vorschau) ](/rest/api/maps/mobility) richten. Für die Anforderung benötigen Sie die ID der Stadt (**metroID**) und die ID der Haltestelle (**stopID**). Weitere Informationen zum Anfordern dieser Parameter finden Sie in der Anleitung zum [Anfordern von Routen für den öffentlichen Nahverkehr](./how-to-request-transit-data.md).

Wir verwenden „522“ als Metro-ID. Dies ist die Metro-ID für das Gebiet „Seattle – Tacoma – Bellevue, WA“. Verwenden Sie „522---2060603“ als Haltestellen-ID. Dies ist die Bushaltestelle mit der Adresse „Ne 24th St & 162nd Ave Ne, Bellevue WA“. Gehen Sie wie folgt vor, um Echtzeitdaten zu den nächsten fünf Ankunftszeiten dieser Haltestelle anzufordern:

1. Öffnen Sie die Postman-App. Wählen Sie **Neu** aus, um die Anforderung zu erstellen. Wählen Sie im Fenster **Create New** (Neu erstellen) die Option **HTTP Request** (HTTP-Anforderung) aus. Geben Sie einen Anforderungsnamen (**Request name**) ein.

2. Wählen Sie auf der Registerkarte „Builder“ (Generator) die HTTP-Methode **GET** aus, und geben Sie die folgende URL ein, um eine GET-Anforderung zu erstellen. Ersetzen Sie `{subscription-key}` durch Ihren Primärschlüssel für Azure Maps.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

3. War die Anforderung erfolgreich, erhalten Sie die folgende Antwort.  Der Parameter „-scheduleType“ definiert, ob die voraussichtliche Ankunftszeit auf Echtzeitdaten oder auf statischen Daten basiert.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe von Mobilitätsdiensten (Vorschau) Daten zu öffentlichen Verkehrsmitteln bzw. Routen anfordern:

> [!div class="nextstepaction"]
> [Anfordern von Daten zu öffentlichen Verkehrsmitteln bzw. Routen](how-to-request-transit-data.md)

Machen Sie sich mit der API-Dokumentation für Azure Maps-Mobilitätsdienste (Vorschau) vertraut:

> [!div class="nextstepaction"]
> [API-Dokumentation zu Mobilitätsdiensten](/rest/api/maps/mobility)