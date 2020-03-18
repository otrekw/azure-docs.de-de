---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/10/2020
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 427117fe47294a1db1fa8d3fa1e46ee1efb91b4d
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128621"
---
## <a name="limitations"></a>Einschränkungen

- VM-Skalierungsgruppen werden zurzeit auf dedizierten Hosts nicht unterstützt.
- Die verfügbaren Größen und Hardwaretypen für dedizierte Hosts variieren je nach Region. Weitere Informationen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

## <a name="create-a-host-group"></a>Erstellen einer Hostgruppe

Eine **Hostgruppe** ist eine neue Ressource, die eine Sammlung dedizierter Hosts darstellt. Sie erstellen eine Hostgruppe in einer Region und einer Verfügbarkeitszone und fügen ihr Hosts hinzu. Bei der Planung für Hochverfügbarkeit stehen zusätzliche Optionen zur Verfügung. Sie können eine oder beide der folgenden Optionen mit Ihren dedizierten Hosts verwenden: 
- Ausdehnen über mehrere Verfügbarkeitszonen. In diesem Fall muss in jeder Zone, die Sie verwenden möchten, eine Hostgruppe vorhanden sein.
- Ausdehnen über mehrere Fehlerdomänen, die physischen Racks zugeordnet sind. 
 
In beiden Fällen müssen Sie die Anzahl der Fehlerdomänen für Ihre Hostgruppe angeben. Wenn Ihre Gruppe keine Fehlerdomänen umfassen soll, verwenden Sie die Anzahl 1 von Fehlerdomänen. 

Sie können auch sowohl Verfügbarkeitszonen als auch Fehlerdomänen verwenden. 

In diesem Beispiel erstellen Sie eine Hostgruppe mit einer Verfügbarkeitszone und zwei Fehlerdomänen. 


1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
1. Wählen Sie links oben **Ressource erstellen** aus.
1. Suchen Sie nach **Hostgruppe**, und wählen Sie dann in den Ergebnissen **Hostgruppen** aus.
1. Wählen Sie auf der Seite **Hostgruppen** die Option **Erstellen** aus.
1. Wählen Sie das Abonnement aus, das Sie verwenden möchten, und wählen Sie dann **Neu erstellen** aus, um eine neue Ressourcengruppe zu erstellen.
1. Geben Sie *myDedicatedHostsRG* unter **Name** ein, und wählen Sie dann **OK** aus.
1. Geben Sie unter **Host group name** (Name der Hostgruppe) die Zeichenfolge *myHostGroup* ein.
1. Wählen Sie unter **Standort** die Option **USA, Osten** aus.
1. Wählen Sie unter **Verfügbarkeitszone** die Option **1** aus.
1. Wählen Sie für **Fault domain count** (Anzahl von Fehlerdomänen) die Option **2** aus.
1. Wählen Sie **Bewerten + erstellen** aus, und warten Sie auf die Überprüfung.
1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus, um die Hostgruppe zu erstellen.

Die Erstellung der Hostgruppe sollte nur wenige Sekunden dauern.

## <a name="create-a-dedicated-host"></a>Erstellen eines dedizierten Hosts

Erstellen Sie nun einen dedizierten Host in der Hostgruppe. Zusätzlich zu einem Namen müssen Sie die SKU für den Host angeben. Die Host-SKU erfasst die unterstützte VM-Serie sowie die Hardwaregenerierung für Ihren dedizierten Host.

Weitere Informationen zu den Host-SKUs und Preisen finden Sie unter [Azure Dedicated Host – Preise](https://aka.ms/ADHPricing).

Beim Festlegen der Anzahl der Fehlerdomänen für die Hostgruppe werden Sie aufgefordert, die Fehlerdomäne für den Host anzugeben.  

1. Wählen Sie links oben **Ressource erstellen** aus.
1. Suchen Sie nach **Dedizierter Host**, und wählen Sie dann in den Ergebnissen **Dedizierter Host** aus.
1. Wählen Sie auf der Seite **Dedizierter Host** die Option **Erstellen** aus.
1. Wählen Sie das Abonnement aus, das Sie verwenden möchten.
1. Wählen Sie *myDedicatedHostsRG* unter **Ressourcengruppe** aus.
1. Geben Sie unter **Instanzdetails** die Zeichenfolge *myHost* für **Name** ein, und wählen Sie als Standort *USA, Osten* aus.
1. Wählen Sie in **Hardwareprofil** die Option *Standard Es3 family – Type 1* (Standard-Es3-Familie – Typ 1) unter **Größenfamilie**, *myHostGroup* unter **Hostgruppe** und dann *1* unter **Fehlerdomäne** aus. Behalten Sie in den restlichen Feldern die Standardwerte bei.
1. Wählen Sie nach Abschluss **Bewerten + erstellen** aus, und warten Sie auf die Validierung.
1. Wenn die Meldung **Überprüfung erfolgreich** angezeigt wird, wählen Sie **Erstellen** aus, um den Host zu erstellen.


