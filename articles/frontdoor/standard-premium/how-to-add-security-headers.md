---
title: Konfigurieren von Sicherheitsheadern mit dem Regelsatz in Azure Front Door Standard/Premium (Vorschau)
description: Dieser Artikel enthält Anleitungen zum Konfigurieren von Sicherheitsheadern mithilfe eines Regelsatzes.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: c08ba57f43969bb2f0ee9c66b6cb4e92879ed258
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "101097968"
---
# <a name="configure-security-headers-with-azure-front-door-standardpremium-preview-rule-set"></a>Konfigurieren von Sicherheitsheadern mit dem Regelsatz in Azure Front Door Standard/Premium (Vorschau)

> [!Note]
> Diese Dokumentation ist für Azure Front Door Standard/Premium (Vorschau) gedacht. Suchen Sie nach Informationen zu Azure Front Door? Informationen finden Sie [hier](../front-door-overview.md).

In diesem Artikel wird gezeigt, wie Sie Sicherheitsheader implementieren, um browserbasierte Sicherheitsrisiken zu vermeiden, z. B. HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy oder X-Frame-Options. Sicherheitsbasierte Attribute können auch mit Cookies definiert werden.

Das folgende Beispiel zeigt Ihnen, wie Sie einen Content-Security-Policy-Header zu allen eingehenden Anforderungen hinzufügen, die dem Pfad in der Route entsprechen. Hier wird in unseren Anwendungen nur die Ausführung von Skripts von unserer vertrauenswürdigen Website **https://apiphany.portal.azure-api.net** zugelassen.

> [!IMPORTANT]
> Azure Front Door Standard/Premium (Vorschau) befindet sich derzeit in der öffentlichen Vorschauversion.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Voraussetzungen

* Bevor Sie Sicherheitsheader konfigurieren können, müssen Sie eine Front Door-Instanz erstellen. Weitere Informationen finden Sie unter [Quickstart: Erstellen einer „Front Door“](create-front-door-portal.md).
* Informieren Sie sich über das [Konfigurieren eines Regelsatzes](how-to-configure-rule-set.md), wenn Sie das Feature „Regelsatz“ noch nicht verwendet haben.

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Hinzufügen eines Content-Security-Policy-Headers im Azure-Portal

1. Wechseln Sie zum Azure Front Door Standard/Premium-Profil, und wählen Sie unter **Einstellungen** die Option **Regelsatz** aus.

1. Klicken Sie auf **Hinzufügen**, um einen neuen Regelsatz hinzuzufügen. Legen Sie einen **Namen** für den Regelsatz und dann einen **Namen** für die Regel fest. Wählen Sie **Aktion hinzufügen** und dann **Antwortheader** aus.

1. Legen Sie den Operator auf **Append** fest, damit dieser Header als Antwort auf alle für diese Route eingehenden Anforderungen hinzugefügt wird.

1. Fügen Sie den Headernamen **Content-Security-Policy** hinzu, und definieren Sie die Werte, die dieser Header akzeptieren soll. In diesem Szenario haben wir *„script-src ‚self‘ https://apiphany.portal.azure-api.net“* ausgewählt.

1. Wenn Sie der Konfiguration alle gewünschten Regeln hinzugefügt haben, müssen Sie den Regelsatz einer Route zuordnen. Dieser Schritt ist *erforderlich*, damit der Regelsatz aktiv werden kann. 

> [!NOTE]
> In diesem Szenario haben wir der Regel keine [Übereinstimmungsbedingungen](concept-rule-set-match-conditions.md) hinzugefügt. Für alle eingehenden Anforderungen, die dem in der zugeordneten Route definierten Pfad entsprechen, wird diese Regel angewendet. Wenn Sie die Regel nur auf eine Teilmenge dieser Anforderungen anwenden möchten, fügen Sie dieser Regel unbedingt Ihre spezifischen **Übereinstimmungsbedingungen** hinzu.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

### <a name="deleting-a-rule"></a>Löschen einer Regel

In den vorherigen Schritten haben Sie den Content-Security-Policy-Header mit einem Regelsatz konfiguriert. Wenn Sie eine Regel nicht mehr wünschen, können Sie den Namen des Regelsatzes und dann „Regel löschen“ auswählen. 

### <a name="deleting-a-rule-set"></a>Löschen eines Regelsatzes

Wenn Sie einen Regelsatz löschen möchten, müssen Sie zuvor seine Zuordnung zu allen Routen aufheben. Eine ausführliche Anleitung zum Löschen eines Regelsatzes finden Sie unter [Konfigurieren eines Regelsatzes](how-to-configure-rule-set.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren einer Web Application Firewall für Front Door finden Sie unter [Azure Web Application Firewall für Azure Front Door](../../web-application-firewall/afds/afds-overview.md).
