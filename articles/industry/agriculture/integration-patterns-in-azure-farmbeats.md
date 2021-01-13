---
title: Architektur von Azure FarmBeats
description: Beschreibt die Architektur von Azure FarmBeats.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: d64c2175072d9979cfda2ea5f75beb34d3ad0d6b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "75475720"
---
# <a name="integration-patterns"></a>Integrationsmuster

Azure FarmBeats ist ein Business-to-Business-Angebot, das in Azure Marketplace verfügbar ist. Mithilfe von FarmBeats lassen sich Datasets mit landwirtschaftlichen Daten anbieterübergreifend aggregieren und verwertbare Erkenntnisse gewinnen. Dazu werden die Datasets zusammengeführt und KI (Künstliche Intelligenz)- oder ML (Machine Learning)-Modelle generiert.

![FarmBeats-Projekt](./media/architecture-for-farmbeats/farmbeats-architecture-1.png)

In den folgenden Abschnitten wird das Integrationsmuster für Azure FarmBeats beschrieben.

## <a name="why-integrate-with-azure-farmbeats"></a>Was spricht für die Integration von Azure FarmBeats?

Dieser Abschnitt richtet sich an Partner, die Ihre Datensysteme (wie Sensoren, Drohnen, Wetterstationen) in Azure FarmBeats integrieren möchten.

Azure FarmBeats ist ein erweiterbares Angebot, das landwirtschaftlichen Betrieben die Möglichkeit bietet, verschiedene historische und in Echtzeit generierte landwirtschaftliche Daten in einer einzigen Plattform zusammenzuführen. Azure FarmBeats unterstützt landwirtschaftliche Unternehmen dabei, Daten im Rahmen eines landwirtschaftlichen Betriebs zu normalisieren, zu aggregieren und in einen Kontext zu setzen.

Durch eine Datenpartnerschaft mit Azure FarmBeats haben Sie die Möglichkeit, Ihre Datenlösungen einem größeren Kundenkreis anzubieten und eine breitere Einführung Ihrer Systeme zu erzielen. Azure FarmBeats bietet eine erweiterbare API-Schicht, die als Datenhub bezeichnet wird und Sie dabei unterstützt, die von Ihren Geräten aufgezeichneten Daten systematisch in einem standardisierten Schema zu erfassen.

Sobald die Daten innerhalb der Azure FarmBeats-Instanz Ihrer Kunden verfügbar sind, können diese auf der Grundlage Ihrer Daten fundiertere Analysen erstellen und effizientere Tools entwickeln.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Integration von Sensordaten finden Sie unter [Integration von Sensordaten](sensor-partner-integration-in-azure-farmbeats.md) und zur Integration von Partnern für Bilddaten unter [Partnerintegration für Bilder](imagery-partner-integration-in-azure-farmbeats.md).
