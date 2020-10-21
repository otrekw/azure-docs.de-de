---
title: Was ist Azure FarmBeats?
description: In diesem Artikel erhalten Sie eine Übersicht über Azure FarmBeats.
author: uhabiba04
ms.topic: overview
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: d9882f23096e19435566323de4d0ebaf7c0af3ef
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331922"
---
# <a name="overview-of-azure-farmbeats-preview"></a>Übersicht über Azure FarmBeats (Vorschau)

Azure FarmBeats ist ein Business-to-Business-Angebot, das in Azure Marketplace verfügbar ist. Es ermöglicht die Aggregation von landwirtschaftlichen Datasets über mehrere Anbieter hinweg. Mit Azure FarmBeats können Sie KI- oder ML-Modelle basierend auf zusammengeführten Datasets erstellen. Mithilfe von Azure FarmBeats können Landwirtschaftsbetriebe sich auf wichtige wertsteigernde Anwendungen konzentrieren, anstatt sich um die schwierigen Aufgaben der Datentechnik kümmern zu müssen.

> [!NOTE]
> Azure FarmBeats befindet sich zurzeit in der öffentlichen Vorschau. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Azure FarmBeats wird ohne Vereinbarung zum Servicelevel bereitgestellt. Unterstützung erhalten Sie über das [Azure FarmBeats-Supportforum](https://aka.ms/farmbeatssupport).

![FarmBeats-Projekt](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

Die Vorschau von Azure FarmBeats ermöglicht Folgendes:

- Bewerten des Betriebszustands anhand von Vegetationsindex und Wasserindex auf der Grundlage von Satellitenbildern
- Abrufen von Empfehlungen dazu, wie viele Sensoren für die Bodenfeuchte verwendet und wo diese platziert werden sollten
- Verfolgen des Betriebszustands durch Visualisierung von Bodendaten, die von Sensoren verschiedener Hersteller gesammelt wurden
- Abrufen einer Karte zur Bodenfeuchte, die auf der Zusammenführung von Satelliten- und Sensordaten basiert
- Gewinnen handlungsrelevanter Erkenntnisse durch das Erstellen von KI/ML-Modellen, die auf aggregierten Datasets aufbauen
- Erstellen oder Erweitern Ihrer digitalen Lösung für den landwirtschaftlichen Betrieb durch Bereitstellen von Hinweisen zum Betriebszustand

## <a name="datahub"></a>Datenhub

Der Azure FarmBeats-Datenhub ist eine API-Ebene, die eine anbieterübergreifende Aggregation, Normalisierung und Kontextualisierung verschiedener Datasets ermöglicht, die landwirtschaftliche Daten enthalten. Mit Azure FarmBeats profitieren Sie von Folgendem:
- **Sensordaten** von drei Herstellern: [Davis Instruments](https://www.davisinstruments.com/product/enviromonitor-gateway/), [Teralytic](https://teralytic.com/) und [Pessl Instruments](https://metos.at/)
- **Satellitenbilder** der Satellitenmission [Sentinel-2](https://sentinel.esa.int/web/sentinel/home) der ESA
- **Drohnenbilder** von drei Anbietern solcher Bilder: [senseFly](https://www.sensefly.com/), [SlantRange](https://slantrange.com/) und [DJI](https://dji.com/)

Der Datenhub ist als erweiterbare API-Plattform konzipiert. Wir arbeiten mit zahlreichen weiteren Anbietern zusammen, um eine Integration in Azure FarmBeats zu ermöglichen und Ihnen so bei der Entwicklung Ihrer Lösung eine größere Auswahl zu bieten.

## <a name="accelerator"></a>Accelerator

Der Azure FarmBeats Accelerator ist eine Beispielwebanwendung, die auf dem Datenhub aufsetzt. Der Accelerator beschleunigt die Entwicklung von Benutzeroberfläche und Modell. Der Azure FarmBeats Accelerator verwendet Azure FarmBeats-APIs. Er visualisiert erfasste Sensordaten als Diagramme und Modellausgaben als Karten. Beispielsweise können Sie mit dem Accelerator schnell einen Agrarbetrieb erstellen und ganz einfach eine Vegetationsindexkarte oder eine Karte zur Sensorplatzierung für diesen Betrieb erstellen.

## <a name="azure-role-based-access-control-azure-rbac"></a>Rollenbasierte Zugriffssteuerung von Azure (Azure RBAC)

Ein Administrator kann mithilfe einer der vordefinierten Rollen Zugriffsregeln für Azure FarmBeats definieren. Über Rollen wird festgelegt, auf welche Bereiche einer Anwendung ein Benutzer zugreifen und welche Aktionen er ausführen darf. In Azure FarmBeats stehen zwei Arten von Rollen (für Benutzer und Partner) zur Verfügung.

### <a name="user-roles"></a>Benutzerrollen

Ein [Administrator kann Benutzer hinzufügen und verwalten](manage-users-in-azure-farmbeats.md) und deren Zugriffsebenen auf der Grundlage von zwei Benutzerrollen definieren: „Administrator“ und „Schreibgeschützt“.

### <a name="partner-roles"></a>Partnerrollen

Ein Administrator kann Azure FarmBeats mehrere Partner als Datenanbieter hinzufügen. Im Anschluss werden die in FarmBeats verfügbaren Partnerrollen und deren Berechtigungen zusammengefasst:

| Partnertyp    |   Aktionen  | `Scope` |
| ---- | -------- | -------- |
| Partner für Sensordaten  |   Erstellen, Lesen, Aktualisieren <br/> <br/> Lesen, Aktualisieren | DeviceModel, Device, SensorModel, Sensor <br/> <br/> ExtendedType |
| Partner für Bilder  |   Erstellen, Lesen, Aktualisieren <br/> <br/> Lesen, Aktualisieren <br/> <br/> Lesen | Scene, SceneFile <br/> <br/> ExtendedType <br/> <br/> Bauernhof |
| Partner für Wetterdaten <br/> <br/>  (demnächst verfügbar) |   Erstellen, Lesen, Aktualisieren <br/> <br/> Lesen, Aktualisieren <br/> <br/> Lesen | WeatherDataModel, WeatherDataLocation, JobType <br/> <br/> ExtendedType <br/> <br/> Bauernhof |

## <a name="resources"></a>Ressourcen

Azure FarmBeats wird kostenlos angeboten, Sie bezahlen nur für die von Ihnen verwendeten Azure-Ressourcen. In den folgenden Quellen finden Sie weitere Informationen zum Angebot:

- In unserem [Azure FarmBeats-Blog](https://aka.ms/farmbeatsblog) bleiben Sie mit den neuesten Nachrichten zu Azure FarmBeats auf dem Laufenden.
- Erhalten Sie Hilfe, indem Sie in unserem [Azure FarmBeats-Supportforum](https://aka.ms/farmbeatssupport) eine Frage stellen.
- Geben Sie uns Feedback, indem Sie in unserem [Azure FarmBeats-Feedbackforum](https://aka.ms/farmbeatsfeedback) ein neues Feature vorschlagen oder für eine Idee abstimmen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Installieren von Azure FarmBeats](install-azure-farmbeats.md)
