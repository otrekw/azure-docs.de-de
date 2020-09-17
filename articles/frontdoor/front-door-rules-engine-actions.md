---
title: Aktionen des Azure Front Door-Regelmoduls
description: Dieser Artikel enthält eine Liste der verschiedenen Aktionen, die Sie mit dem Azure Front Door-Regelmodul durchführen können.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: e1893c32ed486772e56432f6263626d0ee1a65df
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531881"
---
# <a name="azure-front-door-rules-engine-actions"></a>Aktionen des Azure Front Door-Regelmoduls

Im [AFD-Regelmodul](front-door-rules-engine.md) besteht eine Regel aus null oder mehr Übereinstimmungsbedingungen und Aktionen. Dieser Artikel enthält detaillierte Beschreibungen der Aktionen, die Sie im AFD-Regelmodul verwenden können.

Eine Aktion definiert das Verhalten, das auf den Anforderungstyp angewendet wird, der durch mindestens eine Übereinstimmungsbedingung bestimmt wurde. Im AFD-Regelmodul kann eine Regel bis zu fünf Aktionen enthalten, aber nur eine davon darf eine Außerkraftsetzungsaktion (Weiter- oder Umleitung) für die Routenkonfiguration sein.

Die folgenden Aktionen können auch im Azure Front Door-Regelmodul verwendet werden.  

## <a name="modify-request-header"></a>Anforderungsheader ändern

Verwenden Sie diese Aktion, um Header zu ändern, die in den an ihren Ursprung gesendeten Anforderungen vorhanden sind.

### <a name="required-fields"></a>Pflichtfelder

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im **Headernamen** angegebene Header mit dem angegebenen Wert der Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, wird der Wert an den vorhandenen Wert angefügt. | String
Overwrite | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im **Headernamen** angegebene Header mit dem angegebenen Wert der Anforderung hinzugefügt. Wenn der Header bereits vorhanden ist, wird der vorhandene Wert von dem angegebenen Wert überschrieben. | String
Löschen | Wenn diese Option ausgewählt ist, die Regel übereinstimmt und der in der Regel angegebene Header vorhanden ist, wird der Header aus der Anforderung gelöscht. | String

## <a name="modify-response-header"></a>Antwortheader ändern

Verwenden Sie diese Aktion, um Header zu ändern, die in den an Ihre Clients zurückgegebenen Antworten vorhanden sind.

### <a name="required-fields"></a>Pflichtfelder

Aktion | HTTP-Headername | Wert
-------|------------------|------
Anfügen | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im **Headernamen** angegebene Header mit dem angegebenen **Wert** der Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, wird der **Wert** an den vorhandenen Wert angefügt. | String
Overwrite | Wenn diese Option ausgewählt ist und die Regel übereinstimmt, wird der im **Headernamen** angegebene Header mit dem angegebenen **Wert** der Antwort hinzugefügt. Wenn der Header bereits vorhanden ist, überschreibt der **Wert** den vorhandenen Wert. | String
Löschen | Wenn diese Option ausgewählt ist, die Regel übereinstimmt und der in der Regel angegebene Header vorhanden ist, wird der Header aus der Antwort gelöscht. | String

## <a name="route-configuration-overrides"></a>Außerkraftsetzung der Routenkonfiguration 

### <a name="route-type-redirect"></a>Routentyp: Umleiten

Leiten Sie mit dieser Aktion Clients an eine neue URL um. 

#### <a name="required-fields"></a>Pflichtfelder

Feld | BESCHREIBUNG 
------|------------
Umleitungstyp | Wählen Sie den Antworttyp aus, der an den Anforderer zurückgegeben werden soll: „Gefunden“ (302), „Verschoben“ (301), „Temporäre Umleitung“ (307) und „Permanente Umleitung“ (308).
Umleitungsprotokoll | Übereinstimmungsanforderung, HTTP, HTTPS.
Zielhost | Wählen Sie den Hostnamen aus, an den die Anforderung umgeleitet werden soll. Lassen Sie das Feld leer, um den eingehenden Host beizubehalten.
Zielpfad | Definieren Sie den bei der Umleitung zu verwendenden Pfad. Lassen Sie das Feld leer, um den eingehenden Pfad beizubehalten.  
Abfragezeichenfolge | Definieren Sie die bei der Umleitung zu verwendende Abfragezeichenfolge. Lassen Sie das Feld leer, um die eingehende Abfragezeichenfolge beizubehalten. 
Zielfragment | Definieren Sie das bei der Umleitung zu verwendende Fragment. Lassen Sie das Feld leer, um das eingehende Fragment beizubehalten. 


