---
title: 'Erstellen oder Ändern eines Angebots: Azure Marketplace'
description: API zum Erstellen eines neuen oder Aktualisieren eines vorhandenen Angebots.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/29/2020
ms.openlocfilehash: dba47073a6676b17c091af048f583ca3e7712999
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "87420225"
---
# <a name="create-or-modify-an-offer"></a>Erstellen oder Ändern ein Angebots

> [!NOTE]
> Die Cloud-Partnerportal-APIs sind in Partner Center integriert und funktionieren auch weiterhin. Der Übergang führt zu kleineren Änderungen. Beachten Sie die in der [Cloud-Partnerportal-API-Referenz](./cloud-partner-portal-api-overview.md) aufgeführten Änderungen, um sicherzustellen, dass Ihr Code nach dem Übergang zu Partner Center weiterhin funktioniert. CPP-APIs sollten nur für vorhandene Produkte verwendet werden, die bereits vor dem Übergang zu Partner Center integriert wurden. Neue Produkte sollten die Partner Center-APIs für die Übermittlung verwenden.

Dieser Aufruf aktualisiert ein bestimmtes Angebot im Herausgebernamespace oder erstellt ein neues Angebot.

  `PUT https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-Parameter

|  **Name**         |  **Beschreibung**                      |  **Datentyp**  |
|  --------         |  ----------------                     |  -------------  |
| publisherId       |  Herausgeber-ID, z.B. `contoso` |   String |
| offerId           |  Angebots-ID                     |   String        |
| api-version       |  Aktuelle Version der API            |   Date           |
|  |  |  |

## <a name="header"></a>Header

|  **Name**        |  **Wert**               |
|  ---------       |  ----------              | 
| Content-Type     | `application/json`       |
| Authorization    | `Bearer YOUR_TOKEN`      |
|  |  |

## <a name="body-example"></a>Beispiel für Hauptteil

Im folgenden Beispiel wird ein Angebot mit der Angebots-ID (offerId) `contosovirtualmachine` erstellt.

### <a name="request"></a>Anforderung

``` json
  {
      "publisherId": "contoso",
      "offerTypeId": "microsoft-azure-virtualmachines",
      "id": "contosovirtualmachine",
      "offerTypeVersions": {
          "microsoft-azure-virtualmachines": 87,
          "microsoft-azure-marketplace": 39
      },
      "definition": {
          "displayText": "Contoso Virtual Machine Offer",
          "offer": {
          "microsoft-azure-marketplace.title": "Contoso App",
          "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
          "microsoft-azure-marketplace.allowedSubscriptions": ["59160c40-2e25-4dcf-a2fd-6514cb08bf08"],
          "microsoft-azure-marketplace.usefulLinks": [{
              "linkTitle": "Contoso App for Azure",
              "linkUrl": "https://azuremarketplace.microsoft.com"
          }],
          "microsoft-azure-marketplace.categoryMap": [
                {
                    "categoryL1": "analytics",
                    "categoryL2-analytics": [
                        "visualization-and-reporting"
                    ]
                },
                {
                    "categoryL1": "ai-plus-machine-learning",
                    "categoryL2-ai-plus-machine-learning":[
                        "bot-services",
                        "cognitive-services",
                        "other"
                    ]
                }
            ],
          "microsoft-azure-marketplace.smallLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/6218c455-9cbc-450c-9920-f2e7a69ee132.png?sv=2014-02-14&sr=b&sig=6O8MM9dgiJ48VK0MwddkyVbprRAnBszyhVkVHGShhkI%3D&se=2019-03-28T19%3A46%3A50Z&sp=r",
          "microsoft-azure-marketplace.mediumLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/557e714b-2f31-4e12-b0cc-e48dd840edf4.png?sv=2014-02-14&sr=b&sig=NwL67NTQf9Gc9VScmZehtbHXpYmxhwZc2foy3o4xavs%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.largeLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/142485da-784c-44cb-9523-d4f396446258.png?sv=2014-02-14&sr=b&sig=xaMxhwx%2FlKYfz33mJGIg8UBdVpsOwVvqhjTJ883o0iY%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.wideLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/48af9013-1df7-4c94-8da8-4626e5039ce0.png?sv=2014-02-14&sr=b&sig=%2BnN7f2tprkrqb45ID6JlT01zXcy1PMTkWXtLKD6nfoE%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.heroLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/c46ec74d-d214-4fb5-9082-3cee55200eba.png?sv=2014-02-14&sr=b&sig=RfDvjowFGpP4WZGAHylbF2CuXwO2NXOrwycrXEJvJI4%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.screenshots": [],
          "microsoft-azure-marketplace.videos": [],
          "microsoft-azure-marketplace.leadDestination": "None",
          "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
          "microsoft-azure-marketplace.termsOfUse": "Terms of use",
          "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
          "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
          "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
          "microsoft-azure-marketplace.supportContactName": "Jon Doe",
          "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
          "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
          "microsoft-azure-marketplace.publicAzureSupportUrl": "",
          "microsoft-azure-marketplace.fairfaxSupportUrl": ""
      },
      "plans": 
      [
          {
              "planId": "contososkuidentifier",
              "microsoft-azure-virtualmachines.skuTitle": "Contoso App",
              "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
              "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
              "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
              "microsoft-azure-virtualmachines.cloudAvailability": ["PublicAzure"],
              "virtualMachinePricing": {
                  "isByol": true,
                  "freeTrialDurationInMonths": 0
              },
              "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
              "microsoft-azure-virtualmachines.windowsOSType": "Other",
              "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
              "microsoft-azure-virtualmachines.recommendedVMSizes": ["a0-basic", "a0-standard", "a1-basic", "a1-standard", "a2-basic", "a2-standard"],
              "microsoft-azure-virtualmachines.openPorts": [],
              "microsoft-azure-virtualmachines.vmImages": 
              {
                  "1.0.1": 
                  {
                      "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sr=c&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                      "lunVhdDetails": []
                  }
              },
              "regions": ["AZ"]
          }
          ]
      },
      "eTag": "W/\"datetime'2017-06-07T06%3A15%3A40.4771399Z'\"",
      "version": 5
  }
