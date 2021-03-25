---
title: Konfigurieren von Vergleichsbedingungen für Azure Front Door Standard/Premium-Regelsätze
description: Dieser Artikel enthält eine Liste der verschiedenen Vergleichsbedingungen, die für einen Azure Front Door Standard/Premium-Regelsatz verfügbar sind.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 4c65d0e7f80fab59ca7df4849df7117d482352c1
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101098024"
---
# <a name="azure-front-door-standardpremium-preview-rule-set-match-conditions"></a>Vergleichsbedingungen für Regelsätze in Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

In diesem Tutorial wird gezeigt, wie Sie mit der ersten Gruppe von Regeln im Azure-Portal einen Regelsatz erstellen. Eine Regel in einem Azure Front Door Standard/Premium-[Regelsatz](concept-rule-set.md) besteht aus 0 oder mehr Vergleichsbedingungen und einer Aktion. In diesem Artikel werden die Vergleichsbedingungen, die Sie in einem Azure Front Door Standard/Premium-Regelsatz verwenden können, ausführlich beschrieben.

Der erste Teil einer Regel besteht aus einer Übereinstimmungsbedingung oder aus mehreren Übereinstimmungsbedingungen. Eine Regel kann aus bis zu 10 Übereinstimmungsbedingungen bestehen. Eine Übereinstimmungsbedingung gibt bestimmte Typen von Anforderungen an, für die definierte Aktionen ausgeführt werden. Wenn Sie mehrere Übereinstimmungsbedingungen verwenden, werden die Übereinstimmungsbedingungen mit UND-Logik gruppiert. Für alle Übereinstimmungsbedingungen, die mehrere Werte unterstützen (als „durch Leerzeichen getrennt“ angegeben), wird der Operator „ODER“ angenommen.

Sie können eine Übereinstimmungsbedingung z.B. für Folgendes verwenden:

* Filtern von Anforderungen, die auf einer bestimmten IP-Adresse, einem bestimmten Land oder einer bestimmten Region basieren.
* Filtern von Anforderungen nach Headerinformationen
* Filtern von Anforderungen von Mobil- oder Desktopgeräten.
* Filtern von Anforderungen nach Anforderungsdateiname und Anforderungsdateierweiterung
* Filtern von Anforderungen nach Anforderungs-URL, Protokoll, Pfad, Abfragezeichenfolge, POST-Argumenten usw.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

In einem Azure Front Door Standard/Premium-Regelsatz können die folgenden Vergleichsbedingungen verwendet werden:

## <a name="device-type"></a>Gerätetyp

Identifiziert Anforderungen von einem Mobil- oder Desktopgerät.  

#### <a name="required-fields"></a>Pflichtfelder

Operator | Unterstützte Werte
---------|----------------
Gleich, Ungleich | Mobil, Desktop

## <a name="post-argument"></a>POST-Argument

Identifiziert Anforderungen auf der Grundlage von Argumenten, die für die POST-Anforderungsmethode definiert sind, die in der Anforderung verwendet wird.

#### <a name="required-fields"></a>Pflichtfelder

