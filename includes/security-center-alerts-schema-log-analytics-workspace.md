---
title: include file
description: include file
services: data-factory
author: memildin
ms.service: data-factory
ms.topic: include
ms.date: 03/17/2020
ms.author: memildin
ms.custom: include file
ms.openlocfilehash: 7d81799f7fbdb2b41db421daa1a85ec8cde511eb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "79541386"
---
### <a name="the-data-model-of-the-schema"></a>Das Datenmodell des Schemas

|Feld|BESCHREIBUNG|
|----|----|
|**AlertName**|Anzeigename der Warnung|
|**AlertType**|Eindeutiger Warnungsbezeichner|
|**ConfidenceLevel**|(Optional) Konfidenzniveau dieser Warnung (hoch/niedrig)|
|**ConfidenceScore**|(Optional) Numerischer Konfidenzindikator der Sicherheitswarnung|
|**Beschreibung**|Beschreibungstext für die Warnung|
|**DisplayName**|Anzeigename der Warnung|
|**EndTime**|Der Endzeitpunkt der Auswirkung der Warnung (der Zeitpunkt des letzten Ereignisses, das zur Warnung beiträgt)|
|**Entitäten**|Eine Liste der Entitäten im Zusammenhang mit der Warnung. Diese Liste kann eine Mischung aus Entitäten unterschiedlicher Typen enthalten.|
|**ExtendedLinks**|(Optional) Eine Behälter für alle Links im Zusammenhang mit der Warnung. Dieser Behälter kann eine Mischung aus Links für verschiedene Typen enthalten.|
|**ExtendedProperties**|Ein Behälter zusätzlicher Felder, die für die Warnung relevant sind|
|**IsIncident**|Bestimmt, ob es sich bei der Warnung um einen Incident oder eine reguläre Warnung handelt. Ein Incident ist eine Sicherheitswarnung, die mehrere Warnungen in einem Sicherheitsincident aggregiert.|
|**ProcessingEndTime**|UTC-Zeitstempel der Erstellung der Warnung|
|**ProductComponentName**|(Optional) Der Name einer Komponente innerhalb des Produkts, das die Warnung generiert hat.|
|**ProductName**|Konstante („Azure Security Center“)|
|**ProviderName**|nicht verwendet|
|**RemediationSteps**|Manuelle Aktionselemente zum Beheben der Sicherheitsbedrohung|
|**ResourceId**|Vollständige Bezeichner der betroffenen Ressource|
|**Severity**|Der Schweregrad der Warnung (hoch/mittel/niedrig/Information)|
|**SourceComputerId**|eine eindeutige GUID für den betroffenen Server (wenn die Warnung auf dem Server generiert wird)|
|**SourceSystem**|nicht verwendet|
|**StartTime**|Der Startzeitpunkt der Auswirkung der Warnung (der Zeitpunkt des ersten Ereignisses, das zur Warnung beiträgt)|
|**SystemAlertId**|Eindeutiger Bezeichner dieser Instanz der Sicherheitswarnung|
|**TenantId**|der Bezeichner des übergeordneten Azure Active Directory-Mandanten des Abonnements, unter dem sich die überprüfte Ressource befindet|
|**TimeGenerated**|UTC-Zeitstempel der Bewertung (Security Center-Überprüfungszeit) (identisch mit „DiscoveredTimeUTC“)|
|**Typ**|Konstante („SecurityAlert“)|
|**VendorName**|Der Name des Anbieters, der die Warnung ausgelöst hat (z. B. „Microsoft“)|
|**VendorOriginalId**|nicht verwendet|
|**WorkspaceResourceGroup**|wenn die Warnung auf einem virtuellen Computer, einem Server, einer VM-Skalierungsgruppe oder einer App Service-Instanz generiert wird, die Meldungen an einen Arbeitsbereich sendet: Name dieser Ressourcengruppe des Arbeitsbereichs|
|**WorkspaceSubscriptionId**|wenn die Warnung auf einem virtuellen Computer, einem Server, einer VM-Skalierungsgruppe oder einer App Service-Instanz generiert wird, die Meldungen an einen Arbeitsbereich sendet: Abonnement-ID des Arbeitsbereichs|
|||