### <a name="route-type-forward"></a>Routentyp: Weiter

Leiten Sie mit dieser Aktion Clients an eine neue URL weiter. Diese Aktion enthält auch unter Unteraktionen für URL-Umschreibungen und Zwischenspeichern. 

Feld | BESCHREIBUNG 
------|------------
Back-End-Pool | Wählen Sie den Back-End-Pool für die Außerkraftsetzung aus, von dem die Anforderungen verarbeitet werden sollen. Dadurch werden alle vorkonfigurierten Back-End-Pools angezeigt, die sich derzeit in Ihrem Front Door-Profil befinden. 
Weiterleitungsprotokoll | Übereinstimmungsanforderung, HTTP, HTTPS.
URL Rewrite | Verwenden Sie diese Aktion, um den Pfad einer Anforderung auf dem Weg zu Ihrem Ursprung erneut zu generieren. Wenn diese Option aktiviert ist, finden Sie unten weitere erforderliche Felder.
Caching | „Aktiviert“, „Deaktiviert“ Wenn diese Option aktiviert ist, finden Sie unten weitere erforderliche Felder. 

#### <a name="url-rewrite"></a>URL Rewrite

Mit dieser Einstellung können Sie einen optionalen **benutzerdefinierten Weiterleitungspfad** konfigurieren, der beim Erstellen der Anforderung zur Weiterleitung an das Back-End verwendet werden soll.

Feld | BESCHREIBUNG 
------|------------
Benutzerdefinierter Weiterleitungspfad | Definieren Sie den Pfad, an den die Anforderungen weitergeleitet werden sollen. 

#### <a name="caching"></a>Caching

Verwenden Sie diese Einstellungen, um zu steuern, wie Dateien für Anforderungen zwischengespeichert werden, die Abfragezeichenfolgen enthalten, und ob der Inhalt basierend auf allen Parameter oder ausgewählten Parametern zwischengespeichert werden soll. Sie können zusätzliche Einstellungen verwenden, um den Wert für die Gültigkeitsdauer (Time to Live, TTL) zu überschreiben, um zu steuern, wie lange Inhalte im Cache für Anforderungen bleiben, die von den Bedingungen der Regelübereinstimmung angegeben sind. Um das Zwischenspeichern als Aktion zu erzwingen, legen Sie das Feld „Zwischenspeichern“ auf „Aktiviert“ fest. Dabei werden die folgenden Optionen angezeigt: 

Cacheverhalten |  BESCHREIBUNG              
---------------|----------------
Ignorieren von Abfragezeichenfolgen | Sobald das Objekt zwischengespeichert ist, ignorieren alle nachfolgenden Anforderungen die Abfragezeichenfolgen, bis das zwischengespeicherte Objekt abläuft.
Zwischenspeichern jeder eindeutigen URL | Jede Anforderung wird mit einer eindeutigen URL, einschließlich der Abfragezeichenfolge, als eindeutiges Objekt mit eigenem Cache behandelt.
Angegebene Abfragezeichenfolgen ignorieren | Abfragezeichenfolgen von Anforderungs-URLs, die unter der Einstellung „Abfrageparameter“ aufgeführt sind, werden für die Zwischenspeicherung ignoriert.
Angegebene Abfragezeichenfolgen einschließen | Abfragezeichenfolgen von Anforderungs-URLs, die unter der Einstellung „Abfrageparameter“ aufgeführt sind, werden für die Zwischenspeicherung verwendet.

Zusätzliche Felder |  BESCHREIBUNG 
------------------|---------------
Dynamische Komprimierung | Azure Front Door Service komprimiert Inhalte dynamisch im Edgebereich, wodurch die Antwort kleiner ist und schneller erfolgt.
Abfrageparameter | Eine durch Trennzeichen getrennte Liste zulässiger (oder unzulässiger) Parameter, die als Basis für die Zwischenspeicherung verwendet werden sollen.
Cachedauer | Dauer bis Cacheablauf in Tagen, Stunden, Minuten, Sekunden. Alle Werte müssen ganze Zahlen sein. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie Ihre erste [Regelmodulkonfiguration](front-door-tutorial-rules-engine.md) einrichten. 
- Weitere Informationen zu [Übereinstimmungsbedingungen des Regelmoduls](front-door-rules-engine-match-conditions.md)
- Weitere Informationen zum [Azure Front Door-Regelmodul](front-door-rules-engine.md)