```

### <a name="response"></a>Antwort

``` json
 {
         "offerTypeId": "microsoft-azure-virtualmachines",
         "publisherId": "contoso",
         "status": "neverPublished",
         "id": "contosovirtualmachine",
         "version": 1,
         "definition": {
         "displayText": "Contoso Virtual Machine Offer",
         "offer": 
         {
             "microsoft-azure-marketplace-testdrive.videos": [],
             "microsoft-azure-marketplace.title": "Contoso App",
             "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
             "microsoft-azure-marketplace.allowedSubscriptions": 
             [
                 "59160c40-2e25-4dcf-a2fd-6514cb08bf08"
             ],
             "microsoft-azure-marketplace.usefulLinks": 
             [
                 {
                     "linkTitle": "Contoso App for Azure",
                     "linkUrl": "https://azuremarketplace.microsoft.com"
                 }
             ],
             "microsoft-azure-marketplace.categoryMap":
             [
                 {
                    "categoryL1": "analytics",
                    "categoryL2-analytics": [
                    "visualization-and-reporting"
                    ]
                 },
                 {
                    "categoryL1": "ai-plus-machine-learning",
                    "categoryL2-ai-plus-machine-learning": [
                    "bot-services",
                    "cognitive-services",
                    "other"
                    ]
                 }
             ],
             "microsoft-azure-marketplace.smallLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/8affcd28-60a5-4839-adf8-c560e069fd61.png?sv=2014-02-14&sr=b&sig=nGErAgn%2BDUecrX892wcmk32kh0MHgIZeJ5jcKyY%2Fuew%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.mediumLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/39550bca-1110-432c-9ea9-e12b3a2288cd.png?sv=2014-02-14&sr=b&sig=4X0hlkXYtuZOmcYq%2BsbYVZz3k5k26kngcFX6yBAJjNI%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.largeLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/ce3576e3-df12-4074-b0a3-0b8d3f329df1.png?sv=2014-02-14&sr=b&sig=mFhtCUQh%2FbFz10nlIWbqsz6jq5MBZ0M%2F5cIREE9P6V0%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.wideLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/476d6edd-12d3-4414-9def-d2970c4a9de4.png?sv=2014-02-14&sr=b&sig=pg4MDSZjAb8w8D%2FrQ9RT%2BodpynSy%2FlYOvpx0yeam2Bw%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.heroLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/46c85b7b-4438-4e0d-8218-24fb5651727a.png?sv=2014-02-14&sr=b&sig=wIsCOO5%2BDj8NsLVSwwzwTgogF71oA7Q1XjKhNB1ni5g%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.screenshots": [],
             "microsoft-azure-marketplace.videos": [],
             "microsoft-azure-marketplace.leadDestination": "None",
             "microsoft-azure-marketplace.tableLeadConfiguration": {},
             "microsoft-azure-marketplace.blobLeadConfiguration": {},
             "microsoft-azure-marketplace.salesForceLeadConfiguration": {},
             "microsoft-azure-marketplace.crmLeadConfiguration": {},
             "microsoft-azure-marketplace.httpsEndpointLeadConfiguration": {},
             "microsoft-azure-marketplace.marketoLeadConfiguration": {},
             "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
             "microsoft-azure-marketplace.termsOfUse": "Terms of use",
             "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
             "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
             "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
             "microsoft-azure-marketplace.supportContactName": "Jon Doe",
             "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
             "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
             "microsoft-azure-marketplace.publicAzureSupportUrl": "",
             "microsoft-azure-marketplace.fairfaxSupportUrl": ""
         },
         "plans": 
         [
             {
                 "planId": "contososkuidentifier",
                 "microsoft-azure-virtualmachines.skuTitle": "Contoso App (Old Title)",
                 "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
                 "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
                 "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
                 "microsoft-azure-virtualmachines.cloudAvailability": 
                 [
                     "PublicAzure"
                 ],
                 "microsoft-azure-virtualmachines.certificationsFairfax": [],
                 "virtualMachinePricing": {
                     "isByol": true,
                     "freeTrialDurationInMonths": 0
                 },
                 "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
                 "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
                 "microsoft-azure-virtualmachines.recommendedVMSizes": 
                 [
                     "a0-basic",
                     "a0-standard",
                     "a1-basic",
                     "a1-standard",
                     "a2-basic",
                     "a2-standard"
                 ],
                 "microsoft-azure-virtualmachines.openPorts": [],
                 "microsoft-azure-virtualmachines.vmImages": 
                 {
                     "1.0.1": 
                     {
                         "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sr=c&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                         "lunVhdDetails": []
                     }
                 },
                 "regions":
                 [
                     "AZ"
                 ]
             } 
         ]
     },
     "changedTime": "2018-03-28T22:27:13.8363879Z"
 }