Argumentname | Operator | Argumentwert | Umwandlung der Groß-/Kleinschreibung
--------------|----------|----------------|---------------
String | [Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="query-string"></a>Abfragezeichenfolge

Identifiziert Anforderungen, die einen bestimmten Abfragezeichenfolgenparameter enthalten. Dieser Parameter wird auf einen Wert festgelegt, der mit einem bestimmten Muster übereinstimmt. Abfragezeichenfolgenparameter (z.B. **parameter=value**) in der Anforderungs-URL bestimmen, ob diese Bedingung erfüllt ist. Mit dieser Übereinstimmungsbedingung wird ein Abfragezeichenfolgenparameter anhand des Namens identifiziert, und für den Parameterwert werden ein oder mehrere Werte akzeptiert.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Abfragezeichenfolge | Umwandlung der Groß-/Kleinschreibung
---------|--------------|---------------
[Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="remote-address"></a>Remoteadresse

Identifiziert Anforderungen basierend auf dem Standort oder der IP-Adresse des Anforderers.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Unterstützte Werte
---------|-----------------
Geografische Übereinstimmung | Landesvorwahl
IP-Übereinstimmung | IP-Adresse (durch Leerzeichen getrennt)
Keine geografische Übereinstimmung | Landesvorwahl
Keine IP-Übereinstimmung | IP-Adresse (durch Leerzeichen getrennt)

#### <a name="key-information"></a>Wichtige Informationen

* Verwenden Sie die CIDR-Notation.
* Bei mehreren IP-Adressen und IP-Adressblöcken wird der logische OR-Operator verwendet.
    * **IPv4-Beispiel**: Wenn Sie die beiden IP-Adressen *1.2.3.4* und *10.20.30.40* hinzufügen, ist die Bedingung erfüllt, wenn Anforderungen von der Adresse 1.2.3.4 oder 10.20.30.40 eintreffen.
    * **IPv6-Beispiel**: Wenn Sie die beiden IP-Adressen *1:2:3:4:5:6:7:8* und *10:20:30:40:50:60:70:80* hinzufügen, ist die Bedingung erfüllt, wenn Anforderungen von der Adresse 1:2:3:4:5:6:7:8 oder 10:20:30:40:50:60:70:80 eintreffen.
* Die Syntax für einen IP-Adressblock besteht aus der IP-Basisadresse, gefolgt von einem Schrägstrich und der Präfixgröße. Beispiel:
    * **IPv4-Beispiel:** *5.5.5.64/26* entspricht allen Anforderungen, die von den Adressen 5.5.5.64 bis 5.5.5.127 eingehen.
    * **IPv6-Beispiel:** Die Angabe *1:2:3:/48* führt zur Übereinstimmung mit eingehenden Anforderungen von den Adressen 1:2:3:0:0:0:0:0 bis 1:2:3: ffff:ffff:ffff:ffff:ffff.

## <a name="request-body"></a>Anforderungstext

Identifiziert Anforderungen auf der Grundlage eines bestimmten Texts, der im Textkörper der Anforderung vorhanden ist.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Anforderungstext | Umwandlung der Groß-/Kleinschreibung
---------|--------------|---------------
[Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="request-header"></a>Anforderungsheader

Identifiziert Anforderungen, die einen bestimmten Header in der Anforderung verwenden.

#### <a name="required-fields"></a>Pflichtfelder

Headername | Operator | Headerwert | Umwandlung der Groß-/Kleinschreibung
------------|----------|--------------|---------------
String | [Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="request-method"></a>Anforderungsmethode

Identifiziert Anforderungen, die die angegebene Anforderungsmethode verwenden.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Unterstützte Werte
---------|----------------
Gleich, Ungleich | GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE

#### <a name="key-information"></a>Wichtige Informationen

Nur die GET-Anforderungsmethode kann zwischengespeicherten Inhalt in Azure Front Door generieren. Alle anderen Anforderungsmethoden werden per Proxy durch das Netzwerk gesendet.

## <a name="request-protocol"></a>Anforderungsprotokoll

Identifiziert Anforderungen, die das angegebene Protokoll verwenden.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Unterstützte Werte
---------|----------------
Gleich, Ungleich | HTTP, HTTPS

## <a name="request-url"></a>Anfrage-URL

Identifiziert Anforderungen, die mit der angegebenen URL übereinstimmen.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Anfrage-URL | Umwandlung der Groß-/Kleinschreibung
---------|-------------|---------------
[Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

#### <a name="key-information"></a>Wichtige Informationen

Wenn Sie diese Regelbedingung verwenden, achten Sie darauf, dass Sie Protokollinformationen einschließen. Beispiel: *https://www.\<yourdomain\>.com*.

## <a name="request-file-extension"></a>Dateierweiterung der Anforderung

Identifiziert Anforderungen, die die angegebene Dateierweiterung in den Dateinamen in der anfordernden URL einschließen.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Durchwahl | Umwandlung der Groß-/Kleinschreibung
---------|-----------|---------------
[Operatorliste](#operator-list)  | String, Int | Kleinbuchstaben, Großbuchstaben

#### <a name="key-information"></a>Wichtige Informationen

Lassen Sie bei der Erweiterung den vorangestellten Punkt weg. Verwenden Sie also *html* anstelle von *.html*.

## <a name="request-file-name"></a>Dateiname der Anforderung

Identifiziert Anforderungen, die den angegebenen Dateinamen in der anfordernden URL einschließen.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Dateiname | Umwandlung der Groß-/Kleinschreibung
---------|-----------|---------------
[Operatorliste](#operator-list)| String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="request-path"></a>Anforderungspfad

Identifiziert Anforderungen, die den angegebenen Pfad in der anfordernden URL einschließen.

#### <a name="required-fields"></a>Pflichtfelder

Operator | Wert | Umwandlung der Groß-/Kleinschreibung
---------|-------|---------------
[Operatorliste](#operator-list) | String, Int | Kleinbuchstaben, Großbuchstaben

## <a name="operator-list"></a><a name = "operator-list"></a>Operatorliste

Für Regeln, die Werte aus der Liste der Standardoperatoren akzeptieren, sind die folgenden Operatoren gültig:

* Any
* Equals
* Enthält
* Beginnt mit
* Endet mit
* Kleiner als
* Kleiner als oder gleich
* Größer als
* Größer als oder gleich
* Keine
* Enthält nicht
* Beginnt nicht mit
* Endet nicht mit
* Nicht kleiner als
* Nicht kleiner als oder gleich
* Nicht größer als
* Nicht größer als oder gleich
* Regular Expression

Für numerische Operatoren wie *Kleiner als* oder *Größer als oder gleich* basiert der verwendete Vergleich auf der Länge. Der Wert in der Übereinstimmungsbedingung muss eine ganze Zahl sein, die der Länge entspricht, die Sie vergleichen möchten.

## <a name="regular-expression"></a>Regular Expression

Die folgenden Operationen werden in regulären Ausdrücken nicht unterstützt:

* Rückverweise und Erfassung von Teilausdrücken
* Willkürliche Assertionen mit einer Breite von 0
* Unterroutinenverweise und rekursive Muster
* Bedingte Muster
* Rückverfolgung von Steuerelementverben
* Die Einzelbyte-Anweisung – „\C“
* Die Anweisung für Zeilenvorschubübereinstimmung – „\R“
* Die Startanweisung zum Zurücksetzen der Übereinstimmung – „\K“
* Callouts und eingebetteter Code
* Atomische Gruppierung und besitzanzeigende Quantifizierer

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [Regelsätze](concept-rule-set.md).
* Erfahren Sie, wie Sie [Ihren ersten Regelsatz konfigurieren](how-to-configure-rule-set.md).
* Weitere Informationen über [Regelsatzaktionen](concept-rule-set-actions.md)
