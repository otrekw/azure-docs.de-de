---
title: Benannte Entitäten für personenbezogene Informationen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/16/2020
ms.author: aahi
ms.openlocfilehash: c50beef5c9c5dcae7edd487e8bf3d192ba557865
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662749"
---
> [!NOTE]
> Um geschützte Gesundheitsinformationen (PHI) zu ermitteln, verwenden Sie den `domain=phi`-Parameter und die Modellversion `2020-04-01` oder höher.
>
> Beispiel: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/entities/recognition/pii?domain=phi&model-version=2020-04-01`
 
Die folgenden Entitätskategorien werden zurückgegeben, wenn Sie Anforderungen an den `/v3.1-preview.1/entities/recognition/pii`-Endpunkt senden.

| Category   | Unterkategorie | BESCHREIBUNG                          | Ab Modellversion | Notizen |
|------------|-------------|--------------------------------------|------------------------|---|
| Person     | –         | Namen von Personen  | `2019-10-01`  | Wird auch mit `domain=phi`zurückgegeben |
| PersonType | –         | Von einer Person eingenommene Position oder Rolle | `2020-02-01` | |
| PhoneNumber | – | Telefonnummern (nur US- und EU-Telefonnummern) | `2019-10-01` | Wird auch mit `domain=phi`zurückgegeben |
|Organization  | – | Firmen, politische Gruppen, Musikgruppen, Sportvereine, Regierungsstellen und öffentliche Organisationen  | `2019-10-01` | Nationalitäten und Religionen werden in diesem Entitätstyp nicht berücksichtigt.  |
|Organization | Medizin | Medizinische Unternehmen und Gruppen | `2020-04-01` |  |
|Organization | Börse | Börsengruppen | `2020-04-01` |  |
| Organization | Sport | Sportbezogene Organisationen | `2020-04-01` |  |
| Adresse | – | Vollständige Postanschrift  | `2020-04-01` | Wird auch mit `domain=phi`zurückgegeben |
| EU-GPS-Koordinaten | – | GPS-Koordinaten für Standorte innerhalb der Europäischen Union.  | `2019-10-01` |  |
| Email | – | E-Mail-Adressen | `2019-10-01` | Wird auch mit `domain=phi`zurückgegeben   |
| URL | – | URLs zu Websites | `2019-10-01` | Wird auch mit `domain=phi`zurückgegeben |
| IP | – | Netzwerk-IP-Adressen | `2019-10-01` | Wird auch mit `domain=phi`zurückgegeben |
| Datetime | – | Datums- und Uhrzeitangaben | `2019-10-01` |  | 
| Datetime | Date | Kalenderdatumsangaben | `2019-10-01` | Wird auch mit `domain=phi`zurückgegeben |
| Menge | – | Zahlen und numerische Mengen. | `2019-10-01` |  |
| Menge | Age | Altersangaben | `2019-10-01` | | |

## <a name="azure-information"></a>Azure-Informationen

Diese Entitätskategorie beinhaltet identifizierbare Azure-Informationen, Authentifizierungsinformationen und Verbindungszeichenfolgen eingeschlossen. Erst ab der Modellversion `2019-10-01` verfügbar. Wird nicht mit dem `domain=phi`-Parameter zurückgegeben

| Unterkategorie                           | BESCHREIBUNG                                                                 |
|---------------------------------------|-----------------------------------------------------------------------------|
| Azure DocumentDB-Authentifizierungsschlüssel             | Autorisierungsschlüssel für einen Azure Cosmos DB-Server.                           |
| Verbindungszeichenfolge für Azure-IaaS-Datenbank und Azure SQL | Verbindungszeichenfolge für eine Azure-IaaS-Datenbank (Infrastructure-as-a-Service) und Azure SQL |
| Azure SQL-Verbindungszeichenfolge           | Verbindungszeichenfolge für eine Datenbank in Azure SQL-Datenbank.                                |
| Azure IoT-Verbindungszeichenfolge           | Verbindungszeichenfolge für Azure IoT.                        |
| Azure-Veröffentlichungseinstellung: Kennwort        | Kennwort für die Azure-Veröffentlichungseinstellungen.                                        |
| Azure Redis Cache-Verbindungszeichenfolge   | Verbindungszeichenfolge für einen Redis-Cache.                             |
| Azure SaaS                             | Verbindungszeichenfolge für Azure SaaS (Software-as-a-Service).                     |
| Azure Service Bus-Verbindungszeichenfolge   | Verbindungszeichenfolge für eine Azure Service Bus-Instanz.                                 |
| Azure Storage-Kontoschlüssel             | Schlüssel für ein Azure-Speicherkonto.                                   |
| Azure Storage-Kontoschlüssel (generisch)   | Generischer Schlüssel für ein Azure-Speicherkonto.                           |
| SQL Server-Verbindungszeichenfolge          | Verbindungszeichenfolge für einen Computer mit SQL Server.                                         |

## <a name="identification"></a>Identifikation

[!INCLUDE [supported identification entities](./identification-entities.md)]
