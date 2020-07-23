---
title: Suchen nach einem Ort mit dem Suchdienst von Azure Maps | Microsoft Azure Maps
description: In diesem Artikel erfahren Sie, wie Sie mithilfe des Suchdiensts von Microsoft Azure Maps einen Ort für die Geocodierung und inverse Geocodierung suchen.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 2d8c7ce62f7c592c396fa1ea7a7f5e7dc7df2dc1
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86517585"
---
# <a name="search-for-a-location-using-azure-maps-search-services"></a>Suchen nach einem Ort mit dem Suchdienst von Azure Maps

Beim Azure Maps-[Suchdienst](https://docs.microsoft.com/rest/api/maps/search) handelt es sich um eine Gruppe von RESTful-APIs, die Entwickler bei der Suche nach Adressen, Orten, Brancheneinträgen (nach Name oder Kategorie) und anderen geografischen Informationen unterstützen. Zusätzlich zur Unterstützung der traditionellen Geocodierung kann mit den Diensten eine inverse Geocodierung für Adressen und Querstraßen basierend auf Breiten- und Längengraden durchgeführt werden. Die von der Suche zurückgegebenen Werte für den Breiten- und Längengrad können als Parameter in anderen Azure Maps-Diensten verwendet werden, z. B. Dienste für [Routen](https://docs.microsoft.com/rest/api/maps/route) und [Wetter](https://docs.microsoft.com/rest/api/maps/weather).

In diesem Artikel wird Folgendes behandelt:

* Anfordern von Breiten- und Längenkoordinaten für eine Adresse (Geocode-Adresse) mithilfe der [API für die Adresssuche]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddress)
* Suchen nach einer Adresse oder einem Point of Interest (POI) mithilfe der [API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* Suchen nach einer Adresse mit deren Eigenschaften und Koordinaten
* Ausführen einer [inversen Adresssuche](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), um den Koordinatenort in eine Straßenadresse zu übersetzen
* Suchen nach der Querstraße mithilfe der [API für die inverse Adresssuche nach Querstraßen](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreversecrossstreet)

## <a name="prerequisites"></a>Voraussetzungen

Zum Ausführen der in diesem Artikel beschriebenen Schritte müssen Sie zunächst ein Azure Maps-Konto erstellen und Ihren Abonnementschlüssel für Ihr Maps-Konto abrufen. Befolgen Sie die Anleitung unter [Erstellen eines Kontos](quick-demo-map-app.md#create-an-azure-maps-account), um ein Azure Maps-Kontoabonnement zu erstellen. Führen Sie außerdem die Schritte unter [Abrufen des Primärschlüssels](quick-demo-map-app.md#get-the-primary-key-for-your-account) aus, um den Primärschlüssel für Ihr Konto abzurufen. Weitere Informationen zur Authentifizierung in Azure Maps finden Sie unter [Verwalten der Authentifizierung in Azure Maps](./how-to-manage-authentication.md).

In diesem Artikel wird die [Postman-App](https://www.getpostman.com/apps) zum Erstellen von REST-Aufrufen verwendet. Sie können jedoch auch Ihre bevorzugte API-Entwicklungsumgebung verwenden.

## <a name="request-latitude-and-longitude-for-an-address-geocoding"></a>Anfordern des Breiten- und Längengrads für eine Adresse (Geocodierung)

In diesem Beispiel wird die Azure Maps-[API „Get Search Address“](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) verwendet, um die Straßenadresse in Breiten- und Längenkoordinaten zu konvertieren. Sie können eine vollständige oder unvollständige Adresse an die API übergeben. Sie erhalten dann eine Antwort, die detaillierte Adresseigenschaften, z. B. Straße, PLZ und Land/Region, sowie Positionswerte mit Breiten- und Längengraden umfasst.

Wenn Sie über eine Gruppe von Adressen für die Geocodierung verfügen, können Sie [API „Post Search Address Batch“](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressbatch) verwenden, um einen Batch von Abfragen in einem einzigen API-Aufruf zu senden.

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Address Search** (Adresssuche).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein, und wählen Sie ggf. ein Autorisierungsprotokoll aus.

![Adresssuche](./media/how-to-search-for-address/address_search_url.png)

| Parameter | Vorgeschlagener Wert |
|---------------|------------------------------------------------| 
| HTTP-Methode | GET |
| Anfrage-URL | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) | 
| Authorization | No Auth |

3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein: 

![Adresssuche](./media/how-to-search-for-address/address_search_params.png) 

| Schlüssel | Wert | 
|------------------|-------------------------| 
| api-version | 1.0 | 
| subscription-key | \<your Azure Maps key\> | 
| Abfrage | 400 Broad St, Seattle, WA 98109 | 

4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an. 

In diesem Fall haben Sie eine Abfrage für eine vollständige Adresse angegeben und erhalten im Antworttext ein einzelnes Ergebnis. 

5. Ändern Sie in „Params“ (Parameter) die Abfragezeichenfolge in den folgenden Wert: 

    ```plaintext 
        400 Broad, Seattle 
    ``` 

6. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**: 

| Schlüssel | Wert | 
|-----|------------| 
| typeahead | true | 

Das Flag **typeahead** weist die API für die Adresssuche an, die Abfrage als unvollständige Eingabe zu verarbeiten und ein Array von Vorhersagewerten zurückzugeben.

## <a name="using-fuzzy-search-api"></a>Verwenden der API für die Fuzzysuche

Die Azure Maps-[API für die Fuzzysuche](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ist der Dienst, den Sie verwenden sollten, wenn Sie die Benutzereingaben für eine Suchabfrage nicht kennen. Die API kombiniert POI-Suche (Point of Interest) und Geocodierung zu einer kanonischen „einzeiligen Suche“. So kann die API beispielsweise Eingaben beliebiger Adress- oder POI-Token-Kombinationen verarbeiten. Sie kann auch mit einer kontextabhängigen Position (Breitengrad-Längengrad-Paar) gewichtet werden, die durch eine Koordinate oder einen Radius vollständig eingeschränkt ist. Oder sie kann allgemeiner ohne jeglichen geografiebezogenen Ankerpunkt ausgeführt werden.

Die meisten Suchabfragen sind zur Leistungssteigerung und Verringerung ungewöhnlicher Ergebnisse standardmäßig auf `maxFuzzyLevel=1` festgelegt. Diese Standardeinstellung kann bei Bedarf pro Anforderung durch Übergeben des Abfrageparameters `maxFuzzyLevel=2` oder `3` überschrieben werden.

### <a name="search-for-an-address-using-fuzzy-search"></a>Suchen nach einer Adresse mithilfe der Fuzzysuche

1. Öffnen Sie die Postman-App, klicken Sie auf „New“ (Neu) und „Create New“ (Neu erstellen), und wählen Sie **GET request** (GET-Anforderung) aus. Geben Sie **Fuzzy search** (Fuzzysuche) als Name der Anforderung ein, wählen Sie eine Sammlung oder einen Ordner aus, in der bzw. dem die Anforderung gespeichert wird, und klicken Sie auf **Save** (Speichern).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.

    ![Fuzzysuche](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parameter | Vorgeschlagener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Authorization | No Auth |

    Das Attribut **json** im URL-Pfad legt das Antwortformat fest. In diesem Artikel wird JSON verwendet, um die Benutzerfreundlichkeit und Lesbarkeit zu verbessern. Sie finden die verfügbaren Antwortformate in der Definition für **Get Search Fuzzy** der [Referenz zur funktionalen API von Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:

    ![Fuzzysuche](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<your Azure Maps key\> |
    | Abfrage | pizza |

4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an.

    Die mehrdeutige Abfragezeichenfolge „pizza“ hat 10 [POI-Ergebnisse](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) sowohl in der Kategorie „Pizza“ als auch in der Kategorie „Restaurant“ zurückgegeben. In jedem Ergebnis werden eine Adresse, Werte für den Breiten- und Längengrad, ein Viewport und Einstiegspunkte für den Standort zurückgegeben.
  
    Die Ergebnisse für diese Abfrage variieren und sind mit keinem bestimmten Referenzstandort verbunden. Mithilfe des **countrySet**-Parameters können Sie nur die Länder/Regionen angeben, die von Ihrer Anwendung abgedeckt werden sollen. Das Standardverhalten besteht darin, die ganze Welt zu durchsuchen, wodurch unnötige Ergebnisse zurückgegeben werden können.

5. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |------------------|-------------------------|
    | countrySet | US |
  
    Die Ergebnisse werden nun durch den Ländercode begrenzt, und die Abfrage gibt Pizzerien in den USA zurück.
  
    Für Ergebnisse, die sich auf einen Standort beziehen, können Sie einen interessanten Ort abfragen und die zurückgegebenen Werte für den Breiten- und Längengrad im Aufruf des Diensts für die Fuzzysuche verwenden. In diesem Fall verwenden Sie den Suchdienst für die Rückgabe des Standorts der Space Needle in Seattle sowie die Werte für den Breiten- und Längengrad zur Eingrenzung der Suche.
  
6. Geben Sie unter „Params“ (Parameter) die folgenden Schlüssel-Wert-Paare ein, und klicken Sie auf **Send** (Senden):

    ![Fuzzysuche](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Schlüssel | Wert |
    |-----|------------|
    | lat | 47.620525 |
    | lon | -122.349274 |


## <a name="search-for-a-street-address-using-reverse-address-search"></a>Suchen nach einer Adresse mithilfe der inversen Adresssuche

Die Azure Maps-API [Get Search Address Reverse]( https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) (Suchadresse invers abrufen) hilft bei der Übersetzung einer Koordinate (Beispiel: 37.786505, -122.3862) in eine für den Menschen verständliche Anschrift. Dies wird meistens bei Anwendungen für die Nachverfolgung benötigt, bei denen Sie einen GPS-Feed von dem Gerät oder der Ressource erhalten und wissen möchten, welche Adresse sich an dieser Koordinate befindet.
Wenn Sie über eine Gruppe von Koordinatenpositionen für die inverse Geocodierung verfügen, können Sie [API „Post Search Address Reverse Batch“](https://docs.microsoft.com/rest/api/maps/search/postsearchaddressreversebatch) verwenden, um einen Batch von Abfragen in einem einzigen API-Aufruf zu senden.


1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Reverse Address Search** (Inverse Adresssuche).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.
  
    ![URL für inverse Adresssuche](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parameter | Vorgeschlagener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Authorization | No Auth |
  
3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:
  
    ![Parameter für inverse Adresssuche](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<your Azure Maps key\> |
    | Abfrage | 47.591180,-122.332700 |
  
4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an.

    Die Antwort enthält Schlüsseladressinformationen über Safeco Field.
  
5. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | number | true |

    Wenn der Abfrageparameter [number](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) mit der Anforderung gesendet wird, enthält die Antwort möglicherweise die Straßenseite (links oder rechts) und auch eine versetzte Position für diese Hausnummer.
  
6. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | returnSpeedLimit | true |
  
    Wenn der Abfrageparameter [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) festgelegt wird, wird in der Antwort die angegebene Geschwindigkeitsbegrenzung zurückgegeben.

7. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | returnRoadUse | true |

    Wenn der Abfrageparameter [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) festgelegt wird, wird in der Antwort die Straßennutzung für die inverse Geocodierung von Straßen zurückgegeben.

8. Fügen Sie das folgende Schlüssel-Wert-Paar zum Abschnitt **Params** (Parameter) hinzu, und klicken Sie auf **Senden**:

    | Schlüssel | Wert |
    |-----|------------|
    | roadUse | true |

    Mithilfe des Abfrageparameters [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) können Sie die Abfrage der inversen Geocodierung auf eine bestimmte Art der Straßennutzung beschränken.
  
## <a name="search-for-cross-street-using-reverse-address-cross-street-search"></a>Suchen nach der Querstraße mithilfe der inversen Adresssuche für Querstraßen

1. Klicken Sie in Postman auf **New Request (Neue Anforderung)**  | **GET request (GET-Anforderung)** , und nennen Sie sie **Reverse Address Cross Street Search** (Inverse Adresssuche für Querstraßen).

2. Wählen Sie auf der Registerkarte „Builder“ die HTTP-Methode **GET** aus, und geben Sie die Anforderungs-URL für Ihren API-Endpunkt ein.
  
    ![Inverse Adresssuche für Querstraßen](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parameter | Vorgeschlagener Wert |
    |---------------|------------------------------------------------|
    | HTTP-Methode | GET |
    | Anfrage-URL | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Authorization | No Auth |
  
3. Klicken Sie auf **Params** (Parameter), und geben Sie die folgenden Schlüssel-Wert-Paare zur Verwendung als Abfrage- oder Pfadparameter in der Anforderungs-URL ein:
  
    | Schlüssel | Wert |
    |------------------|-------------------------|
    | api-version | 1.0 |
    | subscription-key | \<your Azure Maps key\> |
    | Abfrage | 47.591180,-122.332700 |
  
4. Klicken Sie auf **Send** (Senden), und sehen Sie sich den Antworttext an.

## <a name="next-steps"></a>Nächste Schritte

- Machen Sie sich mit der [REST API-Dokumentation zum Suchdienst](https://docs.microsoft.com/rest/api/maps/search) von Azure Maps vertraut.
- Informieren Sie sich über [Bewährte Methoden für den Azure Maps-Suchdienst](https://docs.microsoft.com/azure/azure-maps/how-to-use-best-practices-for-search) und über das Optimieren Ihrer Abfragen.
