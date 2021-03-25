---
title: Was ist eine Route für Azure Front Door Standard/Premium?
description: Dieser Artikel bietet grundlegende Informationen dazu, wie Azure Front Door Standard/Premium entscheidet, welche Routingregel für eine eingehende Anforderung verwendet werden soll.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: db026c4903aa30a0a4c8154af8ad6eeb4b72b706
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "101098028"
---
# <a name="what-is-azure-front-door-standardpremium-preview-route"></a>Was ist eine Route für Azure Front Door Standard/Premium (Vorschau)?

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

Eine Azure Front Door Standard/Premium-Route definiert, wie der Datenverkehr verarbeitet wird, wenn die eingehende Anforderung in der Azure Front Door-Umgebung eintrifft. Über die Routeneinstellungen wird eine Zuordnung zwischen einer Domäne und einer Back-End-Ursprungsgruppe definiert. Durch Aktivieren der erweiterten Features, z. B. „Muster für Abgleich“ oder „Regelsatz“, lässt sich der Datenverkehr genauer steuern.

Die Front Door Standard/Premium-Routingkonfiguration besteht aus zwei Hauptteilen: „linke Seite“ und „rechte Seite“. Die eingehende Anforderung wird mit der linken Seite der Route abgeglichen, und die rechte Seite definiert, wie die Anforderung verarbeitet wird.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

### <a name="incoming-match-left-hand-side"></a>Abgleich der eingehenden Anforderung (linke Seite)

Die folgenden Eigenschaften bestimmen, ob die eingehende Anforderung mit der Routingregel (also der linken Seite) übereinstimmt:

* **HTTP-Protokolle** (HTTP/HTTPS)
* **Hosts** (z.B. www\.foo.com, \*.bar.com)
* **Pfade** (z.B.: /\*, /users/\*, /file.gif)

Diese Eigenschaften werden intern erweitert, sodass jede Kombination aus Protokoll/Host/Pfad ein potenzieller Treffer ist.

### <a name="route-data-right-hand-side"></a>Weiterleiten der Daten (rechte Seite)

Die Entscheidung darüber, wie die Anforderung verarbeitet werden soll, richtet sich danach, ob für die Route Zwischenspeichern aktiviert ist. Wenn keine zwischengespeicherte Antwort verfügbar ist, wird die Anforderung an das entsprechende Back-End weitergeleitet.

## <a name="route-matching"></a>Routenabgleich

In diesem Abschnitt wird erläutert, wie der Abgleich mit einer bestimmten Front Door-Routingregel funktioniert. Das grundlegende Konzept besteht darin, immer den **ersten genauesten Treffer** zu verwenden, wobei nur die „linke Seite“ in Betracht gezogen wird.  Der Abgleich erfolgt zunächst basierend auf dem HTTP-Protokoll, dann basierend auf dem Front-End-Host, dann basierend auf dem Pfad.

### <a name="frontend-host-matching"></a>Abgleich mit Front-End-Hosts

Beim Abgleich mit Front-End-Hosts wird die unten definierte Logik verwendet:

1. Es wird nach allen Routingregeln mit einem exakten Treffer für den Host gesucht.
2. Wenn kein exakter Treffer für einen Front-End-Host gefunden wird, wird die Anforderung abgelehnt und der Fehler „400 – Ungültige Anforderung“ gesendet.

Um diesen Prozess genauer zu erklären, betrachten wir eine Beispielkonfiguration von Front Door-Routen (nur linke Seite):

| Routingregel | Front-End-Hosts | `Path` |
|-------|--------------------|-------|
| Ein | foo.contoso.com | /\* |
| B | foo.contoso.com | /users/\* |
| C | www\.fabrikam.com, foo.adventure-works.com  | /\*, /images/\* |

Wenn die folgenden eingehenden Anforderungen an Front Door gesendet würden, würden sie mit den folgenden Routingregeln von oben abgeglichen:

| Front-End-Host für eingehende Anforderungen | Abgeglichene Routingregel(n) |
|---------------------|---------------|
| foo.contoso.com | A, B |
| www\.fabrikam.com | C |
| images.fabrikam.com | Fehler 400: Ungültige Anforderung |
| foo.adventure-works.com | C |
| contoso.com | Fehler 400: Ungültige Anforderung |
| www\.adventure-works.com | Fehler 400: Ungültige Anforderung |
| www\.northwindtraders.com | Fehler 400: Ungültige Anforderung |

