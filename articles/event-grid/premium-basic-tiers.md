---
title: Premium- und Basic-Tarif für Azure Event Grid
description: In diesem Artikel erfahren Sie, inwiefern sich der Premium-Tarif vom Basic-Tarif für Azure Event Grid unterscheidet und wann welcher Tarif verwendet werden sollte.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 03/11/2020
ms.author: babanisa
ms.openlocfilehash: 7ae73337ccccc2b7d2402cfa2a54ae93552d2465
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2020
ms.locfileid: "79300334"
---
# <a name="azure-event-grid-premium-and-basic-tiers"></a>Premium- und Basic-Tarife für Azure Event Grid
Für Azure Event Grid stehen zwei Tarife zur Verfügung: **Premium** und **Basic**. Der Basic-Tarif basiert auf Verbrauch bzw. auf der nutzungsbasierten Bezahlung. Sie erhalten alle grundlegenden Tools zum Veröffentlichen/Abonnieren, die Sie benötigen, um Event Grid für ereignisgesteuerte Programmiermodelle verwenden zu können. Der Premium-Tarif geht noch einen Schritt weiter und beinhaltet Sicherheitsfeatures für Unternehmen. Er befindet sich in einer frühen **Vorschauphase**, und viele der Features sind noch in der Entwicklung.

## <a name="overview"></a>Übersicht
Alle benutzerdefinierten Themen und Domänen in Event Grid gehören entweder zum Basic- oder zum Premium-Tarif. Ab der API-Version `2020-04-01-preview` kann der gewünschte Tarif bei der Thema- oder Domänenerstellung ausgewählt werden. Standardwert ist der Basic-Tarif. Bei mit älteren API-Versionen erstellten Themen und Domänen wird standardmäßig der Basic-Tarif verwendet. Informationen zum Ändern des Tarifs für Ihre Themen und Domänen finden Sie unter [Aktualisieren des Tarifs](update-tier.md).

## <a name="capabilities-and-features"></a>Funktionen und Features

In der folgenden Tabelle sind die Unterschiede zwischen den Tarifen aufgeführt:

|       &nbsp;                                           | Basic           | Premium        |
| ------------------------------------------------------ | --------------- | -------------- |
| IP-Firewallregeln für eingehenden Datenverkehr                          | Vorschau  | Vorschau |
| Diensttags für ausgehenden Datenverkehr                                | Vorschau  | Vorschau |
| Privater Endpunkt: VNET-Integration für eingehenden Datenverkehr          | Nicht verfügbar   | Vorschau |

## <a name="availability"></a>Verfügbarkeit
Während der anfänglichen Vorschauphase sind Themen und Domänen mit privater Endpunktintegration in folgenden Regionen verfügbar:

- USA, Osten
- USA, Westen 2
- USA, Süden-Mitte

## <a name="pricing-and-quotas"></a>Preise und Kontingente
Preisdetails für den Basic-Tarif finden Sie unter [Event Grid – Preise](https://azure.microsoft.com/pricing/details/event-grid/). Die Preise für den Premium-Tarif wurden noch nicht bekannt gegeben, und der Tarif kann bis zur Bekanntgabe der Preise kostenlos genutzt werden.

Die vorhandenen Kontingente für die Themen- und Domänenanzahl sowie für den Durchsatz gelten bis zur Bekanntgabe der Preise des Premium-Tarifs sowohl für Ressourcen im Basic-Tarif als auch für Ressourcen im Premium-Tarif.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in folgenden Artikeln:

- Informationen zum Upgraden vom Basic- auf den Premium-Tarif finden Sie im Artikel [Aktualisieren des Tarifs](update-tier.md). 
- Sie können die IP-Firewall für Ihre Event Grid-Ressource so konfigurieren, dass der Zugriff über das öffentliche Internet auf eine bestimmte Gruppe von IP-Adressen oder IP-Adressbereichen beschränkt wird. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Konfigurieren der IP-Firewall für Azure Event Grid-Themen oder -Domänen (Vorschau)](configure-firewall.md).
- Sie können private Endpunkte konfigurieren, um den Zugriff auf ausgewählte virtuelle Netzwerke zu beschränken. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Konfigurieren privater Endpunkte für Azure Event Grid-Themen oder -Domänen (Vorschau)](configure-private-endpoints.md).