```

> [!NOTE]
> Um dieses Angebot zu ändern, fügen Sie der oben angegebenen Anforderung einen **If-Match**-Header hinzu, der auf * festgelegt ist. Verwenden Sie denselben Anforderungstext wie oben, aber ändern Sie die Werte wie gewünscht. 

### <a name="response-status-codes"></a>Antwortstatuscodes

| **Code**  |  **Beschreibung**                                                                            |
| --------  |  ---------------                                                                            |
|  200      | `OK`. Die Anforderung wurde erfolgreich verarbeitet, und das Angebot wurde erfolgreich geändert.           |
|  201      | `Created`. Die Anforderung wurde erfolgreich verarbeitet, und das Angebot wurde erfolgreich erstellt.   |
|  400      | `Bad/Malformed request`. Der Fehlerantworttext enthält ggf. weitere Informationen.            |
|  403      | `Forbidden`. Der Client hat keinen Zugriff auf den angeforderten Namespace.                     |
|  404      | `Not found`. Die Entität, auf die im Client verwiesen wird, gibt es nicht.                           |
|  412      | Der Server erfüllt nicht alle Voraussetzungen, die die anfordernde Person in der Anforderung angegeben hat. Der Client sollte das ETAG prüfen, das mit der Anforderung gesendet wurde. |
|  |  |

## <a name="uploading-artifacts"></a>Hochladen von Artefakten

Artefakte, etwa Bilder und Logos, sollten freigegeben werden, indem sie in einen zugänglichen Speicherort im Internet hochgeladen und anschließend jeweils als URI in die PUT-Anforderung eingefügt werden (siehe obiges Beispiel). Das System erkennt, dass diese Dateien nicht im Azure Marketplace-Speicher vorhanden sind, und lädt diese Dateien in den Speicher herunter.  Daher werden Sie feststellen, dass zukünftige GET-Anforderungen eine Azure Marketplace-Dienst-URL für diese Dateien zurückgeben.

## <a name="categories-and-industries"></a>Kategorien und Branchen

Wenn Sie ein neues Angebot erstellen, müssen Sie eine Kategorie für Ihr Angebot im Marketplace angeben. Optional können Sie für einige Angebotstypen auch Branchen angeben. Geben Sie je nach Angebotstyp unter Verwendung der spezifischen Schlüsselwerte in den folgenden Tabellen die zutreffenden Kategorien/Branchen für Ihr Angebot an.

### <a name="azure-marketplace-categories"></a>Azure Marketplace-Kategorien

Diese Kategorien und Schlüssel gelten für folgende Angebotstypen: Azure-Apps, VMs, Core-VMs, Container, Container-Apps, IoT Edge-Module und SaaS. Fett und kursiv formatierte Textelemente (z. B. ***Analyse***) sind Kategorien, und normal formatierte Textelemente (z. B. „Datenerkenntnisse“) sind zugehörige Unterkategorien. Verwenden Sie die genauen Schlüsselwerte, ohne den Zeichenabstand oder die Groß-/Kleinschreibung zu ändern.

| Category | SaaS-Schlüssel | Azure-App-Schlüssel | Schlüssel für VMs, Container, Container-Apps, IoT Edge-Module und Core-VMs |
| --- | --- | --- | --- |
| ***Analyse*** | ***analytics*** | ***analytics-azure-apps*** | ***analytics-amp*** |
| Datenerkenntnisse | data-insights | data-insights | data-insights |
| Datenanalyse | data-analytics | data-analytics | data-analytics |
| Big Data | big-data | bigData | big-data |
| Predictive Analytics | predictive-analytics | predictive-analytics | predictive-analytics |
| Echtzeit-/Streaminganalyse | real-time-streaming-analytics | real-time-streaming-analytics | real-time-streaming-analytics |
| Andere | Sonstige | other-analytics | Sonstige |
| ***KI und Machine Learning*** | ***ArtificialIntelligence*** | ***ai-plus-machine-learning*** | ***ai-plus-machine-learning*** |
| Botdienste | bot-services | bot-services | bot-services |
| Cognitive Services | cognitive-services | cognitive-services | cognitive-services |
| ML-Dienst | ml-service | ml-service | ml-service |
| Automatisiertes maschinelles Lernen | automated-ml | automated-ml | automated-ml |
| Geschäftsprozessautomatisierung/robotergesteuerte Prozessautomatisierung | business-robotic-process-automation | business-robotic-process-automation | business-robotic-process-automation |
| Datenbeschriftung | data-labelling | data-labelling | data-labelling |
| Vorbereitung der Daten | data-preparation | data-preparation | data-preparation |
| Knowledge Mining | knowledge-mining | knowledge-mining | knowledge-mining |
| ML-Vorgänge | ml-operations | ml-operations | ml-operations |
| Andere | other-AI-plus-machine-learning | Sonstige | Sonstige |
| ***Blockchain*** | ***blockchain*** | ***blockchain*** | ***blockchain*** |
| App-Beschleuniger | app-accelerators | app-accelerators | app-accelerators |
| Ledger mit einem Knoten | single-node-ledger | single-node-ledger | single-node-ledger |
| Ledger mit mehreren Knoten | multi-node-ledger | multi-node-ledger | multi-node-ledger |
| Tools | Tools | Tools | Tools |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Compute*** | ***compute-saas*** | ***compute-azure-apps*** | ***compute*** |
| Anwendungsinfrastruktur | appInfra | appInfrastructure | application-infrastructure |
| Betriebssysteme | clientOS | clientOS | operating-systems |
| Cache | cache | cache | cache |
| Andere | other-compute | other-compute | Sonstige |
| ***Container*** | ***containers*** | ***containers*** | ***containers*** |
| Container-Apps | container-apps | container-apps | container-apps |
| Containerimages | container-images | container-images | container-images |
| Erste Schritte mit Containern | get-started-with-containers | get-started-with-containers | get-started-with-containers |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Datenbanken*** | ***databases-saas*** | ***database*** | ***databases*** |
| NoSQL-Datenbanken | nosql-databases | nosql-databases | nosql-databases |
| Relationale Datenbanken | relational-databases | relational-databases | relational-databases |
| Ledger-/Blockchain-Datenbanken | ledger-blockchain-databases | ledger-blockchain-databases | ledger-blockchain-databases |
| Data Lakes | data-lakes | data-lakes | data-lakes |
| Data Warehouse | data-warehouse | data-warehouse | data-warehouse |
| Andere | other-databases | other-databases | Sonstige |
| ***Entwicklertools*** | ***developer-tools-saas*** | ***developer-tools-azure-apps*** | ***developer-tools*** |
| Tools | tools-developer-tools | tools-developer-tools | tools-developer-tools |
| Skripts | In Skripts | In Skripts | In Skripts |
| Entwicklerdienst | devService | devService | developer-service |
| Andere | other-developer-tools | other-developer-tools | Sonstige |
| ***DevOps*** | ***devops*** | ***devops*** | ***devops*** |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Identität*** | ***Identität*** | ***Identität*** | ***Identität*** |
| Zugriffsverwaltung | access-management | access-management | access-management |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Integration*** | ***integration*** | ***integration*** | ***integration*** |
| Nachrichten | messaging | messaging | messaging |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Internet der Dinge (IoT)*** | ***IoT*** | ***internet-of-things-azure-apps*** | ***internet-of-things*** |
| IoT-Basisdienste | – | iot-core-services | iot-core-services |
| IoT Edge-Module | – | iot-edge-modules | iot-edge-modules |
| IoT-Lösungen | iot-solutions | iot-solutions | iot-solutions |
| Datenanalyse und Visualisierung | data-analytics-and-visualization | data-analytics-and-visualization | data-analytics-and-visualization |
| IoT-Konnektivität | iot-connectivity | iot-connectivity | iot-connectivity |
| Andere | other-internet-of-things | other-internet-of-things | Sonstige |
| ***IT- und Verwaltungstools*** | ***ITandAdministration*** | ***it-and-management-tools-azure-apps*** | ***it-and-management-tools*** |
| Verwaltungslösungen | management-solutions | management-solutions | management-solutions |
| Geschäftsanwendungen | businessApplication | businessApplication | business-applications |
| Andere | other-it-management-tools | other-it-management-tools | Sonstige |
| ***Überwachung und Diagnose*** | ***monitoring-and-diagnostics*** | ***monitoring-and-diagnostics*** | ***monitoring-and-diagnostics*** |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Medien*** | ***media*** | ***media*** | ***media*** |
| Media Services | media-services | media-services | media-services |
| Inhaltsschutz | content-protection | content-protection | content-protection |
| Live- und On-Demand-Streaming | live-and-on-demand-streaming | live-and-on-demand-streaming | live-and-on-demand-streaming |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Migration*** | ***migration*** | ***migration*** | ***migration*** |
| Datenmigration | data-migration | data-migration | data-migration |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Mixed Reality*** | ***mixed-reality*** | ***mixed-reality*** | ***mixed-reality*** |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Netzwerk*** | ***networking*** | ***networking*** | ***networking*** |
| Appliance-Manager | appliance-managers | appliance-managers | appliance-managers |
| Konnektivität | Konnektivität | Konnektivität | Konnektivität |
| Firewalls | Firewalls | Firewalls | Firewalls |
| Load Balancer | load-balancers | load-balancers | load-balancers |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Security*** | ***security*** | ***security*** | ***security*** |
| Identitäts- und Zugriffsverwaltung | identity-and-access-management | identity-and-access-management | identity-and-access-management |
| Bedrohungsschutz | threat-protection | threat-protection | threat-protection |
| Information Protection | information-protection | information-protection | information-protection |
| Andere | Sonstige | Sonstige | Sonstige |
| ***Storage*** | ***storage-saas*** | ***storage-azure-apps*** | ***storage*** |
| Sicherung und Wiederherstellung | Sicherung | Sicherung | backup-and-recovery |
| Unternehmenshybridspeicher | enterprise-hybrid-storage | enterprise-hybrid-storage | enterprise-hybrid-storage |
| Dateifreigabe | file-sharing | file-sharing | file-sharing |
| Verwaltung des Datenlebenszyklus | data-lifecycle-management | data-lifecycle-management | data-lifecycle-management |
| Andere | other-storage | other-storage | Sonstige |
| ***Web*** | ***web*** | ***web*** | ***web*** |
| Blogs und CMSs | blogs-and-cmss | blogs-and-cmss | blogs-and-cmss |
| Web-Apps für Einsteiger | starter-web-apps | starter-web-apps | starter-web-apps |
| E-Commerce | ecommerce | ecommerce | ecommerce |
| Web-App-Frameworks | web-apps-frameworks | web-apps-frameworks | web-apps-frameworks |
| Web-Apps | web-apps | web-apps | web-apps |
| Andere | Sonstige | Sonstige | Sonstige |
||||

### <a name="microsoft-appsource-categories"></a>Microsoft AppSource-Kategorien

Diese Kategorien und Schlüssel gelten für folgende Angebotstypen: SaaS, Power BI-App, Dynamics 365 Business Central, Dynamics 365 for Customer Engagement und Dynamics 365 for Operations. Fett und kursiv formatierte Textelemente (z. B. ***Analyse***) sind Kategorien, und normal formatierte Textelemente (z. B. „Erweiterte Analyse“) sind zugehörige Unterkategorien. Verwenden Sie die genauen Schlüsselwerte, ohne den Zeichenabstand oder die Groß-/Kleinschreibung zu ändern.

| Category | SaaS-Schlüssel | Schlüssel für Dynamics 365 Business Central, Dynamics 365 for Customer Engagement und Dynamics 365 for Operations | Power BI-App-Schlüssel |
| --- | --- | --- | --- |
| ***Analyse*** | ***analytics*** | ***Analyse*** | ***Analyse*** |
| Erweiterte Analyse | advanced-analytics | advanced-analytics | advanced-analytics |
| Visualisierung und Berichterstellung | visualization-reporting | visualization-reporting | visualization-reporting |
| Andere | Sonstige | other-analytics | other-analytics |
| ***KI und Machine Learning*** | ***ArtificialIntelligence*** | ***ai-plus-machine-learning-dynamics*** | ***ai-plus-machine-learning-appsource*** |
| KI für Unternehmen | ai-for-business | ai-for-business | ai-for-business |
| Bot-Apps | bot-apps | bot-apps | bot-apps |
| Andere | other-ai-plus-machine-learning | other-ai-plus-machine-learning | other-ai-plus-machine-learning |
| ***Kollaboration*** | ***Kollaboration*** | ***Kollaboration*** | ***collaboration*** |
| Kontakte und Personen | contact-people | contact-people | contact-and-people |
| Besprechungsmanagement | meeting-management | meeting-management | meeting-management |
| Websitedesign und -verwaltung | site-design-management | site-design-management | site-design-and-management |
| Aufgaben- und Projektmanagement | task-project-management | task-project-management | task-and-project-management |
| Sprach- und Videokonferenzen | voice-video-conferencing | voice-video-conferencing | voice-and-video-conferencing |
| Andere | other-collaboration | other-collaboration | Sonstige |
| ***Compliance und rechtliche Angelegenheiten*** | ***compliance*** | ***compliance*** | ***compliance-and-legal*** |
| Steuern und Prüfungen | tax-audit | tax-audit | tax-and-audit |
| Rechtliche Hinweise | Rechtliche Hinweise | Rechtliche Hinweise | legal |
| Daten, Governance und Datenschutz | data-governance-privacy | data-governance-privacy | data-governance-and-privacy |
| Integrität und Sicherheit | health-safety | health-safety | health-and-safety |
| Andere | other-compliance-legal | other-compliance-legal | Sonstige |
| ***Kundendienst*** | ***CustomerService*** | ***CustomerService*** | ***customer-service*** |
| Kontaktcenter | contact-center | contact-center | contact-center |
| Persönlicher Dienst | face-to-face-service | face-to-face-service | face-to-face-service |
| Backoffice und Personaldienste | back-office-employee-service | back-office-employee-service | back-office-and-employee-service |
| Wissens- und Fallmanagement | knowledge-case-management | knowledge-case-management | knowledge-and-case-management |
| Soziale Medien und Omnichannel-Kundenbindung | social-media-omnichannel-engagement | social-media-omnichannel-engagement | social-media-and-omnichannel-engagement |
| Andere | other-customer-service | other-customer-service | Sonstige |
| ***Finanzen*** | ***Finanzen*** | ***Finanzen*** | ***finance*** |
| Buchhaltung | accounting (Buchhaltung) | accounting (Buchhaltung) | accounting (Buchhaltung) |
| Ressourcenverwaltung | asset-management | asset-management | asset-management |
| Analyse, Konsolidierung und Berichterstellung | analytics-consolidation-reporting | analytics-consolidation-reporting | analytics-consolidation-and-reporting |
| Kredite und Inkasso | credit-collections | credit-collections | credit-and-collections |
| Compliance- und Risikomanagement | compliance-risk-management | compliance-risk-management | compliance-and-risk-management |
| Andere | other-finance | other-finance | Sonstige |
| ***Personalverwaltung*** | ***HumanResources*** | ***HumanResources*** | ***human-resources*** |
| Talentakquise | talent-acquisition | talent-acquisition | talent-acquisition |
| Talentmanagement | talent-management | talent-management | talent-management |
| Personalverwaltung | hr-operations | hr-operations | hr-operations |
| Personalplanung und Analyse | workforce-planning-analytics | workforce-planning-analytics | workforce-planning-and-analytics |
| Andere | other-human-resources | other-human-resources | Sonstige |
| ***Internet der Dinge (IoT)*** | ***IoT*** | ***internet-of-things-dynamics*** | ***internet-of-things-appsource*** |
| Asset-Management und Betriebsführung | asset-management-operations | asset-management-operations | asset-management-and-operations |
| Verbundene Produkte | connected-products | connected-products | connected-products |
| Intelligente Lieferkette | intelligent-supply-chain | intelligent-supply-chain | intelligent-supply-chain |
| Predictive Maintenance | predictive-maintenance | predictive-maintenance | predictive-maintenance |
| Remoteüberwachung | remote-monitoring | remote-monitoring | remote-monitoring |
| Sicherheit | safety-security | safety-security | safety-and-security |
| Intelligente Infrastruktur und Ressourcen | smart-infrastructure-resources | smart-infrastructure-resources | smart-infrastructure-and-resources |
| Fahrzeuge und Mobilität | vehicles-mobility | vehicles-mobility | vehicles-and-mobility |
| Andere | other-internet-of-things | other-internet-of-things | Sonstige |
| ***IT- und Verwaltungstools*** | ***ITandAdministration*** | ***ITandAdministration*** | ***it-and-management-tools*** |
| Verwaltungslösungen | management-solutions | management-solutions | management-solutions |
| Geschäftsanwendungen | businessApplication | businessApplication | business-applications |
| Andere | other-it-management-tools | other-it-management-tools | Sonstige |
| ***Marketing*** | ***Marketing*** | ***Marketing*** | ***marketing*** |
| Werbung | advertisement | advertisement | advertisement |
| Analytics | analytics-marketing | analytics-marketing | analytics-marketing |
| Kampagnenmanagement und -automatisierung | campaign-management-automation | campaign-management-automation | campaign-management-and-automation |
| E-Mail-Marketing | email-marketing | email-marketing | email-marketing |
| L2: Veranstaltungs- und Ressourcenmanagement | events-resource-management | events-resource-management | events-and-resource-management |
| Forschung und Analyse | research-analytics | research-analytics | research-and-analysis |
| Soziale Medien | social-media | social-media | social-media |
| Andere | other-marketing | other-marketing | Sonstige |
| ***Betrieb und Lieferkette*** | ***OperationsSupplyChain*** | ***OperationsSupplyChain*** | ***operations-and-supply-chain*** |
| Asset- und Produktionsmanagement | asset-production-management | asset-production-management | asset-and-production-management |
| Bedarfsvorhersage | demand-forecasting | demand-forecasting | demand-forecasting |
| Informationsverwaltung und Konnektivität | information-management-connectivity | information-management-connectivity | information-management-and-connectivity |
| Planung, Einkauf und Berichterstellung | planning-purchasing-reporting | planning-purchasing-reporting | planning-purchasing-and-reporting |
| Qualitäts- und Servicemanagement | quality-service-management | quality-service-management | quality-and-service-management |
| Vertriebs- und Auftragsmanagement | sales-order-management | sales-order-management | sales-and-order-management |
| Transport- und Lagerverwaltung | transportation-warehouse-management | transportation-warehouse-management | transportation-and-warehouse-management |
| Andere | other-operations-supply-chain | other-operations-supply-chain | Sonstige |
| ***Produktivität*** | ***Produktivität*** | ***Produktivität*** | ***productivity*** |
| Inhaltserstellung und -verwaltung | content-creation-management | content-creation-management | content-creation-and-management |
| Sprachen und Übersetzungen | language-translation | language-translation | language-and-translation |
| Dokumentverwaltung | document-management | document-management | document-management |
| E-Mail-Verwaltung | email-management | email-management | email-management |
| Suche und Referenzierung | search-reference | search-reference | search-and-reference |
| Andere | other-productivity | other-productivity | Sonstige |
| Gamification | Gamification | Gamification | gamification |
| ***Vertrieb*** | ***Vertrieb*** | ***Vertrieb*** | ***Vertrieb*** |
| Teleshopping | telesales | telesales | telesales |
| CPQ (Configure, Price, Quote) | configure-price-quote | configure-price-quote | configure-price-quote |
| Vertragsverwaltung | contract-management | contract-management | contract-management |
| CRM | crm | crm | crm |
| E-Commerce | e-commerce | e-commerce | e-commerce |
| Anreicherung von Geschäftsdaten | business-data-enrichment | business-data-enrichment | business-data-enrichment |
| Vertriebsoptimierung | sales-enablement | sales-enablement | sales-enablement |
| Andere | other-sales | other-sales | other-sales |
| ***Geolocation*** | ***geolocation*** | ***geolocation*** | ***geolocation*** |
| Karten | maps | maps | maps |
| Nachrichten und Wetter | news-and-weather | news-and-weather | news-and-weather |
| Andere | other-geolocation | other-geolocation | other-geolocation |
||||

### <a name="microsoft-appsource-industries"></a>Microsoft AppSource-Branchen

Diese Branchen und Schlüssel gelten für folgende Angebotstypen: SaaS, Power BI-App, Dynamics 365 Business Central, Dynamics 365 for Customer Engagement und Dynamics 365 for Operations. Fett und kursiv formatierte Textelemente (z. B. ***Automobile***) sind Kategorien, und normal formatierte Textelemente (z. B. „Automobilbau“) sind zugehörige Unterkategorien. Verwenden Sie die genauen Schlüsselwerte, ohne den Zeichenabstand oder die Groß-/Kleinschreibung zu ändern.

| Branche | Schlüssel für SaaS, Dynamics 365 Business Central, Dynamics 365 for Customer Engagement und Dynamics 365 for Operations | Power BI-App-Schlüssel |
| --- | --- | --- |
| ***Automobile*** | ***Automobile*** | ***automotive*** |
| Automobilbau | AutomotiveL2 | AutomotiveL2 |
| ***Landwirtschaft*** | ***Landwirtschaft*** | ***agriculture*** |
| Sonstige – nicht segmentiert | Agriculture\_OtherUnsegmented | other-unsegmented |
| ***Distribution*** | ***Distribution*** | ***distribution*** |
| Großhandel | Großhandel | wholesale |
| Päckchen- und Paketversand | ParcelAndPackageShipping | parcel-and-package-shipping |
| ***Education*** | ***Education*** | ***education*** |
| Hochschulbildung | HigherEducation | higher-education |
| Grund- und Sekundarschulbildung/K-12 | PrimaryAndSecondaryEducationK12 | primary-and-secondary-education |
| Bibliotheken und Museen | LibrariesAndMuseums | libraries-and-museums |
| ***Finanzdienstleistungen*** | ***FinancialServices*** | ***financial-services*** |
| Bankwesen und Kapitalmärkte | BankingAndCapitalMarkets | banking-and-capital-markets |
| Versicherung | Versicherung | Versicherung |
| ***Behörden*** | ***Behörden*** | ***government*** |
| Verteidigung und Nachrichtendienste | DefenseAndIntelligence | defense-and-intelligence |
| Öffentliche Sicherheit und Justiz | PublicSafetyAndJustice | public-safety-and-justice |
| Zivilbehörde | CivilianGovernment | civilian-government |
| ***Gesundheitswesen*** | ***HealthCareandLifeSciences*** | ***healthcare*** |
| Kostenträger für Gesundheitssystem | HealthPayor | health-payor |
| Gesundheitsdienstleister | HealthProvider | health-provider |
| Arzneimittel | Arzneimittel | Pharmazeutika |
| ***Fertigung und Ressourcen*** | ***Manufacturing*** | ***manufacturing-and-resources*** |
| Chemie und Agrochemie | ChemicalAndAgrochemical | chemical-and-agrochemical |
| Diskrete Fertigung | DiscreteManufacturing | discrete-manufacturing |
| Energieversorgung | Energieversorgung | energy |
| ***Einzelhandel und Konsumgüter*** | ***RetailandConsumerGoods*** | ***retail-and-consumer-goods*** |
| Konsumgüter | ConsumerGoods | consumer-goods |
| Einzelhändler | Einzelhändler | retailers |
| ***Medien und Kommunikation*** | ***MediaAndCommunications*** | ***media-and-communications*** |
| Medien und Unterhaltung | MediaandEntertainment | media-and-entertainment |
| Telekommunikation | Telekommunikation | telecommunications |
| ***Dienstleistungsunternehmen*** | ***ProfessionalServices*** | ***professional-services*** |
| Rechtliche Hinweise | Rechtliche Hinweise | legal |
| Professionelle Partnerdienstleistungen | PartnerProfessionalServices | partner-professional-services |
| ***Architektur und Bauwesen*** | ***ArchitectureAndConstruction*** | ***architecture-and-construction*** |
| Sonstige – nicht segmentiert | ArchitectureAndConstruction\_OtherUnsegmented | other-unsegmented |
| ***Hotel- und Gaststättengewerbe, Tourismus*** | ***HospitalityandTravel*** | ***hospitality-and-travel*** |
|    Hotels und Freizeit | HotelsAndLeisure | hotels-and-leisure |
| Transport und Verkehr | TravelAndTransportation | travel-and-transportation |
| Restaurants und Gastronomie | RestaurantsAndFoodServices | restaurants-and-food-services |
| ***Andere Branchen des öffentlichen Sektors*** | ***OtherPublicSectorIndustries*** | ***other-public-sector-industries*** |
| Forstwirtschaft und Fischerei | ForestryAndFishing | forestry-and-fishing |
| Gemeinnützige Organisationen | Nonprofits | nonprofits |
| ***Immobilien*** | ***RealEstate*** | ***real-estate*** |
| Sonstige – nicht segmentiert | RealEstate\_OtherUnsegmented | other-unsegmented |
|||
