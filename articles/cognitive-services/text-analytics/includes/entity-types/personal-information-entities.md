---
title: Benannte Entitäten für personenbezogene Informationen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: 97167485dae155670f0eb83fc3ef9cb658952251
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750404"
---
> [!NOTE]
> Um geschützte Gesundheitsinformationen (PHI) zu ermitteln, verwenden Sie den `domain=phi`-Parameter und die Modellversion `2020-04-01` oder höher.
>
> Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/entities/recognition/pii?domain=phi&model-version=2021-01-15`
 
Die folgenden Entitätskategorien werden zurückgegeben, wenn Sie Anforderungen an den `/v3.1-preview.3/entities/recognition/pii`-Endpunkt senden.


| Category   |  BESCHREIBUNG                          |
|------------|-------------|
| [Person](#category-person)      |  Namen von Personen  |
| [PersonType](#category-persontype) | Von einer Person eingenommene Position oder Rolle |
| [Telefonnummer](#category-phonenumber) |Telefonnummern (nur US- und EU-Telefonnummern) |
| [Organisation](#category-organization) |  Firmen, Gruppen, Regierungsstellen und andere Organisationen  |
| [Adresse](#category-address) | Vollständige Postanschrift  |
| [E-Mail](#category-email) | E-Mail-Adressen   |
| [URL](#category-url) | URLs zu Websites  |
| [IP](#category-ip) | Netzwerk-IP-Adressen  |
| [DateTime](#category-datetime) | Datums- und Uhrzeitangaben | 
| [Menge](#category-quantity) | Zahlen und numerische Mengen.  |
| [Azure-Informationen](#azure-information) | Identifizierbare Azure-Informationen (beispielsweise Authentifizierungsinformationen)  |
| [Identifikation](#identification) | Finanz- und landesspezifische Identifikation  |

### <a name="category-person"></a>Kategorie: Person

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Person

    :::column-end:::
    :::column span="2":::
        **Details**

        Namen von Personen Wird auch mit `domain=phi`zurückgegeben
      
    :::column-end:::
:::row-end:::

### <a name="category-persontype"></a>Kategorie: PersonType

Diese Kategorie enthält die folgende Entität:


:::row:::
    :::column span="":::
        **Entität**

        PersonType

    :::column-end:::
    :::column span="2":::
        **Details**

        Von einer Person eingenommene Position oder Rolle
      
    :::column-end:::
:::row-end:::

### <a name="category-phonenumber"></a>Kategorie: PhoneNumber

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        PhoneNumber

    :::column-end:::
    :::column span="2":::
        **Details**

        Telefonnummern (nur US- und EU-Telefonnummern) Wird auch mit `domain=phi`zurückgegeben
      
    :::column-end:::
:::row-end:::


### <a name="category-organization"></a>Kategorie: Organization

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Organization

    :::column-end:::
    :::column span="2":::
        **Details**

        Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Unterkategorien

Die Entität in dieser Kategorie kann die folgenden Unterkategorien aufweisen.

:::row:::
    :::column span="":::
        **Entitätsunterkategorie**

        Medizin

    :::column-end:::
    :::column span="2":::
        **Details**

        Medizinische Unternehmen und Gruppen
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Börse

    :::column-end:::
    :::column span="2":::

        Börsengruppen 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Sport

    :::column-end:::
    :::column span="2":::

        Sportbezogene Organisationen
      
    :::column-end:::

:::row-end:::


### <a name="category-address"></a>Kategorie: Adresse

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Adresse

    :::column-end:::
    :::column span="2":::
        **Details**

        vollständige Postanschrift
      
    :::column-end:::
:::row-end:::

### <a name="category-email"></a>Kategorie: Email

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        Email

    :::column-end:::
    :::column span="2":::
        **Details**

        E-Mail-Adressen
      
    :::column-end:::
:::row-end:::

### <a name="category-url"></a>Kategorie: URL

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        URL

    :::column-end:::
    :::column span="2":::
        **Details**

        URLs zu Websites 
      
    :::column-end:::
:::row-end:::

### <a name="category-ip"></a>Kategorie: IP

Diese Kategorie enthält die folgende Entität:

:::row:::
    :::column span="":::
        **Entität**

        IP

    :::column-end:::
    :::column span="2":::
        **Details**

        Netzwerk-IP-Adressen 
      
    :::column-end:::
:::row-end:::

### <a name="category-datetime"></a>Kategorie: Datetime

Diese Kategorie enthält die folgenden Entitäten:

:::row:::
    :::column span="":::
        **Entität**

        Datetime

    :::column-end:::
    :::column span="2":::
        **Details**

        Datums- und Uhrzeitangaben 
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Unterkategorien

Die Entität in dieser Kategorie kann die folgenden Unterkategorien aufweisen.

:::row:::
    :::column span="":::
        **Entitätsunterkategorie**

        Date

    :::column-end:::
    :::column span="2":::
        **Details**

        Kalenderdatumsangaben
      
    :::column-end:::
:::row-end:::

### <a name="category-quantity"></a>Kategorie: Menge

Diese Kategorie enthält die folgenden Entitäten:

:::row:::
    :::column span="":::
        **Entität**

        Menge

    :::column-end:::
    :::column span="2":::
        **Details**

        Zahlen und numerische Mengen.
      
    :::column-end:::
:::row-end:::

#### <a name="subcategories"></a>Unterkategorien

Die Entität in dieser Kategorie kann die folgenden Unterkategorien aufweisen.

:::row:::
    :::column span="":::
        **Entitätsunterkategorie**

        Age

    :::column-end:::
    :::column span="2":::
        **Details**

        Altersangaben
      
    :::column-end:::
:::row-end:::

### <a name="azure-information"></a>Azure-Informationen

Diese Entitätskategorien beinhalten identifizierbare Azure-Informationen (einschließlich Authentifizierungsinformationen und Verbindungszeichenfolgen). Wird nicht mit dem `domain=phi`-Parameter zurückgegeben

:::row:::
    :::column span="":::
        **Entität**

        Azure DocumentDB-Authentifizierungsschlüssel 

    :::column-end:::
    :::column span="2":::
        **Details**

        Autorisierungsschlüssel für einen Azure Cosmos DB-Server.   
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Verbindungszeichenfolge für Azure-IaaS-Datenbank und Azure SQL

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für eine Azure-IaaS-Datenbank (Infrastructure-as-a-Service) und Azure SQL
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure SQL-Verbindungszeichenfolge

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für eine Datenbank in Azure SQL-Datenbank.
      
    :::column-end:::

:::row-end:::
:::row:::
    :::column span="":::

        Azure IoT-Verbindungszeichenfolge  

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für Azure IoT. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure-Veröffentlichungseinstellung: Kennwort  

    :::column-end:::
    :::column span="2":::

        Kennwort für die Azure-Veröffentlichungseinstellungen.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Redis Cache-Verbindungszeichenfolge 

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für einen Redis-Cache.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure SaaS 

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für Azure SaaS (Software-as-a-Service).
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Service Bus-Verbindungszeichenfolge

    :::column-end:::
    :::column span="2":::

        Verbindungszeichenfolge für eine Azure Service Bus-Instanz.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Storage-Kontoschlüssel 

    :::column-end:::
    :::column span="2":::

       Schlüssel für ein Azure-Speicherkonto. 
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        Azure Storage-Kontoschlüssel (generisch)

    :::column-end:::
    :::column span="2":::

       Generischer Schlüssel für ein Azure-Speicherkonto.
      
    :::column-end:::
:::row-end:::
:::row:::
    :::column span="":::

        SQL Server-Verbindungszeichenfolge 

    :::column-end:::
    :::column span="2":::

       Verbindungszeichenfolge für einen Computer mit SQL Server.
      
    :::column-end:::
:::row-end:::

### <a name="identification"></a>Identifikation

[!INCLUDE [supported identification entities](./identification-entities.md)]
