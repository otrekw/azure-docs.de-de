---
title: 'Zusammenarbeit mit anderen: QnA Maker'
description: ''
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 967fdae49f904f6c1cb450b637a8dbc5c481b135
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776883"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Zusammenarbeiten mit anderen Autoren und Redakteuren

Nutzen Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) für Ihre QnA Maker-Ressource, um mit anderen Autoren und Redakteuren zusammenzuarbeiten.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>Zugriff auf die QnA Maker-Ressource

Alle Berechtigungen werden durch die Berechtigungen gesteuert, die für die QnA Maker-Ressource festgelegt wurden. Bei diesen Berechtigungen handelt es sich um Berechtigungen für Lese-, Schreib-, Veröffentlichungs- und Vollzugriff.

Dieses RBAC-Feature umfasst Folgendes:
* Azure Active Directory (AAD) ist vollständig mit der schlüsselbasierten Authentifizierung für Besitzer und Mitwirkende abwärtskompatibel. Kunden können in ihren Anforderungen entweder die schlüsselbasierte Authentifizierung oder die RBAC-basierte Authentifizierung verwenden.
* Da die Steuerung auf der Ressourcenebene und nicht auf der Ebene der Wissensdatenbank erfolgt, können Autoren und Redakteure schnell allen Wissensdatenbanken in der Ressource hinzugefügt werden.

## <a name="access-is-provided-by-a-defined-role"></a>Zugriff durch eine definierte Rolle

[!INCLUDE [RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Authentifizierungsfluss

Das folgende Diagramm zeigt den Ablauf für die Anmeldung beim QnA Maker-Portal und die Verwendung der Erstellungs-APIs aus Sicht des Autors:

> [!div class="mx-imgBorder"]
> ![Dieses Diagramm zeigt den Ablauf für die Anmeldung beim QnA Maker-Portal und die Verwendung der Erstellungs-APIs aus Sicht des Autors.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Schritte|Beschreibung|
|--|--|
|1|Vom Portal wird ein Token für die QnA Maker-Ressource abgerufen.|
|2|Vom Portal wird die entsprechende QnA Maker-Erstellungs-API (APIM) aufgerufen und das Token übergeben (anstelle von Schlüsseln).|
|3|Das Token wird von der QnA Maker-API überprüft.|
|4 |Von der QnA Maker-API wird der QnAMaker-Dienst aufgerufen.|

Wenn Sie die [Erstellungs-APIs](../How-To/collaborate-knowledge-base.md) aufrufen möchten, informieren Sie sich ausführlicher über die Einrichtung der Authentifizierung.

## <a name="authenticate-by-qna-maker-portal"></a>Authentifizieren über das QnA Maker-Portal

Wenn Sie das QnA Maker-Portal verwenden, um Inhalte zu erstellen und mit anderen zusammenzuarbeiten, werden sämtliche Zugriffsberechtigungen durch das QnA Maker-Portal verwaltet, nachdem Sie [der Ressource die entsprechende Rolle für einen Projektmitarbeiter hinzugefügt](../How-To/collaborate-knowledge-base.md) haben.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Authentifizieren über QnA Maker-APIs und -SDKs

Wenn Sie zum Erstellen von Inhalten sowie für die Zusammenarbeit mit anderen die APIs verwenden (entweder per REST oder über die SDKs), müssen Sie zur Verwaltung der Authentifizierung [einen Dienstprinzipal erstellen](../../authentication.md#assign-a-role-to-a-service-principal).

## <a name="next-step"></a>Nächster Schritt

* Entwerfen Sie eine Wissensdatenbank für [Sprachen](design-language-culture.md) und für [Clientanwendungen](integration-with-other-applications.md).