### <a name="path-matching"></a>Pfadabgleich

Azure Front Door Standard/Premium ermittelt den Front-End-Host und filtert die Routingregeln, um sicherzustellen, dass nur die Routen mit diesem Front-End-Host verwendet werden. Anschließend filtert Front Door die Routingregeln basierend auf dem Anforderungspfad. Hierbei wird eine ähnliche Logik wie bei den Front-End-Hosts verwendet:

1. Es wird nach allen Routingregeln mit einem exakten Treffer für den Pfad gesucht.
2. Wenn kein exakter Pfad vorhanden ist, wird nach Routingregeln mit einem übereinstimmenden Platzhalterpfad gesucht.
3. Wenn keine Routingregeln mit einem übereinstimmenden Pfad gefunden werden, wird die Anforderung abgelehnt und eine HTTP-Antwort „400: Ungültige Anforderung“ gesendet.

>[!NOTE]
> Alle Pfade ohne Platzhalter gelten als exakte Treffer für den Pfad. Selbst wenn ein Pfad mit einem Schrägstrich endet, wird er dennoch als exakter Treffer betrachtet.

Sehen wir uns weitere Beispiele an, um dies genauer zu erläutern:

| Routingregel | Front-End-Host    | `Path`     |
|-------|---------|----------|
| Ein     | www\.contoso.com | /        |
| B     | www\.contoso.com | /\*      |
| C     | www\.contoso.com | /ab      |
| D     | www\.contoso.com | /abc     |
| E     | www\.contoso.com | /abc/    |
| F     | www\.contoso.com | /abc/\*  |
| G     | www\.contoso.com | /abc/def |
| H     | www\.contoso.com | /path/   |

In dieser Konfiguration würde sich folgende Treffertabelle ergeben:

| Eingehende Anforderung    | Übereinstimmende Route |
|---------------------|---------------|
| www\.contoso.com/            | Ein             |
| www\.contoso.com/a           | B             |
| www\.contoso.com/ab          | C             |
| www\.contoso.com/abc         | D             |
| www\.contoso.com/abzzz       | B             |
| www\.contoso.com/abc/        | E             |
| www\.contoso.com/abc/d       | F             |
| www\.contoso.com/abc/def     | G             |
| www\.contoso.com/abc/defzzz  | F             |
| www\.contoso.com/abc/def/ghi | F             |
| www\.contoso.com/path        | B             |
| www\.contoso.com/path/       | H             |
| www\.contoso.com/path/zzz    | B             |

>[!WARNING]
> </br> Wenn keine Routingregeln für einen exakt übereinstimmenden Front-End-Host mit einem Routingpfad zur Abdeckung aller Fälle (`/*`) vorhanden sind, gibt es für keine Routingregel einen Treffer.
>
> Beispielkonfiguration:
>
> | Route | Host             | `Path`    |
> |-------|------------------|---------|
> | Ein     | profile.contoso.com | /api/\* |
>
> Treffertabelle:
>
> | Eingehende Anforderung       | Übereinstimmende Route |
> |------------------------|---------------|
> | profile.domain.com/other | Keine. Fehler 400: Ungültige Anforderung |

### <a name="routing-decision"></a>Routingentscheidung

Nachdem Azure Front Door Standard/Premium eine einzelne übereinstimmende Routingregel ermittelt hat, muss bestimmt werden, wie die Anforderung verarbeitet werden soll. Wenn Azure Front Door Standard/Premium für die übereinstimmende Routingregel über eine zwischengespeicherte Antwort verfügt, wird die Anforderung an den Client gesendet. Als Nächstes wird ausgewertet, ob Sie über einen Regelsatz für die übereinstimmende Routingregel verfügen. Wenn kein Regelsatz definiert ist, wird die Anforderung unverändert an den Back-End-Pool weitergeleitet. Andernfalls wird der Regelsatz in der konfigurierten Reihenfolge ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über das [Erstellen einer Instanz von Front Door Standard/Premium](create-front-door-portal.md).
