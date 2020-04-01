---
title: 'Schnellstart: Java-Clientbibliothek für die Bing-Entitätssuche'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 8c987aa14e922573d01aa35fab609edf01e109b4
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "79136615"
---
Verwenden Sie diese Schnellstartanleitung, um unter Verwendung der Bing-Entitätssuche-Clientbibliothek für Java mit der Suche nach Entitäten zu beginnen. Die Bing-Entitätssuche verfügt zwar über eine REST-API, die mit den meisten Programmiersprachen kompatibel ist, die Clientbibliothek ist jedoch eine einfache Möglichkeit, den Dienst in Ihre Anwendungen zu integrieren. Den Quellcode für dieses Beispiel finden Sie auf [GitHub](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingEntitySearch).

## <a name="prerequisites"></a>Voraussetzungen

* Das [Java Development Kit (JDK)](https://www.oracle.com/technetwork/java/javase/downloads/)

* Die Bing-Entitätssuche-Clientbibliothek für Java

Installieren Sie die Abhängigkeit für die Bing-Entitätssuche-Clientbibliothek mithilfe von Maven, Gradle oder einem anderen Abhängigkeitsverwaltungssystem. Die POM-Datei für Maven erfordert die folgende Deklaration:

```xml
<dependency>
  <groupId>com.microsoft.azure.cognitiveservices</groupId>
  <artifactId>azure-cognitiveservices-entitysearch</artifactId>
  <version>1.0.2</version>
</dependency>
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Erstellen und Initialisieren eines Projekts

1. Erstellen Sie in Ihrer bevorzugten IDE oder in Ihrem bevorzugten Editor ein neues Java-Projekt, und importieren Sie die folgenden Bibliotheken.

    ```java
    import com.microsoft.azure.cognitiveservices.entitysearch.*;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.EntitySearchAPIImpl;
    import com.microsoft.azure.cognitiveservices.entitysearch.implementation.SearchResponseInner;
    import com.microsoft.rest.credentials.ServiceClientCredentials;
    import okhttp3.Interceptor;
    import okhttp3.OkHttpClient;
    import okhttp3.Request;
    import okhttp3.Response;
    
    import java.io.IOException;
    import java.util.ArrayList;
    import java.util.List;
    ```

2. Erstellen Sie eine Variable für Ihren Abonnementschlüssel.

    ```java
    String subscriptionKey = "your-key-here"
    ```

## <a name="create-a-search-client"></a>Erstellen eines Suchclients

1. Implementieren Sie den `dominantEntityLookup`-Client, der Ihren API-Endpunkt und eine Instanz der `ServiceClientCredentials`-Klasse erfordert. Sie können den unten angegebenen globalen Endpunkt oder den Endpunkt der [benutzerdefinierten Unterdomäne](../../../../cognitive-services/cognitive-services-custom-subdomains.md) verwenden, der im Azure-Portal für Ihre Ressource angezeigt wird.

    ```java
    public static EntitySearchAPIImpl getClient(final String subscriptionKey) {
        return new EntitySearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
                new ServiceClientCredentials() {
                //...
                }
    )};
    ```

    Führen Sie zum Implementieren von `ServiceClientCredentials` die folgenden Schritte aus:

   1. Überschreiben Sie die Funktion `applyCredentialsFilter()` mit einem `OkHttpClient.Builder`-Objekt als Parameter. 
        
       ```java
       //...
       new ServiceClientCredentials() {
               @Override
               public void applyCredentialsFilter(OkHttpClient.Builder builder) {
               //...
               }
       //...
       ```
    
   2. Rufen Sie `builder.addNetworkInterceptor()` in `applyCredentialsFilter()` auf. Erstellen Sie ein neues `Interceptor`-Objekt, und überschreiben Sie seine `intercept()`-Methode, um ein Interceptor-Objekt vom Typ `Chain` zu übernehmen.

       ```java
       //...
       builder.addNetworkInterceptor(
           new Interceptor() {
               @Override
               public Response intercept(Chain chain) throws IOException {
               //...    
               }
           });
       ///...
       ```

   3. Erstellen Sie in der Funktion `intercept` Variablen für Ihre Anforderung. Verwenden Sie `Request.Builder()` zum Erstellen Ihrer Anforderung. Fügen Sie dem Header `Ocp-Apim-Subscription-Key` Ihren Abonnementschlüssel hinzu, und geben Sie `chain.proceed()` für das Anforderungsobjekt zurück.
            
       ```java
       //...
       public Response intercept(Chain chain) throws IOException {
           Request request = null;
           Request original = chain.request();
           Request.Builder requestBuilder = original.newBuilder()
                   .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
           request = requestBuilder.build();
           return chain.proceed(request);
       }
       //...
       ```
## <a name="send-a-request-and-receive-a-response"></a>Senden einer Anforderung und Erhalten einer Antwort

1. Erstellen Sie eine neue Instanz des Suchclients mit Ihrem Abonnementschlüssel. Verwenden Sie `client.entities().search()`, um eine Suchanforderung für die Suchabfrage `satya nadella` zu senden und eine Antwort zu erhalten. 
    
    ```java
    EntitySearchAPIImpl client = getClient(subscriptionKey);
    SearchResponseInner entityData = client.entities().search(
            "satya nadella", null, null, null, null, null, null, "en-us", null, null, SafeSearch.STRICT, null);
    ```

1. Wenn Entitäten zurückgegeben wurden, konvertieren Sie sie in eine Liste. Durchlaufen Sie sie, und geben Sie die dominante Entität aus.

    ```java
    if (entityData.entities().value().size() > 0){
        // Find the entity that represents the dominant entity
        List<Thing> entries = entityData.entities().value();
        Thing dominateEntry = null;
        for(Thing thing : entries) {
            if(thing.entityPresentationInfo().entityScenario() == EntityScenario.DOMINANT_ENTITY) {
                System.out.println("\r\nSearched for \"Satya Nadella\" and found a dominant entity with this description:");
                System.out.println(thing.description());
                break;
            }
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erstellen einer Einzelseiten-Web-App](../../tutorial-bing-entities-search-single-page-app.md)

* [Was ist die Bing-Entitätssuche-API?](../../overview.md)
